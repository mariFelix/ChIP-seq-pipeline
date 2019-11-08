# MANUAL pipeline_ChIP-seq
2015-09-08

Marianne S. Felix

marianne.sabourin-felix.1@ulaval.ca
---------------------------------------

1. INTRODUCTION
2. WARNINGS
3. SOFTWARE DEPENDECIES
4. FILES REQUIREMENTS
5. HOW TO USE IT
6. PRECONDITIONS
7. OUTPUT FILES
8. HOW IT WORKS

## INTRODUCTION

This script process raw ChIP-seq data into aligned BAM files.

## WARNINGS

Caution, this script was tested only on Linux 14.04 LTS and 16.04 LTS distributions and is provided "as is", without warranty.

This script works only with SINGLE END files.

## SOFTWARE DEPENDENCIES

This script requires the following softwares :

 - SRA Toolkit
 - Picard 1.138
 - FastQC
 - Trimmomatic-0.33
 - Bowtie2
 - Samtools

## FILES REQUIREMENTS

The input file format can be either in FASTQ, BAM, SRA or FASTQ.GZ.
The reference genome must be indexed. To do so, run the following command :

```
bowtie2-build -f genomeFile.fa genomeName
```
or
```
bowtie2-buil -f genomeChr1.fa,genomeChr2.fa,genomeChr3.fa genomeName
```

where genomeChr1.fa,...,genomeChr3.fa is the list of genome files.

## HOW TO USE IT

[1]  One must change the access permission of the script as follows :

      chmod +x pipeline_ChIP-seq.sh

[2]  Open a new screen :

      screen -S screenName

 Here is the list of the most used command of the screen :
 
 ```
Create a screen : screen -S screenName
Close           : Ctrl + a, Ctrl + d
List            : screen -ls
Resume          : screen -r screenName
Delete          : screen -S screenName -X quit
```

[3]  Run the script into the opened screen :

      ./pipeline_ChIP-seq.sh

[4]  Choose an option between the three :

```
1 . Help/Manual
2 . Launch pipeline
3 . Quit
```

>The first option display this manual. The second option launch the pipeline and the third option close the program. For the rest of this section, we assume that the option two is chosen.

[5]  Enter either F if you want to process one file or D for a folder if you
     want to process many files followed by [ENTER].

[6]  Enter your filename or folder name followed by [ENTER].

      e.g.: ../relative/path/to/file.bam
      
      e.g.: /absolute/path/to/folder

[7]  Enter path to the indexed reference genome basename, followed by [ENTER]. 

      e.g.: for Mm10.1.bt2 etc. enter : /path/to/index/Mm10

[8]  Type the number of thread(s) you want to use (for trimmomatic and Bowtie2), followed by [ENTER].

> If the analysis are made on a personal computer, 1 thread is recommended to ensure the smooth running of other applications.

[9]  Type the minimum score you want to use for trimmomatic LEADING option, followed by [ENTER].

[10]  Type the minimum score you want to use for trimmomatic TRAILING option, followed by [ENTER].

[11]  Type the minimum length you want to use for trimmomatic MINLEN option, followed by [ENTER].

[12]  Do you want to remove adapters from dataset ? [yes|no]

[12.1]  Type the path to the adapter file ex: /home/App/Trimmomatic-0.33/adapters/TruSeq3-SE.fa, followed by [ENTER].

[12.2]  Do you want to change the ILLUMINACLIP settings ? (Default : ILLUMINACLIP:adapterFile:2:30:10) [yes|no]

[12.2.1]  Type the maximal seed mismatch you want to allow for trimmomatic ILLUMINACLIP option (Default=2), followed by [ENTER].

[12.2.2]  Type the minimum score for palindrome match you want to allow for trimmomatic ILLUMINACLIP option (Defaut=30), followed by [ENTER].

[12.2.3]  Type the minimum score for simple match you want to allow for trimmomatic ILLUMINACLIP option (Defaut=10), followed by [ENTER].

[13]  Type the number of alignment with different k value (aln per read) you want to do, followed by [ENTER] :

[14]  Enter your k value (aln per read) #X, followed by [ENTER] :

> For a better coverage, one can choose 3 alignments per read. Otherwise, 1 is recommended.

[15] Close the screen to let the analysis run :
     
      Ctrl + a, Ctrl + d
     
[16] Resume your screen to see if the job is completed :

      screen -r screenName

[17] When the job is completed, this message will appear :

      Experiment ExperimentName used X of additional disk space on your device
      and was processed in X hours X minutes and X seconds !
      ------------------------------------------------------------------------
      Final files are in folder ExperimentName/k* ! :)

## PRECONDITIONS

- The input files must exist and be in FASTQ, BAM, SRA or FASTQ.GZ format.
- The reference genome file must be indexed.

## OUTPUT FILES

There will be a folder named yearMonthDay_hourMinSec. 

Each folder will contain these types of files :

    k1/file_sorted-k1.bam
      /file_sorted-k1.bam.bai
    k3/file_sorted-k3.bam
      /file_sorted-k3.bam.bai

    QualityCheck-yearMonthDay_hourMinSec/file_fastqc/
                                        /file_trim_fastqc/

    settingSummary-yearMonthDay_hourMinSec.txt

    yearMonthDay_hourMinSec.log

Where file_sorted-k*.bam are the final files, file_fastqc are the FastQC quality analysis folders, settingSummary-yearMonthDay_hourMinSec.txt is the summary of the command and yearMonthDay_hourMinSec.log is the log of the processing.

## HOW IT WORKS

This script writes the summary of the command in a summary file and store the processing information in a log file. It detects the input file format (FASTQ, BAM, SRA or FASTQ.GZ) and will convert it into FASTQ.GZ format (with SRA Toolkit or Picard). Then, a FastQC quality analysis is performed on these files (with FastQC). A trimming is done (with Trimmomatic) according to the user's parameters. Then, the alignment of these files is made (with Bowtie2). These file are then sorted (with Samtools). The final files are stored in their corresponding k value directory.

