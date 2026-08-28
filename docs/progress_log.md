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

---

## Entry 3 — 2026-08-03

### Objectives for today
Retrieve apoptosis-associated proteins from Reactome R-HSA-109581
and KEGG hsa04210 and construct a reconciled reference target set
for downstream source-to-target network reconstruction.

### Stage 2 — Apoptosis-Associated Protein Collection

#### Data sources
- Reactome: pathway R-HSA-109581 (Intrinsic Pathway for Apoptosis)
  URL: https://reactome.org/PathwayBrowser/#/R-HSA-109581
- KEGG: pathway hsa04210 (Apoptosis)
  URL: https://www.genome.jp/pathway/hsa04210

#### Method
- Reactome queried via REST API:
  GET https://reactome.org/ContentService/data/participants/R-HSA-109581
  Filtered for: schemaClass = ReferenceGeneProduct, stId starts with 'uniprot:'
- KEGG queried via REST API in three steps:
  Step 1: GET https://rest.kegg.jp/link/hsa/hsa04210 — gene IDs
  Step 2: GET https://rest.kegg.jp/list/[gene_ids] — gene symbols (batches of 10)
  Step 3: GET https://rest.kegg.jp/conv/uniprot/hsa — full human UniProt mapping
- Both lists reconciled by gene symbol (uppercase comparison)

#### Results — Authoritative Stage 2 numbers

| Item | Count |
|------|-------|
| Reactome R-HSA-109581 proteins | 165 |
| KEGG hsa04210 proteins | 137 |
| Proteins in both sources | 40 |
| Reactome only | 125 |
| KEGG only | 97 |
| **Total reference set** | **262** |
| Duplicate UniProt IDs | 0 |
| Missing UniProt IDs | 0 |

Arithmetic check: 165 + 137 - 40 = 262

#### Proteins confirmed in both sources (40 core targets)
AKT1, AKT2, AKT3, APAF1, BAD, BAK1, BAX, BBC3, BCL2, BCL2L1,
BCL2L11, BID, BIRC2, CASP3, CASP6, CASP7, CASP8, CASP9, CYCS,
DFFA, DFFB, DIABLO, FADD, FAS, FASLG, GZMB, LMNB1, MAPK1, MAPK3,
MAPK8, PMAIP1, RIPK1, SPTAN1, TNFRSF10A, TNFRSF10B, TNFSF10,
TP53, TRADD, TRAF2, XIAP

#### Target set definition
Two datasets are produced for different downstream uses:

1. Reference set (262 proteins) — used in Stage 8 enrichment analysis
   File: data/processed/apoptosis_genes_final.csv

2. Core target set (40 proteins) — used in Stage 6 PathLinker
   reconstruction. Confirmed in both Reactome and KEGG.
   File: data/processed/apoptosis_core_targets.csv

#### Six-question QC validation
Q1. All entries unique by UniProt ID: YES
Q2. All entries have valid UniProt IDs: YES (100% coverage)
Q3. Duplicated gene-protein mappings: None
Q4. Missing UniProt IDs: None
Q5. Shared between both databases: 40 proteins
Q6. Core proteins listed above

#### Problems encountered
No technical problems encountered during data extraction
or reconciliation. The KEGG /conv/uniprot/hsa endpoint provides the human-wide
gene-to-UniProt mapping rather than a pathway-specific mapping.
The full mapping table was therefore downloaded and filtered
to the genes belonging to hsa04210.

### Output files

| File | Description | Entries |
|------|-------------|----------|
| data/raw/apoptosis_reactome.csv | Raw Reactome proteins | 165 |
| data/raw/apoptosis_kegg.csv | Raw KEGG proteins | 137 |
| data/processed/apoptosis_genes_final.csv | Full reference set | 262 |
| data/processed/apoptosis_core_targets.csv | Core PathLinker targets | 40 |

### Next steps (Stage 3 — Notebook 03)
- Map all 112 RP protein-coding gene symbols to UniProt accessions
- Verify UniProt accessions for all 262 apoptosis reference proteins
- Produce unified identifier table for network construction in Stage 4

---
## Entry 4 — 2026-08-13

### Stage 4 — PPI Network Construction (interim results)

#### What was done
Queried STRING v12 API for direct interaction partners of 112 curated RP protein-coding genes.

