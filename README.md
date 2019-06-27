# Mitobin
Scripts and data for the mitobin program



# References
This program will work with any reference FASTA as long as it has the specific header format associated with mitobin. FAA or FNA is fine. Make sure genes are oriented in the same direction.

Header:
>UNIQUE_ID|GENE|Kingdom|Phylum|Class|Order|Family|Genus|Species

example: 
>AB016606.1.7887.8569|ATP6|Eukaryota|Metazoa|Chordata|unknown_class|Squamata|Scincidae|Plestiodon|Plestiodon egregius

# Useful files
### 'synonyms_updated.tsv'
a simple lookup from some random gene id to a well defined set of genes. Currently only supports the 13 core animal mitochondrial PCG.

examples: CO-I COX1, MTCO1 COX1, CO2  COX2

### 'expanded_ncbi_taxonomy.tsv'
a simplifuied version of the NCBI named and nodes taxonomy files.

example line: 859970  Eukaryota;Metazoa;Arthropoda;Hexapoda;unknown_superclass;Insecta;Pterygota;Amphiesmenoptera;Lepidoptera;Noctuoidea;Noctuidae;Noctuinae;Feraxinia;Feraxinia serena




### MIDORI reference preperation

Why reinvent the wheel? Midori is a curated mitochodnrial gene database: [Publication](https://academic.oup.com/bioinformatics/article/34/21/3753/5033384) and [Website](http://www.reference-midori.info/)

The script 'midori_mit_reference_download.sh' will recursvely download the database for each mitochodnrial gene. The script URL will need to be updated when midori updates the database. I selectively choose the RDP format to download since its the most similiar to mitobin.

The conversion to the mitobin format is a bit complex for this database since FAA translations are not available. The nucleotide versions are easy.

A couple files are required for this. 'synonyms_updated.tsv', 'expanded_ncbi_taxonomy.tsv', and 'genetic_codes_dates_taxids.txt'
The 'genetic_codes_dates_taxids.txt' file is a lookup from taxid to the mitochodrial genetic code.



### GENBANK refseq muitchondrial genomes

mitobin.py will install this database natively if no reference file exists so no need to worry. we got you.






