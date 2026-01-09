# Hands-on session ChIPseq

### IN ORDER TO RUN THE EXERCISE IN CLASS WE ARE GOING TO USE A SUBSET OF THE INITIAL FILES

1️⃣ Install conda environment:
```bash
conda env create -f ChIPSeq.yml
```
2️⃣ Activate and check hands-on session environment:
```bash
conda activate ChIPSeq
conda list
```
3️⃣ Peak calling on bam files:
```bash
macs2 callpeak -t HNF4A_chr16.bam -c Input_chr16.bam -f BAM -g 2.7e9 -n HNF4A --outdir macs2
```
4️⃣ Extracting genomic coordinates of narrow peaks:

```bash
awk -v OFS="\t" '{print $1,$2,$3}' macs2/HNF4A_peaks.narrowPeak > macs2/HNF4A_peaks.bed
```

5️⃣ Preprocessing BAM files:
```bash
samtools sort -O bam  -o HNF4A_chr16_sorted.bam  HNF4A_chr16.bam
samtools sort -O bam  -o Input_chr16_sorted.bam  Input_chr16.bam

samtools index HNF4A_chr16_sorted.bam
samtools index Input_chr16_sorted.bam
```
6️⃣ Generating bigwig files for visualization on the IGV browser:
```bash
bamCoverage -b HNF4A_chr16_sorted.bam -o HNF4A_chr16_sorted.bw  --normalizeUsing RPKM
bamCoverage -b Input_chr16_sorted.bam -o Input_chr16_sorted.bw  --normalizeUsing RPKM
```
7️⃣ Visualization in IGV browser hg19 genome:
```bash
igv
```
- Gene CES1 peak in the promoter
- Gene ZFPM1 peaks in gene body
- Gene KCNG4 no peaks
- Region chr16:46,260,767-48,860,903 artifact

8️⃣ Homer analysis:
```bash
tar -xJf chr16.fa.tar.xz
```
Annotate peaks

```bash
annotatePeaks.pl macs2/HNF4A_peaks.bed chr16.fa -gtf chr16.gtf > macs2/HNF4A_annotated_peaks.txt
```
Motif analysis

```bash
findMotifsGenome.pl macs2/HNF4A_peaks.bed chr16.fa macs2/HNF4A_motifs -size given
```
