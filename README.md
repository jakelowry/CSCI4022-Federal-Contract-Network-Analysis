# Federal Contract Network Analysis (FY2025)
**Course:** CSCI 4022 — Advanced Data Science, CU Boulder  
**Author:** Jacob Lowry

## Overview
Network analysis of FY2025 federal contract award data from USAspending.gov, 
applying MinHash entity resolution, HITS link analysis, Apriori association 
rules, and k-means/GMM clustering to profile contractors in the federal 
spending network.

## Data
Download from [USAspending.gov](https://www.usaspending.gov):
- Award Type: Definitive Contracts
- Time Period: FY2025
- Level: Prime Award Summaries

## Notebooks
1. `1_data_loading_cleaning.ipynb` — Data ingestion and preprocessing
2. `2_minhash_entity_resolution.ipynb` — Contractor name deduplication
3. `3_hits_algorithm.ipynb` — Hub/Authority scoring
4. `4_apriori_association_rules.ipynb` — NAICS co-occurrence analysis
5. `5_clustering.ipynb` — Contractor profile clustering

## Requirements
```bash
pip install pandas numpy matplotlib seaborn datasketch mlxtend scikit-learn networkx
```