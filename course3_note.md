### 1.RAG & finetune
RAG：基于检索，占用上下文tokens多，可实时更新；(检索增强生成)
finetune:基于微调，成本高，不能实时更新。

### 2.RAG pipeline：
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/9b7b1ec6-e2f3-411e-9dd5-daac7ecc1344)

input -> 向量化input(sentence transformer) -> 与Vector DB数据匹配相似度(Chroma Vector DB ) -> 相关知识prompt

input + prompt -> 新的prompt -> LLM -> output

### 3.langchain 简介：
LangChain通过为各种LLM提供通用接口来简化应用程序的开发流程，帮助开发者自由构建LLM应用。LangChain的核心组成模块:
- 链(Chains):将组件组合实现端到端应用，通过一个对象封装实现一系列LLM操作；
    Eg.<font color='pink'>检索问答链</font>，覆盖实现了RAG(检索增强生成)的全部流程；
