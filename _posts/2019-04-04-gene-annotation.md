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

This structure is often found in prokaryotic DNA preceding the start codon by 3-7 base pairs. This structure is characterized by the "AGGAGG" sequence. This sequence can vary slightly across genes so for this program only 5 base pairs are required to match the "AGGAGG" sequence.

#### Opening reading frame length

An open reading frame (ORF) is the region of DNA that is transcribed between the start and stop codon. A candidate gene must be a reasonable length for it to be considered valid. For this project a minimum ORF leength of 100 base pairs was used.

####  The Program

The following **FindPattern** function searches a string (*search_text*) for a specific pattern (*pattern*). The indices to begin and stop searching can be specified by *start_loc* and *stop_loc* respectively. The *threshold* parameter determines the number of characters in the string that must match *pattern*. Finally, the *increment* parameter declares how many characters are skipped between searches. The funciton returns -1 if no match is found.

```python
def FindPattern(pattern, search_text, start_loc, stop_loc, increment, threshold):
    pattern_len = len(pattern)
    text_len = len(search_text)
    for i in range(start_loc, stop_loc + 1, increment):
        count = 0
        j = i
        for k in range(0, pattern_len):
            if search_text[j] == pattern[k]:
                count = count + 1
            j = j + 1
        if count >= threshold:
            return i
    return -1
```


This function can be used to search genetics for specific sequences in genetic data. Here is a snippet from the plasmid sequence of enterococcus bacteria that will be used as an example.

```
...TTCAAGACAATTTCAGCAGTTGAACGAGATCGGAATGGATATTATATATGAAGAGAAAGTTTCAGGA
GCAACAAAGGATCGCGAGCAACTTCAAAAAGTGTTAGACGATTTACAGGAAGATGACATCATTTATGTTA
CAGACTTAACTCGAATCACTCGTAGTACACAAGATCTATTTGAATTAATCGATAACATACGAGATAA...
```

The **FindPattern** function can be used to find all of the gene markers described above. The first step is to find a start codon in the sequence.

```python
i = 0
while i < len1 - 102:
    atg = FindPattern('ATG', seq, i, len1 - 103, 1, 3)
    if atg == -1:
        break
    i = atg + 3
```

This code searches for the "ATG" pattern in the sequence (*seq*), starting at 1 and iterating through the whole sequence. The sequence stops 103 base pairs before the end of the sequence for efficiency because any start codon past that point will result in an ORF shorter than 100 base pairs. If no start codon is found in the sequence the loop breaks and the program terminates. Otherwise, the start location is avanced by three and the program proceeds to the next step: finding a stop codon.

```python
stops = []
    stop1 = FindPattern('TAG', seq, i, len1 - 3, 3, 3)
    if stop1 > -1:
        stops.append(stop1)
    stop2 = FindPattern('TGA', seq, i, len1 - 3, 3, 3)
    if stop2 > -1:
        stops.append(stop2)
    stop3 = FindPattern('TAA', seq, i, len1 - 3, 3, 3)
    if stop3 > -1:
        stops.append(stop3)
    if len(stops) == 0:
        continue
    firststop = min(stops)
```

The program searches the sequence for each of the 3 possible stop codons. If no valid stop codon is found the loop continues and searches for a new start codon. If any stop codon is found the program takes only the first one because it will terminate transcription.

Next the length of the ORF is calculated. If the ORF that was found is too short the loop continues and searches for a new start codon.

```python
ORFlen = firststop - atg
    if ORFlen < 99:
        continue
```

Finally the program searches for the shine-dalgarno seqeunce. The threshold for this search is given as 5 to allows one irreuglar character in the sequence. If no shine-dalgarno sequence is found the
```python
aggagg = FindPattern('AGGAGG', seq, atg - 13, atg - 9, 1, 5)
    if aggagg == -1:
        continue
```
#### Output sample

The full source code can be found [here](https://github.com/spencernewellevans/gene-locator)
