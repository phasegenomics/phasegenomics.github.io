---
layout: post
title: "Aligning and QCing Phase Genomics Hi-C Data"
date: 2019-09-19
description: "Proper alignment and QC of Hi-C data is critical to getting great analytical results. Phase Genomics Hi-C kits include protocol optimizations that may make the data slightly different than what you may be used to with other Hi-C preps. Our specific recommendations for aligning, QCing, and optionally filtering data from our protocols will help you get the most out of your data."
---
Proper alignment of Hi-C data is critical to getting great results. It's also the best way to QC a Hi-C library — Phase Genomics recommends obtaining 1-5M read pairs and using them for QC prior to deep sequencing whenever practical.

Our Hi-C kits and protocols have been optimized to generate more useful Hi-C reads by doing things like reducing the proportion of split reads in the library. This means that the guidelines offered for other forms of Hi-C data may be incorrect with data from our kits, and that tools which infer library quality from the proportion of split reads (such as [HiCUP](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4706059/)) do not generate a reliable QC signal.

We recommend the following steps for alignment and QC of our data. It's the method we use ourselves as well - there are no proprietary alignment tricks we use.

1   Alignment
---------------------
### Short Answer
Align your Hi-C data with:
> `bwa mem -5SP [assembly.fasta] [fwd_hic.fastq] [rev_hic.fastq] | samtools view -S -h -b -F 2316 > [aligned.bam]`

If you would like to use `samblaster` to flag PCR duplicates (we do), you can insert it into the pipeline like this:
> `bwa mem -5SP [assembly.fasta] [fwd_hic.fastq] [rev_hic.fastq] | samblaster | samtools view -S -h -b -F 2316 > [aligned.bam]`

