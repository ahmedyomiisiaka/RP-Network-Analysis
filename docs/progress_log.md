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

# Entry 1 — 01 August 2026

## Objective

To initialize the research project, organise the GitHub repository, review the approved research proposal, and prepare the computational workflow before beginning data collection.

---

## Activities Completed

- Created the GitHub repository:
  https://github.com/ahmedyomiisiaka/RP-Network-Analysis

- Added the project README containing:
  - Project overview
  - Research objectives
  - Planned methodology
  - Computational workflow
  - Databases
  - References

- Added a `.gitignore` file to exclude:
  - Raw datasets
  - Large network files
  - Python cache files
  - Temporary files
  - Google Colab checkpoint files

- Carefully reviewed the approved MSc research proposal.

- Compared the research methodology with the planned computational workflow.

- Divided the project into eleven clearly defined stages to ensure reproducibility.

- Prepared the project structure for implementation in Google Colab.

---

## Project Pipeline

- Stage 0 – Project initialization and documentation
- Stage 1 – Collection and curation of RP-associated genes
- Stage 2 – Collection of apoptosis pathway genes
- Stage 3 – Gene identifier mapping (UniProt)
- Stage 4 – Protein interaction network construction (STRING & IntAct)
- Stage 5 – Context-specific network filtering
- Stage 6 – Source-target pathway reconstruction
- Stage 7 – Network topology analysis
- Stage 8 – Pathway enrichment analysis
- Stage 9 – Candidate target prioritization
- Stage 10 – Validation using retinal single-cell RNA-seq
- Stage 11 – Visualization and thesis figure generation

---

## Files Created

- README.md
- .gitignore
- docs/progress_log.md

---

## Challenges

No technical issues were encountered during project initialization.

---

## Next Steps

Begin **Stage 1**:

- Download the curated Retinitis Pigmentosa gene list from RetiGene.
- Verify the reported RP-associated genes.
- Remove duplicate entries if necessary.
- Create the master RP gene dataset.
- Document every processing step.

---

## Notes

This project will be developed using reproducible computational methods. Every analysis step, code implementation, parameter selection, software package, database version, and generated output will be documented throughout the project to ensure transparency and reproducibility.
