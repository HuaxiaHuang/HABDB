# HABDB-AS: Algal Species Annotation Database

HABDB-AS is a specialized sub-database of HABDB, focusing on the accurate species annotation of Harmful Algal Bloom (HAB) species in metagenomic datasets. This repository is structured around three core modules: 18S rRNA, Genome, and CDS.

## **DATA ARCHITECTURE & RATIONALE**

### Genome and CDS Supplement

Genomic data provides the most precise and comprehensive genetic information. However, Reference Genome resources for HAB species are extremely scarce. Currently, only 16 HAB species (<10% of our checklist) have marked Reference Genomes in NCBI. Furthermore, quality assessments reveal that the only two available dinoflagellate genomes are highly fragmented, making them insufficient for downstream in-depth analysis.

To overcome this limitation, we collected Coding Sequence (CDS) datasets for corresponding HAB species as a crucial supplement. We have curated a strictly filtered CDS set across 50 species and performed comprehensive Codon Usage Bias (CUB) analysis. These data provide essential genomic features for improving future genome binning algorithms. Detailed NCBI resource status can be found in the file: `HAB_ReferenceGenome_NCBI_Status.xlsx`.

To facilitate downstream data mining, the CUB analysis results are systematically structured in the `HAB-AS-CDS-CodonUsage` directory:

* **`CUB_Metrics/`**: Contains the detailed codon usage indices for each CDS, including key parameters such as sequence length (`Length_AA`), GC content distribution (`GC`, `GC1-3`, `GC12`, `GC3s`), Effective Number of Codons (`ENC`), and Parity Rule 2 biases (`PR2_A3_ratio`, `PR2_G3_ratio`).
* **`CUB_Analysis_Plots/`**: Provides visual representations of codon usage patterns (e.g., ENC-plots, PR2-plots, Neutrality plots) for each HAB species.
* **`RSCU_Matrices/`**: Contains the calculated Relative Synonymous Codon Usage (RSCU) matrices, detailing specific amino acid and codon preferences for each species.
* **`HAB_Species_CUB_MasterTable.csv`**: A master summary table consolidating all calculated CUB metrics and RSCU values across all analyzed HAB species.

### 18S rRNA Annotation Database (Kraken2)

Given current data availability, comprehensive environmental monitoring of HABs still relies heavily on the broadly sequenced 18S rRNA gene. We have manually curated and verified 259 high-quality 18S rRNA sequences (covering 92.5% of the HAB species checklist) to serve as the custom taxonomy database for Kraken2 (HABs_18S_sequences.fasta).

#### [!] Important Note on Dereplication:

Kraken2 utilizes the Lowest Common Ancestor (LCA) algorithm. Including too many homologous sequences of closely related species can cause k-mer matches to default to higher taxonomic ranks, drastically reducing resolution at the genus or species level. Therefore, HABs_18S_sequences.fasta strictly utilizes a single representative sequence per HAB species to maximize annotation accuracy.

If you require comprehensive data for sub-species diversity or phylogenetic tree construction, please refer to the complete archive in Sheet2 of HABDB-AS-18SrRNA-information.xlsx, which catalogs all available historical 18S rRNA sequences.

#### DATA AVAILABILITY (ZENODO)

Due to GitHub's file size restrictions, the massive datasets including HAB Genomes, the CDS collections, and the pre-built Kraken2 18S rRNA database are uniformly hosted on Zenodo.

Download Link:` [@@@@@@@@@@@@@@@@Insert your Zenodo DOI link here]`

#### LOCAL KRAKEN2 DATABASE CONSTRUCTION GUIDE

If you prefer to build the Kraken2 database locally from the provided FASTA file, please follow the standard workflow below. (Refer to the Kraken2 Official Manual for more details: `https://github.com/DerrickWood/kraken2/wiki/Manual#introduction)`

##### Step 1. Create and enter the taxonomy directory:

```
mkdir -p /your_path/HABs_Final_Kraken2_db/taxonomy
cd /your_path/HABs_Final_Kraken2_db/taxonomy
```

##### Step 2. Download the latest NCBI taxonomy dump and its MD5 checksum:

```
wget https://ftp.ncbi.nlm.nih.gov/pub/taxonomy/new_taxdump/new_taxdump.tar.gz
wget https://ftp.ncbi.nlm.nih.gov/pub/taxonomy/new_taxdump/new_taxdump.tar.gz.md5
```

##### Step 3. Verify file integrity:

```
md5sum -c new_taxdump.tar.gz.md5
```

##### Step 4. Extract the taxonomy files:

```
tar -xvzf new_taxdump.tar.gz
```

##### Step 5. Add the curated FASTA file to the Kraken2 library:

```
kraken2-build --add-to-library /your_path/HABs_Final_Kraken2_db/HABs_18S_sequences.fasta --db /your_path/HABs_Final_Kraken2_db
```

##### Step 6. Build the database:

```
kraken2-build --build --db /your_path/HABs_Final_Kraken2_db
```

##### Step 7. Inspect the built database:

```
kraken2-inspect --db /your_path/HABs_Final_Kraken2_db
```

##### DOWNSTREAM ANALYSIS

After performing species annotation using the custom HAB-Kraken2 database, you can utilize the `HAB_Gettax.pl` script to summarize and compile the taxonomy results.
