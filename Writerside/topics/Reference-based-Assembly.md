# Reference-based Assembly
<card-summary>
    Generate a consensus sequences by aligning paired-end sequencing reads against a known reference genome.
</card-summary>

This document provides instructions for performing a reference-based assembly of *Vibrio cholerae*. 
You can perform the steps on raw sequencing data you have generated and copied onto your computer (see instructions to 
set up your project directory below). 

<include from="Creating-a-Project-Directory.md" element-id="intro-table"/>

This tutorial will take you through reference-based assembly from raw sequencing data. 
The process comprises five main steps, including a sequence assessment step:

1. Align paired-end FASTQ files to a reference genome, generating BAM files.
2. Call variants in the BAM file relative to the reference.
3. Generate a consensus sequence from the variants. 
4. Mask known recombinant regions from the consensus sequence.
5. Assess the quality of raw sequencing reads and alignment.

The `bacpage assemble` command will perform all of these steps on all the samples it automatically detects in your 
project directory's `input/` folder.

## Running the assembly pipeline
Running the pipeline is easiest from your project directory. 
If you have not created a project directory yet, complete the <a href="Creating-a-Project-Directory.md"> Creating a 
Project Directory</a> instructions to generate it.
<procedure type="steps">
    <step>
        Activate the bacpage environment:
        <code-block lang="bash">mamba activate bacpage</code-block>
    </step>
    <step>
        Navigate to your project directory.
        <code-block lang="bash" >cd ~/[project-directory-name]</code-block>
    </step>
    <step>
        Generate consensus sequences and calculate quality metrics for the samples in your project directory with a 
        single command:
        <code-block>bacpage assemble .</code-block>
        <note>
            This command might take a few hours to complete depending on the number of samples being processed, and 
            the cores available on your computer
            </note>
    </step>
</procedure>

This command will generate a consensus sequence in FASTA format for each of your samples and place them in 
<code><b>[project-path]</b>/results/consensus_sequences/<b>[sample]</b>.masked.fasta</code>. 

An HTML report containing alignment and quality metrics for your samples can be found at 
<code><b>[project-path]</b>/results/reports/    qc_report.html</code>. 
It can be opened and viewed in any web browser.

<procedure title="Useful options">
    <step>
        <b>bacpage</b> will search through the provided project directory for <code>config.yaml</code> and <code>sample_data.csv</code>. 
        If its unable to find them, or if you would like to keep them in other locations, you can specify their paths 
        using the <code>--configfile</code> and <code>--samples</code> options, respectively.
    </step>
    <step>
        By default, <b>bacpage</b> will automatically use all the processors of your computer to parallelize jobs. 
        If you would like to limit the amount of processors available to bacpage, specify the maximum number of cores to
        use with the <code>--threads</code> option.
    </step>
    <step>
        By default, the pipeline will align reads against a <i>Vibrio cholerae</i> reference included with the program. 
        If you are studying another pathogen, you can change the "<i>reference</i>" parameter in 
        <code>config.yaml</code> to another reference sequence (must be in FASTA format).
    </step>
</procedure>

## Handling errors
If the command completed successfully, you should see something like this on screen:

```Bash
Successfully performed reference-based assembly of your samples.
Consensus sequences are available at results/consensus/.
Quality metrics of your input data and consensus sequences are available at results/reports/qc_report.html

Generate a phylogenetic tree incorporating these samples using `bacpage phylogeny`.
Or, determine the presense of antimicrobial resistance genes using `bacpage profile`.
```

Otherwise, a traceback error will be printed to the screen if the program was unsuccessful. 
In some cases, the error will be resolved by rerunning the bacpage command:
<code-block lang="bash" >
bacpage assemble .
</code-block>

If the error is still unresolved, feel free to create an issue on our 
<a href="https://www.github.com/CholGen/bacpage/">GitHub repo</a>.

## Automatic detection of samples
The `bacpage assemble` command will try to automatically detect your samples using the files present in 
the `input/` folder of the project directory. The command will save information about the samples it found in a file 
called `sample_data.csv` in your project directory.
This file has the following required columns:
<deflist type="narrow">
    <def title="sample">the name or identifier of each sample</def>
    <def title="read1">the absolute pathname of the FASTQ file corresponding to the first set of reads for a sample (generally containing “R1” in the filename)</def>
    <def title="read2">the absolute pathname of the FASTQ file corresponding to the second set of reads for a sample (generally containing “R2” in the filename)</def>
</deflist>
<tip>
    By default, bacpage will parse sample names by extracting the first portion of the filename after splitting it with an underscore 
    (<shortcut>_</shortcut>) character. 
    If you would like to change this behavior, delete the `sample_data.csv` file, and change the deliminator and 
    index to parse using the <code>--delim</code> and <code>--index</code> options. You can also specify the data 
    for your samples by editing this file manually in a program like Excel. `bacpage assemble` will find the updated 
    file next time you run the command.  
</tip>