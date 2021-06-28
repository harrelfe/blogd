---
title: Incorrect Covariate Adjustment May Be More Correct than Adjusted Marginal Estimates
author: Frank Harrell
date: '2021-06-28'
modified: ''
slug: robcov
tags:
  - 2021
  - generalizability
  - RCT
  - regression
link-citations: yes
summary: 'This  article provides a demonstration that the perceived non-robustness of nonlinear models for covariate adjustment in randomized trials may be less of an issue than the non-transportability of marginal so-called robust estimators.'
header:
  caption: ''
  image: ''
---

<!-- title: Non-Robustness of Covariate Adjustment is a Non-Issue<br><small>or</small><br>Non-Robust Covariate Adjustment May Be More Accurate Than Robust Marginal Covariate Adjustment<br><small>or</small><br>Incorrect Covariate Adjustment Is More Correct than Adjusted Marginal Estimation -->

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {
    skipTags: ['script', 'noscript', 'style', 'textarea', 'pre']
  }
});
</script>
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
</script>


<style>
.table {
  width: 50%;
}
</style>

# Background 

In a randomized clinical trial (RCT) it is typical for several participants' baseline characteristics to be much more predictive of the outcome variable Y than treatment is predictive of Y.  Covariate adjustment in an RCT gains power by making the analysis more consistent with the data generating model, i.e., by accounting for outcome heterogeneity due to wide distributions of baseline prognostic factors.  When Y is continuous and random errors have a normal distribution, it is well known that classical covariate adjustment improves power over an unadjusted analysis no matter how poorly the model fits.  Lack of fit makes the random errors larger, but not as large as omitting covariates entirely.  Nonlinear models such as logistic or Cox regression have no error term to absorb lack of fit, so lack of fit changes (usually towards zero) parameter estimates for all terms in the model, including the treatment effect.  But the most poorly specified model is one that assumes all covariate effects are nil, i.e., one that does not adjust for covariates.  Even ill-fitting models will provide more useful treatment effect estimates than a model that ignores covariates.  See [this](https://hbiostat.org/bbr/md/ancova.html) for details and references about covariate adjustment in RCTs.

Because they are more easily connected to causal inference, many statisticians and epidemiologists like marginal effect estimates.  Such estimates can be adjusted for covariate imbalance but they can hide outcome heterogeneity.  As seen with numerical examples below, an adjusted marginal estimate is essentially just the crude marginal estimate, except in the situation where there is an imbalance in a pre-specified baseline covariate that is not counter-balanced by the opposite imbalance in another pre-specified covariate.  Marginal adjusted estimates can adjust for such covariate imbalances, which can occur primarily in small studies in which covariate balance was not ensured by design.   But importantly, even in the case of perfect balance, adjusted marginal estimation allows one to estimate the true standard error by recognizing that within-treatment risks are heterogeneous.  Here is an example for a single treatment arm study.

Suppose that a sample of 100 subjects had 40 females and 60 males, and that 10 of the females and 30 of the males had disease.  The marginal estimate of the probability of disease is 40/100 = 0.4, and the variance of the estimator assuming constant risk (i.e., assuming risk for females = risk for males) is `\(\frac{0.4\times 0.6}{100} = 0.0024\)`.  But with knowledge of each person's sex we compute the variance using sex-specific risk estimates as follows.  The marginal estimate combines the sex-specific estimates according to the sex distribution in the sample (a hint that what is about to happen doesn't apply to the population).  This estimate is `\(0.4 \times 0.25 + 0.6 \times 0.5 = 0.4\)`, identical to the marginal estimate.  But the true and estimated variances are not the same as that computed in the absence of knowledge of subjects' sex.  The estimated variance is `\(0.4^{2}\times \frac{0.25 \times 0.75}{40} + 0.6^{2}\times \frac{0.5 \times 0.5}{60} = 0.00225\)` which is smaller than 0.0024 due to the fact that the correct variance recognizes that males and females do not have the same outcome probabilities.  