#### Parameters used
- Database: STRING v12
- Species: Homo sapiens (taxonomy ID: 9606)
- Confidence threshold: 0.400 (medium, exploratory — final threshold pending supervisor confirmation)
- Query method: one protein at a time via /api/tsv/interaction_partners

#### Results

| Item | Value |
|------|-------|
| Genes successfully queried | 105 of 112 |
| Failed queries | 7 |
| Total unique edges (score >= 0.400) | 9,835 |
| Total unique proteins in network | 4,132 |
| Seed proteins (RP genes) in network | 105 |
| New interacting partners | 4,027 |

#### Score distribution

| Score range | Edges |
|-------------|-------|
| 0.400-0.499 | 3,590 |
| 0.500-0.699 | 3,410 |
| 0.700-0.899 | 1,635 |
| 0.900-1.000 | 1,200 |

#### Network size at different thresholds

| Threshold | Edges | Proteins |
|-----------|-------|----------|
| 0.400 | 9,835 | 4,132 (105 seeds + 4,027 partners) |
| 0.500 | 6,245 | 2,608 (105 seeds + 2,503 partners) |
| 0.700 | 2,835 | 1,292 (102 seeds + 1,190 partners) |
| 0.900 | 1,200 | 662 (79 seeds + 583 partners) |

#### Failed queries — 7 genes
- CFAP418: gene symbol not found in STRING v12
- RNU4-2, RNU6-1, RNU6-2, RNU6-8, RNU6-9: small nuclear RNA genes — not proteins, cannot be in a protein interaction database
- SAXO6: very recently added to RetiGene, not yet in STRING v12

#### Key biological finding
The 20 highest-degree partner proteins are all known retinal disease genes not in the 112-gene RP input set (ABCA4, GUCA1B, ROM1, CEP290, RPGRIP1, CNGA3, GNAT2 etc.). This confirms STRING is returning biologically relevant interactions.

#### Pending supervisor confirmation
1. Final confidence threshold to use
2. Handling of 5 RNU RNA genes (recommend exclusion from protein set)
3. STRING + IntAct integration strategy

#### Output files
- data/raw/string_interactions_raw.tsv
- data/processed/ppi_interactions.tsv
- data/processed/ppi_nodes.tsv

### Next steps
- Confirm threshold with supervisor
- Query IntAct for same input genes
- Begin Stage 5: context-specific annotation

---
## Entry 5 — 2026-08-14

### Stage 5 — Context-Specific Network Filtering (Filters A and B)

#### What was done
Applied two biological filters to the Stage 4 STRING network (9,835 edges, 4,132 proteins) to retain only interactions biologically plausible in retinal photoreceptor context.

#### Filter A — COMPARTMENTS (subcellular co-localisation)

| Property | Value |
|----------|-------|
| Database | COMPARTMENTS (Jensen Lab) |
| File | human_compartment_integrated_full.tsv |
| URL | https://download.jensenlab.org/ |
| Access date | 2026-08-13 |
| Score cutoff | >= 3 (medium-high confidence) |
| Broad GO terms excluded | Yes (cellular_component, organelle, etc.) |

Method: For each interaction edge, retrieved specific subcellular compartments for both proteins (score >= 3, broad terms excluded). If the two compartment sets overlap, the edge is retained. If no overlap exists, the edge is removed. Proteins not found in COMPARTMENTS are retained conservatively.

Results:
- Edges input: 9,835
- Edges removed (no compartment overlap): 523
- Edges retained: 9,312

#### Filter B — Human Protein Atlas (retinal expression)

| Property | Value |
|----------|-------|
| Database | Human Protein Atlas v25.1 |
| File | rna_tissue_consensus.tsv |
| URL | https://www.proteinatlas.org/about/download |
| Access date | 2026-08-13 |
| Expression cutoff | nTPM > 0 (any detectable expression in retina) |

Method: For each edge passing Filter A, retrieved nTPM value in retinal tissue for both proteins. If either protein has nTPM = 0 in retina, the edge is removed. Proteins not found in HPA are retained conservatively.

Results:
- Edges input: 9,312
- Edges removed (not expressed in retina): 389
- Edges retained: 8,923

#### Filtering summary

