---
title: "Longitudinal Data: Think Serial Correlation First, Random Effects Second"
author: Frank Harrell
date: '2022-03-15'
modified: '2022-03-16'
slug: re
categories: []
tags:
  - drug-evaluation
  - endpoints
  - measurement
  - RCT
  - regression
  - 2022
header:
  caption: ''
  image: ''
---
Random effects/mixed effects models shine for multi-level data such as measurements within cities within counties within states.  They can also deal with measurements clustered within subjects.  There are at least two contexts for the latter: rapidly repeated measurements where elapsed time is not an issue, and serial measurements spaced out over time for which time trends are more likely to be important.  An example of the first is a series of tests on a subject over minutes when the subject does not fatigue.  An example of the second is a typical longitudinal clinical trial where patient responses are assessed weekly or monthly.  For the first setup, random effects are likely to capture the important elements of within-subject correlation.  Not so much for the second setup, where serial correlation dominates and time ordering is essential.

A random effects model that contains only random intercepts, which is the most common use of mixed effect modeling in randomized trials, assumes that the responses within subject are exchangeable.  This can be seen from the statement of the linear mixed effects model with random intercepts.  For the $i$th subject assessed on the $j$th occasion we have $Y_{ij} = X_{i}\beta + u_{i} + \epsilon_{ij}$ where random effects $u$ might be assumed to have a normal distribution with mean zero and variance $\delta^2$.  Residuals $\epsilon$ are irreducible errors assumed to represent white noise and are all independent of one another.  The $\epsilon$s don’t know any subject boundaries.

Note from the linear mixed model statement that time plays no role in $u$ or $\epsilon$.  Time may play a role as a fixed effect in $X$, but not in the components encoding intra-subject correlation.  Shuffling the time order of measurements within subject does not affect the correlation (nor the final parameter estimates, if time is not part of $X$).  Thus the multiple measurements within subject are _exchangeable_ and the forward flow of time is not respected.  This induces a certain correlation structure within subject: the compound symmetric correlation structure.  A random intercept model assumes that the correlation between any two measurements on the same subject is unrelated to the time gap between the two measurements.  Compound symmetry does not fit very well for most longitudinal studies, which instead usually have a serial correlation structure in which the correlation between two measurements wanes as the time gap widens.  Serial correlations can be added on top of compound symmetry, but as this is not the default in `SAS PROC MIXED` this is seldom used in the pharmaceutical industry.

