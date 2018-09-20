---
layout: post
title: "Preparing Starting Assemblies for Scaffolding with Phase Genomics Hi-C Data"
date: 2018-09-20
description: "Hi-C is a great tool for genome scaffolding, but its success depends crucially on the starting assembly. It is therefore important to ensure that your starting assembly is of sufficient quality for the scaffolding task. Our guidelines will help you make good decisions about what technologies to use and how to understand and improve scaffolding results."
---

Hi-C is a great tool for genome scaffolding, but its success depends crucially on the starting assembly. It is therefore important to ensure that your starting assembly is of sufficient quality for the scaffolding task.


1    Assembly technologies
---------------------
### Recommendations on sequencing technology
The following molecular technologies are __strongly encouraged__ for generating starting contigs or scaffolds:

* PacBio long reads (ideally >80X coverage; minimum 30X)
* Nanopore long reads

The following sequencing technologies are __not encouraged__ for generating starting contigs or scaffolds:

* 10X chromium / linked-read sequencing
* Sanger / dideoxy sequencing (high coverage)
* Illumina short reads (very high coverage)

The following sequencing technologies are __strongly discouraged__ for generating starting contigs or scaffolds:

* Mate-pair sequencing

### Why these technologies?
__Long read assemblies are strongly encouraged__, as they lead to extremely contiguous assemblies due to their ability to completely read through long repetitive sequences. Their relatively low accuracy and high frequency of small indels makes it desirable to perform polishing and error correction for finished genomes, but Hi-C scaffolding works well even with unpolished contigs. 

__Short read assemblies are not encouraged but can be "good enough" for simple genomes with low repeat content__. Short read assemblies tend to collapse repetitive sequences and yield low contiguity assemblies with large numbers of contigs. These assemblies are difficult to scaffold due to their high complexity.

__Mate pair assemblies perform poorly__. While we are still accepting mate pair assemblies at the time of writing, they are quite difficult to work with. Mate pair scaffolding joins short-read contigs together with long-range inserts. Unfortunately, this process is error-prone and leads to many misjoins, leading to extensive chimerism in the resulting scaffolds. Hi-C performs badly when there are high levels of chimerism in the starting assembly.

### Assembly software

2    Other complementary data types
---------------------
If available and within your budget, we strongly recommend using complementary data types to correct or improve your assembly:

* Genetic maps
* BioNano optical maps
 


3    Common problems in assemblies
---------------------
### Low contiguity

### Chimeric contigs

### Heterozygosity

4     Fixing problems in assemblies
---------------------

### Break contigs --> fixes chimeric contigs

### 
