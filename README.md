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
a simplified version of the NCBI named and nodes taxonomy files.

example line: 859970  Eukaryota;Metazoa;Arthropoda;Hexapoda;unknown_superclass;Insecta;Pterygota;Amphiesmenoptera;Lepidoptera;Noctuoidea;Noctuidae;Noctuinae;Feraxinia;Feraxinia serena

##### Constructing an updated taxonomy database.

Locate and download the latest NCBI taxonomy database. Link updated March 07, 2019
```
mkdir ncbi_taxonomy/ && cd ncbi_taxonomy/
wget ftp://ftp.ncbi.nih.gov/pub/taxonomy/new_taxdump/new_taxdump.tar.gz
tar xvzf *.tar.gz
```
You will need the names and nodes file to construct the expanded taxonomy lookup
```
python3 genbank_nodes_and_names_to_taxonomy.py  names.dmp nodes.dmp
```

##### Expand some taxonomic level to the full taxonomic string w/ 'taxonToFullTaxonomy.py'

It takes a list of taxonomic names, if you have more than just species it expects it to be separated by a tab with decreasing taxonomic order (i.e. family\tgenus\tspecies). It works from right to left searching the reference database for the taxon. If it cant find the species it moves up a level and tries to find the next rank. It is pretty much a grep into the 'expanded_ncbi_taxonomy.tsv' for the most specific taxon. Use with caution and curate the results.


## MIDORI reference preperation

Why reinvent the wheel? Midori is a curated mitochodnrial gene database: [Publication](https://academic.oup.com/bioinformatics/article/34/21/3753/5033384) and [Website](http://www.reference-midori.info/)

The script 'midori_mit_reference_download.sh' will recursvely download the database for each mitochodnrial gene. The script URL will need to be updated when midori updates the database. I selectively choose the RDP format to download since its the most similiar to mitobin.

The conversion to the mitobin format is a bit complex for this database since FAA translations are not available. The nucleotide versions are easy.

A couple files are required for this. 'synonyms_updated.tsv', 'expanded_ncbi_taxonomy.tsv', and 'genetic_codes_dates_taxids.txt'
The 'genetic_codes_dates_taxids.txt' file is a lookup from taxid to the mitochodrial genetic code. I looked up the genetic codes for every unique genus, this involved a lot of queries onto the NCBI ftp site and took awhile. A total of nine different genetic codes were found, most are 5, 2, 9, or 4. https://www.ncbi.nlm.nih.gov/Taxonomy/Utils/wprintgc.cgi

```
python3 midori_convert_to_mitobin.py midori_gene.fasta
```


## GENBANK refseq mitochondrial genomes

mitobin.py will install this database natively if no reference file exists so no need to worry. we got you. It basically just download all the genbank files from ftp://ftp.ncbi.nlm.nih.gov/refseq/release/mitochondrion/ and converts them to mitobin FAA and FNA files.


## Some nematode specific databases.

#### QBOL
QBOL database was initiated by the European Plant Protection Agency for quarantined plant-parasitic nematodes. I had to make a sort of webscarping hack since they don't make bulk downloads easy. Use at your won risk. Before I run the script I first grab a list of all the taxon ids and species names from the organisms page.

```
wget "https://qbank.eppo.int/nematodes/organisms"
grep -o "/taxon/.*/" organisms | awk -F'/' 'BEGIN { OFS = ","} {print $3,$4}' | sed 's/">//g' | sed 's/<//g' | grep -v '%' > qbank_ids
python3 qbank_webscrape.py qbank_ids
```



