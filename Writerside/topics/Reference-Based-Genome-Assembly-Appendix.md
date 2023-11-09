# Reference-Based Genome Assembly Appendix

The following instructions will describe the intermediate steps of generating the consensus sequence.
This process can be optionally done instead of running the complete pipeline using the instructions above.

<include from="Genome-Assembly-on-the-Command-Line.md" element-id="intro-table"/>

## STEP 1: Align Paired-End FASTQ Files to a Reference Genome

The first genome assembly step is to take the paired-end FASTQ files produced by a sequencing machine and align them 
against a reference genome. 
This will generate a BAM file for each sample that is needed for future steps of the pipeline.

> By default, the pipeline will align reads against a Vibrio cholerae reference. 
> If you are studying another pathogen, you can change the "<i>reference</i>" parameters in 
> <code>[<control>project-path</control>]/config.yaml</code> to another reference sequence.
> {style="note"}

<procedure type="steps">
    <step>
        Navigate your current working directory to the location of the bioinformatics pipeline 
        (generally <code>~/bacpage</code>). 
        All of the following steps should be conducted in this directory. 
        Type the following into your terminal window and then press <shortcut>Enter</shortcut>:
        <code-block lang="bash">cd ~/bacpage</code-block>
    </step>
    <step>
        Run the <control>alignment_bwa</control> step of the pipeline using the following command.
        <code-block lang="bash">
        snakemake --configfile [project-path]/config.yaml --cores [number-of-processors] --keep-going --until alignment_bwa
        </code-block>
    </step>
</procedure>

## STEP 2: Call Variants Relative to Reference Genome

The next step in genome assembly is to determine differences between the sequenced reads and the reference genome. 
These differences are called variants and describe evolution of the sample from the reference genome 
(typically the earliest genome for an organism).

<procedure type="steps">
    <step>
        Run the variant calling step of the pipeline using the following command:
        <code-block lang="bash" >
            snakemake --configfile [project-path]/config.yaml --cores [number-of-processors] --keep-going --until align_and_normalize_variants
        </code-block>
        This command calls variants for each sample indicated in the sample_data.csv file, filters low-quality and 
        unsupported variants, and normalizes insertions and deletions. 
        A Variant Call Format file (VCF file) will be generated for each sample with the format 
        <code><control>[sample]</control>.filt.norm.vcf.gz</code>
        (see the description of the VCF format on <a href="https://en.wikipedia.org/wiki/Variant_Call_Format">Wikipedia</a>). 
        VCF files for every sample can be found in <code><control>[project-path]</control>/intermediates/illumina/variants/</code>
    </step>
</procedure>

## STEP 3: Generate a Consensus Sequence

We will now generate a genome sequence for each sample by applying the variants to our reference sequence. 
Because this genome is a summary of many sequencing reads, we call this sequence a consensus sequence.

<procedure type="steps">
    <step>
        Run the consensus calling step of the pipeline using the following command:
        <code-block lang="bash" >
            snakemake --configfile [project-path]/config.yaml --cores [number-of-processors] --keep-going --until call_consensus
        </code-block>
    This command generates a consensus sequence for each sample indicated in the <code>sample_data.csv</code> file. 
    A FASTA file containing the consensus sequence will be generated for each sample with the format <code><control>[sample]</control>.consensus.fasta</code>. 
    FASTA files for every sample can be found in <code><control>[project-path]</control>/intermediates/illumina/consensus/</code>.
    </step>
</procedure>

## STEP 4: Mask the Consensus Sequence

We recommend masking regions of the genome that have low coverage and/or are wholly recombinant. 
Masking these regions prevents erroneous results from downstream analyses including typing and phylogenetic inference. 
Low coverage regions can be determined directly from the BAM file generated for each sample, while wholly recombinant 
regions require prior knowledge of the organism being studied. 
As part of the pipeline, we have provided a file indicating the wholly-recombinant regions of the cholera genome. 
If you are studying another organism, you will need to update the recombinant_mask parameter in the config file to 
another organism-specific mask.

<procedure type="steps">
    <step>
        To run the masking step of the pipeline, run the following command:
        <code-block lang="bash" >
            snakemake --configfile [project-path]/config.yaml --cores [number-of-processors] --keep-going --until mask_consensus
        </code-block>
        This command masks the consensus sequence for each of your samples. 
        A FASTA file containing the masked consensus sequence will be generated for each sample with the format <code><control>[sample]</control>.masked.fasta</code>.
        Masked FASTA files for every sample can be found in: <code><control>[project-path]</control>/intermediates/illumina/consensus/</code>
    </step>
</procedure>

## STEP 5: Assess Assembly Quality

A key component of generating genome assemblies is assessing their quality. 
We will generate reports to visually inspect the quality, coverage, and confidence we have in the resultant consensus 
sequences.

<procedure type="steps">
    <step>
        Generate the quality control reports using the following command:
        <code-block lang="bash">
            snakemake --configfile [project-path]/config.yaml --cores [number-of-processors] --keep-going --until generate_complete_report
        </code-block>
        This command  generates a single HTML report containing quality metrics for all samples. The HTML report can be 
        found in <code><control>[project-path]</control>/results/reports/qc_report.html</code>.
    </step>
    <step>
        2. Open this file with a web browser.
    </step>
</procedure>