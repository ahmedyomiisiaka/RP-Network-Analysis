# Methodology

## Analysis of Signaling Pathways in Retinitis Pigmentosa and Identification of Potential Targets for Genome Editing

## 1. Overview

This project applies a network-based systems biology approach to investigate
proteins associated with retinitis pigmentosa (RP), their protein-protein
interaction partners, and the biological context in which these interactions
may occur.

The current workflow consists of:

1. Collection and curation of RP-associated genes.
2. Construction of a protein-protein interaction (PPI) network using STRING.
3. Annotation of protein interactions with subcellular localisation evidence
   from COMPARTMENTS.
4. Annotation of retinal expression using the Human Protein Atlas (HPA).
5. Addition of independent PPI evidence from the Integrated Interactions
   Database (IID).
6. Addition of tissue-specific interaction evidence from TissueNet.
7. Functional annotation and pathway enrichment analysis.
8. Identification of functional groups/modules and biologically relevant
   pathways.
9. Reconstruction and prioritisation of paths between RP-associated proteins
   and selected downstream biological targets using PathLinker.

The main principle of the context-specific analysis is to retain the original
interaction network while progressively adding independent evidence to each
interaction. Interactions are therefore annotated with evidence categories
before a final context-specific filtering decision is made.


## 2. RP Gene Collection and Curation

### 2.1 Data sources

RP-associated genes were collected and curated from the project gene sources,
including:

- RetiGene
- Rivolta et al. (2025)

The combined RP gene list was reviewed to distinguish protein-coding genes
from non-protein-coding loci or entries that could not be used directly for
protein-protein interaction analysis.

The working RP gene set used for network construction is stored in:

`data/processed/rp_genes_for_network.csv`

The complete curated gene information is stored separately to preserve the
provenance of the original RP gene collection.


## 3. Protein-Protein Interaction Network Construction

### 3.1 STRING database

The RP protein interaction network was constructed using STRING for:

- Organism: Homo sapiens
- NCBI taxonomy identifier: 9606

The RP-associated protein-coding genes were used as seed/query proteins.

A minimum STRING combined interaction score of:

`0.400`

was used.

This threshold was confirmed during discussion with the thesis supervisor.

The purpose of using this threshold was to retain a sufficiently broad set of
potential interaction partners before applying biological context information
such as retinal expression, subcellular localisation, and tissue-specific
interaction evidence.

### 3.2 STRING information retained

For each interaction, the following information was retained where available:

- STRING identifier for protein A
- STRING identifier for protein B
- preferred protein/gene name for protein A
- preferred protein/gene name for protein B
- NCBI taxonomy identifier
- STRING combined score
- neighbourhood score
- gene fusion score
- phylogenetic co-occurrence score
- co-expression/association score
- experimental evidence score
- curated database evidence score
- text-mining evidence score
- query gene
- RP seed status of each interacting protein

The main STRING interaction table is:

`data/processed/ppi_interactions.tsv`


## 4. STRING Network Audit

The RP working input contained 112 genes.

Of these, 105 were represented as STRING seed proteins in the resulting
interaction table.

Seven input entries were not represented as STRING seed proteins in the
retrieved network:

- CFAP418
- RNU4-2
- RNU6-1
- RNU6-2
- RNU6-8
- RNU6-9
- SAXO6

The resulting network contained:

- 9,835 unique interactions
- 4,132 unique network proteins
- 105 RP seed proteins represented in the STRING network

The minimum observed STRING score was 0.400 and the maximum was 0.999.

An undirected interaction-pair audit was performed to ensure that the same
protein pair had not been duplicated simply because of reversed interaction
orientation. All 9,835 interactions represented unique undirected pairs.


## 5. Context-Specific Interaction Annotation

The STRING network represents a broad interaction network. Not every
interaction is necessarily biologically relevant in retinal tissue.

Therefore, additional biological databases are being integrated to provide
context-specific evidence.

The current context-specific evidence sources are:

1. COMPARTMENTS — subcellular localisation
2. Human Protein Atlas — retinal expression
3. Integrated Interactions Database (IID) — independent PPI evidence
4. TissueNet — tissue-specific PPI evidence (planned)

