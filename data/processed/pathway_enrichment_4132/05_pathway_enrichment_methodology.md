# Pathway Enrichment — 4,132 STRING Network Proteins

## Documentation date
2026-10-09

## Objective
Identify biological pathways enriched among proteins in the
retinitis pigmentosa (RP) protein-protein interaction network
and preserve the input genes associated with each pathway.

## Input
- Source: Stage 4 STRING protein-protein interaction network.
- Input file: 02_ppi_nodes_4132_gene_symbols.txt
- Unique input identifiers: 4,132.
- Organism: Homo sapiens.
- STRING interaction confidence threshold: 0.400.
- Input: unique protein names from both network endpoints.
- This enrichment uses the full Stage 4 network, not the
  smaller Stage 5 retinal-context-filtered network.

## Enrichment tool
- Tool: Enrichr
- Website: https://maayanlab.cloud/Enrichr/
- Libraries:
  - Reactome Pathways 2024
  - KEGG 2026
- Input submitted: 4,132 identifiers.
- Original enrichment exports preserved unchanged.
- Background: Enrichr's default library-specific statistical
  background; no custom background was supplied.
- Significance criterion: adjusted P-value < 0.05.
- Adjusted P-values were taken directly from Enrichr.

## Results
### Reactome Pathways 2024
- Exported pathways: 1,976.
- Significant pathways: 809.
- Unique participating input genes: 3,011.
- Pathway-gene relationships: 45,486.

### KEGG 2026
- Exported pathways: 349.
- Significant pathways: 227.
- Unique participating input genes: 2,284.
- Pathway-gene relationships: 13,531.

## Quality control
- No missing pathway names.
- No missing pathway gene lists.
- No duplicate pathway names within either export.
- All reported participating genes match the original input.
- All pathway overlap counts match their gene-list lengths.
- No missing genes in the expanded mapping tables.
- No duplicate pathway-gene relationships.
- Expanded mapping counts agree with summary gene counts.

## Files
### Input
- 01_enrichment_input_identifier_audit.tsv
- 02_ppi_nodes_4132_gene_symbols.csv
- 02_ppi_nodes_4132_gene_symbols.txt

### Unmodified Enrichr exports
- Reactome_Pathways_2024_table.txt
- KEGG_2026_table.txt

### Analysis-ready files
- 03_Reactome_2024_pathway_summary.csv
- 03_KEGG_2026_pathway_summary.csv
- 04_Reactome_2024_pathway_gene_mapping.csv
- 04_KEGG_2026_pathway_gene_mapping.csv

## Interpretation and limitations
- Reported pathway genes are overlaps with the submitted
  protein list, not complete pathway membership lists.
- Significant enrichment does not prove causal involvement
  in RP or apoptosis.
- The full network contains interaction partners beyond
  established RP disease genes.
- Gene recognition by Enrichr has not been independently
  confirmed for every submitted identifier.
- Further pathway interpretation and network reconstruction
  require separate analyses.
