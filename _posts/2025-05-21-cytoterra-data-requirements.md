---
layout: post
title: "CytoTerra Data Requirements"
date: 2025-05-21
description: "A description of data requirements for the CytoTerra platform, including pre-processing and QC steps."
---

## Summary of Data Requirements

To ensure optimal performance of the **CytoTerra** platform, datasets should contain **150–250 million Hi-C read pairs**, with at least:

- **100 million high-quality read pairs**
- **≥20% same-strand read pairs**

Reads must be submitted as **separate forward (R1)** and **reverse (R2)** FASTQ files in **gzipped** format. The steps below will help you assess and prepare your data for submission.

---

## 1. Downsampling

If you're unsure how many read pairs you have, use one of the commands below to count lines in each FASTQ file. Run the command on both R1 and R2 files to ensure they are **not corrupted** and contain **matching read counts** (a slight difference is okay).

### For a gzipped FASTQ:

```bash
zcat sample_R1.fastq.gz | wc -l
```

### For an unzipped FASTQ:

```bash
wc -l sample_R1.fastq
```

Since each read occupies 4 lines, divide the total line count by 4 to determine the number of reads. If the dataset exceeds **250 million read pairs**, downsampling is required.

### Downsampling with BBTools

#### 1. **Download and extract BBTools:**

```bash
wget https://sourceforge.net/projects/bbmap/files/latest/download -O bbtools.tar.gz
tar -xvzf bbtools.tar.gz
```

#### 2. **Add to PATH (adjust install path as needed):**

```bash
echo 'export PATH=~/bbtools_install/bbmap:$PATH' >> ~/.bashrc
source ~/.bashrc
```

#### 3. **Run the `reformat.sh` tool to downsample:**

Determine the `samplerate` by dividing your desired target (e.g., 150 million) by your current number of read pairs.  
The example below uses `samplerate=0.2`; adjust this value accordingly.

```bash
reformat.sh \
  in1=example_R1.fastq.gz \
  in2=example_R2.fastq.gz \
  out1=example_downsampled_R1.fastq.gz \
  out2=example_downsampled_R2.fastq.gz \
  samplerate=0.2
```

---

## 2. Hi-C QC

After downsampling, please run our [Hi-C QC tool](https://phasegenomics.github.io/2019/09/19/hic-alignment-and-qc.html) to assess data quality. We recommend that you use the [HG38 assembly](https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_000001405.40/).

---

## 3. Data Manifest

Please download and complete the [Data Manifest](https://phasegenomics.github.io/downloads/cytoterra_file_manifest.xlsx).

---

## 4. Data Upload

Once your data and manifest are ready:

- Provide Phase Genomics with your data manifest to receive **sFTP credentials**
- Use those credentials to upload your files to our secure server by following our [sFTP Guide](https://phasegenomics.github.io/2023/07/28/ftp-guide.html)