# De novo Assembly
<card-summary >
    Generate a de novo assembly of Illumina paired-end sequencing reads.
</card-summary>

This document provides instructions for performing a *de novo* assembly of a bacteria genome.
You can perform the steps on raw sequencing data you have generated and copied onto your computer (see instructions to
set up your project directory below).

<include from="Reference-based-Assembly.md" element-id="assemble-table"/>

This tutorial will take you through *de novo* assembly from raw sequencing data.
The process comprises five main steps, including a sequence assessment step:

1. Trimming paired-end reads of low quality bases and adapters.
2. Stitch overlapping read pairs.
3. Assemble short reads into sets of contigs (i.e. assemblies).
4. Identify genes of interest in assemblies.
5. Assess the quality of raw sequencing reads and assemblies.

The `bacpage assemble` command will perform all of these steps on all the samples it automatically detects in your
project directory's `input/` folder.

## Running the assembly pipeline
Running the pipeline is easiest from your project directory.
If you have not created a project directory yet, complete the <a href="Creating-a-Project-Directory.md">Creating a
Project Directory</a> instructions to generate it.
<procedure type="steps">
    <step>
        Activate the bacpage environment:
        <code-block lang="bash">mamba activate bacpage</code-block>
    </step>
    <step>
        Navigate to your project directory (generally it is in your home directory).
        <code-block lang="bash" >cd [project-path]</code-block>
    </step>
    <step>
        Generate an annotated assembly and calculate quality metrics for each sample in your project directory with a 
        single command:
        <code-block>bacpage assemble --denovo .</code-block>
        <note>
            This is the same command used to perform reference-based assembly, just with the <code>--denovo</code> 
            option.
        </note>
        <note>
            De novo assembly is much more computationally intensive than reference-based assembly. Depending on the 
            number and size of samples you have, it's not uncommon for this command to take hours to complete.
        </note>
    </step>
</procedure>

This command will generate an annotated assembly in generic feature format (GFF) for each of your samples and place 
them in <code><b>[project-path]</b>/results/assembly/<b>[sample]</b>.annotated.gff</code>. 

> The [GFF](http://gmod.org/wiki/GFF3) file is a human-readable file which contains a section for the sequences of 
> the assembly in FASTA format, and a section containing the known genes that were found in the sequences of the 
> assembly. 
> Known genes are derived from the [UniProtKB](https://www.uniprot.org/uniprot/?query=reviewed:yes) database of 
> described Bacteria genes. 
> The GFF file can be used by most programs that take a FASTA file, or you can extract the FASTA section alone. 

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


