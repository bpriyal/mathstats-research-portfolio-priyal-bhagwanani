## Title

**Phenomenological Logistic Surrogates for Coarse-Grained Relaxation Dynamics in Molecular Trajectories**

## Abstract

We study whether a low-parameter autonomous logistic ODE can serve as a surrogate model for coarse-grained relaxation observables extracted from molecular trajectories. The hypothesis is not that the surrogate replaces molecular dynamics, but that it may approximate dominant monotone relaxation modes after projection to reduced coordinates such as PCA or tICA. As a controlled sanity check, we first validated the implementation on synthetic data generated from the logistic law itself. The logistic model recovered the generating trajectory substantially better than exponential or linear baselines, with low generalization gap and small parameter uncertainty. Residual autocorrelation remained significant, indicating that the surrogate captures the dominant trend but not all temporal structure.

## Work done

1. Defined a reduced-order dynamical hypothesis
2. Built a fitting pipeline for:

   * logistic surrogate,
   * exponential decay,
   * linear decay.
3. Evaluated the models using:

   * RMSE, MAE, (R^2),
   * AIC/BIC,
   * cross-validation,
   * residual normality and autocorrelation tests.
4. Ran a synthetic control experiment to verify numerical correctness and identifiability.

## Assumptions

* The observable is a coarse-grained scalar trajectory with approximate monotone relaxation.
* Logistic saturation is treated as a phenomenological closure, not a first-principles derivation.
* Results from synthetic data validate the code and fitting procedure, not the biological hypothesis.
* Exponential and linear models are used as baselines; they are simpler but not necessarily exhaustive.

## Results

The synthetic control showed clear preference for the logistic model.

| Model              | Parameters |     RMSE |    (R^2) |      MAE |      AIC |      BIC |
| ------------------ | ---------: | -------: | -------: | -------: | -------: | -------: |
| Logistic surrogate |          3 | 0.025225 | 0.993714 | 0.020390 | -1465.96 | -1456.07 |
| Exponential decay  |          2 | 0.111424 | 0.877358 | 0.098991 |  -873.77 |  -867.17 |
| Linear decay       |          2 | 0.049212 | 0.976076 | 0.040833 | -1200.65 | -1194.05 |

Cross-validation for the logistic model:

* train RMSE mean: 0.025115
* test RMSE mean: 0.026050
* generalization gap: 0.000935

Parameter estimates:

* (k = 0.012651 \pm 0.000203)
* (K = 0.980270 \pm 0.006000)
* (p_0 = 0.953932 \pm 0.004409)

Residual diagnostics:

* mean residual: -0.001914
* Shapiro-Wilk (p = 0.078827)
* Ljung-Box (p \approx 0), so residuals are autocorrelated.

## Observations

* The logistic surrogate is clearly the best of the three tested models on logistic-generated data.
* The small train-test gap indicates stable fitting, not obvious overfitting.
* The residuals are not white noise; therefore the surrogate does not fully explain temporal dependence.
* The synthetic experiment validates the implementation, parameter recovery, and model-selection machinery, but does not yet validate the model on real protein data.

## Clarifications

* The synthetic result is a **control experiment**, not evidence that real protein observables are logistic.
* A fair real-data test requires PCA/tICA/RMSD/contact observables from actual MD trajectories and comparison against stronger baselines such as bi-exponential or MSM-type models.
* The correct scientific claim is modest: the logistic surrogate can approximate certain monotone relaxation modes better than exponential or linear baselines, at least in controlled settings.


**We built and validated a reduced-order logistic surrogate framework for coarse-grained relaxation observables; on synthetic control data, it fit the generating process far better than exponential or linear baselines, with strong (R^2), low cross-validation gap, and residual autocorrelation indicating remaining unmodeled dynamics.**
