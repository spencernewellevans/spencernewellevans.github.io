---
title: "Prokaryotic Gene Annotation"
date: 2019-03-18
tags: [Bioinformatics, Python]
header:
    #image: "images/mcrpc/image.jpg"
excerpt: "Bioinformatics, Python"
---

## Problem definition

Identifying genes is an important problem in genetic research.

Automating this process allows much faster sequencing and analysis of genetic data.

## Gene markers

There are several genetic markers that are useful in automated gene annotation. These markers can be used to indentify and locate genes in a large strings of genetic data.

![gene-annotation](/images/ORF.jpg)

#### Start codon

The start codon is defined by the "ATG" sequence in genomic data. This sequence acts as a marker that starts the transcription of DNA.

#### Stop codons

Stop codon can occur as "TAG", "TGA" and "TAA" sequences. This codon signifies the end of transcription.

#### Shine-Dalgarno Sequence

This structure is often found in prokaryotic DNA preceding the start codon by 7-13 base pairs. This structure is characterized by the "AGGAGG" sequence. This sequence can vary slightly across genes so for this program only 5 base pairs are required to match the "AGGAGG" sequence.

#### Opening reading frame length

An open reading frame (ORF) is the region of DNA that is transcribed between the start and stop codon. A candidate gene must be a reasonable length for it to be considered valid. For this project a minimum ORF leength of 100 base pairs was used.

#### Code snippets

#### Output sample

The full source code can be found [here](https://github.com/spencernewellevans/gene-locator)
