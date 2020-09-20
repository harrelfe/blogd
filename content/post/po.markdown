                                                                                                                ---
title: Violation of Proportional Odds is Not Fatal
author: Frank Harrell
date: '2020-09-20'
modified: ''
slug: po
tags:
  - 2020
  - accuracy-score
  - RCT
  - regression
  - hypothesis testing
  - metric
link-citations: yes
summary: 'Many researchers worry about violations of the proportional hazards assumption when comparing treatments in a randomized study.  Besides the fact that this frequently makes them turn to a much worse approach, the harm done by violations of the proportional odds assumption usually to not prevent the proportional odds model from providing a reasonable treatment effect assessment.'
header:
  caption: ''
  image: ''
---

<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>



<p class="rquote">Clearly, the dependence of the proportional odds model on the assumption of proportionality can be overstressed.  Suppose that two different statisticians would cut the same three-point scale at different cut points. It is hard to see how anybody who could accept either dichotomy could object to the compromise answer produced by the proportional odds model. — <a href="https://onlinelibrary.wiley.com/doi/abs/10.1002/sim.3603">Stephen Senn</a>
</p>

# Background 

The Wilcoxon Mann-Whitney (WMW) two-sample rank-sum test for comparing two groups on a continuous or ordinal outcome variable Y is a much liked statistical test, for good reason.  It is robust to 'outliers' and is virtually as powerful as the two-sample t-test even if that test's normality assumption holds.  The k-sample generalization of the Wilcoxon test is the Kruskal-Wallis test, and a regression model generalization of both the Wilcoxon and Kruskal-Wallis tests is the proportional odds (PO) semiparametric regression model for ordinal or continuous Y.  The PO model has advantages of the WMW and Kruskal-Wallis tests in that it can handle covariate adjustment and allow for arbitrarily many ties in Y all the way to the case where Y is binary, at which point the PO model is the binary logistic model.

