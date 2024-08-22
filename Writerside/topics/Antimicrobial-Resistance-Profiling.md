# Antimicrobial Resistance Profiling
<card-summary>
    Screen your assemblies or consensus sequences for antimicrobial resistance or virulence genes. 
</card-summary>

This document provides instructions for screening your assemblies or consensus sequences for antimicrobial resistance 
or virulence genes.

<procedure type="choices" title="Important notes for following this tutorial" id="intro-table">
    <step>
        <code>Text with a gray background in monospace font</code> represents commands to type in.
    </step>
    <step>
        <b>Bold text surrounded by []</b> is something you will have to replace with your own folder, path, or sample 
        name.
    </step>
    <step>
        This tutorial assumes you have set up the bioinformatics pipeline on your computer, and have performed assembly 
        on your samples. We recommend <i>de novo</i> assembly for AMR profiling, but the pipeline will accept consensus 
        sequences if assemblies are not available. If neither type of assembly has been performed, complete the 
        <a href="De-novo-Assembly.md">De Novo Assembly</a> or 
        <a href="Reference-based-Assembly.md">Reference-based Assembly</a> instructions before proceeding.
    </step>
</procedure>

This tutorial will take you through profiling your samples for antimicrobial resistance genes. Briefly, this profiling 
process involves searching the nucleotide and amino acid sequence of your assemblies for genes present in public 
databases of antimicrobial resistance genes. Generally, profilers use rapid alignment programs like <code>BLAST</code>
to efficiently search through your sequences.

> We caution that the presence of an antimicrobial resistance gene does not necessarily indicate that the isolate 
> carrying the gene is resistant to the corresponding antibiotic. 
> This is because AMR genes must be expressed to confer resistance.
> Alternatively, some AMR genes, particularly those on plasmids, have a greater correlation between genotype and 
> phenotype. 
> Whereas sometimes an isolate may gain resistance to an antibiotic by mutational processes which would be missed by 
> the profiling described below.
> 
> Therefore, we caution readers against over-interpretation and recommend confirming resistance with an appropriate  
> laboratory assay.
{style="warning"}

## Setting up the project directory

This analysis assumes you have completed [Creating a Project Directory](Creating-a-Project-Directory.md) and the 
instructions for either [De novo Assembly](De-novo-Assembly.md) or [Reference-based Assembly](Reference-based-Assembly.md)  and have assembled 
sequences from raw sequencing reads for each of your samples.

<procedure type="steps">
    <step>
        Activate the bacpage environment:
        <code-block lang="bash">mamba activate bacpage</code-block>
    </step>
    <step>
        Navigate to your project directory:
        <code-block lang="bash">cd [project-path]</code-block>
    </step>
    <step>
        Confirm that there are assembled sequences in your project directory. bacpage will search for annotated 
        assemblies (.gff) and FASTA files (.fasta) in <code>results/assembly/</code>,
        <code>results/consensus/</code>, and the base project directory. Check the contents of these locations with the 
        following commands:
        <code-block lang="bash" >
            ls results/assembly/
            ls results/consensus/
            ls
        </code-block>
    </step>
</procedure>

## Running the profiling pipeline.

We will now profile the sequences in your project directory for antimicrobial resistance genes.

<procedure type="steps">
    <step>
        Run the profiling pipeline with the following command
        <code-block>bacpage profile .</code-block>
        <note>
            By default, <code>bacpage</code> profiles your sequences against the Comprehensive Antimicrobial Resistance Database 
            (card). If you prefer to use a different database, change the "<i>antibiotic_resistance/database</i>" 
            value in <code><b>[project-path]</b>/config.yaml</code> to any database listed by the command: <code>abricate --list</code>.
        </note>
    </step>
</procedure>

The pipeline will profile your sequences for antimicrobial resistance genes using <code>abricate</code> and save the 
results to <code><b>[project-path]</b>/results/reports/antibiotic_resistance_detailed.tsv</code>. The significant 
columns of the results are as follows:
<deflist type="narrow">
    <def title="FILE">The filename this hit came from</def>
    <def title="SEQUENCE">The sequence in the filename</def>
    <def title="GENE">The name of the antimicrobial resistance gene detected</def>
    <def title="ACCESSION">The accession number of the antimicrobial resistance gene. Search this on NCBI for more info.</def>
    <def title="%COVERAGE">Proportion of gene covered by your sample</def>
    <def title="%IDENTITY">Proportion of exact nucleotide matches between the database and your sample</def>
    <def title="PRODUCT">Gene product of the antimicrobial resistance gene</def>
    <def title="RESISTANCE">Putative antibiotic resistance conferred by gene, separated by <shortcut>;</shortcut></def>
</deflist>

A summary of the results is also written to <code><b>[project-path]</b>/results/reports/antibiotic_resistance.tsv</code>.
This summary reports the presence or absence antimicrobial resistance genes detected across all of your samples. Absent 
genes are denoted with a period (<shortcut>.</shortcut>), while presence is indicated by its **%COVERAGE**.