One could say that marginal stratified/adjusted estimation doesn't gain anything (other than occasional adjustment for chance baseline imbalances when doing marginal estimation for differences), but it makes up for using the wrong variance formulas all along when computing crude marginal estimates.  Any time you see `\(\frac{p \times (1-p)}{n}\)` for the variance of a proportion, remember that this formula assumes that `\(p\)` applies equally to all `\(n\)` subjects.

Marginal adjusted treatment differences work in the same way.  The adjusted marginal approach can be used to derive other interesting quantities such as marginal adjusted hazard or odds ratios, but for the risk difference scale, marginal adjusted estimates are essentially simple marginal differences but with smaller and more honest standard errors that are computed by recognizing that different subjects have different risks, even if on the same treatment.  As opposed to estimating relative treatment effect conditional on covariate values, marginal estimands that account for covariates (outcome heterogeneity) are based on differences in average predicted values.

Advocates of marginal treatment effect estimates for nonlinear models such as [Benkeser, Díaz, Luedtke, Segal, Scharfstein, and Rosenblum](https://pubmed.ncbi.nlm.nih.gov/32978962)[^ben] cite as one of the main advantages of the method its robustness to model misspecification.  In their approach, gains in efficiency from covariate adjustment can result, and certain types of model lack of fit are in effect averaged out.  But it is this averaging that makes the resulting treatment effect in an RCT hard to interpret.  Their marginal treatment effect uses a regression model as a stepping stone to an effect estimator in which estimates are made on a probability scale and averaged.  For example, if Y is binary, one might fit a binary logistic regression model on the baseline covariates and the treatment variable (Benkeser et al. prefer to fit separate models by treatment, omitting the treatment indicator from each.)  Then for every trial participant one obtains, for example, the estimated probability that Y=1 under treatment A, then for the same covariate values, the probability that Y=1 under treatment B.  The estimates are averaged over all subjects and then subtracted to arrive at a marginal treatment effect estimate.

There are problems with this approach:

* it changes the estimand to something that is not applicable to individual patient decision making
* it estimates the difference in probabilities over the distribution of _observed covariate values_ and is dependent on the covariate distributions of participants actually entering the trial 
* this fails to recognize that RCT participants are not a random sample from the target population; RCTs are valid when their designs result in representative _treatment effects_, and they do not require representative _participants_ 
* to convert marginal estimates to estimates that are applicable to the population requires the RCT sample to be a probability sample and for the sampling probabilities from the population to be known
* these sampling weights are almost never known; RCTs are almost always based on convenience samples
* the marginal approach makes assessment of differential treatment effect (interactions) difficult

The claim that ordinary conditional estimates are not robust also needs further exploration.  Here I take a simple example where there are two strong covariates---age and sex---and age has a very nonlinear true effect on the log odds that Y=1.   Suppose that the investigators do not know much about flexible parametric modeling (the use of regression splines, etc.) but assume that age has a linear effect, and does the covariate adjustment assuming linearity.  Suppose also that sex is omitted from the model.  What happens?  Is the resulting conditional odds ratio (OR) for treatment valid?  We will see that it is not exactly correct, but that it can be more valid than the marginal estimate.  In regression analysis one can never get the model "correct."  Instead, modeling is a question of approximating the effects of baseline variables that explain outcome heterogeneity.  The better the model the more complete the conditioning and the more accurate the patient-specific effects that are estimated from the model.  Omitted covariates or under-fitting strong nonlinear relationships results in effectively conditioning on only part of what one would like to know.  This partial conditioning still results in useful estimates, and the estimated treatment effect will be somewhere between a fully correctly adjusted effect and a non-covariate-adjusted effect.

# Simulation Model

Assume a true model as specified below:

`$$\Pr(Y = 1 | T,A,S) = \mathrm{expit}(\alpha + \beta_{1} [T=B] + \beta_{2} A + \beta_{3} (A - 65)_{+} + \\ \beta_{4} [S=\mathrm{male}])$$`

where `\(A\)` is age, treatment `\(T\)` is A or B, `\(S\)` is sex, and `\(\mathrm{expit}\)` denotes the inverse of the logit function, i.e., `\(\frac{1}{1 + \exp(-u)}\)`.  `\(u_{+}\)` is defined as `\(u\)` if `\(u > 0\)` and 0 otherwise, and `\([u]\)` is 1 if `\(u\)` is true and 0 otherwise.  We assume the effect of using treatment B instead of treatment A raises the odds that Y=1 by a factor of 2.0, i.e., the treatment effect is OR=2 so that `\(\beta_{1}=\log(2)=0.6931\)`.  The age effect is a linear spline with slope change at 65y.  Assume the true age effect is given by the initial slope of `\(\beta_{2}=0.01\)` and the increment in slope starting at age 65 is `\(\beta_{3}=0.07\)`.  Assume that `\(\beta_{4}=0.5\)` and `\(\alpha=0\)`.  Then the true relationships are given in the following graph.


```r
a <- 0; b1 <- log(2); b2 <- 0.01; b3 <- 0.07; b4 <- 0.5
w <- expand.grid(age=seq(35, 85, length=100), sex=c('female', 'male'), tx=c('A','B'))
w <- transform(w, xb=a + b1 * (tx == 'B') + b2 * age + b3 * pmax(age - 65, 0) +
               b4 * (sex == 'male'))

wf <- subset(w, sex=='female')
ggplot(w, aes(x=age, y=xb, color=tx, linetype=sex)) + geom_line() + ylab('logit')
```

<img src="/post/robcov_files/figure-html/truemod-1.png" width="672" />

Simulate a clinical trial from this model, with 2000 participants in each treatment arm.  Assume that the age distribution for those volunteering for the trial has a mean of 70 and a standard deviation of 8.


```r
simdat <- function(n, mage, sdage=8, fem=0.5, a = 0,
                   b1 = log(2), b2 = 0.01, b3 = 0.07, b4 = 0.5) {
  age   <- rnorm(n, mage, sdage)
  sex   <- sample(c('female', 'male'), n, replace=TRUE, prob=c(fem, 1. - fem))
  tx    <- c(rep('A', n/2), rep('B', n/2))
  logit <- a + b1 * (tx == 'B') + b2 * age + b3 * pmax(age - 65, 0) +
	         b4 * (sex == 'male')
  prob  <- plogis(logit)
  y     <- ifelse(runif(n) <= prob, 1, 0)
  data.frame(age, sex, tx, y)
}
set.seed(1)
d <- simdat(n=4000, mage=70)
dd <- datadist(d); options(datadist='dd')
```

First fit the correct structure---a linear spline in age and with the sex variable included---to make sure we can approximately recover the truth with this fairly large sample size.


```r
f <- lrm(y ~ tx + lsp(age, 65) + sex, data=d)
f
```


 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx + lsp(age, 65) + sex, data = d)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 4000</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 196.93</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.080</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.664</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 676</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 4</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.732</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.329</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 3324</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 2.080</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.329</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 2×10<sup>-12</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.093</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.092</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.134</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -0.2723</td>
