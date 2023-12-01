# Creating a Project Directory
<card-summary >
    Prepare for an analysis by creating a project directory to hold all the files for your analysis.
</card-summary>

The first step for setting up a **bacpage** analysis is creating a directory specifically for the sequencing data you
are analyzing.
This directory is where you will run analyzes and save outputs and is called your **project directory**.
Importantly, while we recommend creating a project directory for each new sequencing run, you can run multiple analyses
(for example, reference-based and *de novo* assembly) in that same directory.

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

## 1. Creating a project directory from a template
We recommend you set up your project directory in your HOME directory to ensure all sequencing data and analysis
results are in the same place.

<procedure type="steps">
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
        <code-block>bacpage setup [project-directory-name]</code-block>
        <note>Replace <code><b>[project-directory-name]</b></code> with the informative name you created above.</note>
    </step>
    <step>
        Navigate within your project directory and review it's absolute path with <code>pwd</code>.
        <code-block lang="bash">
            cd [project-directory-name]
            pwd
        </code-block>
        We will refer to the absolute path of you project directory as <code><b>[project-path]</b></code>
    </step>
    <step>
        View the contents of your project directory:
        <code-block lang="bash">
            ls
        </code-block>
    </step>
</procedure>

Your project should contain two files and a directory:
```Bash
[project-directory-name]
├── config.yaml
└── input/
```

* `config.yaml` is a configuration file which contains the parameters and options for the analysis.
  Parameters may be altered to suit your analysis, however the default options should be appropriate for most analysis.
* The `input/` directory will hold the input data you would like to use for the pipeline
  (generally, paired-end FASTQ files).

## 2. Adding input data
To complete the setup of the project directory, you will need to add input data.

<procedure type="steps">
    <step>
        Locate the input data you want to use in the assembly pipeline. 
        This pipeline requires demultiplexed FASTQ files (i.e., two FASTQ files per sample). 
    </step>    
    <step>
        Using the file finder on your computer or command line, move this data into the <code>input/</code> directory of
        your project directory (i.e., the folder you just created above).
    </step>
</procedure>