The WMW test involves the Mann-Whitney U statistic.  U statistics were developed by [Hoeffding](https://projecteuclid.org/euclid.aoms/1177730196) in 1948.  A U statistic is an average over all possible pairs, triplets, quadruplets, etc., of observations of a _kernel_.  For WMW the kernel is a 0/1 indicator signifying whether a randomly chosen observation from treatment group B is larger than a randomly chosen observation in group A.  So the Wilcoxon test tests whether values of Y in group B tend to be larger than those in group A, i.e., tests for _stochastic ordering_.  The U statistic summary measures the degree to which Y values from B are separated from those in A, with a value of 0.5 indicating no tendency for values if B to be greater or less than those in A.  This statistic is a _concordance probability_, also called a _c-index_.  It is a measure of discrimination that is also equal to the area under a receiver operating characteristic curve.

For binary, ordinal, or continuous variables X and Y, rank correlation coefficients are excellent measures for quantifying the degree of association between X and Y.  When X is binary such as a treatment indicator, rank correlation measures the separation of Y values that is apparently explained by treatment group X.  The rank correlation coefficient that corresponds to the Wilcoxon test is Somers' `\(D_{yx}\)` where the order of `\(yx\)` signifies that ties in x are ignored and ties in y are not.  For comparing treatments A and B we do not draw a random pair of observations from the same treatment, but only consider observations in different treatment groups (drop ties in x).  In the binary outcome situation, on the other hand, we use `\(D_{xy}\)`.

`\(D_{yx}\)` is 0 if the c-index equals 0.5, i.e., there is no separation in Y values that can be explained by the treatment X.  When c=0, meaning that all Y values for treatment A are larger than all Y values for B, `\(D_{yx}=-1\)`, and when c=1, meaning that all Y values for treatment B are larger than all values for treatment A (perfect separation in the opposite direction as when c=0), `\(D_{yx}=1\)`.  There is a perfect correspondence between the MWM test, the concordance probability c, and Somers' `\(D_{yx}\)`, i.e., you can perfectly compute the other two given any one of these indexes.  `\(D_{yx}\)` is also the difference between the probability of concordance of X and Y and the probability of discordance.

# Relationship Between Log Odds Ratio and Rank Correlation

We use concordance probabilities or `\(D_{yx}\)` without regard to the proportional odds (PO) assumption, and find them quite reasonable summaries of the degree to which Y increases when X increases.  How then is the c-index related to the log odds ratio in the PO model whether or not the PO assumption is satisfied?  There is no closed form solution for the maximum likelihood estimate `\(\hat{\beta}\)` for treatment group in the PO model, but we can run a large number of simulations to describe the extent to which `\(\hat{\beta}\)` tells us the same thing as `\(c\)` or Somers' rank correlation even when PO does not hold.  Consider samples sizes of 10, 11, ..., 100 and 1000 and take 20 random samples at each sample size.  For each sample, take the treatment assignment `x` as a random sample of 0 and 1 each with probability 0.5, and take the Y values as a sample with replacement of the integers 1, ..., n when the sample size is n.  Sampling with replacement will create a variety of ties in the data.  For each sample compute the concordance probability `cstat` and the maximum likelihood estimate for the `x` effect in the PO model.  Because of the presence of sufficient randomness in the data generated (including large observed treatment effects in samples where there is none in the population), proportional odds will be apparently violated many times as judged by sample values.


```r
ns <- c(10:100, 1000)
d <- expand.grid(n = ns, m=1 : 20)
N <- nrow(d)
cstat <- beta <- numeric(N)
set.seed(5) 
for(i in 1 : N) {
  n <- d[i, 'n']
  x <- sample(0 : 1, n, replace=TRUE)
  y <- sample(1 : n, n, replace=TRUE)
  cstat[i] <- somers2(y, x)['C']
  # eps: stricter convergence criterion
  b <- coef(orm(y ~ x, eps=0.000001, maxit=25))
  beta[i] <- b[length(b)]
}
```

```r
plot(cstat, beta)
plot(qlogis(cstat), beta)
```

<img src="/post/po_files/figure-html/sim-1.png" width="672" /><img src="/post/po_files/figure-html/sim-2.png" width="672" />

A very strong and linear relationship is found in the second graph above.  I expected the second graph to be more linear, because the logit transformation of the concordance probability has an unlimited range just as the log odds ratio does.  We can approximate `\(\hat{\beta}\)` with a linear model in the logit of `\(c\)`:


```r
ols(beta ~ qlogis(cstat))
```


 <strong>Linear Regression Model</strong>
 
 <pre>
 ols(formula = beta ~ qlogis(cstat))
 </pre>
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Model Likelihood<br>Ratio Test</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;'>Discrimination<br>Indexes</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 1840</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 10037.60</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.996</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>σ 0.0396</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 1</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup><span style='font-size: 70%;'>2</span></sup><sub style='position: relative; left: -.47em; bottom: -.4em;'><span style='font-size: 70%;'>adj</span></sub> 0.996</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>d.f. 1838</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) 0.0000</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'><i>g</i> 0.636</td>
</tr>
</tbody>
</table>

 
 Residuals
 
 <pre>
        Min         1Q     Median         3Q        Max 
 -0.5527344 -0.0036478 -0.0004424  0.0022669  0.6742581 
 </pre>
 
 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>t</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>t</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> 0.0003</td>
<td style='min-width: 7em; text-align: right;'> 0.0009</td>
<td style='min-width: 7em; text-align: right;'>  0.37</td>
<td style='min-width: 7em; text-align: right;'>0.7080</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>cstat</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 1.5179</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0023</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>654.38</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>

We see that `\(\hat{\beta}\)` is very close to `\(1.52 \times \mathrm{logit}(c)\)`, or inverting the equation we get `\(c = \frac{1}{1 + \exp(-\hat{\beta} / 1.52)}\)`. The quality of the approximation is given by `\(R^2\)`=0.996.

# Example

Consider an analysis of the relationship between cancer treatment (cisplatin vs. no cisplatin) and severity of nausea (6 levels) as analyzed in [Peterson and Harrell 1990](https://www.jstor.org/stable/2347760).  First compute the exact concordance probability, which is a simple linear translation of the WMW statistic.  Note that `\(D_{xy}\)` in the output is really `\(D_{yx}\)` since the order of arguments to the `somers2` function was reversed.


```r
d0 <- data.frame(tx=0, y=c(rep(0, 43), rep(1, 39), rep(2, 13), rep(3, 22),
                           rep(4, 15), rep(5, 29)))
d1 <- data.frame(tx=1, y=c(rep(0, 7), rep(1, 7), rep(2, 3), rep(3, 12),
                           rep(4, 15), rep(5, 14)))
d <- rbind(d0, d1)
d$tx <- factor(d$tx, 0:1, c('No cisplatin', 'cisplatin'))
dd <- datadist(d); options(datadist='dd')
with(d, table(tx, y))
```

```
              y
tx              0  1  2  3  4  5
  No cisplatin 43 39 13 22 15 29
  cisplatin     7  7  3 12 15 14
```

```r
cindex <- with(d, somers2(y, as.numeric(tx) - 1))
cindex
```

```
          C         Dxy           n     Missing 
  0.6472478   0.2944956 219.0000000   0.0000000 
```

```r
cexact <- cindex['C']
```

Now use the `rms` package `lrm` function to compute the overall cisplatin : no cisplatin log odds ratio for effect on severity of nausea.  


```r
f <- lrm(y ~ tx, data=d)
f
```


 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = y ~ tx, data = d)
 </pre>
 
 
 Frequencies of Responses
 
 <pre>
  0  1  2  3  4  5 
 50 46 16 34 30 43 
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
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 219</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 11.42</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.052</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.570</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 1×10<sup>-13</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 1</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 0.356</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.140</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) 0.0007</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 1.428</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.351</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.087</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.115</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.234</td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
</tr>
</tbody>
</table>

 
 <table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>β</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>S.E.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Wald <i>Z</i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>Pr(>|<i>Z</i>|)</th>