<td style='min-width: 7em; text-align: right;'> 0.8159</td>
<td style='min-width: 7em; text-align: right;'>-0.33</td>
<td style='min-width: 7em; text-align: right;'>0.7385</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=B</td>
<td style='min-width: 7em; text-align: right;'>  0.6550</td>
<td style='min-width: 7em; text-align: right;'> 0.0885</td>
<td style='min-width: 7em; text-align: right;'> 7.41</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.0150</td>
<td style='min-width: 7em; text-align: right;'> 0.0132</td>
<td style='min-width: 7em; text-align: right;'> 1.14</td>
<td style='min-width: 7em; text-align: right;'>0.2539</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age'</td>
<td style='min-width: 7em; text-align: right;'>  0.0597</td>
<td style='min-width: 7em; text-align: right;'> 0.0190</td>
<td style='min-width: 7em; text-align: right;'> 3.15</td>
<td style='min-width: 7em; text-align: right;'>0.0016</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>sex=male</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.4661</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0875</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 5.33</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>

```r
ggplot(Predict(f, age, tx, sex='female')) +
  geom_line(data=wf, aes(x=age, y=xb, color=tx, linetype=I(2))) +
  labs(title="Fitted quadratic model and true model for females",
       caption="Solid lines:fitted\nDashed lines:truth")
```

