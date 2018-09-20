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

__Mate pair assemblies perform poorly__. While we are still accepting mate pair assemblies at the time of writing, they are quite difficult to work with. Mate pair scaffolding joins short-read contigs together with long-range inserts. Unfortunately, this process is error-prone and leads to frequent misjoins, leading to extensive chimerism in the resulting scaffolds. Hi-C performs badly when there are high levels of chimerism in the starting assembly (see below).

### Assembly software

Recommended long-read assemblers:

* Canu
* Falcon / Falcon-Unzip
* to be continued...

2    Useful complementary data types
---------------------
If available or within your budget, we strongly recommend using complementary data types to correct or improve your assembly:

* Genetic maps
* BioNano optical maps
 
Genetic maps contain linkage information between contigs, based on the co-segregation of specific markers on those contigs. Genetic maps can be directly used in Proximo scaffolding to constrain placement of contigs in chromosomes.

Optical mapping can be used to correct errors in the assembly and to perform initial scaffolding.

3    Common problems in assemblies
---------------------
### Low contiguity
Low contiguity assemblies tend to have very large numbers of contigs. This greatly increases the complexity of the scaffolding problem, and the opportunities to make mistakes. 

### Chimeric contigs/scaffolds
Erroneously joined sequences in the starting assembly create problems in scaffolding, as different pieces of the same contig or scaffold provide refractory signals. Therefore, this fraction should be as low as possible. Very low levels of chimerism can be accommodated and corrected using Hi-C data. As an example of chimerism in a Hi-C contact map see Figure 1, in which three large chimeric contigs are in the selection (black boxes represent selected contigs). Chimeric contigs contain distinct "squares" of contacts that in turn do not interact with each other.

![three large chimeric contigs selected](https://github.com/phasegenomics/phasegenomics.github.io/blob/assembly_reqs/images/chimeric_contigs.png)

### Heterozygosity
If there is heterozygosity in the starting assembly, this will lead to additional sequences being present that are homologous to some other sequence in the assembly. This can be ok if this is intentional (_e.g._ it is a diploid assembly or contains homeologs), but if the assembly is intended to be a single haploid sequence it will not represent linear sequence well and may contain spurious repeats. In each of these cases, Hi-C reads will be relatively difficult to map uniquely and therefore the signal useable for scaffolding will be weaker overall than for a haploid assembly. For an example of what homologous sequences look like on a Hi-C contact map, see Figure 2. This example shows how complex the situation can get in a repeat-rich genome, in that the homology is observed both within and between contigs.

![homology within and between contigs](https://github.com/phasegenomics/phasegenomics.github.io/blob/assembly_reqs/images/homologous_contigs.png)
Figure 2.


4     Fixing problems in assemblies
---------------------
We have various options for trying to fix up an assembly with the above issues.

### Break contigs --> fixes chimeric contigs
There are a few ways to break starting contigs/scaffolds: 

##### Split on gaps
One is to simply split every sequence on gaps in the assembly, where a gap is defined as a stretch of Ns of at least some length (2 or 10 usually). 

##### Polar_star
If you are using PacBio or Nanopore reads, another method is to use our tool [polar_star](https://github.com/phasegenomics/polar_star). This tool takes your PacBio subreads (or in principle ONT reads), aligns them to your assembly, and breaks the assembly in places where long read coverage fluctuates.

##### Manual contig breaking
Use Juicebox or a similar tool to manually break contigs and make a new assembly.

### Manual polishing
We will frequently perform manual fixes of an assembly using the Juicebox tool. This visualizer and interactive assembly editor is a great way to fix the last 1% of problems that benefit from human eyes after Proximo has done 99% of the work. You can move contigs, invert contigs, and break contigs in this tool. Remaking an assembly FASTA from Juicebox fixes is a little hard and not automatic, but we are working on tools that allow us to do this. 

### Remove homologous sequences

