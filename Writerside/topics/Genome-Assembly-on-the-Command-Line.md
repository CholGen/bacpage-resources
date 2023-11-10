# Reference-Based Bacterial Genome Assembly on the Command Line
In this document, we have provided instructions for performing a reference-based assembly of *Vibrio cholerae*. You can 
perform the steps on raw sequencing data you have generated and copied onto your computer (see instructions to *set up 
your project directory* below). 

<procedure title="Important notes for following this tutorial" id="intro-table">
    <step>
        <code>Text with a gray background in monospace font</code> represents commands to type in.
    </step>
    <step>
        <b>Bold text surrounded by []</b> is something you will have to replace with your own folder, path, or sample 
        name.
    </step>
    <step>
        This tutorial assumes you have set up the bioinformatics pipeline directory on your computer Before starting the
        tutorial below, check if your computer has a folder called <path>bacpage/</path> in the home directory. If you 
        do not have this folder on your computer, complete the 
        <a href="Bioinformatics-Pipeline-Setup.md">Bioinformatics Pipeline Setup</a> instructions to install the 
        necessary files and software before proceeding.
    </step>
</procedure>

<p>This tutorial will take you through reference-based assembly from amplicon sequencing data. The process is comprised 
of five main steps, including a sequence assessment step:</p>
<list type="decimal">
    <li>Align paired-end FASTQ files to a reference genome, generating BAM files.</li>
    <li>Call variants in the BAM file relative to the reference.</li>
    <li>Generate a consensus sequence from the variants.</li>
    <li>Mask known recombinant regions from the consensus sequence.</li>
    <li>Assess the quality of raw sequencing reads and alignment.</li>
</list>

<p>These steps are performed using the <control>snakemake</control> platform. Snakemake is a tool for creating 
reproducible and modular bioinformatic pipelines. Each task in an analysis, including those above, can be written as an 
individual step of a pipeline. Snakemake makes it easier to conduct analyses by reducing the number of commands you need
to type, parallelizing steps across all your samples, and confirming steps were completed successfully.</p> 

Below, you’ll find instructions to use snakemake to run the whole pipeline all at once, as well as instructions 
(in the <a href="Reference-Based-Genome-Assembly-Appendix.md">Appendix</a>) that walk you through each of the above steps one by one. 

## STEP 0: Project Directory Setup
Before starting any bioinformatics analysis, it is good practice to create a directory specifically for the dataset you 
are about to analyze. This directory is where you will run analyzes and save outputs and is called your project 
directory. You will want to set up your project directory inside the <path>bacpage/</path> folder in your HOME directory
to ensure all sequencing data and results are in the same place and can use the same pipeline tools and software.
We will refer to the path of the <path>bacpage/</path> folder as <b>[sequencing-path]</b>. On most machines, the 
<b>[sequencing-path]</b> will be <path>~/bacpage</path>.