The context-specific analysis is performed at the interaction level.

Each row represents one STRING protein-protein interaction, while additional
columns describe evidence associated with that interaction.


## 6. COMPARTMENTS Subcellular Localisation Annotation

### 6.1 Purpose

Two proteins are more biologically plausible interaction partners when they
have evidence supporting localisation to compatible or shared subcellular
locations.

COMPARTMENTS was therefore used to annotate the subcellular localisation of
proteins in the STRING network.

Database:

COMPARTMENTS

Website:

https://compartments.jensenlab.org/

### 6.2 Identifier matching

STRING protein identifiers have the format:

`9606.ENSP00000295408`

whereas the COMPARTMENTS human dataset uses Ensembl protein identifiers such
as:

`ENSP00000295408`

The `9606.` species prefix was therefore removed from the STRING identifiers
before matching them to COMPARTMENTS.

Matching was performed primarily using Ensembl protein identifiers rather
than relying only on gene symbols.

### 6.3 COMPARTMENTS confidence threshold

The supervisor-confirmed COMPARTMENTS threshold was:

`score >= 2.0`

All localisation annotations below this threshold were excluded from the
localisation-overlap assessment.

### 6.4 Removal of broad ontology terms

Very broad Gene Ontology Cellular Component terms can produce apparent
co-localisation without providing useful information about where proteins
could interact.

Broad/non-informative parent terms were therefore excluded before evaluating
localisation overlap.

The purpose of this step was to ensure that a PASS classification reflected
shared informative localisation evidence rather than only membership in a
very general cellular-component category.

### 6.5 Interaction classification

For each STRING interaction, the usable GO Cellular Component annotations of
protein A and protein B were compared.

Interactions were assigned one of three statuses:

#### PASS

Both proteins had usable COMPARTMENTS annotations and shared at least one
retained GO Cellular Component term.

#### NO_OVERLAP

Both proteins had usable localisation information, but no retained
localisation term was shared.

#### UNKNOWN

At least one of the interacting proteins lacked sufficient usable
COMPARTMENTS localisation information under the selected criteria.

### 6.6 Information retained

The annotation table contains information including:

- number of usable compartments for protein A
- number of usable compartments for protein B
- localisation terms for each protein
- shared GO Cellular Component identifiers
- shared compartment names
- number of shared compartments
- maximum shared localisation confidence
- interaction localisation status
- database name
- COMPARTMENTS threshold
- database access/documentation date

### 6.7 Result

Using the supervisor-confirmed COMPARTMENTS score threshold of 2.0:

- Total STRING interactions: 9,835
- COMPARTMENTS-supported interactions: 9,751

The complete COMPARTMENTS annotation is stored in:

`data/processed/compartments_interaction_annotation_t2.tsv`

At this stage, lack of shared localisation evidence is recorded as an
annotation rather than being used alone to permanently delete an interaction.


## 7. Human Protein Atlas Retinal Expression Annotation

### 7.1 Purpose

An interaction relevant to retinal disease should have evidence that its
participating proteins are expressed in retinal tissue.

Retinal expression was therefore investigated using RNA tissue consensus data
from the Human Protein Atlas.

Database:

Human Protein Atlas (HPA)

Website:

https://www.proteinatlas.org/

Dataset:

RNA tissue consensus

Tissue:

Retina

### 7.2 Expression measurement

Expression was represented using normalized transcripts per million:

`nTPM`

The supervisor-confirmed criterion for evidence of retinal expression was:

`retina nTPM > 0`

### 7.3 Interaction-level annotation

For every STRING interaction, retinal expression values were assigned to
protein A and protein B where corresponding HPA records were available.

The interaction was then classified according to whether both proteins had
detectable retinal expression.

The main categories were:

- `BOTH_EXPRESSED`
- `BELOW_THRESHOLD`
- `UNKNOWN`

`BOTH_EXPRESSED` indicates that both interacting proteins had retinal
expression greater than zero.

`BELOW_THRESHOLD` indicates that at least one mapped protein did not satisfy
the selected expression criterion.

`UNKNOWN` indicates that sufficient retinal expression information could not
be assigned to one or both proteins.

