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

---

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

---

## Current Status

| Stage | Status | Key Output |
|-------|--------|------------|
| Stage 0 | ✅ Complete | Project setup, folder structure, documentation |
| Stage 1 | ✅ Complete | 114 RP entries: 112 protein-coding genes + 2 loci |
| Stage 2 | 🔄 In progress | Apoptosis target genes from Reactome and KEGG |
| Stage 3 | ⏳ Pending | UniProt identifier mapping |
| Stage 4 | ⏳ Pending | PPI network construction |
| Stage 5 | ⏳ Pending | Context-specific filtering |
| Stage 6 | ⏳ Pending | Pathway reconstruction |
| Stage 7 | ⏳ Pending | Network topology analysis |
| Stage 8 | ⏳ Pending | Enrichment analysis |
| Stage 9 | ⏳ Pending | Target prioritisation |
| Stage 10 | ⏳ Pending | scRNA-seq validation |
| Stage 11 | ⏳ Pending | Visualisation and figures |

---

## Stage 1 Results — RP Gene Dataset

| Item | Count |
|------|-------|
| RetiGene v1.13 RP entries (accessed 2026-08-01) | 114 |
| Rivolta et al. 2025 Table S1 RP entries | 103 |
| Entries supported by both sources | 103 |
| RetiGene-only entries (post-2025 additions) | 11 |
| Rivolta-only entries | 0 |
| Duplicate gene symbols | 0 |
| **Total final curated entries** | **114** |
| **Protein-coding genes** | **112** |
| **Genomic loci (excluded from network)** | **2** |
| **Working set for network analysis** | **112** |

Inheritance distribution of 112 protein-coding genes:

| Inheritance | Count |
|-------------|-------|
| Autosomal recessive (AR) | 72 |
| Autosomal dominant/recessive (AD-AR) | 21 |
| Autosomal dominant (AD) | 14 |
| X-linked | 5 |

The two locus entries (RP17 locus, Xq27.1 locus) are retained in the
curated disease catalogue but are excluded from all protein-level
analyses (UniProt mapping, STRING, IntAct).

---

## Key Databases

### RP Gene Sources
- [RetiGene](https://retigene.erdc.info) — curated IRD gene database
  (v1.13, accessed 2026-08-01)
- [Rivolta et al. 2025](https://doi.org/10.1016/j.ajhg.2025.08.017)
  — peer-reviewed RP gene atlas, Am J Hum Genet, 112(10):2253–2265

### Apoptosis Pathway
- [Reactome](https://reactome.org) — pathway R-HSA-109581
  (Intrinsic Pathway for Apoptosis)
- [KEGG](https://www.genome.jp/kegg) — pathway hsa04210 (Apoptosis)

### Identifier Mapping
- [UniProt](https://www.uniprot.org) — canonical protein identifiers

### PPI Network Construction
- [STRING](https://string-db.org) — protein interaction network
  (combined score ≥ 0.700, high confidence)
- [IntAct](https://www.ebi.ac.uk/intact) — curated experimental
  protein interactions

### Context-Specific Filtering
- [Human Protein Atlas](https://www.proteinatlas.org) — retinal RNA
  and protein expression
- [GTEx](https://gtexportal.org) — bulk tissue gene expression
  including retina
- [CellxGene](https://cellxgene.cziscience.com) — single-cell
  retinal expression
- [COMPARTMENTS](https://compartments.jensenlab.org) — subcellular
  co-localisation
- [IID](https://iid.ophid.utoronto.ca) — tissue-specific PPI data
- [TissueNet v3](https://netbio.bgu.ac.il/tissuenet) — tissue-resolved
  interaction scores
- [ProteomicsDB](https://www.proteomicsdb.org) — proteomics expression
  evidence

### Validation
- Lukowski et al. (2019) scRNA-seq retinal atlas
  (ArrayExpress: E-MTAB-7316)

---

## Repository Structure
