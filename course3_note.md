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
    Eg.检索问答链，覆盖实现了RAG(检索增强生成)的全部流程；

##### 3.1基于langchian搭建RAG应用：
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/72ea4142-eb99-4f0d-b902-e20e91bdf106)

知识库：
- unstructed loader:将不同文件都同一转化为纯文本格式；
- text spliter:将纯文本分割成chunks；
- sentence transformer:统一转化为向量格式，存储在Chroma的Vector DB中；

用户输入：
- 类似，看图即可；

#### 3.2构建向量数据库：
加载源文件→文档分块→文档向量化
- 确定源文件类型，针对不同类型源文件选用不同的加载器
·核心在于将带格式文本转化为无格式字符串
- 由于单个文档往往超过模型上下文上限，我们需要对加载的文档进行切分
·一般按字符串长度进行分割
·可以手动控制分割块的长度和重叠区间长度
- 使用向量数据库来支持语义检索，需要将文档向量化存入向量数据库
·可以使用任一—种Embedding模型来进行向量化
·可以使用多种支持语义检索的向量数据库，一般使用轻量级的Chroma
