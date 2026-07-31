# RP Network Analysis

**MSc Thesis — Vilnius University, Systems Biology**
**Supervisor:** Doc. Dr. Erinija Pranckevičienė
**Author:** Ahmed Yomi Isiaka
**Year:** 2026

---

## Project Title

Network-Based Analysis of Signalling Pathways in Retinitis Pigmentosa:
Identifying the Protein Subnetwork Linking Disease Genes to Apoptosis

## Overview

Retinitis Pigmentosa (RP) is a genetically heterogeneous inherited retinal disorder
caused by pathogenic variants in more than 100 genes. Although these genes are
functionally diverse, degeneration of photoreceptor cells ultimately converges on
common molecular mechanisms, particularly apoptosis.

The objective of this project is to reconstruct the protein interaction network
connecting RP-associated proteins to apoptosis-related proteins using publicly
available interaction databases. By integrating protein–protein interaction
networks, pathway databases and functional enrichment analyses, this study aims
to identify key intermediate proteins and signaling pathways that may represent
potential therapeutic or genome-editing targets.

## Research Workflow

| Stage | Description |
|-------|-------------|
| Stage 0 | Project initialization, literature review, research planning, and documentation |
| Stage 1 | Collection and curation of Retinitis Pigmentosa (RP)–associated genes |
| Stage 2 | Collection of apoptosis pathway genes from Reactome and KEGG |
| Stage 3 | Gene and protein identifier mapping (HGNC → UniProt) |
| Stage 4 | Construction of the protein–protein interaction (PPI) network using STRING and IntAct |
| Stage 5 | Context-specific network filtering using retinal expression, subcellular localization, and tissue-specific interactions |
| Stage 6 | Source-to-target pathway reconstruction between RP genes and apoptosis pathway proteins |
| Stage 7 | Network topology analysis, hub identification, and community detection |
| Stage 8 | Functional enrichment analysis (Reactome, KEGG, Gene Ontology, WikiPathways) |
| Stage 9 | Prioritization of candidate therapeutic and genome-editing targets |
| Stage 10 | Validation using retinal single-cell RNA sequencing (scRNA-seq) datasets |
| Stage 11 | Network visualization, result interpretation, and thesis figure generation |

## Key Databases

- [RetiGene](https://retigene.erdc.info) — RP gene source list
- [STRING](https://string-db.org) — protein interaction network
- [IntAct](https://www.ebi.ac.uk/intact) — curated experimental interactions
- [Reactome](https://reactome.org) — apoptosis pathway R-HSA-109581
- [KEGG](https://www.genome.jp/kegg) — apoptosis pathway hsa04210
- [Human Protein Atlas](https://www.proteinatlas.org) — retinal expression
- [COMPARTMENTS](https://compartments.jensenlab.org) — subcellular localisation
- [IID](https://iid.ophid.utoronto.ca) — tissue-specific PPIs

## Repository Structure

```
RP-Network-Analysis/
│
├── notebooks/              # Google Colab notebooks (Stage 1–11)
├── data/
│   ├── raw/                # Original downloaded datasets
│   ├── processed/          # Cleaned datasets
│   └── results/            # Analysis outputs
├── scripts/                # Python helper scripts (if required)
├── docs/                   # Daily progress log and documentation
├── figures/                # Thesis figures and diagrams
├── README.md
└── .gitignore
```

## How to Run

The analysis is performed using Google Colab.

Each notebook corresponds to one stage of the research workflow and should be executed sequentially.

| Notebook | Stage |
|----------|-------|
| Notebook 00 | Project setup and documentation |
| Notebook 01 | RP gene collection and curation |
| Notebook 02 | Apoptosis pathway gene collection |
| Notebook 03 | Gene identifier mapping (UniProt) |
| Notebook 04 | STRING and IntAct network construction |
| Notebook 05 | Context-specific network filtering |
| Notebook 06 | Source-to-target pathway reconstruction |
| Notebook 07 | Network topology analysis |
| Notebook 08 | Pathway enrichment analysis |
| Notebook 09 | Candidate target prioritization |
| Notebook 10 | scRNA-seq validation |
| Notebook 11 | Visualization and figure generation |

Each notebook contains:

- Objective
- Input data
- Methods
- Python code
- Output files
- Interpretation of results

## Documentation

Daily progress is recorded in:

```
docs/progress_log.md
```

Each entry includes:

- Date
- Objectives
- Methods used
- Results obtained
- Problems encountered
- Next steps

## References

- Rivolta C., et al. (2025). *RetiGene: A comprehensive gene atlas for inherited retinal diseases*. American Journal of Human Genetics.
- Szklarczyk D., et al. (2023). *STRING v12: protein–protein association networks and functional enrichment analysis*. Nucleic Acids Research.
- Gil D., et al. (2017). *PathLinker: connecting signaling pathways through protein interaction networks*. F1000Research.
- Lukowski S. W., et al. (2019). *A single-cell transcriptome atlas of the adult human retina*. EMBO Journal.

## License

This repository contains research material developed as part of an MSc thesis in Systems Biology at Vilnius University. The code is provided for academic and research purposes.
