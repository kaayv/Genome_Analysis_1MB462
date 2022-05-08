# Bash script to align RNA reads to bins

module load bioinfo-tools
module load bwa samtools

# Create a reference .fa file with db to align to that is a contcatenated file of all bins at a site,
# using code from bwa_workflow.txt (provided by authors of paper in their supplementary materials).
cd /home/vinglis/genome_analysis/GenomeAnalysis/analysis/03_binning/D1_metabat_output
for i in *.fa; do name=`echo $i | awk 'BEGIN{FS="."}{print $2}'`; echo ">"$name >> D1_allbins_concat.fa; grep -v '>' $i >> D1_allbins_concat.fa; done
mv D1_allbins_concat.fa /home/vinglis/genome_analysis/GenomeAnalysis/analysis/05_mapping/D1_bwa_alignment

cd ../D3_metabat_output
for j in *.fa; do name=`echo $j | awk 'BEGIN{FS="."}{print $2}'`; echo ">"$name >> D3_allbins_concat.fa; grep -v '>' $j >> D3_allbins_concat.fa; done
mv D3_allbins_concat.fa /home/vinglis/genome_analysis/GenomeAnalysis/analysis/05_mapping/D3_bwa_alignment

# Move to directory to perform bwa analysis
cd /home/vinglis/genome_analysis/GenomeAnalysis/analysis/05_mapping/D1_bwa_alignment

# Create soft links to trimmed RNA fastq files
ln -s ../../../data/trimmed_data/RNA_trimmed/RNA*D1* .
# Align RNA reads to concatenated fasta file, and convert to a sorted bam format.
# Also produce some index stats.
bwa index D1_allbins_concat.fa 2>D1_index.out
bwa mem -t 2 D1_allbins_concat.fa RNA_trim_D1_1.paired.trimmed.fastq.gz RNA_trim_D1_2.paired.trimmed.fastq.gz > D1_supercontig.RNA.aln.sam 2>D1_bwamem.out
samtools view -@ 2 -bS  D1_supercontig.RNA.aln.sam | 
samtools sort -@ 2 -o D1_supercontig.RNA.aln.bam -
samtools index D1_supercontig.RNA.aln.bam
samtools idxstats D1_supercontig.RNA.aln.bam > D1_supercontig.RNA.aln.stats

# Do the same for site D3
cd ../D3_bwa_alignment
ln -s ../../../data/trimmed_data/RNA_trimmed/RNA*D3* .
bwa index D3_allbins_concat.fa 2>D3_index.out
bwa mem -t 2 D3_allbins_concat.fa RNA_trim_D3_1.paired.trimmed.fastq.gz RNA_trim_D3_2.paired.trimmed.fastq.gz > D3_supercontig.RNA.aln.sam 2>D3_bwamem.out
samtools view -@ 2 -bS  D3_supercontig.RNA.aln.sam | 
samtools sort -@ 2 -o D3_supercontig.RNA.aln.bam -
samtools index D3_supercontig.RNA.aln.bam
samtools idxstats D3_supercontig.RNA.aln.bam > D3_supercontig.RNA.aln.stats
