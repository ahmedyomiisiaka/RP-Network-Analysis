# Stage 6 — RP-to-Apoptosis Pathway Reconstruction

Stage 6 will reconstruct source-to-target paths through the final
Stage-5 retina-context-supported PPI network.

## Input network

`../stage5_context_filtering/11_stage5_context_specific_retinal_network.tsv`

## Sources

Retinitis pigmentosa disease genes.

## Targets

Validated apoptosis-related target proteins.

The exact apoptosis target set will be reviewed before pathway
reconstruction.

## Planned analysis

- define RP source nodes
- define apoptosis target nodes
- convert STRING confidence into appropriate path costs
- perform source-to-target pathway reconstruction
- retain ranked paths
- quantify node/path recurrence
- generate reconstructed RP-to-apoptosis subnetwork

Functional enrichment is not performed as Stage 6.
Formal topology/community analysis follows in Stage 7.
Enrichment follows in Stage 8.
