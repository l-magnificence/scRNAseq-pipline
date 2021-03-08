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
~/SRAToolkit/sratoolkit.current-ubuntu64/bin/fastq-dump SRR11268104 -O ./filename/ # 结果生成：SRR11268104.fastq
~/SRAToolkit/sratoolkit.current-ubuntu64/bin/fastq-dump --fasta SRR11268104 -O ./filename/ #（结果生成：SRR11268104.fasta）
```

### 双端测序 
```
cd ~/data
~/SRAToolkit/sratoolkit.current-ubuntu64/bin/fastq-dump SRR11268104 --split-3 -O ./filename/ # 结果生成：SRR11268104_1.fastq，SRR11268104_2.fastq）
~/SRAToolkit/sratoolkit.current-ubuntu64/bin/fastq-dump SRR11268104 --split-3 --gzip -O ./filename/ #（结果生成：SRR11268104_1.fastq.gz， SRR11268104_2.fastq.gz）
```

## cellranger
```
mkdir ~/data/cellranger_results
cd ~/data/cellranger_results
/Shared_Software/Single_cell/cellranger-4.0.0/bin/cellranger count 
	--id Mouse_10x 
	--fastqs=~/data/ 
	--sample=filename 
	--localcores=40 
	--localmem=100 
	--transcriptome=/Shared_Software/ref_genome/refdata-gex-mm10-2020-A
  ```
  
## velocyto
```
velocyto run10x -m /Shared_Software/ref_genome/mm10_rmsk/mm10_rmsk.gtf 
        ~/data/cellranger_results/Mouse_10x
 	/Shared_Software/ref_genome/refdata-gex-mm10-2020-A/genes/genes.gtf
```