<procedure type="steps">
    <step>
        Navigate to the bioinformatics pipeline folder as described above:
        <code-block lang="bash" >cd ~/bacpage</code-block> 
    </step>
    <step>
        In the <path>bacpage/</path> folder there is a sub-folder called <path>example/</path>. The first step is to 
        make a copy of this folder. You will make a new copy every time you perform a new sequencing run, thus ensuring 
        that the files and folders inside the example directory are set up in the exact same way each time. To copy this
        folder and give it a project-specific name, run the command below. We recommend that you give your project 
        directory an informative name, such as <control>[date]_[sequencing-run-name]</control> (for example: 
        <i>20220609_cholera_run1</i>).
        <p>Type the following into your terminal window and then press Enter:</p>
        <code-block lang="bash" >cp -R example/ [project-directory-name]</code-block>
    </step>
    <step >
        Navigate to your project directory and record the absolute path using.
        <code-block lang="bash">
            cd [project-directory-name]
            pwd
        </code-block>
        We will refer to the absolute path to your project directory as <b>[project-path]</b>. You will need the 
        absolute path for your project directory in Step 6 below.
    </step>
    <step>
        Locate the input data you want to use in the assembly pipeline. This pipeline requires demultiplexed FASTQ files
        (i.e., two FASTQ files per sample). Using the file finder on your computer, move this data into the 
        <path>input/</path> directory of your project directory (i.e., the folder you just created above).
    </step>
    <step>
        Within your project directory, there should be a file called <path>sample_data.csv</path>, which will hold the 
        information about your samples. Open this file in Excel or a similar spreadsheet software, and add the 
        information for each of your samples on an individual row.
        <p>In the <control>sample</control> column, record the name or identifier of each sample (these must be unique 
        and not contain unusual characters like periods “.” or slashes “/”). In the <control>read1</control> column, 
        record the absolute path of the FASTQ file corresponding to the first set of reads for a sample (generally 
        containing “R1” in the filename). In the <control>read2</control> column, record the absolute path of the FASTQ 
        file corresponding to the first set of reads for a sample (generally containing “R2” in the filename).</p>
        <note>
            The absolute path of a file can be determined with the <code>pwd</code> command. If your demultiplexed FASTQ
            files were placed in the <path>input/</path> directory of your project directory as described above, you can
            find their absolute path by running the following command:
            <code-block lang="bash">
                cd [project-path]/input
                pwd
            </code-block>
            The absolute path of your files will be the output of pwd plus “/” plus the file name.
        </note>
        An example completed <path>sample_data.csv</path> file is shown below:
        <table>
        <tr>
        <td>sample</td><td>read1</td><td>read2</td>
        </tr>        
        <tr>
        <td>sample1</td><td>/home/user/bacpage/20220609_run1/input/sample1_S13_L001_R1_001.fastq.gz</td><td>/home/user/bacpage/20220609_run1/input/sample1_S13_L001_R2_001.fastq.gz</td>
        </tr>        
        <tr>
        <td>sample2</td><td>/home/user/bacpage/20220609_run1/input/sample2_S3_L001_R1_001.fastq.gz</td><td>/home/user/bacpage/20220609_run1/input/sample2_S3_L001_R2_001.fastq.gz</td>
        </tr>        
        <tr>
        <td>sample3</td><td>/home/user/bacpage/20220609_run1/input/sample3_S22_L001_R1_001.fastq.gz</td><td>/home/user/bacpage/20220609_run1/input/sample3_S22_L001_R2_001.fastq.gz</td>
        </tr>
        </table>
    </step>
    <step>Save the file after entering the data for your samples.</step>
    <step>
        Within your project directory, open the configuration file called <path>config.yaml</path> with a text editor of
        your choice. This file contains the parameters and options for the analysis. Parameters may be altered to suit 
        your analysis, however the default options should be appropriate for most analysis.
        <p>With the configuration file open in a text editor, replace <control>[project-path]</control> and 
        <control>[sequencing-path]</control> with their absolute paths for the first six parameters.</p>
        <code-block lang="yaml" >
            # General parameters; point to input files.
            run_type: "Illumina"
            samples: "[project-path]/sample_data.csv"
            output_directory: "<b>[project-path]</b>"
            reference: "[sequencing-path]/resources/vc_reference.fasta"
            reference_genes: "[sequencing-path]/resources/cholera_ref_genes/"
            recombinant_mask: "[sequencing-path]/resources/cholera_mask.gff"
        </code-block>
    </step>
    <step>
        Lastly, determine how many processors are available on your computer, since using more processors will make the 
        pipeline run faster. The output of the following command will indicate how many processors you have available. 
        Run the command below, specific to your operating system, and write down the result for later:
        <tabs>
            <tab title="Linux">
                <code-block lang="bash" >grep ^processor /proc/cpuinfo | wc -l</code-block>
            </tab>
            <tab title="Mac">
                <code-block lang="bash" >sysctl -n hw.ncpu</code-block>
            </tab>
        </tabs>
    </step>
</procedure>

## STEP 1: Run the Complete Pipeline
If you would like to run the entire pipeline without walking through each of the intermediate steps, you can generate 
consensus sequences and calculate quality metrics with a single command.
<procedure>
    <step>
        Navigate to the location of the bioinformatics pipeline:
        <code-block lang="bash" >cd ~/bacpage</code-block>
    </step>
    <step>
        Run the pipeline with the following command:
        <code-block lang="bash" >
            snakemake --configfile [project-path]/config.yaml --cores [number-of-processors] --keep-going --until mask_consensus generate_complete_report
        </code-block>
        This will generate a consensus sequence in FASTA format for each of your samples and place them in 
        <path>[project-path]/results/consensus_sequences/[sample].masked.fasta</path>. An HTML report containing 
        alignment and quality metrics for your samples can be found at 
        <path>[project-path]/results/reports/qc_report.html</path>.
</step>
</procedure>    
<tip>
    If a snakemake command is successfully completed, you should see something like this on the screen:
    <code-block>
        [Tue Sep  5 12:09:40 2023]
        Finished job 119.
        253 of 253 steps (100%) done
        Complete log: .snakemake/log/2023-09-05T120148.784555.snakemake.log
    </code-block>
    The job number, step, and total steps will depend on the number of samples you have.
    <p>If a snakemake command was unsuccessful, you will see the following at the end of the output:</p>
    <code-block >
        Error in rule x:
            jobid: 1
            shell:
                command -options arguments
        Shutting down, this might take some time.
        Exiting because a job execution failed. Look above for error message
        Complete log: .snakemake/log/2023-09-05T120148.784555.snakemake.log
    </code-block>
    Record the failed rule, the sample being processed, and the expected output file of the failed rule. You can
    attempt to rerun the incomplete step by running the command again:
    <code-block lang="bash" >
        snakemake --configfile [project-path]/config.yaml --cores [number-of-processors] --keep-going
    </code-block>
</tip>

## STEP 2: Assess the quality of Genome Assembly
A key component of generating genome assemblies is assessing their quality. The bioinformatics pipeline generates a 
report to visually inspect the quality, coverage, and confidence we have in the resultant consensus sequences.
<procedure type="steps">
    <step>
        To review the quality metrics of your samples, open <path>[project-path]/results/reports/qc_report.html</path> 
        with a web browser.
    </step>
</procedure>

<seealso style="cards">
    <a href="Reference-Based-Genome-Assembly-Appendix.md">Instructions</a>
</seealso>