### 7.4 Result

The STRING network contained 9,835 interactions.

Using retinal nTPM > 0:

- 9,202 interactions had both proteins expressed in retina.

The HPA retinal expression annotation is stored in:

`data/processed/hpa_retina_interaction_annotation.tsv`


## 8. Integrated Interactions Database (IID) Annotation

### 8.1 Purpose

The Integrated Interactions Database was used to determine whether STRING
interaction pairs also had independent interaction evidence in another
integrated PPI resource.

Database:

Integrated Interactions Database (IID)

Website:

https://iid.ophid.utoronto.ca/

The human annotated PPI dataset was downloaded and used for interaction-level
comparison.

### 8.2 Interaction matching

STRING and IID protein interactions were treated as undirected interactions.

For example:

`MERTK -- GAS6`

and:

`GAS6 -- MERTK`

represent the same protein pair.

Canonical interaction keys were therefore created by normalising the
interacting gene symbols and sorting the two symbols before matching.

Exact protein/gene-pair matching was then performed between the 9,835 STRING
interactions and the IID human interaction dataset.

### 8.3 Evidence retained from IID

Where available, the following information was retained:

- whether the interaction was found in IID
- IID support category
- evidence type
- experimental evidence
- predicted evidence
- orthology evidence
- experimental detection methods
- PubMed identifiers
- source interaction databases
- number of experimental methods
- number of experimental publications
- number of predicted publications
- total publication count
- detection type
- database name
- access date

### 8.4 Interpretation of IID absence

An interaction that was not found in IID was not automatically classified as
biologically false.

Instead, it was recorded as lacking an exact matching interaction in the
downloaded IID dataset.

This distinction is important because different PPI databases have different
coverage, evidence sources, integration procedures, and update histories.

### 8.5 Result

Of the 9,835 STRING interactions:

- 2,293 interactions had an exact matching interaction in IID
- IID coverage of the STRING network was 23.31%
- 1,527 interactions had experimental IID support

Additional matched interactions had predicted and/or orthology-based
evidence.

The IID annotation table is stored in:

`data/processed/iid_interaction_annotation.tsv`


## 9. Integrated Context-Specific Evidence Table

### 9.1 Purpose

Rather than immediately applying a strict binary filter after each database,
the evidence obtained from different resources was integrated into one master
interaction-level table.

The purpose is to preserve the original STRING interaction network while
recording the biological evidence supporting or questioning each interaction.

The master table is:

`data/processed/context_specific_evidence_master.tsv`

### 9.2 Evidence layers currently integrated

The current master table contains evidence from:

1. STRING
2. COMPARTMENTS
3. Human Protein Atlas
4. IID

TissueNet will be incorporated as an additional tissue-specific evidence
layer.

### 9.3 Evidence-layer indicators

Three current positive-evidence indicators were created:

- `compartments_supported`
- `hpa_retina_supported`
- `iid_supported`

A descriptive count:

`n_context_evidence_layers`

records how many of the three currently completed context layers provide
positive evidence.

This count is used for descriptive summarisation only.

It is NOT interpreted as a biological probability, interaction confidence
score, or indication that an interaction with three evidence layers is
exactly three times stronger than an interaction with one evidence layer.

Each database provides a different type of biological information.

### 9.4 Current integrated results

All 9,835 STRING interactions were retained in the master table.

Current positive evidence-layer distribution:

| Positive evidence layers | Number of interactions |
|---|---:|
| 0 | 30 |
| 1 | 604 |
| 2 | 6,961 |
| 3 | 2,240 |

The current evidence categories are:

| Evidence category | Number of interactions |
|---|---:|
| COMPARTMENTS + HPA | 6,909 |
| COMPARTMENTS + HPA + IID | 2,240 |
| COMPARTMENTS only | 551 |
| HPA only | 52 |
| COMPARTMENTS + IID | 51 |
| No positive evidence in current layers | 30 |
| IID only | 1 |
| HPA + IID | 1 |

These categories describe the currently available evidence and do not yet
represent the final biological classification of the network.


## 10. Quality-Control Example: MERTK-GAS6

The MERTK-GAS6 interaction was used as one representative quality-control
example.

