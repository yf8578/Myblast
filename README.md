# practice-blast
## 前言
今天老师安排了任务，里面有一步是通过blast输入“03_去除rRNA/rRNA参考序列/ncbi_homo_rRNA0701.fasta”，筛选得到与rRNA高度相似的序列（部分区域相似度100%）
好嘛，我想着有现成的网页工具，直接在ncbi上做就好咯，结果发现要比对的序列太多，做不了😭，没得办法，只能通过本地blast进行比对了。
由于之前没做过这样的任务，特此记录。
## blast简介
BLAST（Basic Local Alignment Search Tool）是一套在蛋白质数据库或DNA数据库中进行相似性比较的分析工具。BLAST程序能迅速与公开数据库进行相似性序列比较。
BLAST结果中的得分是对一种对相似性的统计说明。
![image](https://user-images.githubusercontent.com/71922803/190534573-6ba16a8e-24c1-44a9-8f4e-1e392a392135.png|width=100)
<img src="https://user-images.githubusercontent.com/71922803/190534573-6ba16a8e-24c1-44a9-8f4e-1e392a392135.png" width="48">

BLAST 采用一种局部的算法获得两个序列中具有相似性的序列。

`BLAST软件能够将一个目标蛋白或核苷酸序列(称为查询query)与一个数据库进行比较，并识别某个特定阈值以上的与目标序列相似的库序列。在线的BLAST功能只需要将序列输入，然后BLAST，最后会给出相似的库序列(按照同源性高低排列)。在线BLAST明显存在两个缺陷：1.目标序列BLAST后的得到的库序列是多个物种的，如果我们只是要特定物种的话，还需要再自己找。2.如果目标序列有很多，则需要执行很多次，效率低。而本地化的BLAST+能很好的解决上面两个问题。
`
## 
