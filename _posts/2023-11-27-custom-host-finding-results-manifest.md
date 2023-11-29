---
layout: post
title: "Custom Host-finding Results Manifest"
date: 2023-11-27
description: "This is a results manifest for the deliverable files for a custom host-finding project."
---

---------------------
### Custom host-finding includes the following deliverables:
- filtered_master_table.tsv   
- intra_contig_linkages.tsv
- inter_contig_linkages.tsv   
- min_copy_count_roc_plot.png 
- unfiltered_master_table.tsv
- reports / *reports is a directory that contains the following tables and figures for both the filtered and unfiltered datasets*
    - contigs_histogram_adjusted_inter_vs_intra_ratio.png	
    - contigs_histogram_mobile_element_copies_per_cell.png   
    - contigs_host_count_histogram.png                   	
    - contigs_matrix_adjusted_inter_vs_intra_ratio.tsv   	
    - contigs_matrix_mobile_element_copies_per_cell.tsv
    - contigs_heatmap_adjusted_inter_vs_intra_ratio.png[^1]
    - contigs_heatmap_mobile_element_copies_per_cell.png[^1]

### File descriptions

- **intra_contig_linkages.tsv**: Counts of Hi-C links on the same contig.
- **inter_contig_linkages.tsv**: Counts of Hi-C links between two different contigs.
- **min_copy_count_roc_plot.png**: Receiver operating characteristic (ROC) curve is used to determine the optimal copy count cut-off value.
- **unfiltered_master_table.tsv**: This table shows metadata for all mobile-element to host associations, where each row is an association between a mobile element (mobile_contig_name) and a host (cluster_name).
- **filtered_master_table.tsv**: This table shows metadata for all mobile-element to host associations after filtering, where each row is an association between a mobile element (mobile_element) and a host (cluster_name). Mobile element-host linkages are filtered as described in Uritskiy 2021, “Accurate viral genome reconstruction and host assignment with proximity-ligation sequencing”. Adapted from Uritskiy 2021[^2]: “Mobile element-host linkages are filtered to keep only connections with at least 2 Hi-C read links between the mobile element and host MAG, a connectivity ratio of 0.1, and intra-MAG connectivity of 10 links to remove false positives. For the final threshold value, a receiver operating characteristic (ROC) curve is used to determine the optimal copy-icount cut-off value. The optimal cut-off was determined from the ROC curve as the value that produces the point to the top left of the plot, or the cut-off that removed the maximum number of mobile element-host links while still finding at least one host for the maximum number of mobile elements.”


| Column | Description |
| -------|-------------|
| mobile_contig_name | the name of the mobile element contig |
| mobile_contig_length (bp) | the length of the mobile element contig |
| mobile_contig_read_count (reads) | number of Hi-C reads aligning to mobile element contig |
| mobile_contig_read_depth (reads/kbp) | Hi-C read coverage depth of module element contig in entire sample |
| mobile_contig_read_depth_in_this_cluster (reads/kbp) | Hi-C read coverage depth of the module element contig in this MAG (in cases where it is linked to multiple MAGs) |
| cluster_name | host MAG name |
| cluster_length (bp) | host MAG length |
| cluster_read_count (reads) | number of Hi-C reads aligning to host MAG |
| cluster_read_depth (reads/kbp) | Hi-C read coverage depth of host MAG |
| intra_read_count (reads) | number of HiC reads inter-linking contigs in the MAG |
| intra_linkage_density (reads/kbp^2) | density of HiC inter-linking between contigs in the MAG (this is the “expected” linkage) |
| inter_read_count (reads) | number of HiC reads linking the module element to the potential host MAG |
| raw_inter_linkage_density (reads/kbp^2) | density of HiC inter-linking between the mobile contig and the MAG (this is the “actual” linkage) |
| raw_inter_vs_intra_ratio | ratio between the “actual” and “expected” HiC linkage |
| mobile_element_copies_per_cell | estimated number of copies of the mobile element in this MAG |
| adjusted_inter_connective_linkage_density (reads/kbp^2) | density of HiC inter-linking between the mobile contig and the MAG, adjusted for the copy count |
| adjusted_inter_vs_intra_ratio | ratio between the “actual” and “expected” HiC linkage adjusted for copy count (if this is even somewhat close to 1, this is a real host) |




  
[^1]: Please note that samples with many mobile element-host pairs will not have the heatmap figures due to visualization limitations.
[^2]: Uritskiy, G. et al. Accurate viral genome reconstruction and host assignment with proximity-ligation sequencing. 2021.06.14.448389 Preprint at https://doi.org/10.1101/2021.06.14.448389 (2021).



