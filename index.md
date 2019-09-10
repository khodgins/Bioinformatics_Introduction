---
title: "Hands on Workshop: Introduction to Bioinformatics Homepage"
layout: home
menuItem: "General info"
menuPosition: 1
---

<h1>{{ site.courseName }}</h1>

<img src="{{ site.baseurl }}/style/header.jpg" width="100%" alt="Nitobe Memorial Gardens -- CC BY-NC 2.0 - @kardboard604 https://www.flickr.com/photos/moov4/10157685024/" title="Nitobe Memorial Gardens -- flickr @kardboard604">

## Description
The purpose of this course is to provide graduate students with the theoretical knowledge and practical skills for the analysis of high throughput sequence data. The course will entail data retrieval and assembly, alignment techniques, variant calling, gene expression analyses, hypothesis testing, and population genomic and phylogenomic approaches. The course will be presented as a series of short lectures and lab exercises over a one week period during the mid semester break of the second semester.

## Instructors
Dr. Kathryn Hodgins, Dr. Gregory Owens, Dr. Matt McGee, Dr. Sonika Tyagi

## Format
A mix of lecture and tutorial exercises running in a 2-hour block.

## Prequisites
Prior exposure to R and Unix command line is recommended. However, we will be running a crash course on command line the first day. 

## Lectures/tutorials
30 Sept 2019 – 4 Oct 2019
10am to Noon, 1pm to 3pm


## Where
Room 144 (Digilabs), Building 17 School of Biological Sciences


## Basic course structure

The course material is organized in several topics, with slides and coding examples.

To get up to speed on working with a Unix system, take a look at the [unix help](resources/unix_ref.pdf) file. There are some resources there that will help you find the specific command you need for each task.

## Syllabus
1. [Topic 1](./Topic_1/) Workshop overview & sequencing technology and approaches (Day 1 AM)
    Lecture: Workshop overview and goals (Kay)
             Sequencing technology and approaches ()
    Tutorial: Logging onto the server  (Damien, Jerico)
2. [Topic 2](./Topic_2/)  Command line introduction (Day 1 PM)
    Tutorial: Command line introduction (Damien) 
3. [Topic 3](./Topic_3/) Sequence file formats and quality control/trimming (Day 2 AM)
    Lecture: Sequence file formats and quality control/trimming (Kay) 
    Tutorial: QC GBS and RNAseq reads (Kay)
4. [Topic 4](./Topic_4/) Sequence alignment (Day 2 PM) 
    Lecture: Alignment algorithms and tools (Greg)
    Tutorial:  Alignment of GBS reads (Greg)
5. [Topic 5](./Topic_5/) Variant calling (Day 3 AM)
    Lecture: SNP and variant calling (Greg) 
    Tutorial: Call variants for GBS (Greg) 
6. [Topic 6](./Topic_6-7/) Population genomics (Day 3 PM) 
    Lecture: Population genomics (Greg)
    Tutorial: Population genomics (Greg)
7. [Topic 7](./Topic_6-7/) Population genomics and plotting in R (Day 4 AM) 
    Tutorial: Population genomics and plotting in R (Greg)
8. [Topic 8](./Topic_8/) Phylogenetics (Day 4 PM) (Matt)
9. [Topic 9](./Topic_9/) Genome assembly (Day 5 AM) (Sonika)
10. [Topic 10](./Topic_10/) RNAseq + differential expression analysis (Day 5 PM) (Sonika)

## Obtaining all the files on this site

You may use your internet connection to browse this site, or
you may download the entirety of the files on the site in one
constantly updated zip archive
[here](https://github.com/owensgl/biol525D/archive/master.zip)

This method dosesn't require `git`, however, you'll have to manually
update the files this way (by downloading the whole repo again).

To obtain to all the files via git, type:

    git clone https://github.com/owensgl/biol525D.git

To update the all the files at any point in the future, change to the **biol525D** directory that was created by the previous command and type:

    git pull


## Use and modification of these resources

You may use any of the materials provided here, and modify them in any way, provided there is appropriate attribution according the license found below and included with this project.

## License and Copyright

Copyright (C) 2015 S. Evan Staton, Sariel Hubner, Sam Yeaman

Modified work (c) 2016, 2017, 2018 Gregory Owens, Kathryn Hodgins

Modified work (c) 2019 Gregory Owens, Kathryn Hodgins, J.S. Legare

This program is distributed under the MIT (X11) License, which should be distributed with the package.
If not, it can be found here: http://www.opensource.org/licenses/mit-license.php

## About this site:

  This site is powered by GithubPages, and the code backing it is on GitHub [here](https://github.com/owensgl/biol525D/).
