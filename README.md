# RegSNPs-ASB
RegSNPs-ASB is a pipeline for extracting regulatory SNPs from ATAC-seq data. RegSNPs-ASB first call TFBS and heterozygote SNP using published tools. In each TFBS with heterozygote SNP, RegSNPs-ASB will take a generalized linear model to identify allele-specific TF binding events. These functional candidate variants can help us understand the molecular mechanism of complex diseases and provide an essential foundation for experimental follow-up analysis.
## Overall flow chart
![Image of flow chart](https://github.com/SiwenX/RegSNP-ASB/blob/master/Figures/Fig2.png)
## Installation
`git clone https://github.com/SiwenX/RegSNP-ASB.git`
## Requirements
  - Linux working environment 
  - R packages
      - GenomicRanges
      - BSgenome
      - MASS
      - biomaRt
      - fdrtools
  - Linux tools
      - samtools
      - plink2
      - vcffilter
## Usage
  1. Call heterozygote variants
  - `$samtools merge input.bam files` # if you have multiple treatments, you should merge them together to increase the read coverage
  - `$samtools mpileup -uf reference.fa input.bam | bcftools view -Nvcg - > SNP.vcf`
  - `$grep "0/1:" SNP.vcf > hete_SNP.vcf`
  - `$vcffilter -f "DP > 10 & MQ > 20" hete_SNP.vcf > hete_SNP_filtered.vcf` # filter SNP by depth and quality
  2. Call TFBS
  prepare sequence file
  ```
  library("GenomicRanges")
  library('BSgenome')
  peak <- read.table("ATAC.narrowPeak", header = T, stringsAsFactors = FALSE)
  Range <- with(peak, GRanges(seqnames = chr, ranges = IRanges(start=start, end = end), strand = "*"))
  ```

  
  
  - `$fimo <motif file> <sequence file> --o <output dir>` # motif file can be downloaded from http://jaspar.genereg.net/downloads/ 
  sequence file can be generated by Get_fasta.R
  3. 
  
