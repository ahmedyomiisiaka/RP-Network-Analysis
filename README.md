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

Retinitis Pigmentosa (RP) is a genetically heterogeneous inherited retinal
disorder caused by pathogenic variants in more than 100 genes. Although these
genes are functionally diverse, degeneration of photoreceptor cells ultimately
converges on common molecular mechanisms, particularly apoptosis.

The objective of this project is to reconstruct the protein interaction network
connecting RP-associated proteins to apoptosis-related proteins using publicly
available interaction databases. By integrating protein–protein interaction
networks, pathway databases and functional enrichment analyses, this study aims
to identify key intermediate proteins and signalling pathways that may represent
potential therapeutic or genome-editing targets.

## Research Workflow

| Stage | Description |
|-------|-------------|
| Stage 0 | Project initialisation, literature review, research planning, documentation |
| Stage 1 | Collection and curation of RP-associated genes |
| Stage 2 | Collection of apoptosis pathway genes from Reactome and KEGG |
| Stage 3 | Gene and protein identifier mapping (HGNC → UniProt) |
| Stage 4 | PPI network construction using STRING and IntAct |
| Stage 5 | Context-specific network filtering — retinal expression, subcellular localisation, tissue-specific interactions |
| Stage 6 | Source-to-target pathway reconstruction (RP genes → apoptosis proteins) |
| Stage 7 | Network topology analysis, hub identification, and community detection |
| Stage 8 | Functional enrichment analysis (Reactome, KEGG, Gene Ontology, WikiPathways) |
| Stage 9 | Prioritisation of candidate therapeutic and genome-editing targets |
| Stage 10 | Validation using retinal single-cell RNA sequencing (scRNA-seq) datasets |
| Stage 11 | Network visualisation, result interpretation, and thesis figure generation |

## Key Databases

### RP Gene Sources
- [RetiGene](https://retigene.erdc.info) — curated RP gene list
- [Rivolta et al. 2025](https://doi.org/10.1016/j.ajhg.2025.08.017) — 103 confirmed non-syndromic RP genes

### Apoptosis Pathway
- [Reactome](https://reactome.org) — apoptosis pathway R-HSA-109581
- [KEGG](https://www.genome.jp/kegg) — apoptosis pathway hsa04210

### Identifier Mapping
- [UniProt](https://www.uniprot.org) — canonical protein identifiers

### PPI Network Construction
- [STRING](https://string-db.org) — protein interaction network (score ≥ 0.700)
- [IntAct](https://www.ebi.ac.uk/intact) — curated experimental interactions

### Context-Specific Filtering
- [Human Protein Atlas](https://www.proteinatlas.org) — retinal RNA and protein expression
- [GTEx](https://gtexportal.org) — bulk tissue gene expression including retina
- [CellxGene](https://cellxgene.cziscience.com) — single-cell retinal expression
- [COMPARTMENTS](https://compartments.jensenlab.org) — subcellular co-localisation
- [IID](https://iid.ophid.utoronto.ca) — tissue-specific PPI data
- [TissueNet v3](https://netbio.bgu.ac.il/tissuenet) — tissue-resolved interaction scores
- [ProteomicsDB](https://www.proteomicsdb.org) — proteomics expression evidence

### Validation
- Lukowski et al. (2019) scRNA-seq retinal atlas

## Repository Structure

RP-Network-Analysis/
│
├── notebooks/ # Google Colab notebooks (Stage 0–11)
├── data/
│ ├── raw/ # Original downloaded datasets (not on GitHub)
│ ├── processed/ # Cleaned datasets
│ └── results/ # Analysis outputs
├── scripts/ # Python helper scripts (if required)
├── docs/ # Daily progress log and documentation
├── figures/ # Thesis figures and diagrams
├── README.md
└── .gitignore


## How to Run

The analysis is performed using Google Colab.
Each notebook corresponds to one stage and should be executed sequentially.

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
| Notebook 09 | Candidate target prioritisation |
| Notebook 10 | scRNA-seq validation |
| Notebook 11 | Visualisation and figure generation |

Each notebook contains:
- Objective
- Input data
- Methods
- Python code
- Output files
- Interpretation of results

> ⚠️ Large data files (network files, raw downloads) are excluded from this
> repository via `.gitignore`. Only notebooks and scripts are version-controlled.

## Documentation

Daily progress is recorded in `docs/progress_log.md`.

Each entry includes:
- Date
- Objectives
- Methods used
- Results obtained
- Problems encountered
- Next steps

## References

- Rivolta C., et al. (2025). RetiGene: A comprehensive gene atlas for inherited retinal diseases. *Am J Hum Genet*, 112(10):2253–2265.
- Szklarczyk D., et al. (2023). STRING v12: protein–protein association networks and functional enrichment analysis. *Nucleic Acids Res*, 51(D1):D638–D646.
- Gil D., et al. (2017). PathLinker: connecting signalling pathways through protein interaction networks. *F1000Research*, 6:58.
- Lukowski S.W., et al. (2019). A single-cell transcriptome atlas of the adult human retina. *EMBO J*, 38:e100811.
- Binder J.X., et al. (2014). COMPARTMENTS: unification and visualization of protein subcellular localization evidence. *Database*, bau012.
- Kotlyar M., et al. (2019). IID 2018 update: context-specific physical protein–protein interactions. *Nucleic Acids Res*, 47(D1):D581–D589.

## License

This repository contains research material developed as part of an MSc thesis
in Systems Biology at Vilnius University. The code is provided for academic
and research purposes.
