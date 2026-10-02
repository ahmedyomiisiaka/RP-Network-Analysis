# Methodology — RP Network Analysis

**Project:** Network-Based Analysis of Signalling Pathways in Retinitis Pigmentosa  
**Last updated:** 2026-08-30

---

# 1. RP Gene Collection

Retinitis pigmentosa (RP)-associated genes were collected and curated from
RetiGene and Rivolta et al. (2025).

The complete RP catalogue contained 114 entries:

- 112 protein-coding genes
- 2 loci

The two locus entries were retained in the catalogue for provenance but were
excluded from protein-protein interaction analysis because they cannot be
submitted as individual protein identifiers.

The working protein-level RP gene set is stored in:

`data/processed/rp_genes_for_network.csv`

---

# 2. Apoptosis Reference Dataset

Apoptosis-related proteins were collected earlier from Reactome and KEGG.

Sources included:

- Reactome apoptosis-related pathway data
- KEGG pathway hsa04210 (Apoptosis)

The prepared datasets contain:

- 262 apoptosis-associated proteins in the combined reference set
- 40 proteins occurring in both Reactome and KEGG

These datasets were prepared as potential downstream reference/target sets.

However, following supervisor guidance, apoptosis-target definition and
PathLinker analysis were paused while the interaction network and
context-specific annotation were completed.

The exact use of the apoptosis reference proteins in PathLinker will therefore
be confirmed before source-to-target reconstruction is performed.

---

# 3. Protein-Protein Interaction Network Construction

The STRING database was used to construct the candidate protein-protein
interaction network from the RP protein-coding gene set.

**Database:** STRING  
**Species:** Homo sapiens  
**NCBI taxonomy ID:** 9606  
**Minimum STRING combined interaction score:** >= 0.400

The low-to-medium confidence threshold of 0.400 was retained following
supervisor guidance because subsequent biological-context evidence was used to
evaluate the candidate interactions.

The resulting STRING network contained:

- 9,835 unique undirected interactions
- 4,132 unique proteins
- 105 RP seed proteins represented in STRING
- 4,027 additional interacting proteins

Seven RP input entries were not represented as STRING query/seed proteins in
the retrieved interaction network.

The main STRING interaction table is:

`data/processed/ppi_interactions.tsv`

For each interaction, STRING evidence fields were retained, including the
combined interaction score and available evidence-channel scores.

---

# 4. Context-Specific Interaction Annotation

The STRING network represents a broad candidate interaction network.

To evaluate biological relevance in the retinal context, every STRING
interaction was progressively annotated using several independent biological
databases.

The evidence layers used were:

1. COMPARTMENTS — subcellular localisation
2. Human Protein Atlas — retinal RNA expression
3. Integrated Interactions Database (IID) — independent PPI evidence
4. TissueNet v3 — additional PPI evidence

The analysis followed an annotation-first strategy.

All 9,835 STRING interactions were retained in the complete master evidence
table while evidence from each database was added as additional columns.

Final retinal-context filtering was performed only after the evidence layers
had been integrated.

---

## 4.1 COMPARTMENTS — Subcellular Co-localisation

Subcellular localisation information was obtained from the COMPARTMENTS human
all-channels-integrated dataset.

**Database:** COMPARTMENTS  
**Website:** https://compartments.jensenlab.org/  
**Local analysis file:** `compartments_human.tsv`  
**Dataset:** Human, all channels integrated  
**Confidence cutoff:** >= 2.0

STRING Ensembl protein identifiers were converted from the form:

`9606.ENSP00000295408`

to:

`ENSP00000295408`

before matching to the COMPARTMENTS dataset.

Only COMPARTMENTS localisation annotations with confidence score >= 2.0 were
considered.

### Informative localisation criterion

Very broad Gene Ontology Cellular Component parent terms were not considered
sufficient evidence of meaningful protein co-localisation.

Generic high-level terms such as:

- `cell` (GO:0005623)
- `cell part` (GO:0044464)
- `cellular_component`
- `cellular anatomical entity`
- broad intracellular anatomical terms
- broad organelle parent categories

were excluded from establishing localisation overlap.

Generic `Membrane` annotation alone was also not considered sufficient
evidence of specific co-localisation.

For each STRING interaction, the informative localisation annotations of
protein A and protein B were compared.

Interactions were classified as:

- `PASS` — both proteins shared at least one informative localisation
- `NO_OVERLAP` — both proteins had usable localisation information but no
  informative shared localisation
