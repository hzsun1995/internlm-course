### xtuner微调llm
1. finetune
2. XTuner介绍
3. 8GB显卡玩转LLM
4. 动手实战环节

### 1.fintune
LLM的下游应用中，增量预训练和指令跟随是经常会用到两种的微调模式增量预训练微调。
1. 增量预训练
使用场景:让基座模型学习到一些新知识，如某个垂类领域的常识训练数据:文章、书籍、代码等
2. 指令跟随微调
使用场景:让模型学会对话模板,根据人类指令进行对话训练数据:高质量的对话、问答数据
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/2a0e4fe1-461b-415d-a20c-3e826c938104)
指令跟随微调是为了得到能够实际对话的LLM

#### （1）指令微调
介绍指令微调前，需要先了解如何使用LLM进行对话

##### 对话格式
在实际对话时，通常会有三种角色：
- System：给定一些上下文信息，比如“你是一个安全的Al助手”
- User：实际用户，会提出一些问题,比如“世界第一高峰是?”
- Assistant：根据User的输入，结合System的上下文信息，做出回答，比如“珠穆朗玛峰”
在使用对话模型时，通常是不会感知到这三种角色的
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/772620b5-73d3-4b74-a3ef-bcd5305b2b38)

##### 对话模板：
不同的llm在微调时，对话模板的格式也会有所不同。 xtuner 可以根据模型不同自动填充模板内容。
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/486c1f45-40d8-4566-b16b-cd97c5944622)

##### 训练细节：
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/e0dd1702-d5e0-43eb-a6f0-485c037a1e54)

#### （2）增量预训练
无监督；
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/9d8fea22-a534-4408-b6b8-38549cad218b)

#### （3）LoRA & QLoRA
低秩矩阵Adapter;
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/800a1d5a-2339-469c-a9b1-59c2646d52c0)
Lora:只需要保存旁路adapter的参数优化器；减小显存的占用；
Qlora:加载模型就用4bit量化去加载；参数优化器可以在cpu和gpu之间调度；（其实所有方法都能调度）

### 2.xtuner:
微调工具箱；
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/459169e2-4890-4bce-ba9e-014473a06cb7)

#### 快速一览：
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/29f70c38-8689-4b1d-b77f-b028acd9242f)
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/eeb9247d-5233-46b6-a83b-0a2a07a2df21)
- 训练完成之后，得到adapter文件（lora）;
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/d397bccd-90fe-4d9a-8ad2-395209e34aa4)
- xtuner还支持工具类模型的对话：（ms-agent再讲）
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/58f8e188-7ab9-47a8-916d-34c9152c4e82)
- 数据处理引擎：
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/5dfa6539-9e7d-4701-bc97-21da08527510)
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/075e1d86-8335-46d4-b241-3643f7684c82)
- 支持多数据的样本拼接：
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/4996a16a-c601-46ad-9ead-bf31c03726ba)
- 数据集格式：建议使用json/jsonl

### 3.8GB显卡玩转LLM
xtuner默认开启flash-attn加速训练，集成了deepspeed_zero的优化方法（不默认启动）；
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/8b4b0c32-2be5-4972-a6d1-7863c7a1e7ee)
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/c691df77-6502-4b0f-be89-4fbb3b37ae8b)
- qlora可能要用deepspeed_zero2
- 优化前后显存占用情况：
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/b1ce3c1e-3f09-4bf3-a55f-81e3b945ecbf)

