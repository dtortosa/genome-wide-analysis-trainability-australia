# High-Scale Distributed Genomic Pipeline & Parallelized Polygenic Scoring (PGS) Framework

An end-to-end distributed data engineering and statistical modeling pipeline designed to ingest raw, high-density genomic arrays, execute multi-stage quality control (QC) filtering, scale features via imputation, and perform high-scale parallelized linear scoring. 

The system leverages high-performance computing (HPC) environments, distributed data processing, and robust cross-validation frameworks to evaluate predictive stability across a matrix of **3.85 billion genotype calls** ($1,204 \text{ samples} \times 3.2\text{M  variants}$).

---

## 🏗️ Pipeline Architecture

The platform processes high-dimensional genomic and longitudinal phenotypic datasets through four core decoupled engineering phases:

- Raw Array Ingestion (Infinium CoreExome)
- Distributed Genomic Processing & Data-Quality Gating (QC)
- Imputation Scaling & Dimensionality Reduction (PCA)
- Parallelized Polygenic Scoring (PGS) Engine & 100-Fold Validation

### 1. High-Dimensional Matrix Ingestion & Genomic ETL
- **Ingestion Scale:** Processes raw data arrays from 1,422 initial cohorts across ~540,000 highly polymorphic markers (~800 million initial data points).
- Engineered automated data-wrangle scripts utilizing optimized string processing (`AWK`/`Bash`) to re-format and structure genomic streams for fast downstream matrix parsing.

### 2. Multi-Stage Quality Control Gating & Pruning
To guarantee model stability and prevent statistical distortion, the pipeline enforces strict data-integrity filters:
- **Sample Gating:** Eliminates low-confidence samples, retaining a high-quality cohort of 1,204 individuals ($84.7\%$ retention rate).
- **Call-Rate Validation:** Mandates a stringent genotype completion threshold of $>99\%$.
- **Network Pruning & Covariate Adjustments:** Implements genomic relatedness matrix calculations (pruning second-degree or closer relatives) and executes Principal Component Analysis (PCA) for dimensionality reduction to control for hidden population stratification.

### 3. Imputation Scaling Engine
- Scales the high-quality filtered markers up to **~3.2 million variants** using reference panel imputation, generating a dense feature matrix containing **3.85 billion discrete genotype calls**.
- Tracks and monitors imputation quality scores, enforcing an information threshold metric ($INFO / R^2 > 0.95$) to guarantee synthetic feature reliability.

### 4. Parallelized Polygenic Scoring (PGS) & Resampling Engine
- **Distributed Computing Architecture:** Deploys tasks via `PySpark` and Python `multiprocessing` arrays optimized for a multi-core Slurm High-Performance Computing (HPC) cluster.
- **Compute Optimization:** Executes linear combinations of variant effect weights using classical (Linkage Disequilibrium clumping/thresholding) and state-of-the-art Bayesian/linkage models (such as LDAK frameworks).
- **Generalization Guarantees:** Implements a rigorous 100-fold train-test resampling split to bound generalization error and entirely mitigate over-fitting risks.

---

## 📊 Pipeline KPIs & Framework Benchmarks

The automated pipeline maintained exceptional performance criteria and verifiable predictive signals across continuous longitudinal phenotypes:

| Pipeline Metric | Production Threshold |
| :--- | :--- |
| **Genotype Call Rate** | $\ge 99.0\%$ |
| **Imputation Quality ($R^2$)** | $> 0.95$ |
| **Sample Retention Rate** | $84.7\%$ |
| **Model Validation** | $100\text{-fold}$ |
| **Prediction Correlation ($r$)**| $0.056 - 0.085$ |

---

## 🛠️ Technical Stack & Infrastructure

- **Data Engineering & Distributed Computing:** PySpark, Python Multiprocessing, Bash, AWK
- **Infrastructure & Devops:** Singularity Containers, Slurm Workload Manager, High-Performance Computing Cluster Environments
- **Statistical Analytics & Core Protocols:** High-Dimensional Linear Predictors, Principal Component Analysis (PCA), Covariate Matrix Adjustments, LDAK & Classical PGS Algorithms