- `UNKNOWN` — localisation information was insufficient for one or both
  proteins

The complete COMPARTMENTS annotation retained all original STRING
interactions.

Final audited COMPARTMENTS result:

- PASS: 9,751
- NO_OVERLAP: 71
- UNKNOWN: 13
- Total: 9,835

The complete annotation table is:

`data/processed/compartments_interaction_annotation_t2.tsv`

The PASS-only convenience subset is:

`data/processed/compartments_shared_t2.tsv`

### COMPARTMENTS QC

An alternative rerun produced 9,805 PASS interactions because broad terms
including `cell`, `cell part`, and generic `Membrane` were allowed to
establish localisation overlap.

Comparison with the audited analysis showed that 54 interactions changed
classification:

- 51: NO_OVERLAP -> PASS
- 3: UNKNOWN -> PASS

Because these changes were caused by non-specific localisation terms, the
9,805-PASS rerun was rejected.

The authoritative COMPARTMENTS result is therefore:

`9,751 PASS / 71 NO_OVERLAP / 13 UNKNOWN`

---

## 4.2 Human Protein Atlas — Retinal Expression

Retinal RNA-expression evidence was obtained from the Human Protein Atlas RNA
tissue consensus dataset.

**Database:** Human Protein Atlas  
**Website:** https://www.proteinatlas.org/  
**Dataset:** RNA tissue consensus  
**Local analysis file:** `rna_tissue_consensus.tsv`  
**Tissue:** retina  
**Expression measurement:** nTPM  
**Expression criterion:** nTPM > 0

For every STRING interaction, the retinal-expression values of both
participating proteins were assigned where available.

An interaction was considered retinal-expression supported when:

`retina_nTPM_A > 0`

AND

`retina_nTPM_B > 0`

Interactions were classified as:

- `BOTH_EXPRESSED`
- `BELOW_THRESHOLD`
- `UNKNOWN`

Final HPA result:

- BOTH_EXPRESSED: 9,202
- BELOW_THRESHOLD: 437
- UNKNOWN: 196

The HPA annotation table is:

`data/processed/hpa_retina_interaction_annotation.tsv`

Quality control confirmed that no interaction classified as
`BOTH_EXPRESSED` contained an nTPM value <= 0.

---

## 4.3 Integrated Interactions Database (IID)

The Integrated Interactions Database was used as an independent source of
protein-protein interaction evidence.

**Database:** Integrated Interactions Database (IID)  
**Website:** https://iid.ophid.utoronto.ca/  
**Dataset:** Human annotated PPIs  
**Access date:** 2026-08-29

Exact undirected gene-pair matching was performed between STRING and IID.

For example:

`MERTK -- GAS6`

and:

`GAS6 -- MERTK`

were treated as the same interaction.

IID evidence fields retained included:

- interaction presence/absence
- evidence type
- experimental evidence
- predicted evidence
- orthology evidence
- experimental methods
- PubMed identifiers
- source interaction databases
- number of experimental methods
- number of experimental publications
- detection type

Absence from IID was classified as `NOT_FOUND`.

`NOT_FOUND` was not interpreted as evidence that a STRING interaction is
biologically false because databases differ in coverage and evidence sources.

Final IID result:

- IID exact matches: 2,293 / 9,835
- IID coverage of STRING interactions: 23.31%
- Experimental support: 1,527
- Predicted-only support: 625
- Orthology-only support: 104
- Predicted + orthology support: 37
- NOT_FOUND: 7,542

The IID annotation table is:

`data/processed/iid_interaction_annotation.tsv`

IID was used as additional evidence and was not used as a mandatory
retinal-context exclusion criterion.

---

## 4.4 TissueNet v3 — Additional PPI Evidence

TissueNet v3 was used as an additional source of protein-protein interaction
evidence.

**Database:** TissueNet v3  
**Website:** https://netbio.bgu.ac.il/tissuenet  
**PPI file:** `PPI.csv`  
**Additional downloaded files:**  
- `hpa_expression_ptpm.csv`
- `gtex_v8_tpm.csv`

**Access date:** 2026-08-29

The TissueNet PPI dataset uses Ensembl gene identifiers (ENSG), whereas the
STRING-derived network was primarily represented by gene symbols.

An identifier-mapping step was therefore performed before PPI matching.

Gene-symbol to Ensembl-gene mapping used exact mappings from:

