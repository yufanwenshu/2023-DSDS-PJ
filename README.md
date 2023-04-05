# 总览
共分为3个部分：
1. WeiboSentimentClassification：微博评论情感分类模型
2. DepressionAnalysis：抑郁识别模型
3. WeiboSpyder：微博爬虫

# 各部分说明
## 1. WeiboSentimentClassification
该部分在`weibo_senti_100k`数据集上建立了一个基于LSTM神经网络的新浪微博评论情感二分类模型。

### 1.1 数据集：weibo_senti_100k
- 下载地址：[百度网盘](!https://pan.baidu.com/s/1DoQbki3YwqkuwQUOj64R_g)
- 数据概览：包含十万多条带情感标注的新浪微博文本内容。每条数据被标记成正向(1)或负向(0)，正负向数据各5万条。
- 数据来源：[新浪微博](!https://weibo.com/)
- 原数据集：[新浪微博，情感分析标记语料共12万条](!https://download.csdn.net/download/weixin_38442818/10214750)，网上搜集，具体作者、来源不详。
- 加工处理：
    1. 把原来的两份文档合成1分csv文件
    2. 编码统一为`UTF-8`
    3. 去重

#### 字段说明：

|字段|说明|
|---|---|
|label|1表示正向评论，0表示负向评论|
|review|微博内容|
___
### 1.2 神经网络架构
bi-LSTM

## 2. DepressionAnalysis
### 2.1 数据集：Avec2019大赛数据集
AVEC-2019大赛旨在比较用于自动音频、视觉和视听健康和情感感知的多媒体处理方法和机器学习方法，并将来自不同学科的多个社区聚集在一起，特别是视听多媒体社区和那些在心理和社会科学研究表达行为；另一个目标是通过为多模态信息处理提供一个共同的基准测试集来推进健康和情感识别系统。

数据集特点：为了比较在明确的条件下自动健康和情感分析方法的相对优点，采用的数据是具有大量的完全自然行为的未分割的、非原型的和非预先选择的数据。其中抑郁严重程度(PHQ-8问卷)是从与进行临床访谈的虚拟代理人互动的患者的视听记录中评估的。DAIC数据集包含同一批患者的新记录，这次虚拟代理完全由人工智能驱动，也就是说，没有任何人工干预。这些新记录被用作DDS的测试分区，并将有助于理解没有人执行的虚拟代理对抑郁症严重程度自动评估的影响。

本模型所使用的的数据集是AVEC-2019数据集中的文本部分。

**注：该数据集为英文数据集**

### 2.2 神经网络架构
BERT fine-tune

## 3. 微博爬虫
可以爬取指定用户在指定时间段的所有微博文本信息，并将这些信息写入csv文件保存。

### 调用方式
1、首先对每一个用户初始化一个Weibo对象：

```
user = Weibo(weibo_id,since_date)
```
`user_id` 即该用户的微博id，可以是整数也可以是字符串，必填；

`since_date` 是一个整数或者字符串形式的日期，表示爬取的时间范围，默认设置为15，可以不填。since_date值可以是日期，也可以是整数。

如果是整数，代表爬取最近n天的微博，如:
```
user = Weibo(weibo_id=..., since_date=10)
```
代表爬取最近10天的微博，这个说法不是特别准确，准确说是**爬取发布时间从10天前到本程序开始执行时**之间的微博。

如果是日期，代表爬取该日期之后的微博，格式应为“yyyy-mm-dd”，如
```
user = Weibo(weibo_id=..., since_date="2018-01-01")
```

---
代表爬取从2018年1月1日到现在的微博。

2、对已经初始化好的Weibo对象，通过start()方法开始爬取。
```
user.start()
```

爬取的数据会写入`.../weibo/<username>/<user_id>.csv`文件