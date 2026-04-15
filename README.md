# scRNA-seq-pancreatic-islets-

scRNA cell data processing using scanpy and functional analysis  

Samples used come from the following study [1] 
This study examined the functional changes of different pancreatic cells under conditions of metabolic stress (overnutrition).

Processing flow:

Figure 1: Complete Processing flow

Part I - Preprocessing of scRNA-seq reads and matrix creation (Galaxy WF):
 (1) Mapping (RNAstar solo)
 (2) Utility for handling of SC input data (DropletUtils)
 (3) AD object creation

Part II - Preprocessing of reference dataset [2] and its use for annotation of query dataset using scanpy ingest (Jupyter notebook w/python core)
(1) Load and preprocess the reference dataset
(2) Import query dataset from google drive
(3) Update gene annotation for query dataset
(4) Preprocess query dataset
(5) Dimensionality reduction and clustering (query dataset)
(6) Remove cells not expected in query dataset from reference dataset
(7) Intersect genes between two datasets
(8) Map onto a reference dataset using ingest

Part III - SC complete processing pipeline (scanpy standard flow) + trajectory inference + Celltypist cell annotation (Jupyter notebook w/python core) 
 (1) Load AD object from Galaxy
 (2) Filter out reads containing genes that are not of interest (Mitochondrial, ribosomal, hemoglobin genes, pseudogenes etc.)
 (3) Filter cells based on quality
 (4) Doublet detection
 (5) Normalization
 (6) Feature selection
 (7) Dimensionality reduction
 (8) Nearest neighbor graph constuction and visualization (UMAP + tSNE)
 (9) Clustering with Leiden comunity
 (10) Quality control and cell filtering - reassessment
 (11) Cluster annotation
 (12) Create markers for known Pancreatic cell types
 (13) Create dotplots to identify markers per cluster
 (14) Plot top cluster marker genes
 (15) Differentially expressed genes
 (16) Automatic cell type annotation (celltypist)
 (17) Trajectory inference

Part IV - Functional analysis
(1) Split the reference data into training and testing sets
(2) Train the RF classifier
(3) Evaluate the model
(4) Predict labels for query dataset
(5) Hyperparameter tuning
(6) Prediction

References: 
[1] Single-cell RNA Sequencing Uncovers Molecular Mechanisms of Human Pancreatic Islet Dysfunction Under Overnutrition Metabolic Stress (human) 
[2] Tabula sapiens - pancreas.h5ad
