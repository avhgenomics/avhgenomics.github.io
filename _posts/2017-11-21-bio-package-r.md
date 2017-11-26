---
layout: post
title: AVBio Package now has basic support for Bedtools
subtitle: sortBed implemented, with plans to create generalized function
bigimg: /img/implosion_3440x1440.png
---

 **Developing a basic Bedtools wrapper for various projects**

Rmarkdown provides a great way to create reproducible analyses with the code for every step built directly the document.  Its strength is in the diversity of R packages available; however, several bioinformatics tools don't have support in R.  Rmarkdown can get around this by running system commands or marking a section as bash, but compiling some repetitive, commonly used commands can contribute to a mess of code.

To get around this, I wrote a few basic functions that would implement some of the common aspects of the tool *Bedtools*, slightly easing the amount of code in a document that is using bed files for analysis.  At the time these were created, I didn't have access to an R package which could do what was required, though this may be different now.

*The Support of Bedtools in the AVBio package*

Currently, this has two different functions to create and run a system command for *Bedtools*.  This will:

* Sort bed files from an input string,list, or vector, then append a custom suffix in an output directory.  This function also has the option to write a log file in the output directory which notes the time, command used, and what files went where.  This appends, so a record of each file in its current state should always be kept up to date.  For ease of understanding, the initial section of the log output also writes a description of the input, the action, and the output, making it easy for people unfamiliar with coding to follow the "file trail".

* Provide a general wrapper for Bedtools (though somewhat buggy at the moment).  This works by replacing the "sortBed" command with any given command, though only if the given options after utilize a "-i" flag.  For certain others, like "intersectBed", these use a "-a , -b" which will require a little bit of reorganization to incorporate.

Other functions this package currently has:

* Assessing RNA purity for samples with an emphasis on Nanodrop readouts.

* Calculating cDNA reactions for each sample.

* Providing densitometry for shearing gels (mainly for ChIP)

* ddCt Gene expression analysis with biological replicates

* Percent of Total Input ChIP analysis with biological replicates

At some point I would like to open this package to a wider community as these functions are refined towards more general applications.  If you have an interest in discussing these future applications or how these functions are built, please contact me! I would be happy to discuss it.
