# Database Sources and Data Provenance

This document records every external database used in the RP network
analysis: what information was extracted, how it was accessed, what
parameters were applied, and what output was produced.

---

## STRING

| Property | Value |
|----------|-------|
| Database | STRING |
| Version | v12 |
| URL | https://string-db.org |
| Access date | 2026-08-13 |
| Species | Homo sapiens (NCBI taxonomy ID: 9606) |
| Endpoint | /api/tsv/interaction_partners |
| Confidence threshold | ≥ 0.400 (confirmed by supervisor) |
| Query method | One protein at a time |

**Information retained:** STRING protein identifiers, preferred gene names,
combined interaction score, individual evidence channel scores (escore,
dscore, tscore, ascore, nscore, fscore, pscore), RP seed status.

**Output:** `data/processed/ppi_interactions.tsv` (9,835 unique edges)

---

## COMPARTMENTS

| Property | Value |
|----------|-------|
| Database | COMPARTMENTS (Jensen Lab, University of Copenhagen) |
| URL | https://compartments.jensenlab.org |
| Download URL | https://download.jensenlab.org/human_compartment_integrated_full.tsv |
| Access date | 2026-08-13 |
| Score cutoff | ≥ 2.0 (confirmed by supervisor 2026-08-15) |

**What the database contains:** Subcellular localisation evidence for human
proteins from manually curated literature, high-throughput screens,
automatic text mining, and sequence-based prediction. Scores range
from 1 (weak) to 5 (strongest).

**What was extracted:** For each protein, the specific subcellular
compartments with confidence score ≥ 2.0. Broad GO parent terms
(e.g. cellular_component, organelle) were excluded.

**How it was used:** For each STRING interaction, the compartment sets
of both proteins were compared. Shared specific compartments indicate
biological co-localisation is plausible. Result added as annotation column.

**Interaction classifications:**
- co_localised: shared specific compartment found
- no_overlap: no shared specific compartment
- unknown: one or both proteins not in COMPARTMENTS

**Output:** `data/processed/compartments_interaction_annotation_t2.tsv`

**Citation:** Binder JX, et al. (2014). COMPARTMENTS: unification and
visualization of protein subcellular localization evidence.
Database, bau012. https://doi.org/10.1093/database/bau012

---

## Human Protein Atlas (HPA)

| Property | Value |
|----------|-------|
| Database | Human Protein Atlas |
| Version | v25.1 |
| URL | https://www.proteinatlas.org |
| Dataset | RNA tissue consensus (rna_tissue_consensus.tsv) |
| Access date | 2026-08-13 |
| Tissue | Retina |
| Expression cutoff | nTPM > 0 (confirmed by supervisor) |

**What the database contains:** RNA expression data (nTPM values) for
human genes across 51 tissues including retina.

**What was extracted:** nTPM expression value in retinal tissue for
each protein in the network.

**How it was used:** For each STRING interaction, retinal nTPM values
for both proteins were retrieved. An interaction is considered retinally
supported if both proteins have nTPM > 0. Result added as annotation column.

**Interaction classifications:**
- both_expressed: both proteins have retinal nTPM > 0
- one or both not expressed: nTPM = 0
- unknown: protein not found in HPA

**Output:** `data/processed/hpa_retina_interaction_annotation.tsv`

**Citation:** Uhlén M, et al. (2015). Tissue-based map of the human
proteome. Science, 347(6220):1260419.
https://doi.org/10.1126/science.1260419

---

## Integrated Interactions Database (IID)

| Property | Value |
|----------|-------|
| Database | Integrated Interactions Database (IID) |
| URL | https://iid.ophid.utoronto.ca |
| Access date | 2026-08-29 |
| Dataset | Human annotated protein-protein interactions |
| Matching method | Exact undirected gene-pair matching |

**What the database contains:** Human protein-protein interactions with
experimental, predicted, and orthology-based evidence. Each interaction
records evidence type, experimental methods, PubMed IDs, and source databases.

**What was extracted:** For each STRING interaction pair, checked whether
an exact matching undirected gene pair exists in IID. If found, retained
evidence type, number of experimental methods, number of PMIDs, and
detection type.

**How it was used:** Added as annotation column. Absence from IID
(NOT_FOUND) is not treated as evidence that the STRING interaction
is false.

**Interaction classifications:**
- EXPERIMENTAL: direct experimental evidence in IID
- PREDICTED_ONLY: computational prediction only
- ORTHOLOGY_ONLY: orthology-based evidence only
- PREDICTED_AND_ORTHOLOGY: both predicted and orthology
- NOT_FOUND: no exact match in IID

**Results:** 2,293 of 9,835 STRING interactions matched (23.31%).
1,527 interactions have experimental support.

**Output:** `data/processed/iid_interaction_annotation.tsv`

**Citation:** Kotlyar M, et al. (2019). IID 2018 update:
context-specific physical protein-protein interactions.
Nucleic Acids Res, 47(D1):D581-D589.
https://doi.org/10.1093/nar/gky1037

