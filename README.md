# Extending GRADIS: Supervised Learning of Gene Regulatory Networks by Inclusion of Binding Motifs on Promoter Regions

## Project Overview

This Master's project extends the GRADIS (Gene Regulatory Network Inference using Distance-based Supervised learning) framework by incorporating transcription factor binding motif information from promoter regions. The approach enhances gene regulatory network (GRN) inference in *Escherichia coli* by integrating expression data with binding site evidence from multiple biological databases.

## Project Structure

The project is organized into two main components:

### 1. Data Gathering and Analysis (`databases.ipynb`)
- **Data Sources**: Integration of three databases:
  - **RegulonDB**: Curated knowledge of transcriptional regulation in E. coli
  - **Ecocyc**: Encyclopedia of E. coli genes and metabolism
  - **TEC Database**: Transcription profiles for E. coli
  
- **Consensus Database Construction**:
  - Identified transcription factor (TF) binding sites within 500 basepairs upstream of genes
  - Established gene-TF relationships across all three databases
  - Created a consensus database with five columns: `genes`, `TF`, `regDB`, `ecocyc`, `TEC`
  - Database agreement indicators (1 = relationship present, 0 = absent)

### 2. GRADIS Implementation and Extension (`gradis_ex.ipynb`)

#### GRADIS Framework Implementation:
- **Input Data**: E. coli expression profiles (805 samples) and gold standard validated regulations
- **Feature Engineering**:
  - Clustered expression data to 50 centroids using K-means
  - Scaled expression data and constructed edge-weighted complete graph
  - Generated adjacency matrix using Euclidean distance metric
  - Created feature vectors by horizontally stacking matrix rows
- **Labels**: Binary classification (1 = known regulation, 0 = unknown regulation)

#### Data Imbalance Handling:
- Dataset: 152,280 total rows (2066 known regulations, ~150,000 unknown)
- Systematic training approach with balanced sampling
- Unknown class divided by number of known regulations for balanced training sets

#### Extension Methodology:
1. **Initial GRADIS Prediction**: Train SVM model on original feature vectors
2. **Negative Prediction Analysis**: Investigate TF-target relationships predicted as negative
3. **Database Integration Filtering**:
   - **Filter 1**: Relationships with agreement in ≥2 databases
   - **Filter 2**: Relationships with agreement in ≥1 database
4. **Iterative Model Refinement**: Train separate SVM models after each filtering step

#### Evaluation:
- Random selection of negative predictions matching known regulation size
- Train-test split (80-20) for final model evaluation
- Performance metrics: AUC (Area Under ROC Curve) and AUPR (Area Under Precision-Recall Curve)

## Results

| Method | AUC | AUPR |
|--------|-----|------|
| GRADIS (Baseline) | 0.89 | 0.91 |
| GRADIS + Filter 1 (≥1 database agreement) | 0.95 | 0.96 |
| GRADIS + Filter 2 (≥2 database agreement) | 0.94 | 0.96 |

## Key Findings

- Integration of binding motif information significantly improves GRN inference accuracy
- Database consensus filtering effectively identifies false negatives in predictions
- Even single-database agreement provides substantial performance improvement
- The framework demonstrates robust handling of severe class imbalance

## Future Work

- Application to eukaryotic systems with more complex regulatory mechanisms
- Extension to other bacterial species
- Incorporation of additional data types (e.g., ChIP-seq, DNase-seq)
