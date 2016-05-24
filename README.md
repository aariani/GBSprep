# GBSprep

## Tool for preprocessing Illumina reads generated from GBS library protocol

==================================================================================

This program could be used for preprocessing Illumina 1.8+ reads generated by [Genotyping-by-Sequencing (GBS)] (http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0019379) library preparation protocol.

# Step-by-Step pipeline:

The `GBSpep` program preprocess GBS reads in 4 consecutive steps:

1. **Demultiplexing and barcodes removal**: The demultiplexing step identify exact barcode match in the sequence and remove the barcode;

2. **Clip chimeras and adapters**: This second steps clip reads presenting the Restriction Enzyme (RE) site(s) (i.e. possible chimeras, partial digestion or sequencing errors) or the common adapter initial sequence;

3. **Quality trimming**: This step trim the reads based on a 5bp sliding window analysis, averaging the quality over the sliding window. When the average quality drop below a specified cut-off the reads are trimmed;

4. **RE remnant site**: This step check for the presence of the RE remnant site(s). Only the reads that start with the RE remnant site(s) will be kept;  


Only the reads that pass all the 4 steps will be kept for further analysis.

This program is able to preprocess multiple files at once. Just be sure that samples have the same barcode in all the sequencing runs. In case of replicated samples (i.e. the same genotype with different barcodes) they will be merged in a single file if they have the same name in the barcode file.

==================================================================================

## Download this repo

For downloading this folder to your computer first you need to install git.

Let's assume you want to clone this repo into a directory named `proj`, you will need to open the terminal and type:

    mkdir proj
    cd proj
    git clone https://github.com/aariani/GBS-preprocess-tools

==================================================================================

## Run the binary code

The binary code was created from the `GBS_preprocess_tool.py` script in the `source` folder using [PyInstaller] (http://www.pyinstaller.org/) v3.0 program on Ubuntu 14.04.4 Trusty.

###Prepare the environment

1. Create a directory for you project

		mkdir myGBSproject

2. Copy the `GBSprep` script to you project directory. If you have saved the script in the `proj/GBS-preprocess-tool` folder type:

		cp proj/GBS-preprocess-tool/GBSprep myGBSproject

3. Create a raw reads folder and copy your FASTQ files there (**Important**: this script works only with gz formatted fastq files)
 
		mkdir myGBSproject/fastq
		cp myfastqfolder/myfastqfiles.fastq.gz myGBSproject/fastq


Run the script inside the `myGBSproject` folder (Example with CviAII enzyme)

	cd myGBSproject
	./GBSprep -i fastq -o demultiplexed -bc myBarcodefile.txt -s CATG -SR ATG

The script will output the demultiplexed cleaned reads in fq format ready for downstream analysis.

In addition the script will output a `Demultiplexing_stats.txt` file with informations about the total number of reads (after filtering) for each genotype.

For all the option of the code from the command line type:

	./GBSprep -h
	usage: GBSprep [-h] [-i READS] [-o CLEAN_READS] [-bc BC_FILE] [-s RESITES]
               [-SR REREM] [-l MINLEN] [-q MINQ] [-gz] [-ad CONTAMINANT]
               [--remove-remnant-site]

	Program for preprocessing GBS reads
	
	optional arguments:
	  -h, --help            show this help message and exit
	  -i READS, --input-folder READS
	                        The folder with your raw reads in gz compressed format
	                        (REQUIRED)
	  -o CLEAN_READS, --output-folder CLEAN_READS
	                        The output folder for the final cleaned and
	                        demultiplexed reads (REQUIRED)
	  -bc BC_FILE, --barcode-file BC_FILE
	                        The barcode file for your raw reads (REQUIRED)
	  -s RESITES, --restriction_enzyme_site RESITES
	                        The Restriction Enzyme (RE) recognition site
	                        (REQUIRED). If your RE has ambiguous nucleotide you
	                        should write all the possible sites separated by a
	                        comma (ex. For ApeKI you should use -s GCAGC,GCTGC)
	  -SR REREM, --site-remnant REREM
	                        The RE remnant site after the digestion (REQUIRED).
	                        Only reads with the remnant site after the barcode
	                        will be kept. For RE with ambiguous nucleotide you
	                        should write all the possible remnant sites separated
	                        by a comma (as in the -s parameter)
	  -l MINLEN, --min-length MINLEN
	                        Minimum length of reads after quality trimming and
	                        adapter/chimeras clipping (default: 30bp)
	  -q MINQ, --min-qual MINQ
	                        Mean minimum quality (in a sliding window of 5bp) for
	                        trimming reads, assumed Sanger quality (Illumina 1.8+,
	                        default: 20)
	  -gz, --binary-output  Do you want to output the fastq file in a compressed
	                        format (i.e. gz compressed)? This option save disk
	                        space, but writing binary files requires a
	                        considerable higher amount of time (Default: False)
	  -ad CONTAMINANT, --adapter-contaminants CONTAMINANT
	                        The initial sequence of the adapter contaminant
	                        (default: AGATCGG)
	  --remove-remnant-site
	                        Do you want to remove the RE remnant site from the
	                        final cleaned reads? (default: False)



See the [barcode section] (#barcode-file-specifications) for the format of the barcode file

If the binary code is not working on your system you can compile a personal binary code in you PC using [PyInstaller] (http://www.pyinstaller.org/) and the `GBS_preprocess_tool.py` script. 

Please refer to the [PyInstall Manual] (http://pythonhosted.org/PyInstaller/) for further informations.

Another option could be to [run the source code] (#run-the-source-code)

==================================================================================

## Run the source code

If you are using Windows or MAC (and the binary code is not working) please refer to the `GBSprep.py` script for running this program.

On Windows you will probably need to install [Cygwin] (https://www.cygwin.com/) (not tested!).

The `GBSprep.py` script contains all the codes in the `source` folder in a single file.

From the command line type:

	python GBSprep.py -h


The option of the script and the preparation of the environment will be exactly the same as in the [run the binary code] (#run-the-binary-code) section

==================================================================================

## Barcode file specifications

The barcode files if formed by two columns, separated by a space, with the barcode sequence and the name of the genotypes

	barcode1 genotype1
	barcode2 genotype2
	barcode3 genotype3