STRING:

- Interaction score: 0.999

COMPARTMENTS:

- Status: PASS
- Shared localisation evidence was identified.

Human Protein Atlas:

- MERTK retina expression: 15.4 nTPM
- GAS6 retina expression: 5.8 nTPM
- Expression status: BOTH_EXPRESSED

IID:

- Interaction found: Yes
- Support: EXPERIMENTAL
- Number of experimental methods: 2
- Number of experimental publications: 2

Therefore, MERTK-GAS6 currently has positive evidence from all three
completed context-specific evidence layers.


## 11. TissueNet Tissue-Specific Interaction Evidence

### Status

Pending.

### Purpose

TissueNet will be investigated as an additional source of tissue-specific PPI
information.

The purpose is to determine whether interactions in the STRING-derived
network have additional evidence supporting their occurrence in a relevant
tissue context.

Where suitable data are available, TissueNet information will be added as
additional columns to the master evidence table rather than replacing the
existing STRING, COMPARTMENTS, HPA, or IID information.

The database name, version where available, access date, extraction procedure,
identifier-matching procedure, and evidence fields used will be documented.


## 12. Additional Context and Annotation Resources

Additional resources described in the thesis proposal may be incorporated
where they provide relevant and interpretable evidence.

These include resources for:

- tissue expression
- cell-type expression
- tissue-specific protein interactions
- pathway membership
- protein localisation
- disease-relevant biological processes
- condition-specific protein or pathway activity

Potential resources include:

- GTEx
- Human Cell Atlas
- CELLxGENE
- Reactome
- KEGG
- ProteomicsDB

These resources will not automatically be treated as equivalent evidence.
The biological question answered by each database and the type of information
extracted from it will be documented separately.


## 13. Functional Annotation and Enrichment Analysis

### Status

Planned after completion of context-specific evidence integration.

The context-annotated network will be investigated for functional
relationships among its proteins.

Planned analyses include:

- Gene Ontology enrichment
- biological process enrichment
- pathway enrichment
- Reactome pathway analysis
- KEGG pathway analysis
- identification of functional groups/modules
- identification of proteins that participate collectively in related
  biological processes or pathways

Particular attention will be given to processes relevant to RP pathogenesis,
including retinal and photoreceptor biology, cellular signalling,
degeneration, cell death, and apoptosis where supported by the data.


## 14. PathLinker Analysis

### Status

Planned.

PathLinker analysis will be performed after the network has been sufficiently
annotated and the relevant functional/pathway context has been evaluated.

The purpose will be to investigate biologically plausible paths connecting
RP-associated source proteins with selected downstream targets or biological
processes.

The exact source/target definitions, edge weighting strategy, network used,
and PathLinker parameters will be documented before the final analysis is
performed.


## 15. Reproducibility and Data Provenance

Reproducibility is maintained by recording:

- database name
- organism
- database version where available
- download/access date
- input identifiers
- identifier conversion procedures
- thresholds
- filtering/annotation rules
- output filenames
- analysis scripts/notebooks
- methodological decisions

Raw external database downloads are kept separately from processed project
files.

Large raw database files are not committed directly to the GitHub repository.
Instead, their source, version, access date, and processing procedure are
documented so that the analysis can be reproduced.

Processed files used for downstream analysis are stored under:

`data/processed/`

Methodological and database documentation is stored under:

`docs/`


## 16. Current Status

Completed:

- RP gene-list preparation
- STRING PPI network construction
- STRING network audit
- COMPARTMENTS annotation
- HPA retinal-expression annotation
- IID interaction annotation
- integration of COMPARTMENTS, HPA, and IID into a master context-evidence
  table

Current master network:

- 9,835 STRING interactions

Current master evidence table:

`data/processed/context_specific_evidence_master.tsv`

Next:

1. Integrate TissueNet tissue-specific interaction evidence.
2. Evaluate additional relevant context/annotation databases.
3. Finalise the context-specific evidence framework.
4. Perform functional annotation and enrichment analysis.
5. Identify functional groups/modules and pathway relationships.
6. Investigate RP-relevant signalling and cell-death mechanisms.
7. Proceed to PathLinker analysis.