| Stage | Edges | Proteins |
|-------|-------|----------|
| Stage 4 network | 9,835 | 4,132 |
| After Filter A (COMPARTMENTS) | 9,312 | — |
| After Filter B (HPA retina) | 8,923 | 3,738 |
| RP seed proteins retained | — | 105 |

#### Pending
- Filter C: IID — retina-specific interaction evidence
- Filter D: TissueNet v3 — retinal interaction scoring
- Await supervisor review before proceeding

#### Output files
- data/raw/compartments_human.tsv
- data/raw/hpa_rna_consensus.tsv.zip
- data/processed/filter_A_compartments.tsv
- data/processed/filter_A_passed.tsv
- data/processed/filter_B_expression.tsv
- data/processed/filter_B_passed.tsv
- data/processed/network_filtered.tsv

---

---

## Entry 7 — 2026-08-29

### Stage 5 — Context-Specific Annotation (COMPARTMENTS + HPA + IID complete)

#### Supervisor-confirmed parameters (meeting 2026-08-15)
- STRING combined score: ≥ 0.400
- COMPARTMENTS confidence score: ≥ 2.0
- Human Protein Atlas retinal expression: nTPM > 0

#### Important methodological change
Per supervisor guidance, the approach changed from hard filtering
to annotation. All 9,835 STRING interactions are retained in the
master evidence table. Each database adds annotation columns rather
than removing interactions. Final interaction selection will be made
after all evidence layers are integrated.

#### Filter A — COMPARTMENTS (rerun with score ≥ 2.0)

| Property | Value |
|----------|-------|
| Database | COMPARTMENTS (Jensen Lab) |
| File | human_compartment_integrated_full.tsv |
| URL | https://download.jensenlab.org/ |
| Access date | 2026-08-13 |
| Score cutoff | ≥ 2.0 (confirmed by supervisor 2026-08-15) |
| Broad GO terms excluded | Yes |

Results:
- Interactions with shared specific compartment: 9,751 / 9,835
- Interactions without compartment overlap: 84
- Output: data/processed/compartments_interaction_annotation_t2.tsv

#### Filter B — Human Protein Atlas (retinal expression)

| Property | Value |
|----------|-------|
| Database | Human Protein Atlas v25.1 |
| File | rna_tissue_consensus.tsv |
| URL | https://www.proteinatlas.org/about/download |
| Access date | 2026-08-13 |
| Expression cutoff | nTPM > 0 |

Results:
- Both proteins expressed in retina: 9,202 / 9,835
- Output: data/processed/hpa_retina_interaction_annotation.tsv

#### Filter C — IID (Integrated Interactions Database)

| Property | Value |
|----------|-------|
| Database | Integrated Interactions Database (IID) |
| Dataset | Human annotated protein-protein interactions |
| URL | https://iid.ophid.utoronto.ca |
| Access date | 2026-08-29 |
| Matching method | Exact undirected gene-pair matching |

Results:
- IID exact matches: 2,293 / 9,835 (23.31%)
- Experimental support: 1,527 interactions
- NOT_FOUND: not treated as evidence that interaction is false
- Output: data/processed/iid_interaction_annotation.tsv

#### Master evidence table

All three annotation layers integrated into one file.
All 9,835 STRING interactions retained.

| Evidence layers | Interactions |
|----------------|--------------|
| All 3 (COMPARTMENTS + HPA + IID) | 2,240 |
| 2 layers | 6,961 |
| 1 layer | 604 |
| 0 layers | 30 |

Output: data/processed/context_specific_evidence_master.tsv

#### QC example — MERTK–GAS6
- STRING score: 0.999
- COMPARTMENTS: PASS (shared localisation)
- HPA retina: MERTK 15.4 nTPM, GAS6 5.8 nTPM — both expressed
- IID: EXPERIMENTAL (2 methods, 2 PMIDs)
- Evidence layers: 3/3

#### Pending
- TissueNet v3 tissue-specific interaction evidence
- Functional annotation
- Pathway enrichment analysis
- PathLinker

### Output files
| File | Description |
|------|-------------|
| data/processed/compartments_interaction_annotation_t2.tsv | COMPARTMENTS annotation |
| data/processed/hpa_retina_interaction_annotation.tsv | HPA retinal expression annotation |
| data/processed/iid_interaction_annotation.tsv | IID annotation |
| data/processed/context_specific_evidence_master.tsv | Master evidence table |

---
