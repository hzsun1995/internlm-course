# 大模型评测

## 1.统一评测的必要性：
了解模型能力、边界，提供优化方向；
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/50d42206-9bb4-43a2-9c0e-6ea334de2fda)

## 2.测试哪些方面：
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/d021f435-fe97-4f59-9389-7f296377a6b8)

## 3.怎么评测：
base chat的区分：

base:在测试时，单句加入一些额外的指令；

chat:直接对话即可；

## 4.客观评测：
基于正则表达式的方式，提取回答；
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/8cd4ce17-8ccf-4eeb-b66e-6666e4782791)

## 5.主观评测：
可以用GPT-4等模型评测回复的质量；

相关工作：JudgeLM
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/c43e3c37-e4b4-49ba-a028-f5e3e52c44a0)

## 6.用prompt的方式测试敏感性：
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/4057f76c-b6e9-49ad-8e5f-734fd8cee410)
预期模型在以上5个问题上都能答对；如果模型答错了，说明它敏感性就比较大；

## 7.主流LLM评测框架：
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/d635957b-8207-497f-a9e7-ebbc16f88a9e)

## 8.OpenCompass：
在各个维度上进行了一个整合；
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/ac676596-c210-4166-b725-99d5352b4cd3)
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/f2d37b87-8804-404c-9ea0-13fdf75d9ea7)

### 评测流水线设计：
对用户开发和使用非常友好；
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/b78e325d-8592-4305-a22d-4437ed0c2699)

## 9.前沿探索：

#### MMBench:多模态
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/2aba2e46-c881-4d50-bd1b-d38f88964d13)

#### LawBench:法律领域
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/cdb8d249-b238-4fc6-a863-d8729773f73c)

#### MedBench:医疗领域
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/ef984a38-df3f-47c7-bbc9-78fce20da44a)

## 10.总结：
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/646e5721-7a15-40ad-bdca-3f7a8c3ddf7b)

## 代码：
新东西：主观评测的设置；具体参考文档；

关键：如果需要模型的生成有随机性，可以在`generation_kwargs`参数中设置；如do_sample，temoerature，top_K等等；

客观评测中，不会设置此类参数。如需设置，可以去huggingface上看官方的模型参数设置；
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/98f6cfe5-a6c1-44fb-b698-645d7e3fc6c9)