1. Human Protein Atlas
2. GTEx v8 as a secondary mapping source

Final identifier-mapping result:

- Network proteins: 4,132
- Resolved: 4,107
- Unmapped: 23
- Ambiguous: 2
- Overall mapping coverage: 99.39%

For the RP seed proteins:

- RP seed proteins: 105
- Resolved: 105
- Mapping coverage: 100%

Interactions were then compared with TissueNet as undirected ENSG-ENSG pairs.

### TissueNet full STRING-network result

Of the 9,835 STRING interactions:

- 9,767 were testable after identifier mapping
- 929 were found in TissueNet
- 8,838 were not found
- 59 had unresolved mapping
- 9 had ambiguous mapping

Among testable interactions, 9.51% were found in TissueNet.

### Important interpretation

TissueNet was not used as a direct retina-expression database in this
analysis.

The downloaded TissueNet HPA and GTEx expression matrices did not contain an
explicit retina tissue column.

Therefore, direct retinal-expression evidence was provided by the Human
Protein Atlas retina dataset described above.

TissueNet `FOUND` status was retained as additional PPI evidence.

TissueNet `NOT_FOUND` status was not interpreted as proof that an interaction
is biologically absent.

The TissueNet annotation table is:

`data/processed/tissuenet_interaction_annotation.tsv`

---

# 5. Integrated Context-Evidence Table

COMPARTMENTS, HPA, IID, and TissueNet evidence were integrated into one
interaction-level master table.

The complete table contains all 9,835 original STRING interactions.

The four positive evidence indicators are:

- COMPARTMENTS support
- HPA retina support
- IID support
- TissueNet support

The four-layer evidence counts in the complete STRING network were:

- COMPARTMENTS: 9,751
- HPA retina: 9,202
- IID: 2,293
- TissueNet: 929

Number of positive context evidence layers:

- 0 layers: 30 interactions
- 1 layer: 590 interactions
- 2 layers: 6,946 interactions
- 3 layers: 1,383 interactions
- 4 layers: 886 interactions

The complete integrated evidence table is:

`data/processed/context_specific_evidence_master_tissuenet.tsv`

The number of evidence layers is used as a descriptive annotation only and is
not interpreted as a biological probability or quantitative interaction
confidence score.

---

# 6. Final Retinal Context-Specific Network

The final retinal context-specific network was defined using the two primary
biological context criteria agreed for this stage:

1. COMPARTMENTS status = `PASS`
   - COMPARTMENTS score >= 2.0
   - at least one informative shared localisation

2. Human Protein Atlas retinal expression
   - protein A retina nTPM > 0
   - protein B retina nTPM > 0

IID and TissueNet were retained as additional independent PPI evidence layers.

They were not mandatory inclusion criteria because absence from either
database does not demonstrate biological absence of an interaction.

Final result:

- Original STRING interactions: 9,835
- Final retinal-context interactions: 9,149
- Retained percentage: 93.02%
- Unique proteins: 3,748
- RP seed proteins retained: 105 / 105

Interactions excluded from the retinal-context network:

- 602 failed HPA retinal-expression criterion only
- 53 failed COMPARTMENTS criterion only
- 31 failed both COMPARTMENTS and HPA retina
- Total excluded: 686

Additional PPI evidence within the final 9,149 retinal interactions:

- IID supported: 2,240
- TissueNet supported: 898
- Supported by both IID and TissueNet: 886

The final retinal context-specific network is:

`data/processed/retinal_context_specific_network.tsv`

The excluded interactions and their reasons are stored in:

`data/processed/retinal_context_excluded_interactions.tsv`

The original complete 9,835-edge evidence table remains preserved separately.

---

# 7. Representative Quality-Control Example — MERTK–GAS6

The MERTK–GAS6 interaction provides a representative example of evidence
integration.

STRING:

- combined score: 0.999

COMPARTMENTS:

- status: PASS

Human Protein Atlas retina:

- MERTK: 15.4 nTPM
- GAS6: 5.8 nTPM
- status: BOTH_EXPRESSED

IID:

- interaction found
- support status: EXPERIMENTAL
- two experimental methods
- two experimental publications

TissueNet:

- interaction found

Therefore, MERTK–GAS6 satisfies the primary retinal-context criteria and also
has additional independent support from IID and TissueNet.

---

# 8. Reproducibility and Data Provenance

For each external database, the analysis records where available:

