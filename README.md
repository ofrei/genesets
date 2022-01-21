The goal is to annotate MSigDB (https://www.gsea-msigdb.org/gsea/msigdb) and SynGO (https://syngoportal.org/) gene sets to CHR:FROM-TO coordinates (in GRC37 build).
From MSigDB I'm interested in the C5 category, excluding HPO sub-category - which leaves Biological Process (BP), Cellular Component (CC) and Molecular Function (MF) ontologies.

I'm aware of four different ways of refering to a gene:
(1) gene symbol
(2) Ensembl ID
(3) Entrez ID (from NCBI)
(4) HGNC ID

C5 category in MSigGB uses Entrez ID, while SynGO uses HGNC IDs. Historically I used to get coordinates from Ensembl, because of convenient bulk export web interface ( http://grch37.ensembl.org/biomart/martview/ ).  So I've been trying to map Entrez IDs and HGNC IDs to Ensembl IDs. 

The file ``mart_export_14jan2022.txt`` in this repository is downloaded using the link above, after selecting the following columns - which include both HGNC and Entrez IDs. However the mapping is sparse and quite ambiguous.
```
Gene stable ID,Gene stable ID version,Chromosome/scaffold name,Gene description,Gene name,Gene type,Gene Synonym,Gene start (bp),Gene end (bp),HGNC ID,NCBI gene (formerly Entrezgene) ID
```

This is easy for SynGO, as it comes with ``syngo_genes.xlsx`` containing a handy table to map HGN to both Entrez and Ensembl. Unfortunately that table covers only 1233 genes, and I can't use it to map genes in MSigDB ongologies. I also found https://syngoportal.org/convert.html which does a great job of mapping Entrez IDs <-> HGNC IDs (also for all genes in MSigDB). I have the map in ``SynGO_id_convert/idmap.xlsx``. Unfortunately this tool doesn't map to Ensembl ID.

Conclusion is that EntrezID should be mapped to CHR:FROM-TO coordinates from NCBI database, without involving Ensembl.

For more details see [prepare_genesets_v2.ipynb]
