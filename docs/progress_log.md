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

### What was done

**Part A — RetiGene v1.13 (accessed 2026-08-01):**
- Downloaded full gene table manually from https://retigene.erdc.info
- Total IRD genes in database: 528
- Loci entries explicitly excluded from gene-level comparison
- Filtered for: Phenotype contains RP AND Broad category = Non-syndromic or Both
- RP gene entries retrieved: 112 (loci excluded)
- RP loci entries: 2 (RP17 locus, Xq27.1 locus)
- Total RetiGene RP entries: 114

**Part B — Rivolta et al. 2025, Table S1:**
- File: mmc2.xlsx (Am J Hum Genet, 2025)
- Filtered for: Retained = Yes, Category contains RP
- Gene entries: 101
- Loci entries: 2 (RP17 locus, Xq27.1 locus)
- Total Rivolta entries: 103

**Part C — Reconciliation (correct final numbers):**

| Category | Count |
|----------|-------|
| RetiGene v1.13 RP entries | 114 |
| Rivolta et al. 2025 RP entries | 103 |
| Shared entries (both sources) | 103 |
| RetiGene-only (post-2025 additions) | 11 |
| Rivolta-only | 0 |
| Final reconciled entries | 114 |
| Final genes (protein-coding) | 112 |
| Final loci | 2 |

Note: Loci were treated as a distinct entry type from the beginning
of reconciliation and were not included in gene-level comparison.

**Post-2025 RetiGene additions (not in Rivolta 2025):**
BCOR, CREB3, FSD1L, PRPF6, RNU4-2, RNU6-1, RNU6-2,
RNU6-8, RNU6-9, SAXO6, SCLT1

**Inheritance breakdown (112 protein-coding genes):**
- AR: 72
- AD-AR: 21
- AD: 14
- X-linked: 5

### Output files
- data/raw/rp_genes_retigene.csv — RetiGene RP entries (114)
- data/raw/rp_genes_rivolta2025.csv — Rivolta 2025 RP entries (103)
- data/processed/rp_genes_final.csv — Final reconciled list (114 entries)
- data/processed/rp_genes_for_network.csv — 112 protein-coding genes only

### Problems encountered
- RetiGene has no public API — data downloaded manually from website
- Colab session resets required restoring data from Google Drive
- Initial reconciliation included loci in gene comparison — corrected
  by explicitly filtering loci before symbol-level comparison

### Next steps (Stage 2 — Notebook 02)
- Collect apoptosis pathway proteins from Reactome R-HSA-109581
- Collect apoptosis pathway proteins from KEGG hsa04210
- Cross-check both sources and produce final apoptosis target list

---
