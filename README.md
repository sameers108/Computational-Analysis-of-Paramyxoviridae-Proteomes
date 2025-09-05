Paramyxoviridae Proteome Analysis

Structural and Functional Annotation using Bioinformatics Tools

📌 Overview

This repository documents a comprehensive bioinformatics workflow for studying the Paramyxoviridae family of viruses — enveloped, negative-sense, single-stranded RNA viruses that include major pathogens such as Nipah, Measles, and Mumps.

The project combines Linux automation, sequence clustering, structural modeling, and domain annotation to provide a detailed structural and functional map of viral proteins across the family.

Conducted as part of the SRFP Programme under Dr. Arunkumar Krishnan (IISER Berhampur), this project demonstrates how computational pipelines can uncover conserved features, evolutionary innovation, and functional motifs at the proteome scale.

🔬 Biological Motivation

Paramyxoviridae viruses pose major public health risks due to their zoonotic potential and high mutation rates.

Traditional experimental methods (X-ray crystallography, NMR, cryo-EM) are time-intensive and costly.

Bioinformatics pipelines enable rapid, scalable analysis of entire viral proteomes, helping identify conserved drug/vaccine targets and lineage-specific adaptations.

⚙️ Workflow

Linux Environment & Automation

Practiced Bash scripting for data handling.

Commands like cd, ls, grep, awk, sed automated repetitive file operations.

Proteome Collection

Complete proteomes retrieved from NCBI Virus Database using taxonomic identifiers.

Clustering

BLASTCLUST: sequence similarity–based clustering (alignment length criteria).

CD-HIT: redundancy reduction at 90% identity threshold.

Multiple Sequence Alignment

MAFFT used to identify conserved residues and sequence divergence across clusters.

Structure Prediction

Representative proteins modeled with AlphaFold3.

Confidence evaluated using pLDDT scores (>80 = high reliability).

Domain Architecture Analysis

RPS-BLAST against the PFAM database.

Custom in-house scripts added metadata:

domain length, strain name, species taxonomic ID, phyletic distribution.

Structural Comparison

DALI & Foldseek for detecting conserved folds despite low sequence identity.

📊 Key Findings

Conserved Domains

Paramyxo_ncap: defining nucleocapsid RNA-binding domain, essential for replication.

BDV_P40: found in matrix proteins, hinting at evolutionary crossovers.

Accessory motifs (e.g., SR-rich regions) showed lineage-specific variation.

Rare Fusion Architectures

Paramyxo_ncap + Ebola_NP: potential horizontal gene transfer between viral families.

Paramyxo_ncap + DUF5401: domains of unknown function fused to conserved scaffolds.

Paramyxo_ncap + CCDC158: possible mimicry of host coiled-coil domains.

Evolutionary Insights

Strong structural conservation despite sequence divergence (<30% identity).

Lineage-specific adaptations suggest roles in host specificity and immune evasion.

Scalability

Linux scripting + modular tools → workflow easily repurposable for other viral families.

⚙️ Bash Code

#!/bin/bash

cat te | awk '{print $1}' | sort | uniq | sed '/^$/d' > te.ids

esl-sfetch -f /home/ceglab/paramyxo.wf/prot.db/paramyxo.dataset.fa te.ids > te.fa
cd-hit -i te.fa -o te.out -c 1.0 -n 5 -M 0 -T 0

#n=5 for thresholds 0.7 ~ 1.0
#n=4 for thresholds 0.6 ~ 0.7
#n=3 for thresholds 0.5 ~ 0.6
#n=2 for thresholds 0.4 ~ 0.5

mafft --thread 6 --auto te.out | fasta2seqrows > res.seqrows
rm te.ids te.fa te.out te.out.clstr
