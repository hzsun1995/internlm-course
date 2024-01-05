# 课程笔记
书生·浦语大模型全链路开源开放体系-陈恺

### 1.GPT发展路径：
GPT-1(2018.6) $\rightarrow$ GPT-2(2019.2) $\rightarrow$ GPT-3(2020.5) $\rightarrow$ GPT-3.5(2022.3) $\rightarrow$ GPT-4(2023.3)

### 2.专用模型&通用模型
通用：一个模型应对多种任务、多种模态；
专用：针对特定任务，一个模型解决一个问题；

### 3.浦语开发历程：
InternLM 千亿级参数规模LLM发布(2023.6) $\rightarrow$ InternLM 升级，支持8K上下文、26种语言，同时发布7B版本模型和全链条开源工具(2023.7) $\rightarrow$ 书生万卷1.0发布(训练语料库)(2023.8) $\rightarrow$ InternLM-Chat-7B发布，开源Lagent(LLM2Agent)(2023.8) $\rightarrow$ InternLM升级至132B(2023.8) $\rightarrow$ InternLM-20B开源，升级开源工具链(2023.9)

### 4.InternLM不同版本对比：
$\left\{
\begin{aligned}
&  7B:参数规模小;1000B tokens训练;8K上下文;支持工具调用；\\
& ---社区可用最低成本规模模型\\
& 20B:支持工具调用；降低了推理计算量但提高了推理能力；4K上下文，推理时可外推到16K\\
& ---商业场景可开发定制高精度较小模型规模;\\
& 123B:参数量大，模型强大；推理能力强；精准API调用；构建Agent能力强；\\
& ---通用大语言模型能力全面覆盖千亿模型规模
\end{aligned}
\right.$

### 5.模型到应用：
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/1aac2a92-4967-49fe-b65a-28682a97f43d)


### 6.LLM全链条开源框架体系：
- 数据：书生·万卷，2TB数据，多模态数据；
- 预训练： InternLM-Trian ，速度可达3600 tokens/sec/gpu；
- 微调： XTuner ,支持全参、LoRA等等；
- 部署：LMDeploy，全链条部署，性能领先，每秒可生成2000+tokens；
- 评测：OpenCompass，评测框架，可复现80套评测数据集，40万道题目；
- 应用：Lagent/AgentLego，Agent框架。


Cite:https://www.bilibili.com/video/BV1Rc411b7ns/?vd_source=886e6c619dfbedf070eccca3bc89c2ef