<img src="/post/robcov_files/figure-html/fittrue-1.png" width="672" />

Now fit the incorrect model that assumes age is linear and omits sex.  Also compute Huber-White robust variance-covariance estimates.


```r
g <- lrm(y ~ tx + age, data=d, x=TRUE, y=TRUE)
g
```


 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx + age, data = d, x = TRUE, y = TRUE)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 4000</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 158.98</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.065</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.649</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 676</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 2</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.623</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.299</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 3324</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 1.865</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.299</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 5×10<sup>-7</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.084</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.084</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.135</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -2.3459</td>
<td style='min-width: 7em; text-align: right;'> 0.3657</td>
<td style='min-width: 7em; text-align: right;'>-6.41</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=B</td>
<td style='min-width: 7em; text-align: right;'>  0.6434</td>
<td style='min-width: 7em; text-align: right;'> 0.0881</td>
<td style='min-width: 7em; text-align: right;'> 7.31</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.0530</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0053</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 9.96</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>

```r
ggplot(Predict(g, age, tx)) +
  geom_line(data=wf, aes(x=age, y=xb, color=tx, linetype=I(2))) +
  labs(title="Fitted linear-in-age model ignoring sex, and true model for females",
       caption="Solid lines:fitted\nDashed lines:truth")
rob <- robcov(g)
rob
```


 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx + age, data = d, x = TRUE, y = TRUE)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 4000</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 158.98</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.065</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.649</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 676</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 2</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.623</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.299</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 3324</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 1.865</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.299</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 5×10<sup>-7</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.084</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.084</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.135</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -2.3459</td>
<td style='min-width: 7em; text-align: right;'> 0.3524</td>
<td style='min-width: 7em; text-align: right;'>-6.66</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=B</td>
<td style='min-width: 7em; text-align: right;'>  0.6434</td>
<td style='min-width: 7em; text-align: right;'> 0.0882</td>
<td style='min-width: 7em; text-align: right;'> 7.29</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.0530</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0051</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>10.37</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>
<img src="/post/robcov_files/figure-html/fitlin-1.png" width="672" />
The poorly fitting model profited from balanced in the (unknown to it) sex distribution.
The robust standard error estimates did not change the (improperly chosen) model-based standard errors very much in this instance.

Now fit an unadjusted model.


```r
h <- lrm(y ~ tx, data=d)
h
```


 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx, data = d)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 4000</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 55.68</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.023</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.578</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 676</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 1</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.320</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.157</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 3324</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 1.377</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.309</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 2×10<sup>-12</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.044</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.044</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.139</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> 1.3069</td>
<td style='min-width: 7em; text-align: right;'> 0.0546</td>
<td style='min-width: 7em; text-align: right;'>23.93</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=B</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.6390</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0869</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 7.35</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>

Treatment effect estimates and SEs are summarized below.

| Model | `\(\hat{\beta}_{1}\)` | SE | OR |
|-------|-------------|----|------|
|Correct   | 0.66 | 0.09 | 1.92 |
|Linear    | 0.64 | 0.09 | 1.90 |
|Unadjusted| 0.64 | 0.09 | 1.89 |

The difference between fitting the correct and the incorrect models usually results in larger changes in treatment effect estimates and/or standard errors than what we see here, but the main points of this exercise are (1) how far the unadjusted treatment effect estimate is from the true value used in data generation, and (2) the difficulty in interpreting marginal estimates.

# Adjusted Marginal Estimates

The crude marginal proportions of Y=1 stratified by treatment A, B are 0.787 and 0.875.  A slight simplification of the Benkeser estimate (we are not fitting separate models for treatments A and B) is computed below.


