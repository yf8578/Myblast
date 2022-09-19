# practice-blast
## 前言
今天老师安排了任务，里面有一步是通过blast输入“03_去除rRNA/rRNA参考序列/ncbi_homo_rRNA0701.fasta”，筛选得到与rRNA高度相似的序列（部分区域相似度100%）
好嘛，我想着有现成的网页工具，直接在ncbi上做就好咯，结果发现要比对的序列太多，做不了😭，没得办法，只能通过本地blast进行比对了。
由于之前没做过这样的任务，特此记录。
## blast简介
BLAST（Basic Local Alignment Search Tool）是一套在蛋白质数据库或DNA数据库中进行相似性比较的分析工具。BLAST程序能迅速与公开数据库进行相似性序列比较。
BLAST结果中的得分是对一种对相似性的统计说明。
<div align=center>
<img src="https://user-images.githubusercontent.com/71922803/190534573-6ba16a8e-24c1-44a9-8f4e-1e392a392135.png" width="240">
</div>
BLAST 采用一种局部的算法获得两个序列中具有相似性的序列。

`BLAST软件能够将一个目标蛋白或核苷酸序列(称为查询query)与一个数据库进行比较，并识别某个特定阈值以上的与目标序列相似的库序列。在线的BLAST功能只需要将序列输入，然后BLAST，最后会给出相似的库序列(按照同源性高低排列)。在线BLAST明显存在两个缺陷：1.目标序列BLAST后的得到的库序列是多个物种的，如果我们只是要特定物种的话，还需要再自己找。2.如果目标序列有很多，则需要执行很多次，效率低。而本地化的BLAST+能很好的解决上面两个问题。
`
## 下载安装
下载地址：ftp://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/
## 建立数据库
BLAST数据库分为两类，核酸数据库和氨基酸数据库，可以用`makeblastbd`创建。可以用help参数简单看下说明。
```
$ makeblastdb -help
USAGE
  makeblastdb [-h] [-help] [-in input_file] [-input_type type]
    -dbtype molecule_type [-title database_title] [-parse_seqids]
    [-hash_index] [-mask_data mask_data_files] [-mask_id mask_algo_ids]
    [-mask_desc mask_algo_descriptions] [-gi_mask]
    [-gi_mask_name gi_based_mask_names] [-out database_name]
    [-max_file_sz number_of_bytes] [-logfile File_Name] [-taxid TaxID]
    [-taxid_map TaxIDMapFile] [-version]
-dbtype <String, `nucl', `prot'>
```
使用命令makeblasted，参考下面给出的命令和上面的帮助信息进行操作
```
cd /my/NCBI
/share/app/blast/2.11.0/bin/makeblastdb -in GCF_000001405.40_GRCh38.p14_genomic.fa -parse_seqids -blastdb_version 5 -dbtype nucl
```
<div align=center>
<img src="https://user-images.githubusercontent.com/71922803/190936389-70d43be0-f925-4699-92b2-62a6329051d1.png" width="450">
</div>
