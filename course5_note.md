# LMDeploy
- 大模型部署背景
- LMDeploy简介
- 动手实践环节

# 1.大模型部署背景
## (1)模型部署
### 定义:
将训练好的模型在特定软硬件环境中启动的过程，使模型能够接收输入并返回预测结果。为了满足性能和效率的需求，常常需要对模型进行优化，例如模型压缩和硬件加速
### 产品形态:
- 云端、边缘计算端、移动端
### 计算设备
- CPU、GPU、NPU、TPU等

## (2)大模型特点
### 内存开销巨大
庞大的参数量。7B模型仅权重就需要14+G内存采用自回归生成 token，需要缓存Attention的k/v,带来巨大的内存开销
### 动态shape
- 请求数不固定
- Token逐个生成，且数量不定相
### 对视觉模型，LLM结构简单
- Transformers结构，大部分是decoder-only

## (3)大模型部署挑战
### 设备
- 如何应对巨大的存储问题?低存储设备（消费级显卡、手机等）如何部署?
### 推理
- 如何加速token 的生成速度
- 如何解决动态shape，让推理可以不间断如何有效管理和利用内存
### 服务
- 如何提升系统整体吞吐量?
- 对于个体用户，如何降低响应时间?

## (4)大模型部署方案
### 技术点
- 模型并行
- transformer计算和访存优化
- 低比特量化
- Continuous Batch
- Page Attention
- ......
### 方案
- huggingface transformers
- 专门的推理加速框架
### 云端
lmdeploy、vllm、tensorrt-llm、deepspeed
### 移动端
llama.cpp、vllm、mlc-llm

# 2.LMDeploy简介：
LMDeploy是 LLM在英伟达设备上部署的全流程解决方案。包括模型轻量化、推理和服务。
- 项目地址: https://github.com/InternLM/lmdeploy
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/56cd201a-56da-463a-8f5e-6b43938378c0)

## （1）高效推理引擎
- 持续批处理技巧
- Blocked k/v cache
- 深度优化的计算kernels
- 动态分割与融合
## （2）完备易用的工具链
- 量化、推理、服务全流程
- 无缝对接opencompass评测推理
- 多维度推理速度评测工具

### （3）推理性能：
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/9fb2c4be-ae27-4029-9409-422579ffd461)

## （4）核心功能 - 量化
### Q：为什么要做量化？
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/25cb5dce-18dc-4917-8ed7-fb827585eab2)

上表是量化前的，下表是量化后的（weight int4 / kv cache int8）

### Q：为什么做 weight only 量化？
#### 两个基本概念
- 计算密集(compute-bound):

推理的绝大部分时间消耗在数值计算上;

针对计算密集场景，可以通过使用更快的硬件计算单元来提升计算速度，比如量化为W8A8使用INT8 Tensor Core来加速计算。
- 访存密集(memory-bound):

推理时，绝大部分时间消耗在数据读取上;

针对访存密集型场景，一般是通过提高计算访存比来提升性能。

#### LLM是典型的访存密集型任务
常见的 LLM 模型是 Decoder Only架构。推理时大部分时间消耗在逐Token生成阶段(Decoding 阶段)，是典型的访存密集型场景。

如下图，A100的FP16峰值算力为312 TFLOPS，只有在 Batch Size达到128这个量级时，计算才成为推理的瓶颈，但由于LLM模型本身就很大，推理时的KV Cache 也会占用很多显存，还有一些其他的因素影响（如Persistent Batch)，实际推理时很难做到128这么大的 Batch Size。
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/f9c2e671-b56a-4e3e-b1f1-af87ec369d1a)

#### Weight Only量化一举多得
4bit Weight Only量化，将FP16的模型权重量化为INT4，访存量直接降为FP16模型的1/4，大幅降低了访存成本，提高了Decoding 的速度。

加速的同时还节省了显存，同样的设备能够支持更大的模型以及更长的对话长度。

##### 注意：只是存的时候量化，计算的时候要进行反量化，即反量化成fp16再进行计算。反量化也会耗时，这个过程在kernal中完成。

### Q：怎么做Weight Only量化？
- LMDeploy使用MIT HAN LAB开源的AWQ算法，量化为4bit模型
- 推理时，先把4bit权重，反量化回FP16 (在 Kernel内部进行，从Global Memory读取时仍是4bit)，依旧使用的是FP16计算
- 相较于社区使用比较多的GPTQ 算法，AWQ的推理速度更快，量化的时间更短，下图为二者的对比。
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/6297db79-dd00-4028-8a6d-ffedcbd2cf0d)

##### 核心思想：观察到矩阵计算其中有一部分参数是很重要的参数，可以不量化，量化其他参数，则性能得到了极大的保留，显存占用极大下降；
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/0aa5f72e-4eee-4a2d-9b60-83b7fadfdff0)

## （5）核心功能 - 推理引擎 Turbomind
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/6ae7fc56-e6bd-474e-83e4-10c4600897a1)