```r
marg <- function(fit, data, sx=FALSE) {
  prop <- with(data, tapply(y, tx, mean))
  # Compute estimated P(Y=1) as if everyone was on treatment A
  da <- data.frame(tx='A', age=data$age)
  if(sx) da$sex <- data$sex
  pa <- plogis(predict(fit, da))
  # Compute the same as if everyone was on treatment B
  db <- data.frame(tx='B', age=data$age)
  if(sx) db$sex <- data$sex
  pb <- plogis(predict(fit, db))
  odds <- function(x) x / (1. - x)
  ma <- mean(pa); mb <- mean(pb)
  z <- rbind('Marginal covariate adjusted'=c(ma, mb, mb - ma, odds(mb)/odds(ma)),
        'Observed proportions'=c(prop, prop[2] - prop[1], odds(prop[2])/odds(prop[1])))
  colnames(z) <- c('A', 'B', 'B - A', 'OR')
  round(z, 3)
}
marg(g, d)
```

```
                                A     B B - A    OR
Marginal covariate adjusted 0.788 0.874 0.086 1.871
Observed proportions        0.787 0.875 0.088 1.895
```
In this instance, the marginal B:A odds ratio happened to almost equal the true conditional OR, which will not be the case in general. 
For comparison, compute marginal estimates for the correctly fitted model.


```r
marg(f, d, sx=TRUE)
```

```
                                A     B B - A    OR
Marginal covariate adjusted 0.788 0.875 0.087 1.883
Observed proportions        0.787 0.875 0.088 1.895
```

The marginal estimates are very close to the raw proportions, but as Benkeser et al discussed, they have an advantage over crude estimates in that their standard errors are smaller.  Estimates from the incorrect covariate model are virtually the same as from the correct model.

The question now is how to interpret the 0.087 estimated marginal difference in P(Y=1) between treatments.  This difference is a function of all of the values of age observed in the data.  It is specific to the participants volunteering to be in our simulated clinical trial.  Without selecting a probability sample from the population (i.e., without relying on volunteerism), we have no way to weight the individual P(Y=1) estimates to the population.

How does the marginal difference apply to a clinical population in which the age distribution has a mean that is 15 years younger than the volunteers from which our trial participants were drawn and that instead of a 50:50 sex distribution has 0.65 females?  We simulate such a sample.


```r
set.seed(2)
d <- simdat(4000, mage=55, fem=0.65)
f <- lrm(y ~ tx + lsp(age, 65) + sex, data=d)
g <- lrm(y ~ tx + age, data=d)
marg(g, d)
```

```
                                A     B B - A    OR
Marginal covariate adjusted 0.678 0.807  0.13 1.996
Observed proportions        0.677 0.807  0.13 1.997
```

```r
marg(f, d, sx=TRUE)
```

```
                                A     B B - A    OR
Marginal covariate adjusted 0.676 0.808 0.132 2.019
Observed proportions        0.677 0.807 0.130 1.997
```

Traditional covariate adjustment with a misspecified model managed to correctly estimate the treatment effect:


```r
lrm(y ~ tx + age, data=d)
```


 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx + age, data = d)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 4000</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 97.22</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.035</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.597</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 1030</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 2</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.405</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.194</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 2970</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 1.500</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.194</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 2×10<sup>-11</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.076</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.074</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.187</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> 0.0270</td>
<td style='min-width: 7em; text-align: right;'> 0.2558</td>
<td style='min-width: 7em; text-align: right;'>0.11</td>
<td style='min-width: 7em; text-align: right;'>0.9160</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=B</td>
<td style='min-width: 7em; text-align: right;'> 0.6926</td>
<td style='min-width: 7em; text-align: right;'> 0.0743</td>
<td style='min-width: 7em; text-align: right;'>9.32</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>age</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0130</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0046</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>2.84</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>0.0045</td>
</tr>
</tbody>
</table>

```r
lrm(y ~ tx + age + sex, data=d)
```


 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx + age + sex, data = d)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 4000</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 150.88</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.054</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.623</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 1030</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 3</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.520</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.246</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 2970</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 1.681</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.246</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 5×10<sup>-9</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.095</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.094</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.184</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -0.1607</td>
