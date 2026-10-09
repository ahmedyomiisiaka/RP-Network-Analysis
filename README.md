# RP Network Analysis

Network-based analysis of signalling pathways in Retinitis Pigmentosa.

Protein-protein interaction network reconstruction connecting
RP-associated proteins to the apoptosis pathway.

## Pipeline

- Stage 0: Project setup
- Stage 1: RP gene collection
- Stage 2: Apoptosis pathway gene collection
- Stage 3: UniProt identifier mapping
- Stage 4: PPI network construction
- Stage 5: Context-specific filtering
- Stage 6: Pathway reconstruction
- Stage 7: Network topology analysis
- Stage 8: Enrichment analysis
- Stage 9: Target prioritisation
- Stage 10: scRNA-seq validation
- Stage 11: Visualisation

## Structure

```
notebooks/    — analysis notebooks
data/         — processed datasets
docs/         — documentation
scripts/      — helper scripts
figures/      — output figures
```

## Current Analysis Status

The initial RP-associated protein interaction network contains 9,835
unique STRING interactions using a minimum combined STRING score of 0.400.

Context-specific annotation is being performed by integrating multiple
independent biological evidence sources.

Completed evidence layers:

- COMPARTMENTS subcellular localisation (score >= 2.0)
- Human Protein Atlas retinal expression (nTPM > 0)
- Integrated Interactions Database (IID) PPI evidence

Current results:

- 9,751 interactions have COMPARTMENTS localisation support.
- 9,202 interactions have both proteins expressed in retina.
- 2,293 interactions have an exact matching interaction in IID.
- 2,240 interactions currently have positive evidence from all three
  completed context layers.

All 9,835 STRING interactions are currently retained in the master
evidence table. Evidence absence is recorded explicitly rather than
being treated automatically as evidence that an interaction is false.

Current master table:

`data/processed/context_specific_evidence_master.tsv`

### Remaining research stages

Further tissue-specific evidence integration, functional grouping,
pathway reconstruction and target prioritisation remain subject to
supervisor review. The full Stage 4 pathway enrichment analysis
is documented below.

## Stage 4 Network — Pathway Enrichment Analysis

### Objective

Perform pathway enrichment on the 4,132 unique protein-associated
gene symbols from the Stage 4 STRING network and preserve the
pathways, participating genes, statistical results and methodology.

The Stage 4 network contains 9,835 undirected STRING interactions
at a minimum combined score of 0.400.

This enrichment uses the original Stage 4 input, not the smaller
Stage 5 context-filtered network.

### Enrichr

Two pathway libraries were analysed:

| Library | Exported pathways | Significant pathways (adjusted P < 0.05) |
|---|---:|---:|
| Reactome Pathways 2024 | 1,976 | 809 |
| KEGG 2026 | 349 | 227 |

Original exports, pathway summaries and pathway–gene mapping
tables are preserved.

### ToppFun (ToppGene Suite)

Of 4,132 submitted gene symbols, 4,047 were recognized and
85 were unrecognized.

The exported significant pathway records were:

| Source | Significant pathway records |
|---|---:|
| Reactome Pathways | 967 |
| KEGG Legacy Pathways | 86 |
| KEGG Medicus Pathways | 253 |
| **Total** | **1,306** |

Significance threshold: Benjamini–Hochberg FDR < 0.05.

The exported records contain 46,576 pathway–gene relationships
involving 3,034 unique participating genes.

### Pathway-name comparison

Significant pathways were compared using normalized names.

| Comparison | Shared significant pathway names |
|---|---:|
| Enrichr Reactome vs ToppFun Reactome | 577 |
| Enrichr KEGG vs ToppFun KEGG Legacy | 78 |
| Enrichr KEGG vs ToppFun KEGG Medicus | 0 exact-name matches |

The absence of exact-name matches with KEGG Medicus does not
establish an absence of biological overlap.

### Gene-membership comparison

For matching significant pathway names, participating gene sets
were compared using Jaccard similarity.

| Metric | Reactome | KEGG Legacy |
|---|---:|---:|
| Matched pathway names | 577 | 78 |
| ToppFun pathway-ID comparisons | 926 | 78 |
| Mean Jaccard similarity | 0.9279 | 0.7367 |
| Median Jaccard similarity | 1.0000 | 0.7588 |
| Comparisons with identical reported gene sets | 478 | 0 |

Reactome includes multiple ToppFun pathway IDs for some matching
pathway names. Its summary statistics are therefore calculated
across pathway-ID comparisons, not independent pathway names.

### Files and reproducibility

The enrichment results and methodology are available in:

`data/processed/pathway_enrichment_4132/`

The analysis notebook is available at:

`notebooks/Pathway_Enrichment_4132_Proteins.ipynb`

The directory includes original exports, input identifier audits,
pathway summaries, pathway–gene mappings, pathway-name comparisons,
gene-membership comparisons and methodology documentation.

### Interpretation and limitations

- Enrichr and ToppFun use different annotation collections and
  potentially different reference backgrounds.
- ToppFun did not recognize 85 submitted identifiers.
- Matching pathway names do not guarantee identical underlying
  pathway definitions.
- Jaccard similarity describes agreement between reported gene
  sets, not biological validation or statistical confidence.
- KEGG Medicus requires separate investigation because its
  pathway naming and classification differ from conventional KEGG.
- Database versions, identifier recognition and statistical
  settings require verification before final thesis reporting.

No apoptosis-specific pathway filtering or downstream network
reconstruction was performed as part of this enrichment stage.