---

## TissueNet v3

| Property | Value |
|----------|-------|
| Database | TissueNet v3 |
| URL | https://netbio.bgu.ac.il/tissuenet |
| Status | Pending integration |

**Purpose:** Provide tissue-specific interaction scores to further
evaluate whether STRING interactions are supported in retinal tissue.

**Output (planned):** Additional annotation column in
`data/processed/context_specific_evidence_master.tsv`

**Citation:** Basha O, et al. (2015). TissueNet: The database of
human tissue protein-protein interactions.
Nucleic Acids Res, 43(D1):D424-D429.

---

## Master Evidence Table

**File:** `data/processed/context_specific_evidence_master.tsv`

Each row = one original STRING interaction.
All 9,835 STRING interactions retained.

**Current evidence layers:**
1. STRING interaction evidence (score, individual channel scores)
2. COMPARTMENTS subcellular localisation (score ≥ 2.0)
3. HPA retinal expression (nTPM > 0)
4. IID interaction evidence
5. TissueNet (pending)

**No final filtering has been applied.** All interactions are retained
with their evidence annotations. Biological filtering decisions will
be made after all evidence layers are complete.


---

## PANTHER / Gene Ontology — Preliminary Stage 8 Functional Enrichment

**Purpose:** Functional enrichment and overrepresentation analysis of
RP seed proteins and retinal-network interaction partners.

**Resource:** PANTHER Classification System  
**Website:** https://pantherdb.org/  
**Analysis:** PANTHER Overrepresentation Test  
**Organism:** Homo sapiens  
**Reference list:** Homo sapiens — all genes in the PANTHER database  
**Reference-list size reported by PANTHER:** 20,580 genes  
**Statistical test:** Fisher's Exact test  
**Multiple-testing correction:** False Discovery Rate (FDR)  
**Significance threshold used:** FDR < 0.05  

**GO ontology release reported in downloaded result:** 2026-04-28  
**PANTHER Overrepresentation Test release reported in downloaded result:** 2024-08-07  
**Project access / analysis date:** 2026-08-29 to 2026-08-30  

### Gene sets analysed so far

#### RP seed proteins

Input:

`rp_seeds_105.txt`

- curated RP seed genes submitted: 105
- all 105 RP gene symbols recognized by PANTHER
- four symbols had multiple PANTHER mappings:
  - RP1
  - INPP5E
  - SAG
  - ARHGEF18

GO Biological Process result:

- 95 significant terms at FDR < 0.05
- 93 overrepresented
- 2 underrepresented

Main biological themes included retinal function, photoreceptor biology,
phototransduction, visual perception, retina homeostasis, and ciliary biology.

#### Retinal-network interaction partners

Input:

`retinal_network_partners_3643.txt`

- submitted network-partner genes: 3,643
- uniquely mapped: 3,635
- unmapped: 8
- multiple mapping information reported for 159 identifiers

GO Biological Process result:

- 2,105 significant terms at FDR < 0.05
- 2,080 overrepresented
- 25 underrepresented

Prominent biological themes included RNA processing, RNA splicing,
spliceosomal processing, metabolism, gene expression, protein processing,
cellular localization, transport, and ciliary processes.

### GO Biological Process comparison

After excluding the PANTHER `Unclassified` category:

- RP seed terms with GO IDs: 94
- network-partner terms with GO IDs: 2,104
- shared significant terms: 82
- RP-seed-specific significant terms: 12
- partner-specific significant terms: 2,022

### Files

Raw PANTHER exports:

- `PANTHER_GO_BP_RP_seeds_raw.txt`
- `PANTHER_GO_BP_retinal_partners_raw.txt`

Processed results:

- `PANTHER_GO_BP_RP_seeds_clean.tsv`
- `PANTHER_GO_BP_RP_seeds_significant_FDR05.tsv`
- `PANTHER_GO_BP_retinal_partners_clean.tsv`
- `PANTHER_GO_BP_retinal_partners_significant_FDR05.tsv`
- `GO_BP_shared_RP_seeds_and_partners.tsv`
- `GO_BP_RP_seed_specific.tsv`
- `GO_BP_partner_specific.tsv`

### Interpretation rule

GO enrichment identifies functions occurring more frequently in the submitted
gene set than expected relative to the selected reference background.

Significant enrichment does not by itself demonstrate causality or direct
pathway activation.

GO terms are hierarchical and may represent overlapping parent and child
biological concepts; therefore, related significant GO terms are interpreted
as functional themes rather than as fully independent biological findings.

### Current Preliminary Stage 8 Status

Completed:

- GO Biological Process — RP seed proteins
- GO Biological Process — retinal-network partners
- RP seed versus partner GO Biological Process comparison

Pending:

- GO Molecular Function
- GO Cellular Component
- Reactome pathway enrichment
- KEGG pathway enrichment
- functional grouping/module analysis

---
