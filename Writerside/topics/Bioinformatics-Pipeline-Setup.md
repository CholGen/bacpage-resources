# Installing the Pipeline 

This tutorial will take you through the process of installing and updating the bioinformatics pipeline on your computer:
1. [Downloading a desired bioinformatics pipeline from the internet.](#download-pipeline "Skip to `Download the Bioinformatics Pipeline`")
2. [Building a compute environment (i.e., downloading software) needed to run the pipeline.](#build-environment "Skip to `Build the Compute Environment`" )
3. (*Optional*) [Updating the pipeline.](#updating-pipeline "Skip to `Update the pipeline`")

<procedure title="Important notes for following this tutorial" type="choices">
    <step>
        <code>Text with a gray background in monospace font</code> represents commands to type in. Generally commands 
        are one line, however, in this document, commands might wrap to the next line visually. We will add a blank line
        between commands to indicate when multiple commands are present.
    </step>
    <step>
        <path><format color="CornflowerBlue">Monospace blue text</format></path> represents the 
        input and/or output of the terminal. Due to different terminal programs, the appearance of your terminal may 
        look different. 
    </step>
    <step>
        <b>Bold text surrounded by []</b> is something you will have to replace with your own folder, path, or sample 
        name.
    </step>
</procedure>

Before using the Linux command line to perform bioinformatics pipelines, you will need to download and install the 
pipeline and its requirements. 
The information below is specific to the bacterial metagenomics pipeline `bacpage` that can be used for assembly of 
*Vibrio cholerae* genomes, but many of the same principles can be applied to other pipelines hosted on GitHub.

## 1. Navigate to your HOME Directory {id="navigate-home-directory"}
Unless otherwise stated, all commands should be run in your home directory. Generally this is where the command line is 
located if you open a new command prompt (i.e., Terminal window).
<procedure type="steps">
    <step>
        If you are not in your home directory (or are unsure), move to your home directory with any of the following 
        commands:
        <code-block lang="bash">cd ~/</code-block>
        <tip>On some keyboards, tilde <shortcut>~</shortcut> can be difficult to type. If this is the case, you can use 
        an alternative command such as <code lang="bash">cd $HOME</code> or <code lang="bash">cd</code></tip>
    </step>
</procedure>

## 2. Download the Bioinformatics Pipeline {id="download-pipeline"}
Bioinformaticians often make their genome assembly and analysis pipelines publicly available on websites such as 
[www.github.com](https://www.github.com) (commonly referred to as “GitHub”), which is specifically set up for hosting 
code and other types of files. There are two ways to download files and pipelines from GitHub: via your web browser or 
via the command line. To perform bacterial genomics assembly using CholGen pipelines, use one of the methods below:

<tabs>
    <tab title="Command line">
        <procedure type="steps">
            <step>
                Install the git command line program using the following command:
                <code-block lang="bash">sudo apt install git-all</code-block>
                <note>
                    This command will prompt you for your password. When entering your password on the command line, no 
                    characters will appear. However, the command line will still be receiving input so be sure to type 
                    your password correctly before pressing <shortcut>Enter</shortcut>.
                </note>
            </step>
            <step >
                After git is installed, run the following command in your HOME directory to download the folder from 
                GitHub:
                <code-block lang="bash" id="repo-url">git clone https://github.com/CholGen/bacpage.git</code-block>
            </step>
            <step>
                Check the download was correct by viewing the contents of your home directory:
                <code-block lang="bash" >ls</code-block>
                You should see a new directory in your home directory with the name <code>bacpage/</code>.
                <p/>This procedure can be used to download any other pipeline hosted on the GitHub website. If you are 
                interested in loading a different pipeline than the one used here, simply replace the URL in Step 2 with
                the link found on the other pipeline’s GitHub page.
            </step>
            <step>
                Confirm that the pipeline folder (<code>bacpage/</code>) contains 4 sub-folders: 
                <code>bacpage/</code>, <code>example/</code>, <code>config/</code>, and <code>test/</code>
            </step>
        </procedure>
    </tab>
    <tab title="Web browser">
    <procedure type="steps">
        <step> 
            Paste the following link in your web browser.  A file named <path>bacpage.zip</path> will be automatically 
            downloaded.
            <p><a href="https://github.com/CholGen/bacpage/releases/latest/download/pipeline.zip" ></a></p>
        </step>
        <step>
            Unzip this file on your computer and move the resulting folder (named <path>bacpage/</path>) into the HOME 
            directory on your computer. You can do this by dragging the <path>bacpage/</path> folder from your Downloads
            folder into your HOME directory.
        </step>
        <step>
            Confirm that the pipeline folder (<path>bacpage/</path>) contains 5 sub-folders: 
            <path>config/</path>, <path>example/</path>, <path>resources/</path>, <path>test/</path>, and 
            <path>workflow/</path>.
        </step>
    </procedure>
    </tab>
</tabs>

## 3. Build the Compute Environment {id="build-environment"}
In contrast to simple command line programs such as <code>cd</code> and <code>ls</code>, bioinformatic analysis requires
many specialized programs which need to be installed from a wide range of developers, with a wide range of requirements 
and dependencies. To make it easy for others to repeat their exact processes, bioinformaticians can set up computing 
“environments” that contain all software and other setup items needed to follow a particular analysis. They can then 
share the instructions for replicating their compute “environment” on a site such as GitHub, so other people can quickly
set up their computers in the exact same way.

The files you downloaded from GitHub above contain instructions on how to build the environment needed to run all of the
scripts you also downloaded. These instructions will automatically install all of the software you need to run the 
bioinformatics scripts. In order to follow these instructions (and load someone else’s compute environment on your 
computer), you’ll need to first install a piece of software that can read these instructions. 

There are a few different programs that can be used to build environments. <control>Mamba</control> and 
<control>Conda</control> are very common options. We will use Mamba because it tends to work faster 
than Conda, but if you are ever working with another set of instructions that uses 
Conda instead, know that you are doing something very similar!

Follow the instructions below to install Mamba and then use the Mamba software to build the bacterial assembly 
environment on your computer. Please note that you will need a network connection to complete these steps, as you’ll be 
downloading Mamba from the internet, and then using the Mamba software to download other programs and software from the 
web.

<procedure title="Installing Mamba" type="steps">
    <step>
        Navigate to your home directory with the following command:
        <code-block lang="bash" >cd ~/</code-block>
    </step>
    <step>
        Install mamba using the following command:
        <code-block lang="bash" >
        curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-$(uname)-$(uname -m).sh"
        bash Mambaforge-$(uname)-$(uname -m).sh
        </code-block>
        You should have noticed that your command line prompt has changed from:
        <p/><path><format color="CornflowerBlue">[seqlaptop@linuxbox seqlaptop]$ </format></path>
        <p/>to
        <p/><path><format color="CornflowerBlue">(base) [seqlaptop@linuxbox seqlaptop]$</format></path>
        <p/>This indicates that you are now in the <control>base</control> mamba environment, and that all of the 
        software you loaded into that environment (called “base”) is accessible.
        <tip> To get out of the <control>base</control> mamba environment (for example, if you’re unable to access previously
        installed software, run <code>mamba deactivate</code>). When you leave the mamba environment, you will no longer
        be able to access software that you installed when you were inside the mamba environment. To get back into the 
        <control>base</control> mamba environment, run <code>mamba activate</code>).</tip>
    </step>
</procedure>

Rather than installing everything to the base environment, it’s recommended to create task-specific 
environments, containing only software that is needed for a given task. We are now going to create a new 
environment with a different name that will specifically have the assembly pipeline software in it. 

<procedure title="Setting up the pipeline environment">
    <step>
        Navigate to the directory where you downloaded the bioinformatics pipeline above. This should be a directory 
        titled <code>bacpage/</code> in your home directory. 
        <code-block lang="bash" >cd ~/bacpage</code-block>
        You will need to navigate to this directory any time you want to use the pipeline you downloaded from GitHub 
        above. Run <code>pwd</code> and note the path of this directory for future reference.
    </step>
    <step>
        Inside this directory you will find a file called <code>environment.yaml</code>. Feel free to open it in a text 
        editor and look at its structure. The file describes all the software that is required for the pipeline. To tell
        the computer to install all of this software, run the following command:
        <code-block lang="bash">mamba env create -f environment.yaml</code-block>
        <note>This installation may take a few minutes depending on your internet speed. </note>
        This command will create a new environment called <control>bacpage</control>, which is distinct from the default
        <control>base</control> environment discussed above. It can be activated and deactivated in an identical manner 
        to the <control>base</control> environment. 
    </step>
    <step>
        To use a mamba environment, and all the software we just installed, we have to activate it. Do this with the 
        following command:
        <code-block lang="bash" >mamba activate bacpage</code-block>
        Your prompt should have changed again to the following:
        <p/><path><format color="CornflowerBlue">(bacpage) [seqlaptop@linuxbox bacpage]$</format></path>
        <p>This indicates that your terminal is now using the <control>bacpage</control> environment and can use all the
        required software. Use <code>mamba deactivate</code> to return to the default <control>base</control> environment.</p>
    </step>
    <step>
        Install the bacpage command using the following command:
        <code-block lang="bash">pip install .</code-block>
    </step>
    <step>
        Run the following two commands to test that the installation was successful:
        <code-block lang="bash">
        bacpage --help
        bacpage version
        </code-block>
        You should see a help screen and the version number (something like <code>2023.11.15</code>).
        You can rerun the installation if not successful.
    </step>
</procedure>

## 4. Update the pipeline (optional) {id="updating-pipeline"}
Updating the pipeline involves downloading updates from Github, updating the compute environment with mamba, and 
reinstalling the bacpage command.
<procedure>
    <step>
        Navigate to the directory where you downloaded the bioinformatics pipeline above. Generally, this should be a 
        directory titled <code>bacpage/</code> in your home directory.
        <code-block lang="bash" >cd ~/bacpage</code-block>
    </step>
    <step>
        Pull the latest version of the pipeline from Github:
        <code-block lang="bash" >git pull</code-block>
    </step>
    <step>
        Activate the compute environment for the pipeline:
        <code-block lang="bash" >mamba activate bacpage</code-block>
        Check to make sure the environment portion of your prompt changed from 
        <path><format color="CornflowerBlue">(base)</format></path> to 
        <path><format color="CornflowerBlue">(bacpage)</format></path>.
    </step>
    <step>
        Update the environment:
        <code-block lang="bash" >mamba env update -f environment.yaml</code-block>
    </step>
    <step>
        Re-install the bacpage command:
        <code-block lang="bash" >pip install .</code-block>
    </step>
    <step>
        Run the following two commands to test that the installation was successful:
        <code-block lang="bash">
        bacpage --help
        bacpage version
        </code-block>
    </step>
</procedure>

Click the link in the bottom right of the page to continue to an overview of the <b>bacpage</b> 
command.