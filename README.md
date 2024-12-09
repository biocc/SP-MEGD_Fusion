# SP-MEGD_Fusion

Mass spectrometry-based *de novo* sequencing remains challenging due to unexpected missing of certain ion information. We developed an innovative sample preparation approach for the current *de novo* sequencing challenges. The methodology, referred to as SP-MEGD for Single-Pot and Multi-Enzymatic Gradient Digestion, capitalizes on a five-protease gradient digestion by sampling every two hours for a total of 6 hours in an integrative reactor. The SP-MEGD method could engender numerous missed cleavage events and produce various peptide products of diverse lengths with overlapping stretches of residues, enabling efficient database-free *de novo* sequencing.

Fusion is an assembly software tool developed by Yu Lab, which can precisely reconstruct monoclonal antibody sequences with approximately 100% accuracy, including isoleucine/leucine (I/L) differentiation. Notably, Fusion has demonstrated its capability in precisely sequencing monoclonal antibody mixture at a resolution comparable to that of single monoclonal antibody. 

The antibody *de novo* sequencing analysis in this work was finished on the XA-Novo platform (https://xa-novo.com/).


<details><summary>Click here for all citations </summary>

  * SP-MEGD:
    * Xiong Y, Xiao J, Jiang W, et al. Simplified and Rapid Workflow Enhances Throughput of *De Novo* Sequencing of COVID-19 Neutralizing Antibodies[J]. bioRxiv, 2024: 2024.08. 09.607349.

  * Fusion:
    * Jiang W, Xiong Y, Xiao J, et al. Comprehensive assembly of monoclonal and mixed antibody sequences[J]. bioRxiv, 2024: 2024.08. 09.607415.

</details>

## Assembly results

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

### Assembly of Stitch for Known Monoclonal Antibody

Stitch has recently emerged as a state-of-the-art public method for antibody sequence assembly. Users are required to define two main parameters: the cutoff score for peptide reads and the recombined segment orders. The typical recombined segment orders are IGHV * IGHJ IGHC for the heavy chain and IGLV IGLJ IGLC for the light chain. The asterisk ( * ) represents a gap between adjacent segments, which is extended by overhanging peptide reads. We have chosen cutoff scores of 95, 90, 85 and 50 for peptide reads. Scores of 95, 90 and 85 are based on the default parameters from Stitch's paper or batch file examples, while 50 is used as the threshold in Fusion. 

During the first run, we conducted tests using both the stable version Stitch1.4.0 and the latest version Stitch1.5.0 with these peptide read cutoff scores, common recombined segment orders, and other default parameters (./Stitch/Assembly/ * ). However, the actual length of light chain CDR3 for BD5514LH and S2P6LH antibodies exceeds the template. Therefore, we modified the recombined segment order of the light chain to IGLV*IGLJ IGLC for these two antibodies ( ./Stitch/Assembly1/ * ).

<details><summary>Click here for all citations</summary>  
* Likic, V. (2008). The Needleman-Wunsch algorithm for sequence alignment. Lecture at the 7th Melbourne Bioinformatics Course, Bi021 Molecular Science and Biotechnology Institute, University of Melbourne, pp. 1–46.  
<br>  
* Schulte, D., Snijder, J. (2024). A handle on mass coincidence errors in de novo sequencing of antibodies by bottom-up proteomics. *Journal of Proteome Research*.

</details>

## Note

The sequences produced in this study are intended solely for scientific research purposes and are strictly prohibited from commercial use. Any legal issues that may arise from their utilization shall be the sole responsibility of the user.