### More Details
Alignment of Hi-C data typically requires the use of an aligner that has been modified for Hi-C data. This is because Hi-C data intentionally contains paired reads which can come from very far away, or even between chromosomes, following a very different statistical distribution from things like shotgun or mate pair libraries. To our knowledge, only [bwa](http://bio-bwa.sourceforge.net) and [minimap2](https://github.com/lh3/minimap2) have such modifications (please [let us know](mailto:support@phasegenomics.com) if we're wrong!). Also, Heng Li's [hickit](https://github.com/lh3/hickit) has some useful information about aligning Hi-C reads, and may have some helpful examples.

We use `bwa mem` to align Hi-C data, with the `-5`, `-S`, and `-P` options. These options aren't documented that well in the online bwa documentation, but they are documented in the usage of `bwa mem`. `-5` is sometimes called "the Hi-C option" as it was designed to help the aligner handle the statistical properties of Hi-C libraries better, mainly by reducing the amount of secondary and alternate mappings the aligner makes as those cause Hi-C data to become ambiguous. The `-S` and `-P` options cause the aligner not to try to use assumptions about the reads that might be true for shotgun or mate pair libraries in an effort to rescue more reads. In fact, using these options to avoid those rescue efforts usually results in more of the Hi-C data having a useful alignment!

Lastly, it is usually helpful to flag PCR duplicates, both as part of QC (the report below will count these up for you) and so you can remove them later if desired. We use [samblaster](https://github.com/GregoryFaust/samblaster) because it is effective and easy to use. However, there are several tools which can flag PCR duplicates ([Picard](https://broadinstitute.github.io/picard/) has a popular capability to flag PCR duplicates) if you so choose on the aligned BAM file produced during this step.

2   QC
---------------------
### Short Answer
After aligning, run our QC tool [hic_qc.py](https://github.com/phasegenomics/hic_qc) with: 
> `hic_qc.py -b [aligned.bam] -r -o [output_file_prefix]`

The report it generates includes a sequencing recommendation; [contact us](mailto:support@phasegenomics.com) if you don't get a "Pass" and be sure to attach your report.

### More Details
The best way to know if a Hi-C library worked is to look at how much long-range signal is in it. There are also several metrics which correlate with a suspicious library, such as a high number of PCR duplicates or a large number of reads which align to the same position in the genome (this happens when there are very short fragments in the library due for example to two restriction sites being very close together). Our QC script measures these quantities and makes a recommendation about the library based on the result.

* __Pass__ means that from everything we can tell, the library looks to be in great shape. Proceed to full sequencing or analysis with confidence.
* __Mixed Results__ means that the library is good in some ways, but not in others. Perhaps it has a good amount of long range data, but there are also an elevated number of read pairs with MAPQ 0. Usually, data generated from Mixed Results libraries works out just fine (a high MAPQ 0 number can be due to repetitiveness in the assembly, for example), but it is good to know there may have been a few hiccups in the library prep in case troubleshooting is needed down the line.
* __Low Signal__ means that the library contains good Hi-C signal, but it's in lower proportion than usual. These libraries are generally good for generating useful Hi-C data, but you may need to sequence a little deeper than normal to get enough of it. You might consider size selecting the library to discard reads outside the 300-700bp range, as these are unlikely to be good Hi-C junctions. Alternatively, you might just want to prep a new library.
* __Fail__ means that the library, or perhaps the library in combination with a low-contiguity or error-prone assembly, does not look useful. Sometimes size selection can rescue such libraries, but sometimes a new prep is the only way forward. [Contact us](support@phasegenomics.com) if you get a fail and we will help you out.

3   Optional: Filtering Alignments
---------------------
### Short Answer
If you want to filter your Hi-C data (usually not necessary), use our tool [Matlock](https://github.com/phasegenomics/matlock) with:
>`matlock bamfilt -i [aligned.bam] -o [aligned_filtered.bam]`

Feel free to experiment with filtering options, but we recommend starting with the defaults.

### More Details
If your QC wasn't great, you've tried analyzing your data and are being hindered by repeats, or you just feel like there's too much noise in the data, filtering the alignments with Matlock can help. Matlock offers a set of several different kids of filters used for different kinds of situations. Getting familiar with when to use which filter is in part learning how to work effectively with Hi-C data, but the filters are fairly straightforward.

One note about how matlock works: its filters are used to mark which read pairs need to be filtered out, then it filters out all the marked pairs. Read pairs might be flagged for removal for multiple reasons. There is one flag, `-f`, which is a bitmap that controls which filters will actually be used to remove alignments. The other filters only control which read pairs get marked for removal - you generally must set values for those filters AND specify a bitmap for `-f` to filter out alignments. Also, filtering is ALWAYS done on a pairwise basis. If one read in a pair is bad, both reads get filtered out.

* __`-m`__: mark alignments with MAPQ less than a threshold for filtering out. Default: 20
* __`-e`__: mark alignments with an edit distance greater than a threshold for filtering out. Default: 5
* __`-l`__: mark alignments where the target is smaller than a threshold for filtering out. Default: 0
* __`-x`__: mark alignments where the target is a member of a comma-separated list of SEQIDs for filtering out. This is used to remove all Hi-C data for a particular set of contigs/SEQIDs from the BAM. Default: none
* __`-y`__: invert the behavior for contigs specified with -x, only including alignments involving those contigs/SEQIDs instead of excluding. Default: exclude
* __`-f`__: binary flag filter controlling the reasons a read pair should be excluded
   * SAME_SEQID  =  2
   * LOW_MAPQ    =  4
   * XA_SA       =  8
   * NM          = 16
   * SMALLCONTIG = 32
   * EXCLUDE     = 64
   * SA_ONLY     = 128
   * Default: 20 =  LOW_MAPQ | NM 

Unless your library was questionable and you are attempting to rescue it, we don’t recommend using Matlock until after you look at the unfiltered results. This is because for most genomes filtering beyond the above samtools filter may result in more signal being discarded than necessary when there isn't an observable issue with noise stemming from repeats or something similar. You may use bam_to_mate_hist.py to QC your filtered library as well.

