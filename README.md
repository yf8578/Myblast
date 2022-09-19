# practice-blast
## 前言
今天老师安排了任务，里面有一步是通过blast输入“03_去除rRNA/rRNA参考序列/ncbi_homo_rRNA0701.fasta”，筛选得到与rRNA高度相似的序列（部分区域相似度100%）
好嘛，我想着有现成的网页工具，直接在ncbi上做就好咯，结果发现要比对的序列太多，做不了😭，没得办法，只能通过本地blast进行比对了。
由于之前没做过这样的任务，在网上也查了一些资料，特此记录（只是符合自己的需求）。
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
### 自己建立数据库
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
`-dbtype` 是必选参数，nucl, prot，二选一，核酸序列或者氨基酸序列库

`-in` 用于构建搜索库的fasta文件

`-input_type` 默认为fasta文件，其他支持文件asn1_bin,asn1_txt, blastdb

`-parse_seqids` 添加该参数，结果文件将多三个文件，不加默认为3个结果文件

`-out`输出文件的前缀
执行上述代码后将生成后缀为nin,nhr,nsq,nsi,nsd,nog六个文件，至此，自定义搜索库文件生成完成
```
/share/app/blast/2.11.0/bin/makeblastdb -in GCF_000001405.40_GRCh38.p14_genomic.fa -parse_seqids -blastdb_version 5 -dbtype nucl
```
得到的结果入如下
<div align=center>
<img src="https://user-images.githubusercontent.com/71922803/190936389-70d43be0-f925-4699-92b2-62a6329051d1.png" width="450">
</div>
### 下载现有的数据库

```
# 有哪些可供下载的blast数据库？
~/tools/ncbi-blast-2.7.1+/bin$ perl update_blastdb.pl --showall
# 下载NR数据库
~/tools/ncbi-blast-2.7.1+/bin$ nohup perl update_blastdb.pl --decompress nr >out.log 2>&1 &
```

########blast输入的query是fasta格式，db是要给fasta，但是在相同文件夹下必须有blast的索引

```
/share/app/blast/2.11.0/bin/blastn -query /jdfssz1/ST_HEALTH/P20H10200N0033/Wangyingying/cfRNA/pipeline-test/adapter_test/rRNA.4549.fasta -db /jdfssz1/ST_HEALTH/P20H10200N0033/Wangyingying/ref/NCBI/GCF_000001405.40_GRCh38.p14_genomic.fa -out /jdfssz1/ST_HEALTH/P20H10200N0033/Wangyingying/cfRNA/pipeline-test/adapter_test/rRNA.4549-result.txt
```
```
-query: 输入文件路径及文件名;
-out: 输出文件路径及文件名;
-db: 格式化后的数据库路径及数据库名;
-outfmt: 输出文件格式，总共有12种格式，6是tabular格式对应blast的m8格式。
-evalue: 设置输出结果的e-value值;
-num_descriptions: 显示一行描述的结果（类似网页版的结果列表）输出的条数, 默认是500条，不适用于outfmt>4，与max_target_seqs参数不兼容;
-num_alignments: 显示数据库序列的对比结果的数目（具体的比对结果），默认是250个
-max_target_seqs 可显示的比对序列的最大数，默认值是500，不适用于outfmt<=4，与-num_descriptions及-num_alignments不兼容;
-num_threads 线程数
```
![image](https://user-images.githubusercontent.com/71922803/190937246-d8d669e5-b7ff-405e-93a5-080c3a18578f.png)
