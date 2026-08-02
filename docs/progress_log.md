# Daily Progress Log

**Project:** RP Network Analysis — MSc Thesis
**Project Title:**
Network-Based Analysis of Signalling Pathways in Retinitis Pigmentosa:
Identifying the Protein Subnetwork Linking Disease Genes to Apoptosis
**Author:** Ahmed Yomi Isiaka
**Supervisor:** Doc. Dr. Erinija Pranckevičienė
**Institution:** Vilnius University, Faculty of Medicine
**Programme:** MSc Systems Biology

---

## Entry 1 — 2026-08-01

### Objectives for today
- Initialise the research project and GitHub repository
- Review the approved research proposal
- Plan the full computational pipeline

### What was done
- Created GitHub repository: https://github.com/ahmedyomiisiaka/RP-Network-Analysis
- Added README with project overview, pipeline, databases, and references
- Added .gitignore to exclude raw data, large network files, and cache files
- Reviewed the approved MSc research proposal in full
- Planned 12-stage pipeline (Stage 0-11) mapped to databases and tools
- Created Notebook 00: project setup, folder structure, environment check

### Files created
- README.md
- .gitignore
- docs/progress_log.md
- notebooks/Notebook_00_Project_Setup.ipynb

### Problems encountered
- None

### Next steps
- Begin Stage 1: collect RP-associated genes from RetiGene and Rivolta 2025

---

## Entry 2 — 2026-08-01

### Objectives for today
- Collect RP-associated genes from RetiGene database
- Collect RP-associated genes from Rivolta et al. (2025) Table S1
- Cross-check both lists and produce final reconciled gene list
- Verify final dataset programmatically

### Stage 1 — RP Gene Collection and Curation

#### Data sources
- RetiGene database, version 1.13 (accessed 2026-08-01)
- Rivolta et al. (2025), Table S1, Am J Hum Genet, 112(10):2253-2265

#### Method

**Part A — RetiGene v1.13:**
- Downloaded full gene table manually from https://retigene.erdc.info
- Total IRD genes in database: 528
- Filter applied: Phenotype contains "RP" AND
  Broad category = "Non-syndromic" OR "Both"
- Loci explicitly excluded from gene-level comparison
- RP gene entries retrieved: 112 (protein-coding genes)
- RP loci entries retrieved: 2 (RP17 locus, Xq27.1 locus)
- Total RetiGene RP entries: 114

**Part B — Rivolta et al. 2025, Table S1:**
- File: mmc2.xlsx
- Filter applied: Retained = Yes, any Category column contains "RP"
- Gene entries: 101
- Loci entries: 2 (RP17 locus, Xq27.1 locus)
- Total Rivolta entries: 103

**Part C — Reconciliation:**
- Loci treated as distinct entry type from start of reconciliation
- Gene symbols compared independently of loci

#### Results — Authoritative final numbers

| Item | Count |
|------|-------|
| RetiGene v1.13 RP entries | 114 |
| Rivolta et al. 2025 RP entries | 103 |
| Entries supported by both sources | 103 |
| RetiGene-only entries (post-2025) | 11 |
| Rivolta-only entries | 0 |
| Duplicate gene symbols | 0 |
| **Total final entries** | **114** |
| **Protein-coding genes** | **112** |
| **Genomic loci** | **2** |

#### Post-2025 RetiGene additions (not in Rivolta 2025)
BCOR, CREB3, FSD1L, PRPF6, RNU4-2, RNU6-1, RNU6-2,
RNU6-8, RNU6-9, SAXO6, SCLT1

#### Inheritance distribution (112 protein-coding genes)

| Inheritance | Count |
|-------------|-------|
| Autosomal recessive (AR) | 72 |
| Autosomal dominant/recessive (AD-AR) | 21 |
| Autosomal dominant (AD) | 14 |
| X-linked | 5 |

#### Key methodological decision
The two locus entries (RP17 locus, Xq27.1 locus) are retained
in the disease-gene catalogue but will NOT be treated as protein
nodes during network analysis. All downstream stages (UniProt
mapping, STRING, IntAct) will operate on the 112 protein-coding
genes only.

#### Quality control
The final dataset was programmatically verified for:
- Total number of entries: 114 ✓
- Entry type separation: 112 genes + 2 loci ✓
- Source agreement: 103 in both, 11 RetiGene-only ✓
- Duplicate gene symbols: 0 ✓
- Inheritance classification present for all genes ✓

### Output files
| File | Description | Entries |
|------|-------------|---------|
| data/raw/rp_genes_retigene.csv | RetiGene RP entries | 114 |
| data/raw/rp_genes_rivolta2025.csv | Rivolta 2025 RP entries | 103 |
| data/processed/rp_genes_final.csv | Final reconciled list | 114 |
| data/processed/rp_genes_for_network.csv | Protein-coding genes only | 112 |

### Problems encountered
- RetiGene has no public API — data downloaded manually
- Colab session resets required restoring data from Google Drive
- Initial reconciliation included loci in gene comparison — fixed
  by explicitly filtering loci before symbol-level comparison
- Accidental deletion of data/processed/ folder on GitHub — restored

### Next steps (Stage 2 — Notebook 02)
- Collect apoptosis pathway proteins from Reactome R-HSA-109581
- Collect apoptosis pathway proteins from KEGG hsa04210
- Cross-check both sources
- Produce final apoptosis target list with UniProt accessions

---
