# Submitting to NCBI SRA Overview

test this stuff.
<code-block lang="plantuml">
<![CDATA[
    @startuml
    left to right direction
    class Run {
    <b>library_ID</b>: CMR_CEN005NA35
    <b>sample_name</b>: CMR_CEN005NA35
    ---
    <b>filename1</b>: CMR_CEN005NA35.R1.fastq.gz
    <b>filename2</b>: CMR_CEN005NA35.R1.fastq.gz
    ---
    <b>platform</b>: ILLUMINA
    <b>intrument</b>: MiSeq
    ...
    }
    
    class BioSample {
    <b>sample_name</b>: CMR_CEN005NA3
    ---
    <b>organism</b>: Vibrio cholerae O1
    <b>host</b>: Human
    <b>sample_type</b>: stool swab
    <b>collection_date</b>: 2023-04-12
    <b>collection_loc</b>: Cameroon: Centre
    ...
    }
    
    class BioProject {
    <b>study_name</b>: CholGEN Regional Analysis
    <b>study_description</b>: CholGEN is sequencing\nVibrio cholerae in Africa
    ---
    <b>BioSamples</b>: [CMR_CEN005NA3,...]
    <b>Runs</b>: [CMR_CEN005NA3,...]
    }
    
    Run-->BioSample
    Run-->BioProject : Many
    Run-->BioProject
    Run-->BioProject
    Run-->BioProject
    BioSample-->BioProject : Many
    BioSample-->BioProject
    BioSample-->BioProject
    BioSample-->BioProject
    
    @enduml
]]>
</code-block>