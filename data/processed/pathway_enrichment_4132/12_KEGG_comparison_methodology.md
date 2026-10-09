# KEGG Pathway Enrichment Comparison

## Objective
Compare significant KEGG pathway results from Enrichr and ToppFun
while retaining differences between pathway annotation sources.

## Input
4,132 protein-associated gene symbols from the Stage 4 STRING network.

## Enrichr
Pathway library: KEGG 2026.
Exported pathways: 349.
Significant pathways (adjusted P-value < 0.05): 227.

## ToppFun
Pathway sources:
- KEGG Legacy Pathways
- KEGG Medicus Pathways

Significance criterion: FDR Benjamini-Hochberg < 0.05.

Significant exported records:
- KEGG Legacy: 86.
- KEGG Medicus: 253.

## Comparison method
Pathway names were converted to uppercase.
The KEGG_ prefix was removed where present.
Non-alphanumeric character sequences were replaced by underscores.

Exact normalized pathway-name matches were identified separately
for KEGG Legacy and KEGG Medicus.

## Results

### KEGG Legacy versus Enrichr KEGG 2026
Shared significant normalized pathway names: 78.
Enrichr-only names: 149.
ToppFun-only names: 8.

### KEGG Medicus versus Enrichr KEGG 2026
Shared exact normalized pathway names: 0.
Enrichr-only names: 227.
ToppFun-only names: 253.

## Interpretation and limitations
KEGG Legacy and KEGG Medicus are distinct annotation collections.

Zero exact-name matches with KEGG Medicus do not demonstrate
that there is no biological overlap.

Pathway names may differ between annotation versions.
Matching names do not necessarily indicate identical gene memberships.

Differences in identifier recognition, background populations,
annotation versions, and statistical procedures can affect results.

These comparisons concern statistically significant pathway records,
not evidence that individual genes are significantly associated
with retinitis pigmentosa or apoptosis.

## Output
11_KEGG_Enrichr_ToppFun_comparison.csv

The original pathway results and pathway-gene mappings are preserved
separately in the pathway_enrichment_4132 directory.
