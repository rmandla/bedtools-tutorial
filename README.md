# Bedtools Tutorials

This tutorial will hopefully teach you enough about BED files and Bedtools to begin conducting genomic and sequencing analysis. 

Bedtools is a huge package with a wide variety of commands and functions which I don't personally know completely. More important than memorizing every single command and its usage is to instead become adapt at navigating the bedtools documentation and become a pro at using resources like google and peers to address data analysis challenges. I hope that the following tutorial will provide these skills which will continue to be integral to your future bioinformatics projects.

## What are BED files?

BED files are essentially text files which store genomic positional information. They are tab-delimited files, just like TSVs, except they contain no header row. 

Because BED files do not have any official specification, the data per column is largely subjective. However, there are 3 obligatory columns which must always contain the same information.

1. Column 1 must always contain the chromosome or scaffold identity (chr1, chrY, scaffold17532....)
2. Column 2 must always contain the start coordinate
3. Column 3 must always contain the end coordinate. This end value is non-inclusive

Besides these 3 columns, additional columns of other information can be inputted at the discretion of the user. However, in general the column descriptions popularised by the UCSC genome browser are recommended but not required (see the descriptions [here](https://en.wikipedia.org/wiki/BED_(file_format)#Description)).

BED files are often used to list important genomic positions in a smaller format than including positions and sequence data in Fasta, BigWig, BAM/SAM/CRAM, and other such formats. They are incredibly useful for highlighting specific regions of a genome or for inputting into other software packages to get greater information on the described genomic region. For example, a list of putative enhancers can be denoted in a BED file as such:

`chr1	18537	18754	enhancer1`
`chr2	7583726	7594726	enhancer2`

NOTE these BED files are completely different from BED files used in the PLINK software package. Computational biology file formats are disgusting :'(.

## What is Bedtools?

Bedtools is a software package which allows for the easy manipulation and comparision of BED files. For example, lets say we have two BED files for alternatively spliced exons in two cell populations. How can we determine which exons are in both cell populations? Bedtools makes such operations incredibly fast and simple.

### Installing Bedtools

Bedtools can be installed [here](https://bedtools.readthedocs.io/en/latest/content/installation.html). Additionally, a python version of Bedtools called pyBedTools can also be installed from [here](https://daler.github.io/pybedtools/main.html). pyBedTools is useful if you want to run custom python scripts on BED files, but generally not needed.

## Useful Bedtools Commands

Here is a list of the commands which I personally use the most:

| Command | Description | example | further reading |
| ------- | ------- | ------- | ------- |
| `intersect` | find all regions of overlap between two BED files | `bedtools intersect -a 1.bed -b 2.bed` | [link](https://bedtools.readthedocs.io/en/latest/content/tools/intersect.html) |
| `sort` | sort all intervals in a BED file, by default by chromosome then position | `bedtools sort -i 1.bed` | [link](https://bedtools.readthedocs.io/en/latest/content/tools/sort.html) |
| `merge` | combine overlapping regions of a BED file into a single region | `bedtools merge -i 1.bed` | [link](https://bedtools.readthedocs.io/en/latest/content/tools/merge.html) |

Sorting is an incredibly useful tool, both a necessary component of certain Bedtool command such as `merge`, and also generaly works to speed up calculations and decrease RAM usage. So in general, always try and sort your files first, especially if they are large files. 

![](https://bedtools.readthedocs.io/en/latest/_images/speed-comparo.png)
![](https://bedtools.readthedocs.io/en/latest/_images/memory-comparo.png)

A full list of bedtools commands can be found [here](https://bedtools.readthedocs.io/en/latest/content/bedtools-suite.html) and a very in-depth tutorial on using bedtools can also be found [here](http://quinlanlab.org/tutorials/bedtools/bedtools.html).

## Examples

Alright, now lets try out some bedtool commands.

1. First lets download some BED files. Download two h3k27ac CHIP-seq bed narrowPeak files from different tissue types from the [encodeproject.org](https://www.encodeproject.org).
2. After downloading, compare the two bed files. What sequences do they share in common? What sequences are unique?
3. Lets say we want to find the overlapping regions of these two bed files. However, we only want the overlapping sequence which is longest between the two samples. For example, if one BED file has a region of `chr1	1	10` and another has a region of `chr1	5	20`, only output the second interval since it is the longest.
4. We want to find enhancers specific to a single tissue type. How would you go about doing so? Try and create a reasonable list of enhancers specific to a single tissue type from BED files from ENCODE.
5. Nice, now we have a list of putative enhancers. One common method of identifying enhancers is via level of conservation. How conserved are your enhancers? Convert your BED sequences to another species genome library using [liftover](https://genome.ucsc.edu/cgi-bin/hgLiftOver). Then, try finding overlaps between these converted regions and a downloaded set of h3k27ac peaks from ENCODE for your interested tissue type and species you are comparing it to. eg. If you have a list of enhancers in human lung tissue aligned to hg19, convert it to mm9 and compare it to h3k27ac peaks in mouse lung tissue also aligned to mm9.
6. Which of these enhancers occur in intronic regions?
