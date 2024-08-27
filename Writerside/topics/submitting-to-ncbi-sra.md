# SRA Submission Overview

This document will walk you through the process of submitting raw sequencing data to NCBI’s Sequence Read Archive (**SRA**) from Terra. 
The basic steps may vary depending on the type of data your submitting and were the data is currently stored, but for our purposes will be:
1. Transferring sequencing data from Terra to CholGEN storage.
2. Providing sample and sequencing metadata to SRA.
3. Specifying to SRA where data is stored.
4. Releasing data publicly.

We will first describe what SRA is and how data is organized within as it is a valuable resource to any bioinformatician.

## SRA overview
SRA is a publicly available repository of raw high throughput sequencing data. 
The repository contains data from all branches of life, and metagenomic and environmental surveys. 
There is generally no restriction on what can be uploaded to SRA, though there are some extra requirements if you are uploading human sequences. 
The archive is a collaboration between the US’s National Institute of Health, the European Bioinformatics Institute, and the DNA Database of Japan.

## Samples, Runs, and Projects
On SRA, the paired-end sequencing data files (fastq.gz files) generated from a single sample are called a **Run**. 
This is the basic unit of data of SRA and is what we’ll be uploading to SRA. 
Each run contains sequencing data from a single biological sample and information about how the data was generated (for example, what library preparation protocol, sequencer, sequencing kit was used). 
SRA also allows you to upload metadata about the sample including, what organism it is, where and when it was collected, how it was processed and sequenced, etc. 
This data is kept in a separate, but linked object called a **BioSample**. 
It is a separate entity, because technically, you could sequence the same sample multiple times.

Multiple Runs and BioSamples that were generated for the sample purpose or study can be put in collections called **BioProjects**. 
This allows other users to find all the samples that are relevant to your paper, for instance.

Therefore, to submit data to SRA, we need to upload the data, provide technical information about how that data was generated, and provide metadata on the sample that the data was generated from. 