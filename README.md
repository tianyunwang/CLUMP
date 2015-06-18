# CLUMP: CLUstering by Mutation Position

CLUMP (CLustering by Mutation Postion) is an unsupervised clustering of amino acid residue positions where variants occur, without any prior knowledge of their functional importance.

## Citation
If you use CLUMP in your research, please cite:
* Turner T et al. Proteins linked to autosomal dominant and autosomal recessive disorders harbor characteristic rare missense mutation distribution patterns <http://biorxiv.org/content/early/2015/04/27/018648>

## CLUMP requires: 
* R/3.0.0
* python/2.7.3
* numpy/1.8.0
* scipy/0.12.0
* the R library package 'fpc'


## USAGE:

```
python combined.clump.py -f inputfile -p protein_lengths 
```
OPTIONS:
-a allele_frequency(Default=1)
   Remove Mutations Greater than an allele frequency threshold. Default includes every variant.
   
-c inputfile_controls
   Input file for the controls. A set of controls is required to get a statistical significance with the clump score
 

-z number_of_permutations 
   The number of permutations you want to perform for significance testing. 


-m minimum_number_of_mutations(Default=5) 
   The minimum number of mutations in a gene in order to perform CLUMP.

-n normalize(Default=No)
   Do you want to normalize based on protein length. Normalization was not used in the published results.
-t Output Column Titles

Permutation CLUMP works best when the input file only contains one gene.

The input file and the control input file are in the same format:


* Column 1: GENE_HUGO_ID 	       Used as a column placeholder in CLUMP scripts (Can use NA if unavailable)
* Column 2: PROTEIN_ID 	       Required: Must match Protein Id's provided in the protein length file
* Column 3: STUDY_NAME 	       Used as a column placeholder in CLUMP scripts (Can use NA if unavailable)
* Column 4: AMINO_ACID_POSITION  Required: Amino Acid position of the variant
* Column 5: CHROM 	       Used as a column placeholder in CLUMP scripts (Can use NA if unavailable)
* Column 6: POSITION 	       Used as a column placeholder in CLUMP scripts (Can use NA if unavailable)
* Column 7: REF Allele	       Used as a column placeholder in CLUMP scripts (Can use NA if unavailable)
* Column 8: ALT Allele	       Used as a column placeholder in CLUMP scripts (Can use NA if unavailable)
* Column 9: ALLELE_FREQUENCY     Optional column. Allele Frequency is treated as 0 if not provided
* Column 10:DOMAIN	       Optional column

I have provided a set of neutral variation from 1000 Genomes in the format used by CLUMP.

The protein length file is in the format:
```PROTEIN_ID LENGTH```

I have provided a protein length file: protein.2.length.txt


### EXAMPLE 1:  PERMUTATION TESTING OF A SINGLE GENE USING CLUMP

```
python combined.clump.py -f example.inputfile.txt -p protein.2.length.txt -z 10000 -c example.controlfile.txt -t 
```

OUTPUT:

Now clustering: ACADS
Now clumping
GENE    PROTEIN_ID      STUDY_NAME      CLUMP_SCORE_DIFFERENCE(Controls-Cases)  P-value(Probability Cases have a CLUMP score greater than the Controls) P-value(Probability Cases have a CLUMP score less than Controls)        CONTROL_Raw_Clump_Score
ACADS   NP_000008.1     1000Genomes     0.0     0.497   0.503   1.51153082805   1.51153082805


### EXAMPLE 2: RAW CLUMP SCORE

```
python combined.clump.py -f example.inputfile.txt -p protein.2.length.txt
```

OUTPUT:

Now clustering: ACADS
Now clumping
GENE    PROTEIN_ID      STUDY_NAME      Raw_Clump_Score
ACADS   NP_000008.1     1000Genomes     1.51153082805
 

### EXAMPLE 3: HIGH THROUGHPUT PERMUTATION CLUMP ANALYSIS

Permutation testing for clump analysis runs much faster when the input files are split among individual genes. I have created an example with a shell script to demonstrate the easiest way to perform clump permutation testing on a large set of genes.

```
bash runlargescaleclump.sh example3.inputfile.txt example3.controlfile.txt 100
```

