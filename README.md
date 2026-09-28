# BIEN

**BIEN** BIEN is a data−driven machine learning technique that enables predictor/model selection and parameter inference simultaneously. 

The method generates candidate covariate sets across elastic-net tuning parameters, evaluates the generated covariate sets using BIC after unpenalized refitting, and applies backward BIC selection to the best model.

## Installation

The function requires the [`glmnet`](https://cran.r-project.org/package=glmnet) package.

```r
install.packages("glmnet")
```

Download or clone this repository and source the function:
  
  ```r
source("R/bien.R")
```

## Usage

```r
fit <- bien(
  input = c("x1", "x2", "x3", "x4"),
  target = "y",
  data = dat
)
```

where:
  
* `input` is a character vector containing the predictor variable names.
* `target` is the name of the continuous outcome variable.
* `data` is a data frame containing the outcome and predictors.

## Output

The function returns a list containing:
  
* `model`: the final unpenalized linear regression model after backward BIC selection.
* `optimal`: the selected elastic-net `alpha` and `lambda` values.
* `grid`: the combinations of `alpha` and `lambda` evaluated during the search, together with the corresponding model size and BIC.

For example:
  
  ```r
fit$model
fit$optimal
fit$grid
```

Standard R functions can be used with the final model:
  
  ```r
summary(fit$model)
coef(fit$model)
BIC(fit$model)
formula(fit$model)
```

## Method


BIEN consists of five key components, which are briefly described below:
1)	Predictor set generation (Elastic Net): Using Elastic Net, a penalized regression approach, this component generates candidate set of predictors for a given pair of hyperparameter values, which jointly determine the magnitude and the form of regularization applied to control model complexity.
2)	Model estimation (maximum likelihood): Utilizing maximum likelihood estimation, this component estimates the strengths of associations corresponding to each candidate predictor set.  
3)	Model scoring (Bayesian Information Criterion): Candidate models are evaluated using the Bayesian Information Criterion, which balances goodness of fit and complexity to produce a ranking score. 
4)	Model search (truncated grid search): A truncated grid search is conducted to efficiently identify a targeted set of penalty levels and penalty forms to be explored during predictor set generation. 
5)	Model refinement (pruning): The selected model is subsequently refined using backward elimination, iteratively removing predictors that do not contribute significantly to the model.

## Example

```r
set.seed(123)

n <- 500

dat <- data.frame(
  x1 = rnorm(n),
  x2 = rnorm(n),
  x3 = rnorm(n),
  x4 = rnorm(n),
  x5 = rnorm(n)
)

dat$y <- 2 * dat$x1 - 1.5 * dat$x3 + rnorm(n)

fit <- bien(
  input = paste0("x", 1:5),
  target = "y",
  data = dat
)

summary(fit$model)

fit$grid
```

## Requirements

BIEN currently supports:
  
* Linear regression with a continuous outcome.
* Numeric predictors. Categorical predictors should be dummy-coded before being used in BIEN. 

The current implementation does not perform automatic missing-data handling.

## Citation

If you use BIEN in your research, please cite:
  
**Bagherzadeh-Khiabani F, et al. Associational Inference with Many Potential Covariates: Bayesian Information Criterion Elastic Net. Manuscript under review in BMC Medical Research Methodology.**  
## References

For the full BIEN algorithm of BIEN, see Figure 1 in the manuscript above.

For the conceptual framework of BIEN, see Supplementary Figure 4 in:

**Bagherzadeh-Khiabani, Farideh, et al. Longitudinal Patient-Reported Symptom Change Patterns and Prediction of Future Health-Related Quality of Life in Childhood Cancer Survivors: A Machine Learning Approach from the Childhood Cancer Survivor Study and the St. Jude Lifetime Cohort. Cancers 18.10 (2026): 1546.**

## Contact

Questions, bug reports, and suggestions can be submitted through the GitHub Issues page for this repository.
