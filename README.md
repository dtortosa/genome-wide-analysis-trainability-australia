# High-Scale Distributed Genomic Pipeline & Parallelized Polygenic Scoring (PGS) Framework

An end-to-end pipeline for genomic data engineering and statistical modeling. It ingests raw high-density genomic arrays, runs multi-stage quality control (QC), scales features through imputation, and computes polygenic scores in parallel.

The pipeline runs on high-performance computing (HPC) environments and uses distributed processing and cross-validation to assess predictive stability across a matrix of **3.85 billion genotype calls** ($1,204 \text{ samples} \times 3.2\text{M variants}$).

---

## 🏗️ Pipeline Architecture

The pipeline processes high-dimensional genomic and longitudinal phenotypic datasets in four decoupled phases:

- Raw array ingestion (Infinium CoreExome)
- Genomic processing and quality control (QC)
- Imputation and dimensionality reduction (PCA)
- Parallelized polygenic scoring (PGS) and 100-fold validation

### 1. Genomic Data Ingestion & ETL
- **Ingestion scale:** Processes raw data arrays from 1,422 initial samples across ~540,000 polymorphic markers (~800 million data points).
- Automated data-wrangling scripts use string processing (`AWK`/`Bash`) to reformat and structure the genomic data for fast downstream parsing.

### 2. Multi-Stage Quality Control & Pruning
To keep the model stable and avoid statistical distortion, the pipeline applies strict data-integrity filters:
- **Sample filtering:** Removes low-confidence samples, retaining 1,204 individuals ($84.7\%$ retention rate).
- **Call-rate filtering:** Requires a genotype completion rate above $99\%$.
- **Relatedness pruning & covariate adjustment:** Computes a genomic relatedness matrix to prune second-degree or closer relatives, and runs Principal Component Analysis (PCA) for dimensionality reduction to control for hidden population stratification.

### 3. Imputation
- Scales the filtered markers up to **~3.2 million variants** using reference-panel imputation, producing a dense feature matrix of **3.85 billion genotype calls**.
- Tracks imputation quality scores and keeps only variants above an information threshold ($INFO / R^2 > 0.95$) to ensure the imputed features are reliable.

### 4. Parallelized Polygenic Scoring (PGS) & Resampling
- **Distributed computing:** Runs tasks through `PySpark` and Python `multiprocessing` on a multi-core Slurm HPC cluster.
- **Scoring methods:** Computes linear combinations of variant effect weights using both classical methods (linkage-disequilibrium clumping and thresholding) and Bayesian/linkage models (such as LDAK).
- **Generalization:** Uses a 100-fold train-test resampling split to estimate generalization error and reduce over-fitting.

---

## 📊 Pipeline Metrics & Benchmarks

The pipeline met the following thresholds and produced measurable predictive signal across continuous longitudinal phenotypes:

| Pipeline Metric | Threshold |
| :--- | :--- |
| **Genotype Call Rate** | $\ge 99.0\%$ |
| **Imputation Quality ($R^2$)** | $> 0.95$ |
| **Sample Retention Rate** | $84.7\%$ |
| **Model Validation** | $100\text{-fold}$ |
| **Prediction Correlation ($r$)**| $0.056 - 0.085$ |

---

## 🛠️ Technical Stack & Infrastructure

- **Data engineering & distributed computing:** PySpark, Python multiprocessing, Bash, AWK
- **Infrastructure & DevOps:** Singularity containers, Slurm workload manager, HPC clusters
- **Statistics & methods:** High-dimensional linear predictors, Principal Component Analysis (PCA), covariate adjustment, LDAK and classical PGS algorithms
