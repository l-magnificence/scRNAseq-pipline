# scRNAseq-pipline
## [SRAToolkit install](https://github.com/ncbi/sra-tools/wiki/02.-Installing-SRA-Toolkit)
```
mkdir ~/sratoolkit
cd ~/sratoolkit
axel http://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/current/sratoolkit.current-ubuntu64.tar.gz
tar -vxzf sratoolkit.current-ubuntu64.tar.gz
```

## SRA file to fastq 
### 单端测序 
```
cd ~/data
~/SRAToolkit/sratoolkit.current-ubuntu64/bin/fastq-dump SRR11268104 -O ./ # 结果生成：SRR11268104.fastq
~/SRAToolkit/sratoolkit.current-ubuntu64/bin/fastq-dump --fasta SRR11268104 -O ./ #（结果生成：SRR11268104.fasta）
```

### 双端测序 
```
cd ~/data
~/SRAToolkit/sratoolkit.current-ubuntu64/bin/fastq-dump SRR11268104 --split-3 -O ./ # 结果生成：SRR11268104_1.fastq，SRR11268104_2.fastq）
~/SRAToolkit/sratoolkit.current-ubuntu64/bin/fastq-dump SRR11268104 --split-3 --gzip -O ./ #（结果生成：SRR11268104_1.fastq.gz， SRR11268104_2.fastq.gz）
```

## cellranger
```
/Shared_Software/Single_cell/cellranger-4.0.0/bin/cellranger count \
	--id Mouse_10x \
	--fastqs=~/data/fastq/ \
	--sample=Med2_Peri \
	--localcores=40 \
	--localmem=100 \
	--transcriptome=/Shared_Software/ref_genome/refdata-gex-GRCh38-2020-A
  ```
