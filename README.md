# GenomeAssemblingWithPlatanus

First recommendation: don't use platanus, use SPADES!

Project structure: res1 contains files and quality reports before quality control and adapters cutoff, res2 - after 
(and it also contains genome assembly data and its analysis).

## Steps to reproduce
   - ```seqtk sample -s 628 oil_R1.fastq 5000000 > oil_R1_sample.fastq```
   - ```seqtk sample -s 628 oil_R2.fastq 5000000 > oil_R2_sample.fastq```
   - ```seqtk sample -s 628 oilMP_S4_L001_R1_001.fastq 1500000 > oilMP_S4_L001_R1_001_sample.fastq```
   - ```seqtk sample -s 628 oilMP_S4_L001_R2_001.fastq 1500000 > oilMP_S4_L001_R2_001_sample.fastq```
   - ```fastqc *_sample.fastq```
   - ```multiqc .```
   - ```platanus_trim oil_R1_sample.fastq oil_R2_sample.fastq```
   - ```platanus_internal_trim oilMP_S4_L001_R1_001.fastq oilMP_S4_L001_R2_001.fastq```
   - ```fastqc *trimmed```
   - Move non-trimmed data to res1, rename trimmed so it would end with .fastq
   - ```multiqc .```
   - ```platanus assemble    -o platanus    -f *.fastq -t 4```
   - ```platanus scaffold -o platanus -c platanus_contig.fa -IP1 oil_R1_sample.fastq oil_R2_sample.fastq -OP2 oilMP_S4_L001_R1_001.fastq oilMP_S4_L001_R2_001.fastq -t 6```
   - ```platanus gap_close -o platanus -c platanus_scaffold.fa -IP1 oil_R1_sample.fastq oil_R2_sample.fastq -OP2 oilMP_S4_L001_R1_001.fastq oilMP_S4_L001_R2_001.fastq -t 6```
