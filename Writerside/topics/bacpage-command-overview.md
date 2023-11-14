# Pipeline Overview
<card-summary >
    Explanation of the bacpage command and how you can use it to assemble and analyze bacterial 
    pathogen genomes.
</card-summary>

<b>bacpage</b> consists of a number of tools to assemble and analyze bacteria genomes.
Behind the scenes, the tools run Snakemake pipelines which utilize a series of command line bioinformatics tools. 
Snakemake is a tool for creating reproducible and modular bioinformatic pipelines by reducing the number of commands 
you need to type, parallelizing steps across all your samples, and confirming steps were completed successfully. 

<note>
    This document assumes that you've completed the installation of the bacpage mamba environment. If this is not the 
    case, please complete <a href="Bioinformatics-Pipeline-Setup.md"/>.
</note> 

## Command Structure
 
The different tools provided by bacpage are meant to be modular, such that the output of one command serves as input to 
the other commands.
You can access each of the tools through the <code>bacpage</code> program followed by the name of the command, for 
example <code>bacpage phylogeny</code> to reconstruct a phylogeny.

At the present, the following tools are available in bacpage:
<deflist type="narrow">
    <def title="example">
        Generates a project directory for an analysis
    </def>
    <def title="identify_files">
        Automatically searches through a project directory for input FASTQ files and creates a valid sample data file.
    </def>
    <def title="assemble">
        Performs reference-based or <i>de novo</i> assembly on the samples present in a project directory.
    </def>
    <def title="phylogeny">
        Reconstructs a phylogeny using the consensus sequences present in a project directory and an 
        optional background dataset.
    </def>
    <def title="profile">
        Detects the presence and identity of specified gene sets, including antimicrobial resistance genes,
        present in consensus sequences or <i>de novo</i> assemblies.
    </def>
</deflist>

All bacpage commands have command line options which modify how the analysis is run, and a positional argument
which specify the directory a command is to be performed in (if not specified bacpage will default to the current 
working directory).
Therefore the basic command is as follows:
<code-block lang="bash" >bacpage {subcommand} [-options] [project-directory]</code-block>

<tip>
    You can also run each command with the <code>--help</code> option, for example <code>bacpage phylogeny --help</code>, 
    for more information at the command-line.
</tip>