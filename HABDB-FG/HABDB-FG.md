HABDB-FG: Functional Gene Annotation Database

Harmful Algal Blooms (HABs) pose severe threats to global marine ecosystems and public health. With the rapid development of multi-omics, profiling the functional capacity of HAB communities from shotgun metagenomes has become critical. However, applying general-purpose public databases (e.g., KEGG, NCBI) to annotate HAB-specific traits often leads to severe false-positive assignments and unspecific homologous interference, largely due to the massive and complex genomes of dinoflagellates and the fragmentation of reference data.

To solve these issues, we built **HABDB-FG** (Functional Gene Annotation Sub-database), a manually curated, high-resolution integrative database designed for the fast and accurate profiling of core HAB ecological and phenotypic processes from complex metagenomic data.

### DATABASE CONTENT & STRATEGY

HABDB-FG contains a total of **18,961 high-quality sequences** spanning **34 key gene families**. To effectively reduce false-positive detection rates during high-throughput alignments, the database incorporates not only strictly verified seed sequences but also comprehensively expanded homologous sequences recruited from UniProt, TrEMBL, and other major databases.

The database covers three major HAB-related functional processes:

* **Paralytic Shellfish Toxins (PSTs) Biosynthesis:** Covers the core synthesis pathway (11 gene families including *sxtA*, *sxtG*, *sxtB*, etc.), structural modifications (*sxtX*, *sxtO*, *sxtN*, *sxtL*), and transport/regulation modules. Notably, it also prospectively includes putative marine modification genes (*sxtACT*, *sxtSUL*, *sxtDIOX*) to facilitate hypothesis-driven discovery in marine dinoflagellates.
* **Domoic Acid (DA) Biosynthesis:** Includes the core *dab* gene cluster (*dabA*, *dabB*, *dabC*, *dabD*) responsible for the synthesis of this neurotoxin primarily produced by marine diatoms.
* **Dinoflagellate Bioluminescence:** Focuses on the core genetic markers reflecting bioluminescence signal intensity, specifically luciferase (*lcf*) and luciferin-binding protein (*lbp*).

### GENERATED FILES

Three core files are provided for the HABDB-FG module:

1. **`HABs_FuncDB_Full.database`**: The raw text FASTA format file containing all 18,961 curated representative and homologous protein sequences. This file serves as the foundational sequence archive and can be used for custom sequence retrieval or alternative database compilation.
2. **`HABs_Func_db.dmnd`**: The pre-compiled binary database formatted specifically for the **DIAMOND** aligner. This file is ready for immediate, ultra-fast protein alignment (e.g., `diamond blastp` or `diamond blastx`) against your shotgun metagenomic sequences.
3. **`HABs_FuncDB_id2genemap.txt`**: A tab-separated mapping file that links sequence IDs directly to their corresponding functional gene annotations (e.g., matching a specific UniProt ID to the *sxtA* gene). This is essential for converting raw alignment outputs into quantitative functional gene profiles.

### DOWNSTREAM USAGE EXAMPLE

To profile HAB functional genes in your metagenomic dataset using DIAMOND, you can directly use the provided `.dmnd` file:

**Bash**

```
diamond blastx \
  -d /path/to/HABs_Func_db.dmnd \
  -q your_metagenome_reads.fasta \
  -o functional_annotation_results.tsv \
  --evalue 1e-5 \
  --outfmt 6 qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore
```

*After alignment, use `HABs_FuncDB_id2genemap.txt` to aggregate the mapped reads into specific gene family abundances.
