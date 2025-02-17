---
title: Topic 3
permalink: /Topic_3/
topickey: 3
topictitle: "Preprocessing"
---

### Accompanying material

* [Slides](./quality_trimming_Monash_2019.pdf)
* Background reading: [Data preprocessing](./background_reading/Data_preprocessing.pdf), by Robert Schmieder.
* Background reading: Del Fabbro C, Scalabrin S, Morgante M, Giorgi FM (2013) [An Extensive Evaluation of Read Trimming Effects on Illumina NGS Data Analysis](./background_reading/journal.pone.0085024.PDF). PLoS ONE 8(12): e85024. doi:10.1371/journal.pone.0085024




# Code break questions 

1) How many sequences so you have in the fasta file ~/Topic_3/data/Pine_reference_rnaseq_reduced.fa?

Hint: wc –l <file name> provides the number of lines in a file

2) How many sequences do you have in the fastq file ~/Topic_3/data/GBS12_brds_Pi_197A2_100k_R1.fastq?

Hint: for grep ^ indicates the start of the line and $ indicates the end of the line (e.g. grep ^H*?$ <filename> would find all the lines starting with H and ending in ?)

3) How many sequences contain a base with a Phred score of 2 ~/Topic_3/data/GBS12_brds_Pi_197A2_100k_R1.fastq?

# Topic 3: Preprocessing Sequence Data

To practice unix-based command line, run through some of the examples after the quality trimming section to learn how to navigate through the file system and do basic file manipulation.

1. Run prinseq-lite.pl on the unfiltered data first to examine the quality metrics:

(if you need to find which directory you are in, type "pwd" at command prompt)

The first step would be to install the program prinseq.
Move into the ~/Topic_3/scripts folder. Unpack prinseq-lite-0.20.4.tar.gz by executing the following command:

tar -xf prinseq-lite-0.20.4.tar.gz

However, this is already done for you!


move into your ~/Topic_3/data folder and execute the following commands:

```bash
perl ~/Topic_3/scripts/prinseq-lite-0.20.4/prinseq-lite.pl -fastq ~/Topic_3/data/PmdT_147_100k_R1.fq -fastq2 ~/Topic_3/data/PmdT_147_100k_R2.fq -graph_data pmdt_147_100k_graph.txt 
```

Download your graph file to your computer. You can do so by using the following command (note that you should execute this command in a terminal window that is not connected to the server and use your own login information for the username and ip address)

```bash
scp <username@ip.address>:~/Topic_3/*graph.txt <path on your computer where you want the file>
```