I’ve heard something frightening from practicing statisticians who frequently use mixed effects models. Sometimes when I ask them whether they produced a variogram to check the correlation structure they reply “what’s that?”.  A [variogram](https://en.wikipedia.org/wiki/Variogram) is a key diagnostic for longitudinal models in which the time difference between all possible pairs of measurements on the same subject is played against the covariance of the pair of measurements within subject.  These data are pooled over subjects and an average is computed for each distinct time gap occurring in the data, then smoothed.  See [RMS Course Notes](https://hbiostat.org/doc/rms.pdf) Section 7.8.2 Figure 7.4 for an example.

[Faes et al 2009](https://www.tandfonline.com/doi/abs/10.1198/tast.2009.08196) published a highly useful and intuitive paper on estimating the effective sample size for longitudinal data under various correlation structures.  They point out an interesting difference between a compound symmetric (CS) correlation structure and a first-order autoregressive serial correlation structure (AR(1)).  Under compound symmetry there is a limit to the information added by additional observations per subject, whereas for AR(1) there is no limit.  They explained this thus: "Under CS, every measurement within a cluster is correlated in the same way with all other measurements.  Therefore, there is a limit to what can be learned from a cluster and the additional information coming from new cluster members approaches zero with increasing cluster size.  In contrast, under AR(1), the correlation wanes with time lag.  So, with time gap between measurements tending to infinity, their correlation tends to zero and hence the additional information tends to that of a new, independent observation."

Random intercepts comprise $N$ parameters for $N$ subjects.  Even though the _effective_ number of parameters is smaller than $N$, the large number of parameters results in a computational burden and convergence issues.  Random intercept models are extended into random slope-and-intercept models and [random shape models](https://www.jstor.org/stable/27595574) but these entail even more parameters and may be harder to interpret.  In addition, when there is an absorbing state or a level $y$ of the response variable $Y$ such that when a subject has $Y\geq y$ she never recovers to $Y<y$, these situations [cannot be handled by random effects models](https://onlinelibrary.wiley.com/doi/10.1002/sim.9366).  When an incorrect correlation structure is assumed, $\delta^2$ and the effective number of parameters estimated may be large.  See [this](https://hbiostat.org/proj/covid19/orchid.html#markov-model-with-random-effects) for an example where modeling the correlation structure correctly made the random effects inconsequential.

The first model for longitudinal data was the growth curve model.  See [Wishart 1938](https://www.jstor.org/stable/2332221) and [Potthoff and Roy 1964](https://doi.org/10.1093/biomet/51.3-4.313).  Multivariate normality was assumed and no random effects were used.  Generalized least squares is based on these ideas, and can incorporate multiple types of correlation structures without including any random effects.  [Markov models](https://hbiostat.org/proj/covid19) (see especially [here](https://hbiostat.org/proj/covid19/ordmarkov.html) and [here](https://hbiostat.org/bib/markov.html) for references) are more general ways to incorporate a variety of correlation structures with or without random effects.  Markov models are more general because they easily extend to binary, nominal, ordinal, and continuous $Y$.  They are computationally fast and require only standard frequentist or Bayesian software until one gets to the post-model-fit stage of turning transition probabilities into state occupancy probabilities.  A first-order Markov process models transitions from one time period to the next, conditioning the transition probabilities on the response at the previous period as well as on baseline covariates.  To be able to use this model you must have the response variable assessed at baseline.  Responses at previous time periods are treated exactly like covariates in the period-to-period transition models.  Semiparametric models are natural choices for modeling the transitions, allowing $Y$ to be binary, ordinal, or continuous.  Multiple absorbing states can be handled.  Once the model is fitted, one uses a recursive matrix multiplication to uncondition on previous states, yielding current status probabilities, also called state occupancy probabilities.  An example of a first-order [proportional odds](https://hbiostat.org/bib/po) Markov transition model that has quite general application to longitudinal data is below.  Let $Y(t)$ be the response assessed at time period $t$, where $t=1,2,3,...$.

$$P(Y(t)\geq y | X, Y(t-1)) = \mathrm{expit}(\alpha_{y} + X\beta + f(Y(t-1), t))$$

Here the $\alpha$s are intercepts, and there are $k-1$ of them when $Y$ takes on $k$ distinct values.
$\mathrm{expit}(x)$ is $\frac{1}{1 + \exp(-x)}$ and the function $f$ expresses how you want to model the effect of the previous state.  This may require multiple parameters, all of which are treated just like $\beta$.  The strength of effect of $Y(t-1)$ goes along with the strength of the intra-subject correlation, and involvement of $t$ adds further flexibility in correlation patterms.

Generalized estimating equations (GEE) is a flexible way to model longitudinal responses, but it has some disadvantages: it is a large sample method; if does not use a full likelihood function so cannot be used in a Bayesian context; not being full likelihood the repeated observations are not properly "connected" to each other, so dropouts and missed visits must be missing completely at random, not just missing at random as full likelihood methods require.  Generalized least squares, Markov models when no random effects are added, and GEE are all examples of _marginal models_, marginal meaning in the sense of not being conditional on subject so not attempting to estimate individual subjects' trajectories.

Mixed effects models are indispensable when a goal is to estimate the outcome trajectory for an individual subject.  When the goal is instead to make group level estimates (e.g., treatment differences in trajectories) then one can do excellent analyses without using random effects.  Above all, don’t default to only using random intercepts to handle within-subject correlations of serial measurements.  This is unlikely to fit the correlation structure in play.  And it will not lead to the correct power calculation for your next longitudinal study.

## Other Resources

* [Generalized least squares](https://hbiostat.org/rms) RMS Chapter 7, including the table below
* [Markov ordinal longitudinal models](https://hbiostat.org/proj/covid19)
* [Markov state transition model references](http://hbiostat.org/bib/markov.html)
* [References on ordinal longitudinal analysis](http://hbiostat.org/bib/ordSerial.html)
* [Other reference resources](https://hbiostat.org/bib) including papers on longitudinal analysis for clinical researchers
* [Overview of semiparametric models](https://hbiostat.org/bib/po)

<img src="https://hbiostat.org/img/longitudinal.svg" alt="Longitudinal methods summary table">

