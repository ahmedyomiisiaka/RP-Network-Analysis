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
