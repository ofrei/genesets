The goal is to annotate MSigDB (https://www.gsea-msigdb.org/gsea/msigdb) and SynGO (https://syngoportal.org/) gene sets to CHR:FROM-TO coordinates (in GRC37 build).
From MSigDB I'm interested in the C5 category, excluding HPO sub-category - which leaves Biological Process (BP), Cellular Component (CC) and Molecular Function (MF) ontologies, in total 10402 genesets. SynGO adds additional 73 gene sets (on top of what's already in MSigDB, and excluding small genesets of less than 5 genes). These genesets combined have 19366 unique genes.

I'm aware of four different ways of refering to a gene:
(1) gene symbol
(2) Ensembl ID
(3) Entrez ID (from NCBI)
(4) HGNC ID

C5 category in MSigGB uses Entrez ID, while SynGO uses HGNC IDs. Historically I used to get coordinates from Ensembl, because of convenient bulk export web interface ( http://grch37.ensembl.org/biomart/martview/ ).  So I've been trying to map Entrez IDs and HGNC IDs to Ensembl IDs. 

The file ``mart_export_14jan2022.txt`` in this repository is downloaded using the link above, after selecting the following columns - which include both HGNC and Entrez IDs. However the mapping is sparse and quite ambiguous.
```
Gene stable ID,Gene stable ID version,Chromosome/scaffold name,Gene description,Gene name,
Gene type,Gene Synonym,Gene start (bp),Gene end (bp),HGNC ID,NCBI gene (formerly Entrezgene) ID
```

This is easy for SynGO, as it comes with ``syngo_genes.xlsx`` containing a handy table to map HGNC to both Entrez and Ensembl. Unfortunately that table covers only 1233 genes, and I can't use it to map genes in MSigDB ongologies. I also found https://syngoportal.org/convert.html which does a great job of mapping Entrez IDs <-> HGNC IDs (also for all genes in MSigDB). I have the map in ``SynGO_id_convert/idmap.xlsx``. Unfortunately this tool doesn't map to Ensembl ID.

My current solution is to use EnsemblID -> EntrezID mapping provided by Ensembl (part of ``mart_Export_14jan2022.txt``). When EntrezID is missing, I tried to put it from ``SynGO_id_convert/idmap.xlsx``, matching based on HGNC ID. This mapped in total 19151 out of 19366 genes, leavning 215 genes unmaped. I haven't tried to use gene symbols to map the remaining genes.

I'm somewhat concerned to leave out 215 genes.

The conclusion is that EntrezID should be mapped to CHR:FROM-TO coordinates from NCBI database, without involving Ensembl. Sort of obvious, but I didn't expect that mapping humna genes across databases is going to be so difficult :)

For the actual scripts, see [prepare_genesets_v2.ipynb](prepare_genesets_v2.ipynb)