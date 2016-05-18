# GBS preprocess tool (PAGE IN PROGRESS!!!)

## Tool for preprocessing Illumina reads generated from GBS library protocol

==================================================================================

This program could be used for preprocessing Illumina 1.8+ reads generated by [Genotyping-by-Sequencing (GBS)] (http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0019379) library preparation protocol.

# Step-by-Step pipeline:

The `GBSpep` program preprocess GBS reads in 4 consecutive steps:

	1. **Demultiplexing and barcodes removal**: The demultiplexing step identify exact barcode match in the sequence and remove the barcode;

	2. **Clip chimeras and adapters**: This second steps clip reads presenting the Restriction Enzyme (RE) site(s) (i.e. possible chimeras, partial digestion or sequencing errors) or the common adapterinitial sequence;

	3. **Quality trimming**: This step trim the reads based on a 5bp sliding window analysis, averaging the quality over the sliding window. When the average quality drop below a specified cut-off the reads are trimmed;

	4. **RE remnant site**: This step check for the presence of the RE remnant site(s). Only the reads that start with the RE remnant site(s) will be kept.  


Only the reads that pass all the 4 steps will be kept for further analysis.

==================================================================================

## Download this repo

For clone this folder to your computer first you need to install git.
Let us assume you want to clone this repo into a directory named `proj`, you will need to open the terminal and type:

    mkdir proj
    cd proj
    git clone https://github.com/aariani/GBS-preprocess-tools

==================================================================================

## Run the binary code

The binary code was created from the `GBS_preprocess_tool.py` script in the `source` folder using [PyInstaller] (http://www.pyinstaller.org/) v3.0 program on Ubuntu 14.04.4 Trusty.



See the [barcode section] (#barcode-file-specifications) for the format of the barcode file

If the binary code is not working on your system you can compile a personal binary code in you PC using [PyInstaller] (http://www.pyinstaller.org/). 

Please refer to the [Program Manual] (http://pythonhosted.org/PyInstaller/) for further informations.

Another option could be to [run from the source code] (#run-the-source-code)

==================================================================================

## Run the source code

If you are using windows or MAC please refer to the main.py script in the source file for running the program
From the command line type:

	python main.py -h


==================================================================================

## Barcode file specifications
