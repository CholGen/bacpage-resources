# 2. Creating a Project Directory
<card-summary >
    Prepare for an analysis by creating a project directory to hold all the files for your analysis
</card-summary>

The first step for setting up a **bacpage** analysis is creating a directory specifically for the dataset you are about 
to analyze.
This directory is where you will run analyzes and save outputs and is called your **project directory**.

<procedure title="Important notes for following this tutorial" id="intro-table">
    <step>
        <code>Text with a gray background in monospace font</code> represents commands to type in.
    </step>
    <step>
        <b>Bold text surrounded by []</b> is something you will have to replace with your own folder, path, or sample 
        name.
    </step>
    <step>
        This tutorial assumes you have set up the bioinformatics pipeline on your computer before starting the
        tutorial below, check if you can run the <code>bacpage</code> command on the command line. If you cannot, 
        complete the <a href="Bioinformatics-Pipeline-Setup.md">Bioinformatics Pipeline Setup</a> instructions to  
        install the necessary files and software before proceeding.
    </step>
</procedure>

We recommend you set up your project directory in your <tooltip term="HOME">HOME directory</tooltip> to ensure all 
sequencing data and results are in the same place.

<procedure type="steps" title="Creating a project directory from a template">
    <step>
        Activate the bacpage environment:
        <code-block lang="bash">mamba activate bacpage</code-block>
    </step>
    <step>
        Navigate to your HOME directory as described above:
        <code-block lang="bash" >cd ~/</code-block>
    </step>
    <step>
        Create an empty project directory using the <code>bacpage example</code> command. 
        We recommend that you give your project directory an informative name, such as <b>[date]_[sequencing-run-name]</b>
        (for example: <code>20220609_cholera_run1</code>)
        <p/>Type the following into your terminal window and then press <shortcut>Enter</shortcut>:
        <code-block>bacpage example [project-directory-name]</code-block>
        <note>Replace <code><b>[project-directory-name]</b></code> with the informative name you created above.</note>
    </step>
    <step>
        Navigate within your project directory and view its contents:
        <code-block lang="bash">
            cd [project-directory-name]
            ls
        </code-block>
    </step>
</procedure>

Your project should contain two files and a directory:
<code-block>
[project-directory-name]
├── config.yaml
├── sample_data.csv
└── input/
</code-block>

* <code>config.yaml</code> is a configuration file which contains the parameters and options for the analysis. 
Parameters may be altered to suit your analysis, however the default options should be appropriate for most analysis.
* <code>sample_data.csv</code> will hold the information about your samples, but presently contains example samples.
* The <code>input/</code> directory will hold the input data you would like to use for the pipeline 
(generally, paired-end FASTQ files).

To complete the setup of the project directory, you will need to add input data and fill out the <code>sample_data.csv</code>
file.

<procedure type="steps" title="Adding input data">
    <step>
        Locate the input data you want to use in the assembly pipeline. 
        This pipeline requires demultiplexed FASTQ files (i.e., two FASTQ files per sample). 
    </step>    
    <step>
        Using the file finder on your computer or command line, move this data into the <code>input/</code> directory of
        your project directory (i.e., the folder you just created above).
    </step>
    <step>
        Use the bacpage identify_files command to automatically fill out the <code>sample_data.csv</code> file.
        <code-block lang="bash" >
        bacpage identify_files input/
        </code-block>
        <note>As written, this command must be run from the project directory you created above</note>
    </step>
</procedure>

This command will save a completed <code>sample_data.csv</code> in your project directory based on the files present in the <code>input/</code> directory.
This file has the following required columns:
<deflist type="narrow">
    <def title="sample">the name or identifier of each sample</def>
    <def title="read1">the absolute pathname of the FASTQ file corresponding to the first set of reads for a sample (generally containing “R1” in the filename)</def>
    <def title="read2">the absolute pathname of the FASTQ file corresponding to the second set of reads for a sample (generally containing “R2” in the filename)</def>
</deflist>
<tip>
    By default, bacpage will parse sample names by extracting the first portion of the filename after splitting it with an underscore 
    (<shortcut>_</shortcut>) character. 
    If you would like to change this behavior change the deliminator and index to parse using the <code>--delim</code> and 
    <code>--index</code> options.
</tip>