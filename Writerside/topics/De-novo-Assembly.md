# De novo Assembly
<card-summary >
    Generate an assembly by de novo assembly of Illumina paired-end sequencing reads.
</card-summary>

This document provides instructions for performing a *de novo* assembly of a bacteria genome.
You can perform the steps on raw sequencing data you have generated and copied onto your computer (see instructions to
set up your project directory below).

<include from="Genome-Assembly-on-the-Command-Line.md" element-id="intro-table"/>

This tutorial will take you through *de novo* assembly from raw sequencing data.
The process comprises five main steps, including a sequence assessment step:

1. Trimming paired-end reads of low quality bases and adapters.
2. Stitch overlapping read pairs.
3. Assemble short reads into sets of contigs (i.e. assemblies).
4. Identify genes of interest in assemblies.
5. Assess the quality of raw sequencing reads and assemblies.


## STEP 1: Running the assembly pipeline
Running the pipeline is easiest from your project directory.
If you have not created a project directory yet, complete the <a href="Creating-a-Project-Directory.md">Creating a
Project Directory</a> instructions to generate it.
<procedure type="steps">
    <step>
        Navigate to your project directory:
        <code-block lang="bash" >cd ~/[project-directory-name]</code-block>
    </step>
    <step>
        Generate an annotated assembly and calculate quality metrics for each sample in your project directory with a 
        single command:
        <code-block>bacpage assemble --denovo .</code-block>
    </step>
</procedure>

This command will generate a annotated assembly in general feature format (gff) for each of your samples and place them in
<code><b>[project-path]</b>/results/assembly/<b>[sample]</b>.annotated.gff</code>. 

An HTML report containing assembly and quality metrics for your samples can be found at
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
</procedure>