<td style='min-width: 7em; text-align: right;'> 0.2586</td>
<td style='min-width: 7em; text-align: right;'>-0.62</td>
<td style='min-width: 7em; text-align: right;'>0.5343</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=B</td>
<td style='min-width: 7em; text-align: right;'>  0.7128</td>
<td style='min-width: 7em; text-align: right;'> 0.0749</td>
<td style='min-width: 7em; text-align: right;'> 9.52</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.0128</td>
<td style='min-width: 7em; text-align: right;'> 0.0046</td>
<td style='min-width: 7em; text-align: right;'> 2.79</td>
<td style='min-width: 7em; text-align: right;'>0.0053</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>sex=male</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.5809</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0811</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 7.16</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>

```r
lrm(y ~ tx + lsp(age, 65) + sex, data=d)
```


 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx + lsp(age, 65) + sex, data = d)
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Rank Discrim.<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 4000</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 162.36</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.058</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.624</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 1030</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 4</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.534</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.247</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 2970</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 1.706</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.248</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 2×10<sup>-5</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.096</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.095</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.184</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> 0.3295</td>
<td style='min-width: 7em; text-align: right;'> 0.2996</td>
<td style='min-width: 7em; text-align: right;'>1.10</td>
<td style='min-width: 7em; text-align: right;'>0.2714</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tx=B</td>
<td style='min-width: 7em; text-align: right;'> 0.7148</td>
<td style='min-width: 7em; text-align: right;'> 0.0749</td>
<td style='min-width: 7em; text-align: right;'>9.54</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'> 0.0032</td>
<td style='min-width: 7em; text-align: right;'> 0.0055</td>
<td style='min-width: 7em; text-align: right;'>0.58</td>
<td style='min-width: 7em; text-align: right;'>0.5587</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age'</td>
<td style='min-width: 7em; text-align: right;'> 0.1040</td>
<td style='min-width: 7em; text-align: right;'> 0.0328</td>
<td style='min-width: 7em; text-align: right;'>3.17</td>
<td style='min-width: 7em; text-align: right;'>0.0015</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>sex=male</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.5824</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0812</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>7.17</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>

Now instead of an increase in the probability of Y=1 due to treatment B in the mean age 70 group of 0.086 we have an increase of 0.130 in the younger and more female general clinical population that has lower risk.  The 0.086 estimate no longer applies.

# Frequentist Operating Characteristics

## Type I

One may worry that the parametric model that falsely assumed linear age and zero sex effects has poor frequentist operating characteristics.   Let's explore the type I assertion probability `\(\alpha\)` of the linear-in-age logistic model omitting sex, by simulating 5000 trials just like our original trial but with a zero age and sex-conditional treatment effect.  We consider the ordinary Wald statistic for treatment, and the Huber-White robust Wald statistic.  Instead of using sample sizes of 4000 we use 600.  Also compute `\(\alpha\)` using the actual standard errors for the logistic model, and run the likelihood ratio `\(\chi^2\)` test for treatment based on the incorrect model.  Compute `\(\alpha\)` for the marginal adjusted effect test assuming normality and by using the true standard error (to within simulation error, that is).


```r
simoc <- function(m=5000, b1=0., n=600) {
  beta <- betac <- v1 <- v2 <- vc <- margdiff <- lr <- numeric(m)
  w <- 'tx=B'
  for(i in 1 : m) {
    d <- simdat(n, mage=60, b1=b1)
    f <- lrm(y ~ tx + age, data=d, x=TRUE, y=TRUE)
    g <- robcov(f)
    h <- lrm(y ~ tx + lsp(age, 65) + sex, data=d)
    u <- lrm(y ~ age, data=d)
    
    beta[i]  <- coef(f)[w]
    v1[i]    <- vcov(f)[w, w]
    v2[i]    <- vcov(g)[w, w]
    betac[i] <- coef(h)[w]
    vc[i]    <- vcov(h)[w, w]
    margdiff[i] <- marg(f, d)[1, 'B - A']
    lr[i]    <- lrtest(u, f)$stats['L.R. Chisq']
  }
  print(ggfreqScatter(sqrt(v1), sqrt(v2)) + geom_abline(slope=1, intercept=0) +
    xlab('Model-based S.E.') + ylab('Robust S.E.'))
  pow <- function(beta, v) mean(abs(beta / sqrt(v)) > qnorm(0.975))
  r <- rbind('Usual LRM'            = c(SE=sqrt(mean(v1)), Power=pow(beta,     v1)),
             'Robust SE'            = c(SE=sqrt(mean(v2)), Power=pow(beta,     v2)),
             'Actual SE'            = c(SE=sd(beta),       Power=pow(beta,     var(beta))),
             'Correct model, usual' = c(SE=sd(betac),      Power=pow(betac,    vc)),
             'Incorrect model, LR'  = c(SE=NA,             Power=mean(lr > qchisq(0.95, 1))),
             'Marginal'             = c(SE=sd(margdiff),   Power=pow(margdiff, var(margdiff))))
  round(r, 3)
}
set.seed(7)
simoc(b1=0.)
```

