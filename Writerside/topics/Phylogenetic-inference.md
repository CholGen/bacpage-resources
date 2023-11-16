# 5. Phylogenetic Reconstruction
<card-summary>
    Infer a phylogenetic tree from consensus sequences and an optional background dataset. 
</card-summary>

This document provides instructions for an initial analysis, including building a phylogenetic tree, of bacterial 
pathogen sequences. 
This analysis requires you to first generate and assemble consensus genomes from raw sequencing data.

<procedure type="choices" title="Important notes for following this tutorial" id="intro-table">
    <step>
        <code>Text with a gray background in monospace font</code> represents commands to type in.
    </step>
    <step>
        <b>Bold text surrounded by []</b> is something you will have to replace with your own folder, path, or sample 
        name.
    </step>
    <step>
        This tutorial assumes you have set up the bioinformatics pipeline on your computer, and have assembled
        consensus sequences in FASTA format.
        If this is <b>not</b> the case, complete the <a href="Bioinformatics-Pipeline-Setup.md">Bioinformatics Pipeline 
        Setup</a> and <a href="Reference-based-Assembly.md">Reference-based Assembly</a> instructions before proceeding.
    </step>
    <step>
        Phylogenetic analysis requires a background set of genomes to compare your sequences to. Background genomes 
        can help you identify which lineages your sequences belong to, and to determine if newly generated sequences are
        similar to previously published ones. Careful consideration should be given to including genomes which have an 
        appropriate temporal, geographical, and genetic diversity. 
    </step>
</procedure>

This tutorial will take you through the process of reconstructing a phylogeny from pathogen genome sequences. 
The process comprises three main steps:
1. Aligning consensus sequences with each other and, optionally, a background dataset.
2. Masking wholly recombinant regions from the alignment
3. Performing phylogenetic inference

## Setting up the project directory

This analysis assumes you have completed the [Created a Project Directory](Creating-a-Project-Directory.md)} and
[Reference-based Assembly](Reference-based-Assembly.md) instructions and have generated consensus sequences from raw 
sequencing reads for each of your samples.
<procedure type="steps">
    <step>
        Activate the bacpage environment:
        <code-block lang="bash">mamba activate bacpage</code-block>
    </step>
    <step>
        Navigate to your project directory:
        <code-block lang="bash" >cd ~/[project-directory-name]</code-block>
    </step>
    <step>
        Confirm that there are consensus sequences in your project directory. bacpage will search for FASTA files in
        <code><b>[project-path]</b>/results/consensus/</code>, as well as the base project directory, to include in the 
        tree. Check the contents of these locations with the following commands:
        <code-block lang="bash" >
            ls
            ls results/consensus/
        </code-block>
    </step>
</procedure>

## (Optional) Specify background dataset
While it can sometimes be useful to make a phylogenetic tree using only the newly generated sequences, it is generally 
more useful to combine newly generated sequences with a set of previously published sequences, called a "*background 
dataset*." If this section is not completed, a phylogeny will be generated with only the consensus sequences in your 
project directory.

<procedure type="steps">
    <step>
        Locate the background dataset you want to compare to your newly generated sequences. This dataset should be a 
        single FASTA file containing separate entries for each sequence in your background dataset, also called a 
        "<i>multi-FASTA</i>". 
    </step>
    <step>
        We recommend placing the multi-FASTA file in an easy to remember location such as the directory of the 
        bioinformatics pipeline (typically <code>~/bacpage</code>). You can do this using the command line or by 
        simply moving the file using the file browser on your computer.
    </step>
    <step>
        Once you have placed your background dataset FASTA into the resources directory, determine the absolute path of 
        the background dataset. If it was placed in the recommend location, you can find the absolute path by 
        navigating to the resources directory and checking the output of <code>pwd</code>:
        <code-block>
            cd ~/bacpage
            pwd
        </code-block>
        The absolute path of the background dataset will be the output of <code>pwd</code> plus the file name of the 
        background dataset (ending with .fasta).
    </step>
    <step>
        Navigate back to the project directory:
        <code-block lang="bash">cd ~/[project-directory-name]</code-block>
    </step>
    <step>
        Add the absolute path of your background dataset to <code><b>[project-path]</b>/config.yaml</code> in your project directory. Open 
        the configuration file in a text editor, change the value of <code><b>[background-dataset-path]</b></code> 
        on line 8 (see below), change the value of “<i>generate/phylogeny</i>” on line 15 to “True”, and save the file.
        <code-block lang="yaml" >
            background_dataset: "[background-dataset-path]"
        </code-block>
    </step>
</procedure>

## Running the phylogeny reconstruction pipeline
We will now generate a phylogeny including your newly generated genomes.
<procedure type="steps">
    <step>
        Run the phylogenetic inference step of the pipeline by running the following command:
        <code-block lang="bash" >bacpage phylogeny .</code-block>
        Briefly, this step concatenates your newly generated genomes, and the background dataset if its present, into a 
        multi-FASTA alignment. Aligning by concatenating is only possible because all of your newly generated sequences 
        and the background dataset were both assembled by aligning against a common reference. After concatenation, 
        constant sites are removed from the alignment which is then used to  generates a phylogeny using 
        <code>iqtree</code>.
        <p/>The output of this command is a phylogeny in Newick format, located at 
        <code><b>[project-path]</b>/results/<b>[project-directory-name]</b>.ml.tree</code>
        <note>
            We recommend that only sequences that cover at least 90% of the genome be included in the phylogenetic 
            inference. By default, the pipeline will only include sequences in the alignment if they reach this coverage
            threshold. If you want a different stringency, change the "<i>tree_building/required_coverage</i>" value in 
            <code><b>[project-path]</b>/config.yaml</code> to the desired value. 
        </note>
        <note>
            By default, <code>iqtree</code> will use the GTR substitution model and calculate branch support by 
            conducting 1000 bootstraps. These options can be changed by modifying the value of 
            "<i>tree_building/iqtree_parameters</i>" in <code><b>[project-path]</b>/config.yaml</code>.
        </note>
        <step>
            While the tree file is a text file that can be opened and read in a text editor, it is generally not 
            interpretable in this format. We recommend a GUI tree viewer called FigTree to view tree files.
        </step>
        <step>
            From the applications directory on your computer, open FigTree. Click <ui-path>File | Open</ui-path>, and 
            select the newly generated phylogeny in the file browser that opens up. Alternatively, you can open the 
            <code><b>[project-directory-name]</b>.ml.tree</code> file directly from the file browser.
        </step>
        <step>
            The phylogeny should now appear in the main FigTree window. Search through the tree for your samples, 
            determine which samples they are closest to. 
        </step>
    </step>
</procedure>
