## Contents


- [Overview](#overview)
- [License](https://github.com/biocc/SP-MEGD_Fusion/blob/main/LICENSE)
- [System Requirements](#system-requirements)
- [Installation Guide](#installation-guide)
- [Usage Guide](#Usage-guide)
- [Results](https://github.com/biocc/SP-MEGD_Fusion)
- [R code](https://github.com/biocc/SP-MEGD_Fusion)
- [Demo](monoclonal-antibodies/Known_mAbs/Human/S2P6/50ug/)
- Citation

# Overview

Mass spectrometry-based *de novo* sequencing remains challenging due to unexpected missing of certain ion information. We developed an innovative sample preparation approach for the current *de novo* sequencing challenges. The methodology, referred to as SP-MEGD for Single-Pot and Multi-Enzymatic Gradient Digestion, capitalizes on a five-protease gradient digestion by sampling every two hours for a total of 6 hours in an integrative reactor. The SP-MEGD method could engender numerous missed cleavage events and produce various peptide products of diverse lengths with overlapping stretches of residues, enabling efficient database-free *de novo* sequencing.

Fusion is an assembly software tool previously developed by Yu Lab, which can precisely reconstruct monoclonal antibody sequences with approximately 100% accuracy, including isoleucine/leucine (I/L) differentiation. Notably, Fusion has demonstrated its capability in precisely sequencing monoclonal antibody mixture at a resolution comparable to that of single monoclonal antibody. 

The antibody *de novo* sequencing analysis in this work was finished on the XA-Novo platform (https://xa-novo.com/).


<details><summary>Click here for all citations </summary>

  * SP-MEGD:
    * Xiong Y, Xiao J, Jiang W, et al. Simplified and Rapid Workflow Enhances Throughput of *De Novo* Sequencing of COVID-19 Neutralizing Antibodies[J]. bioRxiv, 2024: 2024.08. 09.607349.

  * Fusion:
    * Jiang W, Xiong Y, Xiao J, et al. Comprehensive assembly of monoclonal and mixed antibody sequences[J]. bioRxiv, 2024: 2024.08. 09.607415.

</details>

# System Requirements

## Hardware Requirements

SP-MEGD requires a RTX >= 1080 GPU to support the operations.

## Software Requirements

### OS Requirements

The package development version is tested on *Linux* operating systems. The developmental version of the package has been tested on the following systems:

Linux: Ubuntu >= 16.04  
Mac OSX:  
Windows:  

The CRAN package should be compatible with Windows, Mac, and Linux operating systems.

* Users should have [R](https://www.r-project.org/) version 3.4.0 or higher, and several packages set up from  [CRAN](https://cran.r-project.org/mirrors.html).
* [python](https://www.python.org/) (>= 3.10)
* [numpy](https://numpy.org/)  (>= 1.26.2)
* [pandas](https://pandas.pydata.org/) (>= 2.1.3)

# Installation Guide

## Create a conda environment on Ubuntu 16.04
```
conda create --name SP-MEGD python=3.10 -y
conda activate SP-MEGD
pip install casanovo==3.2.0

#After installation, test that it was successful by viewing the Casanovo command line interface help:
casanovo --help
```

## Installing R version 3.4.2 on Ubuntu 16.04

the latest version of R can be installed by adding the latest repository to `apt`:

```
sudo echo "deb http://cran.rstudio.com/bin/linux/ubuntu xenial/" | sudo tee -a /etc/apt/sources.list
gpg --keyserver keyserver.ubuntu.com --recv-key E084DAB9
gpg -a --export E084DAB9 | sudo apt-key add -
sudo apt-get update
sudo apt-get install r-base r-base-dev
```

which should install in about 20 seconds.

## Package dependencies

Users should install the following packages from an `R` terminal:

```
install.packages(c('ggplot2', 'stringr', 'gridExtra', 'data.table'))
```

# Usage Guide

## 1. Format raw data

De novo sequencing requires mgf (Mascot generic format) files.

You can use Proteowizard `msconvert` to convert your .raw/.mzxml/.mzml files to .mgf format. Proteowizard can be simply installed using [conda](https://anaconda.org/bioconda/proteowizard).

`msconvert preformatted_spectra.raw --mgf --filter "peakPicking vendor" --filter "zeroSamples removeExtra"--filter "titleMaker Run: <RunId>, Index: <Index>, Scan: <ScanNumber>"`

Your .mgf file should now look like this
```
START IONS
TITLE= Run: antibody_tryptic_1, Index: 745, Scan: 756
RTINSECONDS=199.46399
PEPMASS=593.2639
CHARGE=2+
145.9389801 163.0
145.9406433 490.0
145.9423218 762.0
...
END IONS
```

## 2. De novo peptide sequencing

run the following code:

CUDA_VISIBLE_DEVICES=0 nohup casanovo --mode=denovo --peak_path=./denovo/spectrum_HCD.mgf --model=./epoch=9-step=550000.ckpt --config=[./denovo/config.yaml](https://github.com/biocc/SP-MEGD_Fusion/blob/main/monoclonal-antibodies/Known_mAbs/Human/S2P6/50ug/denovo/HCD/config.yaml) --output=./denovo/denovo_HCD >./denovo/run.log 2>&1 &

The pre-trained model (epoch=9-step=550000.ckpt) can be downloaded from here:
https://drive.google.com/drive/folders/1EQIa1XhqHRFAVBYzgLWh60HiMVnlO4dR?usp=drive_link

The running speed is about 1.085322 spectra/s using a RTX 1080 Ti GPU.

## 3. Casanovo result preprocess

Users can use this [script](https://github.com/biocc/SP-MEGD_Fusion/blob/main/monoclonal-antibodies/Known_mAbs/Human/S2P6/50ug/denovo/HCD/Casanovo_process.ipynb) to process the denovo result for the later aseembly.

## 4. Assembly

* Fusion assembler is available for academic users, it can be accessed with the following link: https://xa-novo.com/.
* Stitch assembler is available for academic users, it can be accessed with the following link: https://github.com/snijderlab/stitch.

### 1). Fusion Assembly results

The final sequence assembled by Fusion are displayed in ./Fusion/I-L/*.fasta. In its application, the Fusion algorithm identifies isoleucine or leucine residues by integrating template sequence information with experimental evidence and the detection of diagnostic w-ions, thereby enhancing the precision of isomer discrimination.

./Fusion/I-L/*.fasta look like this.
```
>HC 26,33,51,58,97,114,126
QVQLVQSGAEVKKPGSSVKVSCKASGGTFRSHVISWVRQAPGQGLEWMGGFIPLFGTTIYAQAFQGRVMISADESTSTAYMELSSLRSEDTAVYFCARLFPNGDPNSPEDGFDIWGQGTLVTVSAASTKGPSVFPLAPSSKSTSGGTAALGCLVKDYFPEPVTVSWNSGALTSGVHTFPAVLQSSGLYSLSSVVTVPSSSLGTQTYICNVNHKPSNTKVDKKVEPKSCDKTHTCPPCPAPELLGGPSVFLFPPKPKDTLMISRTPEVTCVVVDVSHEDPEVKFNWYV
>LC 27,32,50,52,89,98,109
DIQMTQSPSSLSASVGDRVTITCQASQDIGNYLNWYQQKPGKAPKLLIYDASHLETGVPSRFSGSGSGTDFTFTISSLQPEDIATYYCQRYDDLPSYTFGQGTKVEIKRTVAAPSVFIFPPSDEQLKSGTASVVCLLNNFYPREAKVQWKVDNALQSGNSQESVTEQDSKDSTYSLSSTLTLSKADYEKH
```
The seven consecutive values represent the following positions: CDR1 start position, CDR1 end position, CDR2 start position, CDR2 end position, CDR3 start position, CDR3 end position, and the conserved region start position.

For the alignment between assembled sequences and actual antibody sequences, or sequences assembled by different algorithm, we utilized the global Needleman-Wunsch algorithm in conjunction with the BLOSUM60 matrix to achieve the most intuitive results. 

In the alignment document, the complementarity-determining regions (CDRs) are color-coded: CDR1 in yellow, CDR2 in blue, and CDR3 in green.

### 2). Assembly of Stitch for Known Monoclonal Antibody

Stitch has recently emerged as a state-of-the-art public method for antibody sequence assembly. Users are required to define two main parameters: the cutoff score for peptide reads and the recombined segment orders. The typical recombined segment orders are IGHV * IGHJ IGHC for the heavy chain and IGLV IGLJ IGLC for the light chain. The asterisk ( * ) represents a gap between adjacent segments, which is extended by overhanging peptide reads. We have chosen cutoff scores of 95, 90, 85 and 50 for peptide reads. Scores of 95, 90 and 85 are based on the default parameters from Stitch's paper or batch file examples, while 50 is used as the threshold in Fusion. 

During the first run, we conducted tests using both the stable version Stitch1.4.0 and the latest version Stitch1.5.0 with these peptide read cutoff scores, common recombined segment orders, and other default parameters (./Stitch/Assembly/ * ). However, the actual length of light chain CDR3 for BD5514LH and S2P6LH antibodies exceeds the template. Therefore, we modified the recombined segment order of the light chain to IGLV*IGLJ IGLC for these two antibodies ( ./Stitch/Assembly1/ * ).

<details><summary>Click here for all citations</summary>  
* Likic, V. (2008). The Needleman-Wunsch algorithm for sequence alignment. Lecture at the 7th Melbourne Bioinformatics Course, Bi021 Molecular Science and Biotechnology Institute, University of Melbourne, pp. 1–46.  
<br>  
* Schulte, D., Snijder, J. (2024). A handle on mass coincidence errors in de novo sequencing of antibodies by bottom-up proteomics. *Journal of Proteome Research*.

</details>

## 5. Coverage-depth

After obtained the assembly sequence, you can run the [R code](https://github.com/biocc/SP-MEGD_Fusion/blob/main/monoclonal-antibodies/Known_mAbs/Human/S2P6/50ug/Fusion/casanovo/50-cleaned/code.txt) to analysis the coverage depth.
The [coveage depth result](https://github.com/biocc/SP-MEGD_Fusion/blob/main/monoclonal-antibodies/Known_mAbs/Human/S2P6/50ug/Fusion/casanovo/50-non_cleaned/coverage-depth.txt) can assist you in evaluating the reliability of assembly results to a certain degree.


Actually,  step 1-4 (ie. antibody de novo sequencing analysis) can finish on the [XA-Novo platform](https://xa-novo.com/) directly. The only prerequisite is to provide a link to the mass spectrometry data along with related species information regarding the detected antibody.

## Note

The sequences deciphered in this study are intended solely for scientific research purposes and are strictly prohibited from commercial use. Any legal issues that may arise from their utilization shall be the sole responsibility of the user.
