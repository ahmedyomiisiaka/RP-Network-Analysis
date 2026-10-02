# Stage 5 — Context-Specific Retinal PPI Filtering

## Purpose

Stage 5 restricts the Stage-4 STRING protein-protein interaction (PPI)
network to interactions supported in a human retinal biological context.

The resulting network is the input network for Stage 6:
RP → apoptosis pathway reconstruction.

---

## Input network

Stage 4 STRING PPI network:

- 9,835 interactions
- 4,132 proteins
- 105 RP seed proteins represented
- Homo sapiens
- STRING minimum combined score: 0.400

Input file:

`00_stage4_input.tsv`

---

## Stage 5 workflow

### 5.1 Human retina expression — Human Protein Atlas

HPA tissue RNA expression was used to determine whether proteins had
expression evidence in human retina.

Output:

`01_hpa_retina_protein_annotation.tsv`

Final protein annotation:

- EXPRESSED: 3,777
- NOT_DETECTED: 282
- UNKNOWN: 73

All 105 RP seeds had HPA retina-tissue expression evidence.

---

### 5.2 Retinal cell-type expression — Human Protein Atlas

HPA single-cell RNA data were used to annotate retinal cell types,
including:

- rod photoreceptors
- cone photoreceptors
- retinal pigment epithelium (RPE)
- retinal amacrine cells
- bipolar cells
- ganglion cells
- horizontal cells

Output:

`02_hpa_retinal_celltype_annotation.tsv`

Rod/cone/RPE evidence was retained as biological context rather than
used as a mandatory filter.

A strict photoreceptor filter would remove RP seeds CLN3 and TOPORS.
TOPORS had RPE expression despite lacking rod/cone signal in this HPA
dataset.

Therefore, absence of expression from an individual HPA retinal cell
type was not interpreted as proof that a protein is biologically
irrelevant to RP.

---

### 5.3 Subcellular localization — COMPARTMENTS

COMPARTMENTS was used to evaluate whether the two proteins of each
interaction had compatible subcellular localization evidence.

A minimum integrated COMPARTMENTS score of 2 was used for localization
annotations. Broad generic cellular-component terms were excluded from
the informative localization set.

Edge classifications:

- PASS: at least one informative shared localization
- NO_OVERLAP: both proteins had usable localization evidence but no
  informative localization overlap
- UNKNOWN: localization information was insufficient for one or both
  endpoints

Output:

`03_compartments_edge_annotation.tsv`

Results:

- PASS: 9,800
- NO_OVERLAP: 25
- UNKNOWN: 10

---

### 5.4 Supporting PPI evidence — IID

The Integrated Interactions Database (IID) was used as an additional
interaction-evidence source.

Output:

`04_iid_edge_annotation.tsv`

Stage-4 interactions:

- EXPERIMENTAL: 1,527
- PREDICTED_ONLY: 662
- ORTHOLOGY_ONLY: 104
- NOT_FOUND: 7,542

NOT_FOUND was treated as absence from the queried IID dataset, not as
evidence that an interaction is biologically absent.

IID was therefore not used as a mandatory exclusion criterion.

---

### 5.4 Supporting PPI evidence — TissueNet

TissueNet was used as an additional PPI/network evidence source.

Identifier mapping:

`05_tissuenet_identifier_mapping_final.tsv`

Interaction annotation:

`06_tissuenet_edge_annotation.tsv`

Results:

- FOUND: 932
- NOT_FOUND: 8,844
- MAPPING_UNRESOLVED: 59

The available TissueNet dataset was not treated as direct
retina-specific evidence. Absence from TissueNet was not used to
exclude an interaction.

---

### 5.5 Pathway context — KEGG

Network proteins were mapped to KEGG pathway annotations.

Output:

`07_kegg_protein_pathway_annotation.tsv`

Protein-level results:

- MAPPED_WITH_PATHWAY: 2,418
- MAPPED_NO_PATHWAY: 1,603
- UNMAPPED: 110
- AMBIGUOUS_PRIMARY_SYMBOL: 1

No RP seed remained unresolved.

KEGG annotations were retained as pathway-context evidence and were not
used as a mandatory interaction filter.

Example:

MERTK maps to KEGG Efferocytosis (hsa04148).

---

### 5.5 Pathway context — Reactome

Network proteins were mapped to Reactome pathways using validated
identifier mapping.

Output:

`08_reactome_protein_pathway_annotation.tsv`

Results:

- MAPPED_WITH_PATHWAY: 3,090
- MAPPED_NO_PATHWAY: 1,019
- MAPPING_UNRESOLVED: 23

Example biological control:

MERTK is associated with Reactome reaction R-HSA-202710:

"MERTK receptor binds ligands (Gas6 or Protein S)"

This supports the role of MERTK/GAS6 in apoptotic-cell clearance and
RPE-related biology. It was not interpreted as evidence that
MERTK/GAS6 directly executes apoptosis.

Reactome annotations were retained as contextual evidence rather than
used as mandatory filters.

---

### 5.6 ProteomicsDB

ProteomicsDB was investigated as an additional protein-level evidence
source.

A retina biological source corresponding to BTO:0001175 was identified.

For the MERTK control, the reviewed UniProt entry Q12866 had
mass-spectrometry evidence in ProteomicsDB.

However, a validated network-wide retina-specific protein extraction
was not obtained using the tested API configuration.

ProteomicsDB was therefore retained as supporting methodological
evidence and was not used as a mandatory filtering criterion.

