# SP-MEGD_Fusion

Mass spectrometry-based de novo sequencing remains challenging due to unexpected missing of certain ion information. We developed an innovative sample preparation approach for the current de novo sequencing challenges. The methodology, referred to as SP-MEGD for Single-Pot and Multi-Enzymatic Gradient Digestion, capitalizes on a five-protease gradient digestion by sampling every two hours for a total of 6 hours in an integrative reactor. The SP-MEGD method could engender numerous missed cleavage events and produce various peptide products of diverse lengths with overlapping stretches of residues, enabling efficient database-free de novo sequencing.

Fusion is an assembly software tool developed by Yu Lab, which can precisely reconstruct monoclonal antibody sequences with approximately 100% accuracy, including isoleucine/leucine (I/L) differentiation. Notably, Fusion has demonstrated its capability in precisely sequencing monoclonal antibody mixture at a resolution comparable to that of single monoclonal antibody. 

The antibody de novo sequencing analysis in this work was finished on the XA-Novo platform (https://xa-novo.com/).


<details><summary>Click here for all citations </summary>

  * SP-MEGD:
    * Xiong Y, Xiao J, Jiang W, et al. Simplified and Rapid Workflow Enhances Throughput of De Novo Sequencing of COVID-19 Neutralizing Antibodies[J]. bioRxiv, 2024: 2024.08. 09.607349.

  * Fusion:
    * Jiang W, Xiong Y, Xiao J, et al. Comprehensive assembly of monoclonal and mixed antibody sequences[J]. bioRxiv, 2024: 2024.08. 09.607415.

</details>

## Assembly results

The final sequence assembled by Fusion are displayed in ./Fusion/I-L/*.fasta. In its application, the Fusion algorithm identifies isoleucine or leucine residues by integrating template sequence information with experimental evidence and the detection of diagnostic w-ions, thereby enhancing the precision of isomer discrimination.

For the alignment between assembled sequences and actual antibody sequences, or sequences assembled by different algorithm, we utilized the global Needleman-Wunsch algorithm in conjunction with the BLOSUM60 matrix to achieve the most intuitive results. 
    * Likic, V.: The needleman-wunsch algorithm for sequence alignment. Lecture given at the 7th Melbourne Bioinformatics Course, Bi021 Molecular Science and Biotechnology Institute, University of Melbourne, 1–46 (2008)

