# scRNAseq-pipline
## [SRAToolkit install](https://github.com/ncbi/sra-tools/wiki/02.-Installing-SRA-Toolkit)
```
mkdir ~/sratoolkit
cd ~/sratoolkit
axel http://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/current/sratoolkit.current-ubuntu64.tar.gz
tar -vxzf sratoolkit.current-ubuntu64.tar.gz
```

## SRA file to fastq 
* ### Single end 
```
cd ~/data
~/SRAToolkit/sratoolkit.current-ubuntu64/bin/fastq-dump SRR11268104 -O ./filename/ # 结果生成：SRR11268104.fastq
~/SRAToolkit/sratoolkit.current-ubuntu64/bin/fastq-dump --fasta SRR11268104 -O ./filename/ #（结果生成：SRR11268104.fasta）
```

* ### Pair end 
```
cd ~/data
~/SRAToolkit/sratoolkit.current-ubuntu64/bin/fastq-dump SRR11268104 --split-3 -O ./filename/ # 结果生成：SRR11268104_1.fastq，SRR11268104_2.fastq）
~/SRAToolkit/sratoolkit.current-ubuntu64/bin/fastq-dump SRR11268104 --split-3 --gzip -O ./filename/ #（结果生成：SRR11268104_1.fastq.gz， SRR11268104_2.fastq.gz）
```
-3实际上指的是分成3个文件
如果结果发现只有一个文件，说明数据不是双端  
如果结果有两个文件，说明是双端文件并且数据质量比较高(没有低质量的reads或者长度小于20bp的reads)
如果结果有三个文件，说明是双端文件，但是有的数据质量不高，存在trim的结果，第三个文件的名字一般是：<srr_id>.fastq， 而且文件也不大，基本可以忽略

* ### --split-files 替代 --split-3
```
~/SRAToolkit/sratoolkit.current-ubuntu64/bin/fastq-dump SRR11268104 --split-files --gzip -O ./filename/
```
如果得到三个文件，即SRR11268104_1.fastq.gz， SRR11268104_2.fastq.gz， SRR11268104_3.fastq.gz，分别对应sample index（这个是加在Illumina测序接头上的，保证多个测序文库可以在同一个flow-cell上或者同一个lane上进行混合测序）、barcode和UMI序列、测序内容（我们所需要的真实测序内容，就是我们的每一个细胞里的RNA测序数据）  
如果得到两个文件，即SRR11268104_1.fastq.gz， SRR11268104_2.fastq.gz，那就分别对应barcode和UMI序列、测序内容

## rename the file
比如，将原来的SRR11268104_1.fastq.gz改成SRR11268104_S1_L001_I1_001.fastq.gz，依次类推，将原来_2的改成R1，将_3改成R2  
或是将原来的SRR11268104_1.fastq.gz改成SRR11268104_S1_L001_R1_001.fastq.gz，原来_2的改成SRR11268104_S1_L001_R2_001.fastq.gz
```
for i in $(cat SRR_list.txt) # SRR_list.txt 包含所有SRR序列号
do
 #echo $i
 #mv $i\_1.fastq.gz $i\_S1_L001_I1_001.fastq.gz
 mv $i\_1.fastq.gz $i\_S1_L001_R1_001.fastq.gz  #mv $i\_2.fastq.gz $i\_S1_L001_R1_001.fastq.gz 
 mv $i\_2.fastq.gz $i\_S1_L001_R2_001.fastq.gz  #mv $i\_3.fastq.gz $i\_S1_L001_R2_001.fastq.gz 
done
```

## cellranger
```
mkdir ~/data/cellranger_results
cd ~/data/cellranger_results
/Shared_Software/Single_cell/cellranger-4.0.0/bin/cellranger count 
	--id Mouse_10x  
	--fastqs=~/data/ 
	--sample=SRR11268104  
	--localcores=40 
	--localmem=100 
	--transcriptome=/Shared_Software/ref_genome/refdata-gex-mm10-2020-A
```
--id：output filename  
--fastqs：path to fastq file  
--sample：fastq.gz文件名当中_S1之前的字段  
--transcriptome：参考基因组

## velocyto
```
velocyto run10x -m /Shared_Software/ref_genome/mm10_rmsk/mm10_rmsk.gtf 
        ~/data/cellranger_results/Mouse_10x
 	/Shared_Software/ref_genome/refdata-gex-mm10-2020-A/genes/genes.gtf
```