```
                        SE Power
Usual LRM            0.182 0.045
Robust SE            0.182 0.045
Actual SE            0.180 0.048
Correct model, usual 0.183 0.046
Incorrect model, LR     NA 0.047
Marginal             0.036 0.049
```

<img src="/post/robcov_files/figure-html/simalpha-1.png" width="672" />

Model-based treatment effect standard errors are indistinguishable from robust standard errors in this example.  For both types of Wald statistics for testing the treatment effect, `\(\alpha\)` was estimated to be very slightly **below** the nominal 0.05 level.
The significant lack of fit caused by assuming that (1) a very strong covariate (age) is linear and (2) the sex effect is zero did not harm the frequentist assertion probability under the null.  The adjusted marginal method had an accurate `\(\alpha\)`, at least when the standard error did not need to be estimated from the data.

## Type II

Now consider power to detect a treatment OR of 1.75 for n=600.  Give the adjusted marginal method the benefit of not having to estimate the standard error of the difference in average probabilities, by using the standard deviation of observed point estimates over the simulation.  For ordinary logistic  covariate adjustment we include both the correct and the incorrect models.  For the marginal method the incorrect model is used.


```r
set.seed(8)
simoc(b1=log(1.75))
```

```
                        SE Power
Usual LRM            0.197 0.808
Robust SE            0.197 0.809
Actual SE            0.200 0.794
Correct model, usual 0.202 0.813
Incorrect model, LR     NA 0.812
Marginal             0.035 0.809
```

<img src="/post/robcov_files/figure-html/simbeta-1.png" width="672" />

Ordinary covariate adjustment resulted in slightly better power (0.813 vs. 0.808) when the correct model was used.  The power of the adjusted marginal comparison (0.809) was virtually the same as these, when the standard error did not need to be estimated.

# Conclusion

The marginal treatment effect estimate involves averaging of unlikes, i.e., it hides the outcome heterogeneity that dictates that the risks of outcomes vary systematically by strong baseline covariates.  This makes the marginal estimate sample-specific, difficult to interpret, and prevents it from applying to target populations.  And critics of traditional covariate conditional models focus on possible non-robustness of nonlinear regression models.  As exemplified in simulations shown here, such criticisms may be unwarranted.

If our simulation results hold more generally, the issue with adjusted marginal estimates is more with estimating magnitudes of treatment effectiveness than with statistical power.  But ordinary covariate adjustment with ill-fitting nonlinear models may be just as powerful as the two-stage robust marginal procedure, while controlling `\(\alpha\)`.

Risk differences are clinically relevant measures of treatment effect.  But because of extreme baseline risk-dependent heterogeneity of risk differences, risk differences should be **covariate-specific** and not averaged.  This is discussed in more detail [here](/post/rdist).

# References

[^ben]: Benkeser D, Díaz I, Luedtke A, Segal J, Scharfstein D, Rosenblum M (2020): Improving precision and power in randomized trials for COVID-19 treatments using covariate adjustment, for binary, ordinal, and time-to-event outcomes.  To appear in _Biometrics_, DOI:10.1111/biom.13377.
