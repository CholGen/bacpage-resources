# Submitting to NCBI SRA Overview

test this stuff.
<code-block lang="plantuml">
    @startuml
    left to right direction
    class Run {
    library_ID : CMR_CEN005NA35
    sample_name : CMR_CEN005NA35
    ---
    filename1 : CMR_CEN005NA35.R1.fastq.gz
    filename2 : CMR_CEN005NA35.R1.fastq.gz
    ---
    platform : ILLUMINA
    intrument : MiSeq
    ...
    }
    
    class BioSample {
    sample_name : CMR_CEN005NA3
    ---
    organism: Vibrio cholerae O1
    host: Human
    sample_type: stool swab
    collection_date: 2023-04-12
    collection_loc: Cameroon: Centre
    ...
    }
    
    class BioProject {
    sample_name : CMR_CEN005NA3
    ---
    organism: Vibrio cholerae O1
    host: Human
    sample_type: stool swab
    collection_date: 2023-04-12
    collection_loc: Cameroon: Centre
    ...
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
</code-block>