# Pathway Gene-Membership Comparison

## Objective
Compare participating genes reported by Enrichr and ToppFun
for significant pathways with matching normalized names.

## Reactome
Shared significant pathway names: 577
ToppFun pathway-ID comparisons: 926
Mean Jaccard similarity: 0.9279
Median Jaccard similarity: 1.0000
Identical reported gene sets: 478

Output:
13_Reactome_gene_membership_comparison.csv

## KEGG Legacy
Shared significant pathway names: 78
ToppFun pathway-ID comparisons: 78
Mean Jaccard similarity: 0.7367
Median Jaccard similarity: 0.7588
Identical reported gene sets: 0

Output:
14_KEGG_Legacy_gene_membership_comparison.csv

## Method
Gene symbols were extracted from the saved pathway-gene
mapping tables.

For each matched normalized pathway name, gene sets were
compared using intersection, set difference, and Jaccard similarity.

Jaccard similarity = number of shared genes divided by
number of genes in the union of both sets.

Each ToppFun pathway ID was retained as a separate record.

## Important limitations
These results compare genes reported in the enrichment
outputs, not necessarily all genes annotated to each pathway.

Reactome contains multiple ToppFun IDs for some normalized
pathway names. Consequently, its mean Jaccard similarity
is calculated across pathway-ID comparisons rather than
independent pathway names.

Matching pathway names does not establish identical
underlying pathway definitions.

Differences in annotation versions, identifier recognition,
gene mapping, and enrichment settings may affect agreement.

KEGG Medicus was not included in gene-membership comparisons
because no exact normalized pathway-name matches were found.

The results do not establish biological causality or
independent experimental validation.