- database name
- organism
- input dataset/file
- identifier system
- access/download date
- thresholds
- matching procedure
- annotation rules
- output filename

Large external raw database files are not treated as project-generated
results.

Processed interaction-level annotation tables are retained separately from
the raw downloaded database resources.

The main Stage 5 outputs are:

- `compartments_interaction_annotation_t2.tsv`
- `hpa_retina_interaction_annotation.tsv`
- `iid_interaction_annotation.tsv`
- `tissuenet_identifier_mapping_resolved.tsv`
- `tissuenet_interaction_annotation.tsv`
- `context_specific_evidence_master_tissuenet.tsv`
- `retinal_context_specific_network.tsv`
- `retinal_context_excluded_interactions.tsv`

---


# 9. Preliminary Functional Annotation and Enrichment Analysis — Stage 8


> **Stage-order clarification:** This enrichment analysis was performed
> before completion of pathway reconstruction. It is retained as
> preliminary Stage 8 work. The formal workflow proceeds from Stage 5
> context-specific filtering to Stage 6 RP-to-apoptosis pathway
> reconstruction, followed by Stage 7 topology/community analysis and
> Stage 8 enrichment.


Functional annotation and enrichment analysis was initiated using the final
retinal context-specific network generated in Stage 5.

The preliminary Stage 8 enrichment input network contained:

- 9,149 retinal context-specific interactions
- 3,748 unique proteins
- 105 RP seed proteins
- 3,643 non-RP retinal-network interaction partners

Three enrichment input sets were prepared:

1. 105 RP seed proteins
2. 3,643 non-RP retinal-network interaction partners
3. 3,748 proteins representing the complete retinal-context network

The RP seed and partner sets were analysed separately so that biological
processes already represented among known RP-associated genes could be
distinguished from broader functional processes represented by the
retinal-network interaction partners.

---

## 9.1 PANTHER GO Biological Process Enrichment — RP Seed Proteins

Gene Ontology Biological Process enrichment was performed using the
PANTHER Overrepresentation Test.

**Organism:** Homo sapiens  
**Reference:** Homo sapiens, all genes in the PANTHER database  
**Annotation dataset:** GO biological process complete  
**Statistical test:** Fisher's Exact test  
**Multiple-testing correction:** False Discovery Rate (FDR)  
**Significance threshold:** FDR < 0.05

The input consisted of 105 RP seed gene symbols.

All 105 RP input genes were recognized by PANTHER.

Four gene symbols showed multiple PANTHER mappings:

- RP1
- INPP5E
- SAG
- ARHGEF18

The original curated gene symbols were retained rather than manually altering
the identifiers.

The PANTHER result contained 95 significant GO Biological Process terms at
FDR < 0.05:

- 93 overrepresented terms
- 2 underrepresented terms

Strongly enriched biological themes included:

- sensory perception of light stimulus
- visual perception
- retina homeostasis
- photoreceptor cell maintenance
- photoreceptor cell development
- photoreceptor cell differentiation
- phototransduction
- cilium assembly
- cilium organization
- protein localization to cilium

These results provide a functional quality-control check showing that the RP
seed set is strongly enriched for retinal, photoreceptor, light-sensing, and
ciliary biology.

---

## 9.2 PANTHER GO Biological Process Enrichment — Retinal-Network Partners

The second enrichment input consisted of 3,643 non-RP proteins from the final
retinal context-specific interaction network.

PANTHER mapping summary:

- submitted partner genes: 3,643
- uniquely mapped genes: 3,635
- unmapped identifiers: 8
- multiple mapping information reported: 159 identifiers

The downloaded PANTHER result contained 2,105 significant GO Biological
Process terms at FDR < 0.05:

- 2,080 overrepresented terms
- 25 underrepresented terms

Prominent functional themes included:

- RNA processing
- RNA splicing
- spliceosomal mRNA processing
- RNA metabolism
- protein metabolism
- protein catabolism
- gene expression
- nucleotide metabolism
- cellular localization and transport
- ciliary transport and organization

The network-partner enrichment therefore represents a broader functional
landscape than the RP seed enrichment.

---

## 9.3 RP Seed versus Retinal-Network Partner Comparison

Significant GO Biological Process terms from the RP seed and retinal-network
partner analyses were compared by GO identifier.

After excluding the PANTHER `Unclassified` category:

