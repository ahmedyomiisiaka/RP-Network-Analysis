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

### Next stage

TissueNet tissue-specific interaction evidence will be integrated next.
This will be followed by functional annotation, enrichment/pathway
analysis, functional grouping, and PathLinker analysis.
