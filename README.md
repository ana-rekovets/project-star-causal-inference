# When Is TSLS Actually LATE? Evidence from Project STAR

Replication and extension of Krueger (1999) evaluating whether TSLS estimates of small-class effects in the Tennessee STAR experiment retain a valid **Local Average Treatment Effect (LATE)** interpretation once covariates are included, following the methodological framework of **Blandhol et al. (2022)**.

> **Research question:** Under what conditions does standard TSLS with covariates identify a non-negatively weighted average of causal effects? Blandhol et al. (2022) prove that this requires the "rich covariates" condition — L[Z|X] = E[Z|X] — which most empirical specifications fail.

---

## Summary of Findings

| Grade | OLS | 2SLS | DDML (βᵣᵢ꜀ₕ) | LATE/ACR (DDML) | LATE/ACR (IPSW) | RESET p-val. | Rel. Spec. Bias |
|-------|-----|------|--------------|----------------|----------------|-------------|-----------------|
| Kindergarten | 5.54 | 5.54 | 5.21 | 4.99 | 5.53 | 0.000 | 0.059 |
| Grade 1 | 7.38 | 7.10 | 5.81 | 7.28 | 7.23 | 0.000 | 0.182 |
| Grade 2 | 5.53 | 6.37 | 4.62 | 6.64 | 6.66 | 0.002 | 0.274 |
| Grade 3 | 5.85 | 7.30 | 4.92 | 6.72 | 7.46 | 0.000 | 0.326 |

*All estimates are in SAT percentile points. Relative specification bias = |β_IV − β_rich| / |β_IV|.*

The Ramsey RESET test rejects the rich covariates condition in all grade cohorts (p < 0.01). By Grade 3, specification bias accounts for 32.6% of the TSLS estimate and exceeds the selection bias the IV strategy was designed to address.

---

## Repository Structure

```
.
├── notebooks/
│   └── analysis_star_tsls_late.ipynb      # Main empirical analysis notebook
├── paper/
│   └── project-star-tsls-late-paper.pdf   # Seminar paper
├── presentation/
│   └── project-star-tsls-late-presentation.pdf   # Presentation slides
├── README.md
└── requirements.txt
```

---

## Methods

### Estimators

The script implements and compares four estimators:

**1. OLS** — Naive benchmark assuming perfect compliance (equation 29 in the paper).

**2. TSLS** — Standard two-stage least squares using initial random assignment as instrument for realised class type (equation 30–31). Replicates Table VII of Krueger (1999).

**3. DDML — Partially Linear Regression** (Chernozhukov et al., 2018): targets βᵣᵢ꜀ₕ, the IV estimand under the theoretical satisfaction of the rich covariates condition. Random Forests partial out nuisance functions E[Y|X] and E[Z|X] nonparametrically, bypassing the functional form restriction.

**4. LATE/ACR** — two approaches:
  - *DDML IIVM*: Interactive IV Model via DoubleML. Targets the unconditional Average Causal Response without the variance-driven TSLS weighting (Słoczyński, 2024).
  - *IPSW*: Inverse Propensity Score Weighting using a Hajek ratio estimator (Tan, 2006; Frölich, 2007). Standard errors via cluster bootstrap (schools as clusters, B = 500).

### Rich Covariates Test

The script applies the **Ramsey RESET test** to the first-stage linear projection of the instrument onto covariates. Rejection indicates L[Z|X] ≠ E[Z|X], meaning the TSLS estimand is not weakly causal in the sense of Blandhol et al. (2022, Theorem 1).

### Specification Bias Decomposition

Two metrics quantify misspecification:
- **Relative specification bias**: |β_IV − β_rich| / |β_IV|
- **Specification vs. selection bias**: |β_IV − β_rich| / |β_OLS − β_rich|

When the latter exceeds 1, the distortion from linear covariate misspecification exceeds the endogeneity bias the IV strategy was designed to eliminate.

---

## Data

The dataset is the Tennessee STAR experiment as processed for Krueger (1999). It is available from:

- **ICPSR** — Study 6386, *Tennessee's Project STAR*
- The replication archive accompanying Krueger (1999), *Quarterly Journal of Economics* 114(2)

Place the cleaned Stata file at `./data/Cleandata.dta` before running. The raw data contain ~11,600 students across Kindergarten through Grade 3 (1985–1989).

**Treatment:** Small class (13–17 students) vs. regular class (22–25) or regular with teacher aide.  
**Outcome:** Percentile rank of average Stanford Achievement Test score (reading, math, word recognition).  
**Instrument:** Initial random assignment to class type at study entry.

---

## Requirements

| Package | Version |
|---------|---------|
| Python | ≥ 3.9 |
| pandas | any |
| numpy | any |
| scipy | any |
| statsmodels | any |
| linearmodels | any |
| scikit-learn | any |
| DoubleML | ≥ 0.7 |

Install all dependencies:

```bash
pip install -r requirements.txt
```

---

## Usage
Open and run:

```bash
# Place data at ./data/Cleandata.dta, then:
notebooks/analysis_star_tsls_late.ipynb
```
The notebook contains the full empirical workflow, including data preparation, estimation, diagnostic testing, and comparison of TSLS with alternative causal estimands.


---

## References

- Blandhol, C., Bonney, J., Mogstad, M., and Torgovitsky, A. (2022). *When is TSLS actually LATE?* NBER Working Paper 29709.
- Chernozhukov, V., et al. (2018). Double/debiased machine learning for treatment and structural parameters. *The Econometrics Journal*, 21(1), C1–C68.
- Imbens, G. W. and Angrist, J. D. (1994). Identification and estimation of local average treatment effects. *Econometrica*, 62(2), 467–475.
- Krueger, A. B. (1999). Experimental estimates of education production functions. *The Quarterly Journal of Economics*, 114(2), 497–532.
- Słoczyński, T. (2024). When should we (not) interpret linear IV estimands as LATE? Working paper.
- Tan, Z. (2006). A distributional approach for causal inference using propensity scores. *JASA*, 101(476), 1619–1637.

---

## License

MIT License. See `LICENSE` for details.