For example:
scp -rp trainee1@sbs-01.erc.monash.edu:~/Topic_3/*graph.txt ~/Dropbox/Documents/bioinformatics_workshop/bioinformatics_workshop_2019_Monash/Topic_3/

and upload it to http://edwards.sdsu.edu/cgi-bin/prinseq/prinseq.cgi to view/download your graphs


```bash
perl ~/Topic_3/scripts/prinseq-lite-0.20.4/prinseq-graphs.pl -i pmdt_147_100k_graph.txt -o pmdt_147_100k_out_graphs.txt -html_all 
```

For a description of the various plot types, see:

http://prinseq.sourceforge.net/manual.html#STANDALONE

Now rerun the above commands on the fastq files named GBS12_brds_Pi_197A2_100k_R#.fastq, which are reads created using the GBS protocol with an enzyme called PST1.

```bash
perl ~/Topic_3/scripts/prinseq-lite-0.20.4/prinseq-lite.pl -fastq ~/Topic_3/data/GBS12_brds_Pi_197A2_100k_R1.fastq -fastq2 ~/Topic_3/data/GBS12_brds_Pi_197A2_100k_R2.fastq -graph_data GBS12_brds_Pi_197A2_100k_graph.txt 
```

Question 1) Compare the two .html files. What kinds of differences do you see in the files? Why do you think these differences are found (hint: think about the types of data you are analyzing)? 


2. Within prinseq (and most other programs), "trimming" will not affect the number of reads you have, but will alter the characteristics of the sequencing, while "filtering" will remove reads that do not pass the criteria. This can result in un-paired reads in the two _1 and _2 files, but some programs (including prinseq) take care of this by outputting separate files for the good (i.e. paired) reads and the unpaired (i.e. singletons) where one read has been filtered. 

Trim off bases from either end with less than a quality score of 10 (-trim_qual_left and -trim_qual_right), and filter any sequences that have less than 70 base pairs (-min_len 70):

```bash
perl ~/Topic_3/scripts/prinseq-lite-0.20.4/prinseq-lite.pl -fastq ~/Topic_3/data/GBS12_brds_Pi_197A2_100k_R1.fastq -fastq2 ~/Topic_3/data/GBS12_brds_Pi_197A2_100k_R2.fastq -log log1 -out_good GBS_filter1 -min_len 70 -trim_qual_left 10 -trim_qual_right 10 
```

Note that you can view the log file (log1) to see the command executed and the output (including the default parameters run)

This level of filtering by minimum length is probably overkill for most applications, but it gives you a chance to see how prinseq is changing the data. 

You should see 2 output files called GBS_filter1_#.fastq and two more that have a "_singletons" suffix. These "singleton" files are those that were left unpaired after the corresponding sequence was filtered from the alternate read direction file.

Try running various combinations of these commands, regraph the results, and see how the statistics have been affected. 

```bash
perl ~/Topic_3/scripts/prinseq-lite-0.20.4/prinseq-lite.pl -fastq GBS_filter1_1.fastq -fastq2 GBS_filter1_2.fastq -graph_data GBS_filter1.txt 
```

Experiment with other filters, such as filtering sequences with a mean quality score below some value (here, Q15):

-min_qual_mean 15

Or removing N's from the left and right hand side of the sequences:

-trim_ns_left 3 (Trim poly-N tail with a minimum length of 3 at the 5'-end)
-trim_ns_right 3 (Trim poly-N tail with a minimum length of 3 at the 3'-end)

Question 2) Try different filtering options for the GBS data  (see http://prinseq.sourceforge.net/manual.html for options) and plot QC graphs. Discuss in a group of four which options you would choose to implement if this was your data.


Low complexity sequences can be filtered using:

-lc_method [dust/entropy] -lc_threshold [7

From the manual: 

	The DUST approach is adapted from the algorithm used to mask low-complexity
	regions during BLAST search preprocessing [5]. The scores are computed based on
	how often different trinucleotides occur and are scaled from 0 to 100. Higher
	scores imply lower complexity. A sequence of homopolymer repeats (e.g. TTTTTTTTT)
	has a score of 100, of dinucleotide repeats (e.g. TATATATATA) has a score around
	49, and of trinucleotide repeats (e.g. TAGTAGTAGTAG) has a score around 32.

	The Entropy approach evaluates the entropy of trinucleotides in a sequence. The 
	entropy values are scaled from 0 to 100 and lower entropy values imply lower 
	complexity. A sequence of homopolymer repeats (e.g. TTTTTTTTT) has an entropy 
	value of 0, of dinucleotide repeats (e.g. TATATATATA) has a value around 16, and
	of trinucleotide repeats (e.g. TAGTAGTAGTAG) has a value around 26.

	Sequences with a DUST score above 7 or an entropy value below 70 can be considered 
	low-complexity. An entropy value of 50 or 60 would be a more conservative choice.


NOTE: prinseq has an order of operations, which it tells you when you run it with the -h option. Be mindful of this, because it will calculate the mean quality score differently if other operations are run first.

There is no best option for trimming/filtering. The choices you make should reflect the application and the state of your data. There is such a thing as too much filtering/trimming. 




# Manipulating files in UNIX and bash 

Below are a bunch of examples for how to manipulate files. They are intended to provide an introduction to the kinds of things you can do in unix, and give you an idea of where to google to find more info. Generally, I have found that googling for what you want to do will almost always yield a scripting solution, often a very easy one using either awk, grep, sed, or some other bash command. 

- To cancel any command, type "control-c". To pause it and send it to run in the background, type "control-z" and then "bg". To bring the job back to the foreground, type "fg"

- to view the processes that are currently running, type "top" and then "q" to quit. 

- to stop a process, type "kill PID", where PID is replaced with the process ID number you see in the "top" list

- cp, mv, rm, rmdir, mkdir will copy, move, remove, remove directory, and make directory, respectively

- FASTQ files have four lines per sequence. To get the first 100 lines (25 sequences):

```bash
head -100 ~/Topic_3/data/PmdT_147_100k_R1.fq
```

- to copy them into a new file, use the ">" character to move information from the left hand side and print it to a file on the right hand side

```bash
head -100 ~/Topic_3/data/PmdT_147_100k_R1.fq > new.fq
```


```bash
head -100 ~/Topic_3/data/PmdT_147_100k_R1.fq | wc -c
```

- to get only the sequence names, but nothing else, use "grep", searching for the characteristic string @HWI-ST, which is at the beginning of each of the reads in these files. Note, that you shouldn't grep for just the "@" character, because it is also a quality score encoding:

```bash
grep @HWI-ST ~/Topic_3/data/PmdT_147_100k_R1.fq
```
- On the contrary, with .fasta files, it is possible to just grep for the ">" character. Sometimes fasta files will have multiple lines of sequence for a given contig, each of which is separated by a newline character. In this case, you can't count the number of contigs just by counting the number of lines in the file. A simple count can be done by grep'ing lines that start with ">":

```bash
grep "^>" ~/Topic_3/data/Pine_reference_rnaseq_reduced.fa | wc -l
```

- If you want to find a specific contig and get all of the sequence from a fasta file, this is a handy one-liner: 

```bash
sed -n '/>comp10454_c2_seq1/,/>/p' ~/Topic_3/data/Pine_reference_rnaseq_reduced.fa | grep -v ">" | tr -d "\n"
```

This uses sed to find everything between the two matches, the first of which is ">comp10454_c2_seq1" and the second is the next ">" character. Note the use of tr -d "\n" to clip the new line characters and return a single contiguous sequence.

- To get the last 10 lines of a file:

```bash
tail -10 ~/Topic_3/data/Pine_reference_rnaseq_reduced.fa
```

- head and tail can be used together to get a few lines of interest:

```bash
head -1000 ~/Topic_3/data/Pine_reference_rnaseq_reduced.fa | tail -10
```

- to change all of the N's to A's, use sed:

```bash
sed 's/N/A/g' file1 > file2
```

- to remove double quotation marks that may occur in R output when the quote=F option has not been used:

```bash
sed 's/\"//g' file > file2
```

- To print the first column of a file that has rows and columns (typical R-output):

```bash
awk '{print $1}' ~/Topic_3/data/cold_hot_expression.txt 
```

- to sort this column and output it to a new file:

```bash
awk '{print $1}' ~/Topic_3/data/cold_hot_expression.txt | sort > new2.txt
```

- to join the sorted column with the old dataset:

```bash
paste ~/Topic_3/data/cold_hot_expression.txt new2.txt > new3.txt
```

- to output only the lines of a file that have a number > 85 in the 12th column:

```bash
awk '{if($12 > 85){print}}' ~/Topic_3/data/sample_blast_results.txt > sample_blast_results_filt.txt
```

This is really useful for filtering BLAST tabular format results. Another nice way to sort blast results to get the best hit for each contig that you queried, sorted by bit score and e-value:

```bash
sort -k1,1 -k12,12nr -k11,11n ~/Topic_3/data/sample_blast_results.txt | sort -u -k1,1 --merge > sample_blast_results_sorted.txt
```

- to output columns 3-8:

```bash
cut -f3-8 ~/Topic_3/data/sample_blast_results.txt > sample_blast_results_sub.txt
```

- to count the number of occurrences of "N" that occur in each line of a file, from the 3rd column onwards:

```bash
awk '{s=0; for (i=3; i <= NF; i++) {if($i == "N"){s=s+1}}}; {print s}' ~/Topic_3/data/sample_depths.txt > file_withnumber_ofNs.txt
```

- to get the mean from all of the non-N entries in the same file, excluding the N's:

```bash
awk 'BEGIN {FS=OFS=" "}{sum=0; n=0; for(i=3;i<=NF;i++){if ($i != "N"){sum+=$i; ++n}} print sum/n}' ~/Topic_3/data/sample_depths.txt > sample_depths_rowmeans.txt
```

NOTE: these operations using awk are MUCH faster than R, especially on large files, and they are easy to integrate into shell scripts.

- to list the files in a directory and pick out all of the fastq files:

```bash
ls | grep fastq$
```

or

```bash
ls *fastq
```

- to move these files to a sub-directory called "sub":

```bash
ff=`ls | grep fastq$`
mv $ff ./sub
```

This stores the contents of the ls \| grep command in a variable calld "ff" which can be used by prefixing it with the "$" sign. This is very useful for moving files around or deleting them.

- to check through a bunch of files and see how many lines are in each, with output to the screen (press enter for each line):

```bash
ff=`ls | grep fastq$`
for i in $ff
do
echo $i
wc -l $i
done
```

If you do much playing around with sequence data, especially whole genome sequence files, learning to use awk, sed, grep, and other bash commands will be incredibly helpful and more efficient than relatively slow approaches such as reading data into R and manipulating it there.
