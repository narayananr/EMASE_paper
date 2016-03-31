# EMASE_paper

# Shell Script to compare EMASE to other methods with simulated data

################################################################################################
# single shell script to go from real data's bam file simulated results
# Step1: estimate expression by emase M2 (and RSEM)
# Step2: use the estimated expression to simulate reads
# Step3: align the simulated reads
# Step4: estimate expression by EMASE M2 from the aligned simulated reads
# Step5: estimate expression by Kallisto from the aligned simulated reads
# Step6: estimate expression by RSEM from the aligned simulated reads
# Step7: align the simulated reads to B6 genome  by TopHat2
# Step8:
#
################################################################################################
#!/bin/bash
#PBS -l nodes=1:ppn=1,walltime=23:00:00
cd $PBS_O_WORKDIR
module load rsem/1.2.19
module load samtools
module load Anaconda
#source activate emase.0.9.8
source activate emase
```
### Define variables
TID=/data/kbchoi/data/mm10/R75-REL1410/C57BL6J/emase.transcriptome.info
g2tID=/data/kbchoi/data/mm10/R75-REL1410/C57BL6J/emase.gene2transcripts.tsv
lenFile=/data/kbchoi/data/mm10/R75-REL1410/NxP/emase.pooled.transcriptome.info
Combined_HDF5=/hpcdata/cgd/RCF1-NxP_VitD/analysis/simulation/param/combined.h5
Combined_BAM=/hpcdata/cgd/RCF1-NxP_VitD/analysis/simulation/param/combined.bam
OPATH=/hpcdata/cgd/RCF1-NxP_VitD/analysis/simulation/reproduce/bam_to_all/try2
RSEM_Index=/data/kbchoi/data/mm10/R75-REL1410/NxP/rsem
Combined_ONAME=combined
```
#count-alignments -i ${Combined_HDF5} -g ${g2tID} -o ${OPATH}/${Combined_ONAME}
run-emase -i ${Combined_HDF5} -g ${g2tID} -L ${lenFile} -M 2 -r 68 -m 1 -o ${OPATH}/${Combined_ONAME}_m2
#rsem-calculate-expression -p 1 --no-bam-output --fragment-length-mean 280 --fragment-length-sd 50 --time --bam ${ALNFILE} ${INDEXBASE} ${OPATH}/${ONAME}_rsem
simulate-reads -m 2 -R /data/kbchoi/data/mm10/R75-REL1410/C57BL6J/ -H /data/kbchoi/data/mm10/R75-REL1410/NxP/ -p ${OPATH}/${Combined_ONAME}_m2.isoforms.effective_read_counts -X 12 -r 68 -e 0.2 -L -o emase_m2

~
~
