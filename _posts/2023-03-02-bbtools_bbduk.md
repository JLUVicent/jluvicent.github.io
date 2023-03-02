---
layout: article
title: BBtools_bbduk安装使用
tags: 宏基因组
mathjax: true
key: A-2023-03-02_2


---

<!--more-->

***

# BBTools

BBTools是一套快速、多线程的生物信息学工具，用于分析DNA和RNA序列数据。开源无限制使用

处理常见的序列如`fastq,fasta,sam`等等，需要`java`(version7 ro higher)，可以再多平台使用。

该套件包括以下模块：

> • [bbduk](https://jgi.doe.gov/data-and-tools/software-tools/bbtools/bb-tools-user-guide/bbduk-guide/) – filters or trims reads for adapters and contaminants using *k*-mers •[ bbmap](https://jgi.doe.gov/data-and-tools/software-tools/bbtools/bb-tools-user-guide/bbmap-guide/) – short-read aligner for DNA and RNA-seq data • [bbmerge](https://jgi.doe.gov/data-and-tools/software-tools/bbtools/bb-tools-user-guide/bbmerge-guide/) – merges overlapping or nonoverlapping pairs into a single reads • [reformat](https://jgi.doe.gov/data-and-tools/software-tools/bbtools/bb-tools-user-guide/reformat-guide/) – converts sequence files between different formats such as fastq and fasta

## 下载安装

[下载地址](https://sourceforge.net/projects/bbmap/)

[安装指导](https://jgi.doe.gov/data-and-tools/software-tools/bbtools/bb-tools-user-guide/installation-guide/)

![image-20230220092357751](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20230220092357751.png)

![image-20230220092548160](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20230220092548160.png)

```
# 下载文件
$ wget https://nchc.dl.sourceforge.net/project/bbmap/BBMap_39.01.tar.gz
$ cd (installation parent folder)
# 解压文件
$ tar -xvzf BBMap_(version).tar.gz
# 测试安装 cd到安装文件夹下
$ ./stats.sh in=/home/yanziming/vicent/bbmap/resources/phix174_ill.ref.fa.gz
A       C       G       T       N       IUPAC   Other   GC      GC_stdev
0.2399  0.2144  0.2326  0.3130  0.0000  0.0000  0.0000  0.4471  0.0000

Main genome scaffold total:             1
Main genome contig total:               1
Main genome scaffold sequence total:    0.005 MB
Main genome contig sequence total:      0.005 MB        0.000% gap
Main genome scaffold N/L50:             1/5.386 KB
Main genome contig N/L50:               1/5.386 KB
Main genome scaffold N/L90:             1/5.386 KB
Main genome contig N/L90:               1/5.386 KB
Max scaffold length:                    5.386 KB
Max contig length:                      5.386 KB
Number of scaffolds > 50 KB:            0
% main genome in scaffolds > 50 KB:     0.00%


Minimum         Number          Number          Total           Total           Scaffold
Scaffold        of              of              Scaffold        Contig          Contig  
Length          Scaffolds       Contigs         Length          Length          Coverage
--------        --------------  --------------  --------------  --------------  --------
    All                      1               1           5,386           5,386   100.00%
   5 KB                      1               1           5,386           5,386   100.00%
```

## 安装java依赖

```
# 地址：https://www.oracle.com/java/technologies/downloads/
# 查看系统版本号 ubuntu
$ uname -a
Linux server1 4.15.0-197-generic #208-Ubuntu SMP Tue Nov 1 17:23:37 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
$ wget https://download.oracle.com/java/19/latest/jdk-19_linux-x64_bin.tar.gz
$ tar -zxvf jdk-19_linux-x64_bin.tar.gz
# 编辑.bashrc文件，添加以下命令
JAVA_HOME=/home/yanziming/vicent/jdk-19.0.2
CLASSPATH=$JAVA_HOME/lib/
PATH=$PATH:$JAVA_HOME/bin
export PATH JAVA_HOME CLASSPATH
```

## bbduk使用

`ktrim=r`模式下，一旦在读取中匹配参考`kmer`，该`kmer`和右侧的所有碱基将被修剪，只留下左侧的碱基；这是适配器修剪的正常模式。

`k=23`：在参考基因组中存储`23-mers`,

```
# 预处理
# step1 ./bbduk.sh  in1=() in2=() out1=() out2=() ref=adapters.fa ktrim=r k=23 mink=11 hdist=1 minlen=50 tpe tbo
./bbduk.sh  in1=/home/yanziming/vicent/data_set/synthetic_metagenomic_yeast/shotgun/SRR1262938/SRR1262938_1.fastq in2=/home/yanziming/vicent/data_set/synthetic_metagenomic_yeast/shotgun/SRR1262938/SRR1262938_2.fastq out1=/home/yanziming/vicent/data_set/synthetic_metagenomic_yeast/shotgun/SRR1262938/out_SRR1262938_1.fastq out2=/home/yanziming/vicent/data_set/synthetic_metagenomic_yeast/shotgun/SRR1262938/out_SRR1262938_2.fastq ref=/home/yanziming/vicent/bbmap/resources/adapters.fa ktrim=r k=23 mink=11 hdist=1 minlen=50 tpe tbo
# step2 ./bbduk.sh  in1=() in2=() out1=() out2=() trimq=10 qtrim=r ftm=5 minlen=50
./bbduk.sh  in1=/home/yanziming/vicent/data_set/synthetic_metagenomic_yeast/shotgun/SRR1262938/out_SRR1262938_1.fastq in2=/home/yanziming/vicent/data_set/synthetic_metagenomic_yeast/shotgun/SRR1262938/out_SRR1262938_2.fastq out1=/home/yanziming/vicent/data_set/synthetic_metagenomic_yeast/shotgun/SRR1262938/out1_SRR1262938_1.fastq out2=/home/yanziming/vicent/data_set/synthetic_metagenomic_yeast/shotgun/SRR1262938/out2_SRR1262938_2.fastq trimq=10 qtrim=r ftm=5 minlen=50
# step3  ./bbduk.sh  in1=() in2=() out1=() out2=() ftl=10
./bbduk.sh in1=/home/yanziming/vicent/data_set/synthetic_metagenomic_yeast/shotgun/SRR1262938/out1_SRR1262938_1.fastq in2=/home/yanziming/vicent/data_set/synthec_metagenomic_yeast/shotgun/SRR1262938/out2_SRR1262938_2.fastq out1=/home/yanziming/vicent/data_set/synthetic_metagenomic_yeast/shotgun/SRR1262938/finre_SRR1262938_1.fastq out2=/home/yanziming/vicent/data_set/synthetic_metagenomic_yeast/shotgun/SRR1262938/finre_SRR1262938_2.fastq ftl=10
```

`shotgun`数据

![image-20230220130645498](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20230220130645498.png)

`HiC`数据

![image-20230221182226799](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20230221182226799.png)