---
layout: post
title: "Proximeta Results Manifest"
date: 2023-09-05
description: "This results manifest describes the deliverables for a standard ProxiMeta project."
---


The following manifest lists all files found in the ProxiMeta tarball:

sample-name_all_files.tar.gz

## Main directory

The file contains 6 tarballs.

| folder name | source tarball |
| ----------- | -------------- |
| inputs                        | sample-name_amr_files.tar.gz |
| microbial_genomes             | sample-name_input_files.tar.gz |
| sample-name_AMR_Files         | sample-name_metabolism_files.tar.gz |
| sample-name_Metabolism_Files  | sample-name_microbial_genome_files.tar.gz |
| sample-name_Plasmid_Files     | sample-name_plasmid_files.tar.gz |
| sample-name_Viral_Files       | sample-name_viral_files.tar.gz |

### Inputs
all//inputs:
- sample-name_contigs.fasta
- sample-name_s3_m1.bam

### Microbial Genomes
all//microbial_genomes:
- sample-name_clusters
  - bin_x.fasta  
  - ...
- sample-name_proximeta_report.tsv

### AMR Files
all//sample-name_AMR_Files:
- amr_annotations/  
  - AMR_annotations_summary.tsv
- amr_host_connections/
  - amr_gene_to_host_summary.tsv 
  - amr_host_content.tsv         
  - heatmaps/
    - amr_contig_to_host_heatmap.png
    - amr_contig_to_host_matrix.tsv 
    - amr_gene_to_host_heatmap.png
    - amr_gene_to_host_matrix.tsv

### Metabolism Files
all//sample-name_Metabolism_Files:
- metabolism_results/
  - cluster_annotation_gff/
    - bin_x.gff
    - ...
  - cluster_modules_matrix_complete.tsv
  - cluster_protein_sequences/
    - bin_x.faa
    - ...
  - cluster_module_completion/
    - bin_x.tsv
    - ...
  - cluster_pathways_info
    - bin_x.tsv
    - ...
  - cluster_search_results
    - bin_x.blast.tsv
    - ...
  - cluster_modules_matrix_complete.png 
  - cluster_pathways_plots
    - bin_x
      - map000n.pathview.png

### Plasmid Files
all//sample-name_Plasmid_Files:
- ProxiphageDocumentationPlasmid.html 
- plasmid_MAGs/
  - binned_plasmid_contigs_summary.tsv
  - figures/
    - plasmid_quality_barplot.png 
    - plasmid_quality_plot.png
  - pMAG_sequences/
    - pMAG_x.fasta
    - ...
  - plasmid_mags_summary.tsv                      
- plasmid_hosts/
  - figures/
    - plasmid_heatmap_mobile_element_copies_per_cell.png   
    - plasmid_host_count_histogram.png
    - plasmid_histogram_mobile_element_copies_per_cell.png
  - plasmid_host_associations_filtered.tsv
  - plasmid_host_associations_unfiltered.tsv
- pMAG_hosts/
  - pMAG_host_associations_filtered.tsv
  - pMAG_host_associations_unfiltered.tsv
  - figures/
    - pMAG_heatmap_mobile_element_copies_per_cell.png
    - pMAG_host_count_histogram.png
    - pMAG_histogram_mobile_element_copies_per_cell.png
- plasmid_annotations/
  - integrated_plasmid_annotation_summary.tsv
  - plasmid_annotation_summary.tsv
  - integrated_plasmid_contigs.fasta
  - plasmid_contigs.fasta

### Viral Files
all//sample-name_Viral_Files:
- ProxiphageDocumentationVirus.html 
- viral_MAGs
  - binned_viral_contigs_summary.tsv
  - figures/
    - viral_quality_barplot.png 
    - viral_quality_plot.png
  - vMAG_sequences
    - vMAG_x.fasta
  - viral_mags_summary.tsv                      
- viral_hosts
  - figures/
    - viral_heatmap_mobile_element_copies_per_cell.png
    - viral_host_count_histogram.png
    - viral_histogram_mobile_element_copies_per_cell.png
  - viral_host_associations_filtered.tsv
  - viral_host_associations_unfiltered.tsv
- vMAG_hosts
  - figures/
    - vMAG_heatmap_mobile_element_copies_per_cell.png
    - vMAG_host_count_histogram.png
    - vMAG_histogram_mobile_element_copies_per_cell.png
  - vMAG_host_associations_filtered.tsv
  - vMAG_host_associations_unfiltered.tsv                       
- viral_annotations
  - all_viral_contigs.fasta
  - prophage_sequences.fasta
  - viral_annotations_summary.tsv
  - viral_protein_annotations.tsv