---

## Evidence integration

All Stage-5 evidence was integrated into a single edge-level master
table:

`09_stage5_master_evidence_table.tsv`

The table contains:

- 9,835 interactions
- 97 columns
- no duplicate STRING interaction pairs

It preserves the original STRING evidence together with retinal,
cell-type, localization, external PPI and pathway annotations.

---

## Candidate filtering rules

Alternative context-filtering strategies were compared before choosing
the final rule.

Output:

`10_candidate_filter_comparison.tsv`

The alternatives included:

- retina RNA only
- photoreceptor RNA only
- primary retinal-cell RNA only
- COMPARTMENTS only
- combinations of expression and localization evidence

This comparison was performed before defining the final network rather
than selecting an arbitrary filter threshold after seeing the result.

---

## Final Stage-5 filtering rule

An interaction was retained when BOTH conditions were satisfied:

1. BOTH endpoint proteins had HPA human-retina expression evidence

AND

2. COMPARTMENTS status was PASS.

In logical form:

`both_retina_expressed == True`

AND

`compartments_status == "PASS"`

HPA retinal cell-type evidence, IID, TissueNet, KEGG, Reactome and
ProteomicsDB were retained as supporting annotations rather than
mandatory exclusion criteria.

This prevents absence from an individual database or single-cell
dataset from being interpreted as biological absence.

---

## Final context-specific retinal network

Final network:

`11_stage5_context_specific_retinal_network.tsv`

Results:

- Stage-4 interactions: 9,835
- retained interactions: 9,184
- excluded interactions: 651
- retention: 93.38%
- retained proteins: 3,764
- retained RP seeds: 105 / 105

Excluded interactions are documented in:

`12_stage5_excluded_interactions.tsv`

Exclusion reasons:

- retina-expression criterion only: 616
- COMPARTMENTS criterion only: 18
- both criteria: 17

---

## Graph quality control

Node-level QC:

`13_stage5_final_network_node_qc.tsv`

The final network contains:

- 3,764 nodes
- 9,184 edges
- 1 connected component
- largest-component coverage: 100%
- 105 / 105 RP seeds in the connected component

Degree statistics:

- mean degree: 4.88
- median degree: 1
- maximum degree: 432

RP-seed degree:

- mean: 97.34
- median: 85
- minimum: 6
- maximum: 432

The degree calculations at this stage are graph quality-control
statistics only. Formal hub/topological prioritization is reserved for
Stage 7.

---

## STRING confidence in final network

Combined-score distribution:

- 0.400–0.499: 3,280 interactions
- 0.500–0.699: 3,168
- 0.700–0.899: 1,573
- 0.900–1.000: 1,163

Minimum score: 0.400
Mean score: 0.616
Median score: 0.556
Maximum score: 0.999

---

## Biological control

The MERTK–GAS6 interaction was retained.

Evidence:

- STRING combined score: 0.999
- MERTK: HPA retina EXPRESSED
- GAS6: HPA retina EXPRESSED
- COMPARTMENTS: PASS
- IID: EXPERIMENTAL
- TissueNet: FOUND
- pathway-context evidence available from KEGG and Reactome

This provides a useful positive control for the evidence-integration
workflow.

---

## Important interpretation

The final network should be described as a:

**retina-context-supported PPI network**

It should NOT be described as a retina-exclusive interactome.

Likewise:

- NOT_FOUND in IID/TissueNet does not mean that an interaction is false.
- absence from one HPA retinal cell type does not prove biological absence.
- pathway membership does not prove that a protein executes apoptosis.
- STRING confidence is interaction evidence, not a direct measure of
  disease causality.

---

## Stage 5 final files

| File | Purpose |
|---|---|
| `00_stage4_input.tsv` | Frozen Stage-4 input |
| `01_hpa_retina_protein_annotation.tsv` | HPA retina expression |
| `02_hpa_retinal_celltype_annotation.tsv` | HPA retinal cell types |
| `03_compartments_edge_annotation.tsv` | Subcellular localization |
| `04_iid_edge_annotation.tsv` | IID PPI evidence |
| `05_tissuenet_identifier_mapping_final.tsv` | TissueNet identifier mapping |
| `06_tissuenet_edge_annotation.tsv` | TissueNet interaction evidence |
| `07_kegg_protein_pathway_annotation.tsv` | KEGG pathway context |
| `08_reactome_protein_pathway_annotation.tsv` | Reactome pathway context |
| `09_stage5_master_evidence_table.tsv` | Integrated evidence master table |
| `10_candidate_filter_comparison.tsv` | Comparison of candidate filtering rules |
| `11_stage5_context_specific_retinal_network.tsv` | FINAL Stage-5 network |
| `12_stage5_excluded_interactions.tsv` | Excluded edges and reasons |
| `13_stage5_final_network_node_qc.tsv` | Final graph QC |
| `14_stage5_methodology_summary.txt` | Concise methodology summary |

Debug/checkpoint Reactome files are not part of the final
supervisor-facing Stage-5 output.

---

## Input to Stage 6

The primary Stage-6 network input is:

`11_stage5_context_specific_retinal_network.tsv`

Stage 6 will reconstruct paths between:

**Sources:** RP disease genes

and

**Targets:** validated apoptosis-related genes.

Stage 6 is pathway reconstruction.

Functional enrichment and formal hub/community analysis are later
stages and should not be confused with Stage 6.