- RP seed GO terms with standard GO IDs: 94
- partner GO terms with standard GO IDs: 2,104
- shared significant GO terms: 82
- RP-seed-specific significant GO terms: 12
- partner-specific significant GO terms: 2,022

Shared biological processes included:

- sensory perception of light stimulus
- visual perception
- retina homeostasis
- cilium assembly
- cilium organization
- photoreceptor cell maintenance
- photoreceptor cell development
- photoreceptor cell differentiation
- phototransduction
- retina development

The RP-seed-specific terms were dominated by specialized photoreceptor
functions including:

- photoreceptor outer-segment organization
- visible-light phototransduction
- G protein-coupled opsin signalling
- transducin-mediated opsin signalling
- vitamin A metabolism
- photoreceptor morphogenesis
- protein localization to the photoreceptor outer segment

The partner-specific enrichment revealed broader functional systems,
particularly:

- RNA splicing
- spliceosomal processing
- mRNA processing
- RNA metabolism
- nucleotide metabolism
- gene expression
- cellular localization
- metabolic processes

The term `partner-specific` means statistically significant in the
retinal-network partner enrichment but not significant in the RP-seed
enrichment under the same analysis settings. It does not mean that the
corresponding process is biologically absent from RP.

---

## 9.4 Preliminary Stage 8 Files Generated

Enrichment input files:

- `rp_seeds_105.txt`
- `retinal_network_partners_3643.txt`
- `all_retinal_network_proteins_3748.txt`

GO Biological Process result files:

- `PANTHER_GO_BP_RP_seeds_raw.txt`
- `PANTHER_GO_BP_RP_seeds_clean.tsv`
- `PANTHER_GO_BP_RP_seeds_significant_FDR05.tsv`
- `PANTHER_GO_BP_retinal_partners_raw.txt`
- `PANTHER_GO_BP_retinal_partners_clean.tsv`
- `PANTHER_GO_BP_retinal_partners_significant_FDR05.tsv`
- `GO_BP_shared_RP_seeds_and_partners.tsv`
- `GO_BP_RP_seed_specific.tsv`
- `GO_BP_partner_specific.tsv`

---

## 9.5 Preliminary Stage 8 Status

Completed so far:

- enrichment input preparation and QC
- GO Biological Process enrichment of RP seed proteins
- GO Biological Process enrichment of retinal-network partners
- RP seed versus retinal-network partner GO Biological Process comparison

Pending:

- GO Molecular Function enrichment
- GO Cellular Component enrichment
- Reactome pathway enrichment
- KEGG pathway enrichment
- functional grouping/module interpretation
- integration of enrichment results with subsequent pathway/network analyses

This preliminary Stage 8 enrichment work is preserved for reference and will be revisited after Stage 6 pathway reconstruction and Stage 7 network-topology analysis.

---

# 10. Next Analytical Phase

Preliminary Stage 8 functional enrichment analysis has been performed.

GO Biological Process enrichment has been completed for RP seed proteins and
retinal-network partners, including comparison of shared and set-specific
significant processes.

When Stage 8 is formally resumed, enrichment analysis may be extended to:

1. GO Molecular Function enrichment;
2. GO Cellular Component enrichment;
3. Reactome pathway enrichment;
4. KEGG pathway enrichment;
5. identification of functional groups/modules in which proteins participate
   collectively;
6. biological interpretation of retinal degeneration, signalling, cell-death,
   ciliary, metabolic, RNA-processing, and other enriched mechanisms;
7. PathLinker/source-to-target reconstruction after functional context has
   been established and reviewed.

---

# 11. Current Project Status

Completed:

- RP gene collection and curation
- STRING PPI network construction
- STRING network audit
- COMPARTMENTS localisation annotation
- HPA retinal-expression annotation
- IID interaction annotation
- TissueNet identifier mapping
- TissueNet PPI annotation
- four-layer context-evidence integration
- final Stage 5 QC audit
- definition of retinal context-specific network
- preliminary Stage 8 enrichment input preparation
- RP-seed GO Biological Process enrichment
- retinal-network-partner GO Biological Process enrichment
- RP-seed versus partner GO Biological Process comparison

Current final retinal network:

- 9,149 interactions
- 3,748 proteins
- 105 RP seed proteins retained

Current preliminary Stage 8 status:

- GO Biological Process analysis complete
- GO Molecular Function pending
- GO Cellular Component pending
- Reactome enrichment pending
- KEGG enrichment pending
- functional grouping/module analysis pending
