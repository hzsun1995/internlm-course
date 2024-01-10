## 1.RAG & finetune
RAG：基于检索，占用上下文tokens多，可实时更新；
finetune:基于微调，成本高，不能实时更新。

### 2.RAG pipeline：
![image](https://github.com/hzsun1995/internlm-course/assets/136775620/9b7b1ec6-e2f3-411e-9dd5-daac7ecc1344)

input -> 向量化input(sentence transformer) -> 与Vector DB数据匹配相似度(Chroma Vector DB ) -> 相关知识prompt

input + prompt -> 新的prompt -> LLM -> output

