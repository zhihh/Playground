# RAG Tutorial
> 做好RAG只要一星期，但是上线要一年。

## 为什么需要RAG
不会私域问题，无法用于企业。

LLM的训练数据来自于公开的互联网，据最近的人工智能大会报告，目前互联网上高质量可用的公开数据已经被用光了。

### 应用场景
AI客服：
企业可以用于，智能客服，以前的客户听不懂又说不清，但现在是大模型不是听不懂说不清，而是不知道。

政策查询：

AI搜索：

## RAG的三个核心
### 知识库
* 收集知识：数据库、数据格式、
*  切分文档：保证split的语义连贯性。统一性和完整性。太长语义不统一，太短语义不完全。
    * 先按句子切分（保证统一性），相同语义的句子聚类（保证完整性）
*  文本向量
    * 去重词袋法，不去重词袋法，tf-idf（稀疏向量）
    * 词嵌入（稠密向量）[Huggingface Embedding Leaderboard](http://mteb-leaderboard.hf.space/?benchmark_name=MTEB%28Multilingual%2C+v2%29)
*  向量存储：Chroma, FAISS

### 检索
1. 基于文本相似度的检索（余弦相似度、欧式距离）（如何结合这两个呢）
2. 基于关键字的检索 关键字映射原文档或chunk

### 回答
1. 提示词构造
2. 模型选择：开源本地、闭源云端

## 10种优化技巧
> RAG不是算法问题，是工程问题
### 检索优化
#### small to big
小文本块映射到大文本块
检索时：小文本块
生成时：大文本块

* 摘要检索
* 子问题检索
* 句子窗口检索

#### 多路召回（效率）
关键词查询、相似度查询等多种查询召回的文档，进行基于位置和出现次数的评分排序，选择前列的

#### Rerank （准确度）
将选择出来的文档，和问题，给到Rerank模型（Cross-Encoder模型），进行评分，以Rerank文档。
去Huggingface找模型排名

### 生成优化
#### refine模式
多次调用，逐个注入文档块。
#### 多文档场景下的refine模式
把文档块再分组打包（聚类、基于相似性分组）
#### tree_summarize
树形归并法

### 检索优化
#### 提问改写 （基于规则的改写/基于模型的改写）
* 提问不规范
* 多轮对话 （指代消解）
* 复杂提问

#### 利用元数据
检索前过滤，检索中评估相关性、检索后重排、生成时提升用户体验（提供参考文档来源）

## 如何评估RAG
### RAG的评估指标
准确率：直接站在用户视角，看答案是否符合实际情况
忠实度：生成的内容是否忠于提供的上下文或背景信息
召回率、精确率、F1：大模型的容错性不错，因此着重提高召回率
### 评估方法
* 人工评估
* 模型自动评估
    * 问题/作答
    * GT/作答

## 项目经理的视角
### 项目启动的N个问题
* 可行性
* 复杂度
* 开发周期
* 人力成本
* 预期成效
### 如何规划技术工作
小步快跑，快速迭代
构造流程，先得到base版本，确定评估体系，逐步迭代与优化。不要陷在某个细节某个技术里面

> 这不是结束，这甚至不是结束的开始，这只是开始的结束。--丘吉尔

# Error Record
### 连不上 langsmith
报错：`warning:  Cannot uninstall package; `RECORD` file not found at: .venv/lib/python3.12/site-packages/urllib3-2.4.0.dist-info/RECORD`

参考[Cannot uninstall numpy, RECORD file not found](https://stackoverflow.com/questions/68886239/cannot-uninstall-numpy-1-21-2-record-file-not-found)

* Delete the particular folder with the version number from the site-pakages (not the folder that has Python modules and files)