</tr>
</thead>
<tbody>
<tr>
<td style='min-width: 7em; text-align: left;'>y≥1</td>
<td style='min-width: 7em; text-align: right;'>  1.0189</td>
<td style='min-width: 7em; text-align: right;'> 0.1717</td>
<td style='min-width: 7em; text-align: right;'> 5.93</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>y≥2</td>
<td style='min-width: 7em; text-align: right;'>  0.0118</td>
<td style='min-width: 7em; text-align: right;'> 0.1546</td>
<td style='min-width: 7em; text-align: right;'> 0.08</td>
<td style='min-width: 7em; text-align: right;'>0.9390</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>y≥3</td>
<td style='min-width: 7em; text-align: right;'> -0.2996</td>
<td style='min-width: 7em; text-align: right;'> 0.1569</td>
<td style='min-width: 7em; text-align: right;'>-1.91</td>
<td style='min-width: 7em; text-align: right;'>0.0562</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>y≥4</td>
<td style='min-width: 7em; text-align: right;'> -0.9868</td>
<td style='min-width: 7em; text-align: right;'> 0.1718</td>
<td style='min-width: 7em; text-align: right;'>-5.75</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>y≥5</td>
<td style='min-width: 7em; text-align: right;'> -1.7254</td>
<td style='min-width: 7em; text-align: right;'> 0.1994</td>
<td style='min-width: 7em; text-align: right;'>-8.66</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>tx=cisplatin</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.9112</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.2714</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 3.36</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>0.0008</td>
</tr>
</tbody>
</table>

```r
b <- coef(f)['tx=cisplatin']
cest <- plogis(b / 1.52)
```

The concordance probability estimated from the average log odds ratio is 0.646 which is to be compared with the exact value of 0.647.  The agreement is excellent despite non-PO which is analyzed formally using a partial PO model [here](https://hbiostat.org/R/rmsb/blrm.html#partial-proportional-odds-model).

# Conclusion

When PO does not hold, the odds ratio from the proportional odds model represents a kind of average odds ratio, and there is an almost one-to-one relationship between the odds ratio (anti-log of `\(\hat{\beta}\)` and the concordance probability `\(c\)` (which is a simple translation of the Wilcoxon statistic).  No model fits data perfectly, but as Stephen Senn stated in the quote that opened this article, the approximation offered by the PO model remains quite useful.  And a unified PO model analysis is decidedly better than turning to inefficient and arbitrary analyses of dichotomized values of Y.

If you like the Wilcoxon test for comparing an ordinal response variable Y across treatments, or you like standard rank correlation measures for describing the strength of association between X and Y, you must like the PO model for summarizing treatment effects on ordinal (or continuous) Y.  In a clinical trial we are interested in estimating the degree to which a treatment favorably redistributes patients across levels of Y in order to get a unified analysis of how a treatment improves patient outcomes.  The PO model does that.

The place where a serious departure from the parallelism/PO assumption makes a large difference is in estimating treatment effects on individual outcome levels.  For example, an overall odds ratio indicating that treatment benefits patients on an array of nonfatal outcomes may be in the opposite direction of how the treatment affects mortality.  Though the overall average treatment effect estimated by assuming PO may rightfully claim a positive net clinical benefit of treatment, one can get a different picture when estimating the mortality affect ignoring all the other outcomes.  This may be addressed using the  _partial proportional odds model_ of [Peterson and Harrell, 1990](https://www.jstor.org/stable/2347760) as implemented [here](https://hbiostat.org/R/rmsb/blrm.html) and discussed in the COVID-19 context [here](https://hbiostat.org/proj/covid19/statdesign.html#univariate-ordinal-outcome).
