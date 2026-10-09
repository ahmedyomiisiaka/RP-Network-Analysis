# ToppFun Pathway Enrichment and Reactome Comparison

Date documented: 2026-10-09

## 1. Research objective
Identify annotated biological pathways associated with the
4,132 proteins in the Stage 4 STRING protein interaction network
and retain the genes participating in each pathway.

## 2. Input dataset
Input: 4,132 unique gene symbols from the Stage 4 network.
The original input list was preserved without automatic remapping.

## 3. ToppFun enrichment
Tool: ToppFun, ToppGene Suite.
Website: https://toppgene.cchmc.org/

Input identifiers: HGNC gene symbols.
Submitted identifiers: 4,132.
Recognized identifiers: 4,047.
Unrecognized identifiers: 85.

Pathway-category annotated input genes: 3,499.
Pathway annotations before applied cutoff: 3,697.
Reference genes in pathway category: 14,250.

Selected pathway sources:
- Reactome Pathways
- KEGG Legacy Pathways
- KEGG Medicus Pathways

Background: ToppFun default category-specific reference.
Statistical correction: FDR Benjamini-Hochberg.
Significance cutoff: FDR < 0.05.
Gene limits: 1 to 2,000.

## 4. ToppFun exported results
Total significant exported pathway records: 1,306.
Reactome: 967.
KEGG Legacy: 86.
KEGG Medicus: 253.

Total pathway-gene relationships: 46,576.
Unique genes in exported pathway results: 3,034.

Quality control:
- Gene-count mismatches: 0.
- Missing participating genes: 0.
- Duplicate pathway-ID/source/gene relationships: 0.

Original export:
06_ToppFun_original_results.txt

Derived files:
07_ToppFun_pathway_summary.csv
08_ToppFun_pathway_gene_mapping.csv

## 5. Enrichr comparison
Enrichr pathway libraries:
- Reactome Pathways 2024
- KEGG 2026

Enrichr unique participating genes across exported results: 3,239.
ToppFun unique participating genes across exported results: 3,034.
Shared participating genes: 3,004.
Enrichr-only genes: 235.
ToppFun-only genes: 30.

These gene comparisons concern exported pathway memberships,
not necessarily significant gene-level associations.

## 6. Significant Reactome pathway comparison
Enrichr significant Reactome records: 809.
ToppFun significant Reactome records: 967.
ToppFun unique normalized pathway names: 608.

Shared significant normalized pathway names: 577.
Enrichr-only normalized names: 232.
ToppFun-only normalized names: 31.

Total distinct significant normalized names: 840.

Comparison file:
09_Reactome_Enrichr_ToppFun_comparison.csv

## 7. Important methodological limitations
1. Enrichr and ToppFun use different pathway annotation versions.
2. Their background populations and identifier recognition may differ.
3. Enrichr exports include nonsignificant pathways; the ToppFun
   export contains pathways passing the selected FDR cutoff.
4. Normalized pathway-name agreement does not guarantee identical
   pathway definitions or participating gene sets.
5. ToppFun records with distinct IDs were preserved even when
   pathway names were identical.
6. No causal relationship between RP genes and apoptosis can be
   inferred from enrichment alone.
7. The 85 unrecognized ToppFun identifiers require separate
   documentation and possible investigation.

## 8. Next research stage
Review pathway results with the thesis supervisor before selecting
specific pathways or conducting further network reconstruction.
