# Reference-based Assembly
<card-summary>
    Generate a consensus sequences by aligning paired-end sequencing reads against a known reference genome.
</card-summary>

This document provides instructions for performing a reference-based assembly of *Vibrio cholerae*. 
You can perform the steps on raw sequencing data you have generated and copied onto your computer (see instructions to 
set up your project directory below). 

<include from="Genome-Assembly-on-the-Command-Line.md" element-id="intro-table"/>

This tutorial will take you through reference-based assembly from raw sequencing data. 
The process comprises five main steps, including a sequence assessment step:

1. Align paired-end FASTQ files to a reference genome, generating BAM files.
2. Call variants in the BAM file relative to the reference.
3. Generate a consensus sequence from the variants. 
4. Mask known recombinant regions from the consensus sequence.
5. Assess the quality of raw sequencing reads and alignment.


## Running the assembly pipeline
Running the pipeline is easiest from your project directory. 
If you have not created a project directory yet, complete the <a href="Creating-a-Project-Directory.md"> Creating a 
Project Directory</a> instructions to generate it.
<procedure type="steps">
    <step>
        Navigate to your project directory.
        <code-block lang="bash" >cd ~/[project-directory-name]</code-block>
    </step>
    <step>
        Generate consensus sequences and calculate quality metrics for the samples in your project directory with a 
        single command:
        <code-block>bacpage assemble .</code-block>
    </step>
</procedure>

This command will generate a consensus sequence in FASTA format for each of your samples and place them in 
<code><b>[project-path]</b>/results/consensus_sequences/<b>[sample]</b>.masked.fasta</code>. 

An HTML report containing alignment and quality metrics for your samples can be found at 
<code><b>[project-path]</b>/results/reports/qc_report.html</code>. 
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
        By default, the pipeline will align reads against a *Vibrio cholerae* reference included with the program. 
        If you are studying another pathogen, you can change the "<i>reference</i>" parameters in 
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


