# Federal Contract Network Analysis (FY2025)
**Course:** CSCI 4022: Advanced Data Science, CU Boulder  
**Author:** Jacob Lowry

## Overview
Network analysis of FY2025 federal definitive contract award data from
USAspending.gov. A multi-stage pipeline applies four course algorithms to
uncover latent structure in federal procurement:

The pipeline consists of two stages feeding into a final clustering:
1. **MinHash entity resolution**: deduplicates contractor name variants
2. **HITS algorithm**: scores agencies as hubs and contractors as authorities
   in the federal spending network
3. **K-Means / GMM clustering**: profiles contractors into economic tiers
   based on network authority, total obligations, and service diversity

Additionally, **Apriori association rule mining** is performed as a standalone
analysis of NAICS service co-occurrence patterns across agency procurement
baskets, providing supplementary insight into government-wide procurement norms
independent of the clustering pipeline.

## Data
Download from [USAspending.gov](https://www.usaspending.gov):
- Time Period: FY25
- Award Type: Contracts -> Subtype: Definitive Contracts
- Level: Prime Award Summaries (Award-level, not Transaction-level)

The download will result in a .zip file which contains 2 CSV files, one for Prime Awards and one for Sub-Awards.

Place the downloaded Prime Awards CSV in the repo root as `Contracts_PrimeAwardSummaries.csv`
before running any notebooks.

## Scope Note
The original proposal intended to analyze all ~5.7M federal contracts across
all award types. Due to USAspending.gov infrastructure limitations on bulk
CSV exports and the analytical advantages of focusing on the highest-signal
subtype, scope was reduced to Definitive Contracts (~73,365 rows). See
Notebook 1 for full justification.

## Notebooks
The core pipeline runs through notebooks 1, 2, 3, and 5 in order. Notebook 4
is a standalone analysis and does not feed into notebook 5.

| Notebook | Input | Output | Role |
|---|---|---|---|
| `1_data_cleaning.ipynb` | `Contracts_PrimeAwardSummaries.csv` | `contracts_cleaned.csv` | Pipeline |
| `2_minhash_entity_resolution.ipynb` | `contracts_cleaned.csv` | `contracts_resolved.csv` | Pipeline |
| `3_hits_algorithm.ipynb` | `contracts_resolved.csv` | `contracts_with_hits.csv`, `corporate_family_authority_scores.csv` | Pipeline |
| `4_apriori_association_rules.ipynb` | `contracts_resolved.csv` | `apriori_frequent_itemsets.csv`, `apriori_association_rules.csv` | Standalone |
| `5_clustering.ipynb` | `contracts_with_hits.csv`, `corporate_family_authority_scores.csv` | `contractor_clusters.csv` | Pipeline |

## Key Findings
- The federal definitive contracting network is extremely hub-concentrated:
  the Department of Defense holds a hub score of 0.959, with NASA being the only
  other agency with a visible score at linear scale
- 6 corporate families (Lockheed Martin, Boeing, Raytheon, Electric Boat,
  Northrop Grumman, Huntington Ingalls) dominate authority scores and form
  the Defense Prime cluster with median obligations of $130.9B
- IT and management consulting services co-occur universally across agency
  procurement baskets, a pattern that holds even after excluding DoD,
  confirming it is a government-wide norm
- The contractor market follows a strongly hierarchical structure: 6 Defense
  Primes, 9,641 Mid-Tier Specialists, and 13,150 Small Niche Contractors

## Requirements
```bash
pip install pandas numpy matplotlib seaborn datasketch mlxtend scikit-learn networkx roman
```

## Acknowledgments
This project used generative AI assistance (Claude, Anthropic) for help with troubleshooting error messages and debugging. All code, analytical decisions, interpretations, and
written content are the author's own.
