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
- Planned 12-stage pipeline (Stage 0–11) mapped to databases and tools
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

### What was done

**Part A — RetiGene v1.13:**
- Downloaded full gene table manually from https://retigene.erdc.info
- Total genes in database: 528 (all IRDs)
- Filtered for: Phenotype contains "RP" AND Broad category = Non-syndromic or Both
- RP genes retrieved: 114

**Part B — Rivolta et al. 2025, Table S1:**
- Uploaded mmc2.xlsx (Am J Hum Genet, 2025)
- Filtered for: Retained = Yes, Category contains RP
- Genes: 101 + 2 loci = 103 total entries

**Part C — Reconciliation:**
- Genes in both sources: 101 (high confidence)
- Only in RetiGene: 13 (newer additions since paper)
- Only in Rivolta: 0
- Final gene list: 116 total entries (114 genes + 2 loci)

### Output files
- data/raw/rp_genes_retigene.csv — RetiGene RP genes
- data/raw/rp_genes_rivolta2025.csv — Rivolta 2025 RP genes
- data/processed/rp_genes_final.csv — Final reconciled list

### Problems encountered
- RetiGene has no public API — data downloaded manually from website
- Colab session reset required re-uploading mmc2.xlsx

### Next steps (Stage 2 — Notebook 02)
- Collect apoptosis pathway proteins from Reactome R-HSA-109581
- Collect apoptosis pathway proteins from KEGG hsa04210
- Cross-check and produce final apoptosis target list

---
