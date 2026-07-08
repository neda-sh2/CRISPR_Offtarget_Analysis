# CRISPR Off-Target Mutation Analysis: Read Alignment and Variant Calling

## Overview
This project implements a complete computational pipeline to detect and characterize CRISPR-induced on-target and off-target mutations from paired-end sequencing data. Reads are aligned to the mouse reference genome using BWA-MEM, variants are called with bcftools, and Python analysis classifies mutations as intended edits or off-target effects.

## Biological Question
How many off-target mutations does a CRISPR guide RNA induce across chromosome 2, and how are they distributed relative to the intended cut sites?

Off-target analysis is a critical safety assessment in any CRISPR therapeutic development pipeline — unintended edits in coding regions or regulatory sequences could have harmful consequences, so computational profiling from sequencing data is a standard required step before advancing to preclinical studies.

## Data
- **CRISPR paired-end sequencing reads** (FASTQ format, R1 + R2)  
  Download: `!gdown 1-96T1PZKA_FQeD_ZK5z9USaHLP3jVdRO` and `!gdown 1-BXGr3XVGtd9Tx6PCHSp4hepK41GGCTS`
- **mm10 reference genome, chromosome 2** (pre-indexed, tar archive)  
  Download: `!gdown 1a8CP4P5zkzIBiw1EleqJiSwDW0VZcAar`

All files download automatically within the notebook.

## Methods & Tools

**Command-line bioinformatics tools:**
| Tool | Purpose |
|---|---|
| BWA-MEM | Align paired-end reads to reference genome |
| SAMtools | Convert SAM → BAM, sort, and index alignments |
| bcftools | Variant calling (mpileup → call) and quality filtering |

**Python analysis:**
| Library | Purpose |
|---|---|
| `cyvcf2` | Fast VCF file parsing |
| `pandas` | DataFrame manipulation |
| `matplotlib` | Variant distribution visualization |

**Pipeline:**
1. Extract chromosome 2 from mm10 reference; build BWA index
2. Align paired-end reads with BWA-MEM → SAM
3. Convert to BAM, sort by coordinate, index with SAMtools
4. Call variants: bcftools mpileup → bcftools call → VCF
5. Filter variants (QUAL > 20, depth > 10, then QUAL > 60)
6. Parse VCF into pandas DataFrame with cyvcf2
7. Classify variants as on-target (at 9 intended cut sites) vs. off-target
8. Visualize off-target distribution, genotype breakdown, and quality scores

## Key Results
- On-target mutations detected at the intended CRISPR cut sites on chromosome 2
- Off-target mutations identified and characterized across the rest of chromosome 2
- Genotype distribution of off-target mutations (heterozygous vs. homozygous ALT) reported — heterozygous off-target edits indicate single-allele cutting, consistent with lower-affinity off-target binding

## How to Run

**Option 1 — Google Colab (recommended)**
1. Upload `CRISPR_Offtarget_Analysis.ipynb` to Google Colab
2. Run all cells top to bottom — all data downloads automatically, tools installed via apt/pip

**Option 2 — Local Linux/Mac environment**
```bash
# Install tools (Linux)
sudo apt install bwa samtools bcftools

git clone https://github.com/yourusername/crispr-offtarget-analysis
cd crispr-offtarget-analysis
pip install -r requirements.txt
# Download data files using the gdown commands in the notebook
jupyter notebook CRISPR_Offtarget_Analysis.ipynb
```

## Project Structure
```
crispr-offtarget-analysis/
├── README.md
├── CRISPR_Offtarget_Analysis.ipynb
├── requirements.txt
└── data/       # Not tracked — all files download via gdown within the notebook
```
