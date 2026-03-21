# project-star-causal-inference
Empirical code and my M.Sc. seminar paper evaluating TSLS specification bias in the Project STAR experiment. Implements Double/Debiased Machine Learning (DDML) and IPSW to critique standard IV models.

# When Is TSLS Actually LATE? Evidence from Project STAR

**Author:** Anastasiia Rekovets  
**Institution:** Ludwig-Maximilians-Universität München (M.Sc. Economics)  

## Project Overview
This repository contains the empirical code and manuscript for my seminar paper evaluating the causal validity of Two-Stage Least Squares (TSLS) estimates when covariates are included. Building on the methodological framework of Blandhol et al. (2022), this project re-evaluates the canonical Tennessee STAR class-size experiment (Krueger, 1999).

Standard empirical practice often interprets TSLS estimates as Local Average Treatment Effects (LATE). This project demonstrates that when the "rich covariates" condition fails, standard linear IV estimators suffer from specification bias and negative weighting.

## Key Methods & Findings
* **Econometric Methods:** Replicated baseline OLS and TSLS models, conducted Ramsey RESET tests for functional form misspecification, and implemented Double/Debiased Machine Learning (DDML) and Inverse Probability Score Weighting (IPSW) for robust causal estimation.
* **Findings:** The Ramsey RESET test strongly rejects the rich covariates condition in the STAR data ($p < 0.01$). 
* **Specification Bias:** DDML estimates reveal that standard TSLS overstates the achievement effect of small classes by up to 32% in later grades due to parametric misspecification.

## Repository Structure
* `/data`: Contains the `Cleandata.dta` dataset used for the analysis.
* `/notebooks`: Contains the Jupyter Notebook `Causal Inference Empirics Code.ipynb` with all data preparation, statistical testing, and machine learning implementations.
* `/paper`: Contains the final seminar paper PDF.

## How to Run the Code
1. Clone this repository.
2. Install the required dependencies using: `pip install -r requirements.txt`
3. Run the Jupyter Notebook located in the `/notebooks` directory.
