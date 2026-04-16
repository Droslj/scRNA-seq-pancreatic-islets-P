# Case study: Behaviour of Pancreatic cells exposed to Metabolic stress

This study examined the functional changes of different pancreatic cells under conditions of metabolic stress (overnutrition). I performed complete scRNA cell raw data processing using different platforms and tools. Final functional analysis was performed using scanpy. Samples used for this case study come from the following study [1] 

# Processing flow

Complete processing flow [*] was divided into four stages and is depicted in the Figure 1:

![Complete processing flow](Images/Complete_processing_flow.png)

**Figure 1: Complete Processing flow**

## Part I - Preprocessing

Part I included preprocessing of scRNA-seq reads and matrix creation (Galaxy WF). Following steps were performed in the galaxy environment (usegalaxy.eu):

 (1) Mapping (RNAstar solo)
 (2) Utility for handling of SC input data (DropletUtils)
 (3) AD object creation

## Part II - Integration of annotation and metadata 

Part II of the processing included preprocessing of reference dataset [2] and its use for annotation of query dataset using scanpy ingest. It was performed on Google colab environment (Jupyter notebook w/python core - code can be made available on request) Following steps were performed: 

 (1) Load and preprocess the reference dataset
 (2) Import query dataset from google drive
 (3) Update gene annotation for query dataset
 (4) Preprocess query dataset
 (5) Dimensionality reduction and clustering (query dataset)
 (6) Remove cells not expected in query dataset from reference dataset
 (7) Intersect genes between two datasets
 (8) Map onto a reference dataset using ingest

## Part III - single cell object Processing pipeline

Part III of the processing included downstream processing pipeline for SC (scanpy standard flow). It was performed on Google colab environment (Jupyter notebook w/python core - code can be made available on request). Following steps were performed:
 
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

## Part IV - Functional analysis

Part IV of the processing included downstream Functional analysis of the obtained (**) results. Following analysis was performed:

 (1) Conversion of the source .rds file into h5ad format (sceasy convert - usegalaxy.eu)
 (2) Inspection, clean up and QC of the final adata object
 (3) Single cell object processing w/scanpy
 (4) Predict labels for query dataset
 (5) HVG feature selection, scaling and dimensionality reduction
 (6) Nearest neighbor graph construction
 (7) Batch detection (see Figure 2) and removal (see Figure 3)
 (8) Functional analysis - see following section.

 # Biological Analysis of the Results 

Final part of the analysis was interpreting obtained results in the light of metabolic stress conditions to which the cells were exposed. After processing steps (see Part IV, steps (1) - (6), plotting clusters of cell families in the data matrix revealed batch effect (see Figure 2).

![Batch effect](Images/Batch_effect.png)

**Figure 2: UMAP plots (batch, treatment, cell.type.final) - batch effect** 

After batch removal (using Harmony, Part IV, step (7)), the batch effect was not detected any more, see Figure 3.

![Batch effect](Images/Batch_effect_removed.png)

**Figure 3: UMAP plots - batch effect removed**

**Comment**

Comparing middle plot for Acinar, Ductal and Alpha cells clusters shows complete overlapping of treated and control conditions. This confirms that these cell types are relatively stable in their overall transcriptomic identity (resilience).
Beta cells cluster on the other hand shows some separation between Control and Treated conditions, which denotes change of transcriptomic identity.

![UMAP](Images/Cell_type_cluster.png)

**Figure 4: UMAP plot of cell families**

**Comment**

The UMAP plot of cell families shows also rare epsilon cells, which are closely related to alpha cells. Separate plot is required to highlight their position in the UMAP plot to reveal the proximity to the alpha cells cluster.

![Epsilon](Images/Eps_Alpha_cluster.png)

**Figure 5: UMAP plot w/highlighted alpha/epsilon cell clusters**

**Comment**

The UMAP plot reveals that they are indeed near the alpha cluster, but they maintain their transcriptomic identity. Another important plot is the UMAP plot of Ghrelin they secrete.


![UMAP](Images/Ghrelin.png)

**Figure 6: UMAP plot of Ghrelin**

References: 
 [1] Single-cell RNA Sequencing Uncovers Molecular Mechanisms of Human Pancreatic Islet Dysfunction Under Overnutrition Metabolic Stress (human) 
 [2] Tabula sapiens - pancreas.h5ad

Notes:
 (*) First three parts of the analysis were performed using a subset of full data available on the NCBI.
 (**) Part IV - the Functional analysis was performed on the raw data set provided by the authors of the study (section Supplementary file). The .rds file was first converted to h5ad file format in the Galaxy platform and then imported into pyhton environment. Functional analysis was performed using this file, not the file obtained in the Parts I-III 
