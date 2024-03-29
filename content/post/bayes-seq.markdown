---
title: 'Continuous Learning from Data: No Multiplicities from Computing and Using Bayesian Posterior Probabilities as Often as Desired'
author: 'Frank Harrell'
date: 2017-10-09
modified: 2021-07-29
slug: bayes-seq
tags:
  - bayes
  - sequential
  - RCT
  - 2017
---

<p class="rquote">
(In a Bayesian analysis) It is entirely appropriate to collect data
until a point has been proven or disproven, or until the data collector
runs out of time, money, or patience.<br>— <a href="http://psycnet.apa.org/doi/10.1037/h0044139">Edwards, Lindman, Savage (1963)</a>
<br><br>
Imagine that the military has developed a pattern recognition algorithm to determine whether a distant object is a tank. Initially an image results in an 0.8 probability of the object being a tank. The object moves closer and some fog clears. The probability is now 0.9, and the 0.8 has become completely irrelevant. Contrast with a frequentist way of thinking: Of all the times a tested object wasn’t a tank, the probability of acquiring tank-like image characteristics at some point grows with the number of images acquired. The frequency of image acquisition (data looks) and the image sampling design alter the probability of finding a tank-like image, but that way of looking at the problem is divorced from the real-time decision to be made based on current cumulative data. An earlier tank image is of no interest once a later, clearer image has been acquired. Analogously, with multiple data looks in a clinical trial where there is no treatment effect, the probability of having extreme data at some point grows with the number of looks. This affects type I error but not the true chance that the treatment works. Traditional statistics has multiplicity issues arising from giving more than one chance for data to be extreme (by taking more than one look at the data).
</p>

# Introduction

Bayesian inference, which follows the *likelihood principle*, is not
affected by the experimental design or intentions of the investigator.
P-values can only be computed if both of these are known, and as been
described by
[Berry](http://amstat.tandfonline.com/doi/abs/10.1080/00031305.1987.10475458)
(1987) and others, it is almost never the case that the computation of
the p-value at the end of a study takes into account all the changes in
design that were necessitated when pure experimental designs encounter
the real world.

When performing multiple data looks as a study progress, one can
accelerate learning by more quickly abandoning treatments that do not
work, by sometimes stopping early for efficacy, and frequently by
arguing to extend a promising but as-yet-inconclusive study by adding
subjects over the originally intended sample size. Indeed the whole
exercise of computing a single sample size is thought to be voodoo by
most practicing statisticians. It has become almost comical to listen to
rationalizations for choosing larger detectable effect sizes so that
smaller sample sizes will yield adequate power.

Multiplicity and resulting inflation of type I error when using
frequentist methods is real. While Bayesians concern themselves with
“what did happen?” frequentists must consider “what might have
happened?” because of the backwards time and information flow used in
their calculations. Frequentist inference must envision an indefinitely
long string of identical experiments and must consider extremes of data
over potential studies and over multiple looks within each study if
multiple looks were intended. Multiplicity comes from the chances (over
study repetitions and data looks) you give data to be more extreme (if
the null hypothesis holds), not from the chances you give an effect to
be real. It is only the latter that is of concern to a Bayesian.
Bayesians entertain only one dataset at a time, and if one computes
posterior probabilities of efficacy multiple times, it is only the last
value calculated that matters.

To better understand the last point, consider a probabilistic pattern
recognition system for identifying enemy targets in combat. Suppose the
initial assessment when the target is distant is a probability of 0.3 of
being an enemy vehicle. Upon coming closer the probability rises to 0.8.
Finally the target is close enough (or the air clears) so that the
pattern analyzer estimates a probability of 0.98. The fact that the
probability was &lt;0.98 earlier is of no consequence as the gunner
prepares to fire a canon. Even though the probability may actually
decrease while the shell is in the air due to new information, the
probability at the time of firing was completely valid based on then
available information.

This is very much how an experimenter would work in a Bayesian clinical
trial. The stopping rule is unimportant when interpreting the final
evidence. Earlier data looks are irrelevant. The only ways a Bayesian
would cheat would be to ignore a later look if it is less favorable than
an earlier look, or to try to pull the wool over reviewers’ eyes by
changing the prior distribution once data patterns emerge.

The meaning and accuracy of posterior probabilities of efficacy in a
clinical trial are mathematical necessities that follow from Bayes’
rule, if the data model is correctly specified (this model is needed
just as much by frequentist methods). So no simulations are needed to
demonstrate these points. But for the non-mathematically minded,
simulations can be comforting. For everyone, simulation code exposes the
logic flow in the Bayesian analysis paradigm.

One other thing: when the frequentist does a sequential trial with
possible early termination, the sampling distribution of the statistics
becomes extremely complicated, but must be derived to allow one to
obtain proper point estimates and confidence limits. It is almost never
the case that the statistician actually performs these complex
adjustments in a clinical trial with multiple looks. One example of the
harm of ignoring this problem is that if the trial stops fairly early
for efficacy, efficacy will be overestimated. On the other hand, the
Bayesian posterior mean/median/mode of the efficacy parameter will be
perfectly calibrated by the prior distribution you assume. If the prior
is skeptical and one stops early, the posterior mean will be “pulled
back” by a perfect amount, as shown in the simulation below.

We consider the simplest clinical trial design for illustration. The
efficacy measure is assumed to be normally distributed with mean μ and
variance 1.0, μ=0 indicates no efficacy, and μ&lt;0 indicates a
detrimental effect. Our inferential jobs are to see if evidence may be
had for a positive effect and to see if further there is evidence for a
clinically meaningful effect (except for the futility analysis, we will
ignore the latter in what follows). Our business task is to not spend
resources on treatments that have a low chance of having a meaningful
benefit to patients. The latter can also be an ethical issue: we’d like
not to expose too many patients to an ineffective treatment. In the
simulation, we stop for futility when the probability that μ&lt;0.05
exceeds 0.9, considering μ=0.05 to be a minimal clinically important
effect.

The logic flow in the simulation exposes what is assumed by the Bayesian
analysis.

1.  The prior distribution for the unknown effect μ is taken as a
    mixture of two normal distributions, each with mean zero. This is a
    skeptical prior that gives an equal chance for detriment as for
    benefit from the treatment. Any prior would have done.
2.  In the next step it is seen that the Bayesian does not consider a
    stream of identical trials but instead (and only when studying
    performance of Bayesian operating characteristics) considers a
    stream of trials with **different** efficacies of treatment, by
    drawing a single value of μ from the prior distribution. This is
    done independently for 50,000 simulated studies. Posterior
    probabilities are not informed by this value of μ. Bayesians operate
    in a predictive mode, trying for example to estimate Prob(μ&gt;0) no
    matter what the value of μ.
3.  For the current value of μ, simulate an observation from a normal
    distribution with mean μ and SD=1.0. \[In the code below all n=500
    subjects’ data are simulated at once then revealed one-at-a-time.\]
4.  Compute the posterior probability of efficacy (μ&gt;0) and of
    futility (μ&lt;0.05) using the original prior and latest data.
5.  Stop the study if the probability of efficacy ≥0.95 or the
    probability of futility ≥0.9.
6.  Repeat the last 3 steps, sampling one more subject each time and
    performing analyses on the accumulated set of subjects to date.
7.  Stop the study when 500 subjects have entered.

What is it that the Bayesian must demonstrate to the frequentist and
reviewers? She must demonstrate that the posterior probabilities
computed as stated above are accurate, i.e., they are well calibrated.
From our simulation design, the final posterior probability will either
be the posterior probability computed after the last (500th) subject has
entered, the probability of futility at the time of stopping for
futility, or the probability of efficacy at the time of stopping for
efficacy. How do we tell if the posterior probability is accurate? By
comparing it to the value of μ (unknown to the posterior probability
calculation) that generated the sequence of data points that were
analyzed. We can compute a smooth nonparametric calibration curve for
each of (efficacy, futility) where the binary events are μ&gt;0 and μ&lt;0.05, respectively. For the subset of the 50,000 studies that were
stopped early, the range of probabilities is limited so we can just
compare the mean posterior probability at the moment of stopping with
the proportion of such stopped studies for which efficacy (futility) was
the truth. The mathematics of Bayes dictates the mean probability and
the proportion must be the same (if enough trials are run so that
simulation error approaches zero). This is what happened in the
simulations.

For the smaller set of studies not stopping early, the posterior
probability of efficacy is uncertain and will have a much wider range.
The calibration accuracy of these probabilities is checked using a
nonparametric calibration curve estimator just as we do in validating
risk models, by fitting the relationship between the posterior
probability and the binary event μ&gt;0.

The simulations also demonstrated that the posterior mean efficacy at
the moment of stopping is perfectly calibrated as an estimator of the
true unknown μ.

Simulations were run in R and used functions in the R Hmisc and rms
package. The results are below. Feel free to take the code and alter it
to run any simulations you’d like.

``` r
require(rms)
```

``` r
knitrSet(lang='blogdown', echo=TRUE)
gmu  <- htmlGreek('mu')
half <- htmlSpecial('half')
geq  <- htmlTranslate('>=')
knitr::read_chunk('fundefs.r')
```

# Specification of Prior

The prior distribution is skeptical against large values of efficacy, and assumes that detriment is equally likely as benefit of treatment. The prior favors small effects. It is a 1:1 mixture of two normal distributes each with mean 0. The SD of the first distribution is chosen so that P(μ &gt; 1) = 0.1, and the SD of the second distribution is chosen so that P(μ &gt; 0.25) = 0.05. Posterior probabilities upon early stopping would have the same accuracy no matter which prior is chosen as long as the same prior generating μ is used to generate the data.

``` r
sd1 <- 1    / qnorm(1 - 0.1)
sd2 <- 0.25 / qnorm(1 - 0.05)
wt  <- 0.5   # 1:1 mixture
pdensity <- function(x) wt * dnorm(x, 0, sd1) + (1 - wt) * dnorm(x, 0, sd2)
x <- seq(-3, 3, length=200)
plot(x, pdensity(x), type='l', xlab='Efficacy', ylab='Prior Degree of Belief')
```

<img src="/post/bayes-seq_files/figure-html/skepprior-1.png" width="672" />

# Sequential Testing Simulation

``` r
simseq <- function(N, prior.mu=0, prior.sd, wt, mucut=0, mucutf=0.05,
                postcut=0.95, postcutf=0.9,
                ignore=20, nsim=1000) {
  prior.mu <- rep(prior.mu, length=2)
  prior.sd <- rep(prior.sd, length=2)
  sd1 <- prior.sd[1]; sd2 <- prior.sd[2]
  v1 <- sd1 ^ 2
  v2 <- sd2 ^ 2
  j <- 1 : N
  cmean <- Mu <- PostN <- Post <- Postf <- postfe <- postmean <- numeric(nsim)
  stopped <- stoppedi <- stoppedf <- stoppedfu <- stopfe <- status <-
    integer(nsim)
  notignored <- - (1 : ignore)

  # Derive function to compute posterior mean
  pmean <- gbayesMixPost(NA, NA, d0=prior.mu[1], d1=prior.mu[2],
                                 v0=v1, v1=v2, mix=wt, what='postmean')
  
  for(i in 1 : nsim) {
    # See http://stats.stackexchange.com/questions/70855
    component <- if(wt == 1) 1 else sample(1 : 2, size=1, prob=c(wt, 1. - wt))
    mu <- prior.mu[component] + rnorm(1) * prior.sd[component]
    # mu <- rnorm(1, mean=prior.mu, sd=prior.sd) if only 1 component
    
    Mu[i] <- mu
    y  <- rnorm(N, mean=mu, sd=1)
    ybar <- cumsum(y) / j    # all N means for N sequential analyses
    pcdf <- gbayesMixPost(ybar, 1. / j,
                          d0=prior.mu[1], d1=prior.mu[2],
                          v0=v1, v1=v2, mix=wt, what='cdf')
    post  <- 1 - pcdf(mucut)
    PostN[i] <- post[N]
    postf <- pcdf(mucutf)
    s <- stopped[i] <-
      if(max(post) < postcut) N else min(which(post >= postcut))
    Post[i]  <- post[s]   # posterior at stopping
    cmean[i] <- ybar[s]   # observed mean at stopping
    # If want to compute posterior median at stopping:
    #    pcdfs <- pcdf(mseq, x=ybar[s], v=1. / s)
    #    postmed[i] <- approx(pcdfs, mseq, xout=0.5, rule=2)$y
    #    if(abs(postmed[i]) == max(mseq)) stop(paste('program error', i))
    postmean[i] <- pmean(x=ybar[s], v=1. / s)
    
    # Compute stopping time if ignore the first "ignore" looks
    stoppedi[i] <- if(max(post[notignored]) < postcut) N
    else
      ignore + min(which(post[notignored] >= postcut))
    
    # Compute stopping time if also allow to stop for futility:
    # posterior probability mu < 0.05 > 0.9
    stoppedf[i] <- if(max(post) < postcut & max(postf) < postcutf) N
    else
      min(which(post >= postcut | postf >= postcutf))
    
    # Compute stopping time for pure futility analysis
    s <- if(max(postf) < postcutf) N else min(which(postf >= postcutf))
    Postf[i] <- postf[s]
    stoppedfu[i] <- s

    ## Another way to do this: find first look that stopped for either
    ## efficacy or futility.  Record status: 0:not stopped early,
    ## 1:stopped early for futility, 2:stopped early for efficacy
    ## Stopping time: stopfe, post prob at stop: postfe

    stp <- post >= postcut | postf >= postcutf
    s <- stopfe[i] <- if(any(stp)) min(which(stp)) else N
    status[i] <- if(any(stp)) ifelse(postf[s] >= postcutf, 1, 2) else 0
    postfe[i] <- if(any(stp)) ifelse(status[i] == 2, post[s],
                                     postf[s]) else post[N]
  }
  list(mu=Mu, post=Post, postn=PostN, postf=Postf,
       stopped=stopped, stoppedi=stoppedi,
       stoppedf=stoppedf, stoppedfu=stoppedfu,
       cmean=cmean, postmean=postmean,
       postfe=postfe, status=status, stopfe=stopfe)
}
```

``` r
set.seed(1)
z <- simseq(500, prior.mu=0, prior.sd=c(sd1, sd2), wt=wt, postcut=0.95,
            postcutf=0.9, nsim=50000)
mu      <- z$mu
post    <- z$post
postn   <- z$postn
st      <- z$stopped
sti     <- z$stoppedi
stf     <- z$stoppedf
stfu    <- z$stoppedfu
cmean   <- z$cmean
postmean<- z$postmean
postf   <- z$postf
status  <- z$status
postfe  <- z$postfe
rmean <- function(x) formatNP(mean(x), digits=3)
k  <- status == 2
kf <- status == 1
```

-   Run 50,000 <b>different</b> clinical trials (differ on amount of efficacy)

-   For each, generate μ (true efficacy) from the prior

-   Generate data (n=500) under this truth

-   ½ of the trials have zero or negative efficacy

-   Do analysis after 1, 2, …, 500 subjects studied

-   Stop the study when 0.95 sure efficacy &gt; 0, i.e., stop the instant the posterior prob. that the unknown mean μ is positive is ≥ 0.95

-   Also stop for futility: the instant P(μ &lt; 0.05) ≥ 0.9

-   20393 trials stopped early for efficacy

-   28438 trials stopped early for futility

-   1169 trials went to completion (n=500)

-   Average posterior prob. of efficacy at stopping for efficacy: 0.961

-   Of trials stopped early for efficacy, proportion with μ &gt; 0: 0.960

-   Average posterior prob. of futility at stopping for futility: 0.920

-   Of trials stopped early for futility, proportion with μ &lt; 0.05: 0.923

The simulations took about 25 seconds in total.

# Calibration of Posterior Probabilities of Efficacy for Studies Going to Completion

Above we saw perfect calibration of the probabilities of efficacy and futility upon stopping. Let’s now examine the remaining probabilities, for the 1169 trials going to completion. For this we use the same type of nonparametric calibration curve estimation as used for validating risk prediction models. This curve estimates the relationship between the estimated probability of efficacy (Bayesian posterior probability) and the true probability of efficacy.

``` r
k <- status == 0
pp <- postfe[k]
truly.efficacious <- mu[k] > 0
v <- val.prob(pp, truly.efficacious)
```

<img src="/post/bayes-seq_files/figure-html/cal-1.png" width="672" />

The posterior probabilities of efficacy tended to be between 0.45 (had they been much lower the trial would have been stopped for futility) and 0.95 (the cutoff for stopping for efficacy). Where there are data, the nonparametric calibration curve estimate is very close to the line of identity. Had we done even more simulations we would have had many more non-stopped studies and the calibration estimates would be even closer to the ideal. For example, when the posterior probability of efficacy is 0.6, the true probability that the treatment was effective (μ actually &gt; 0) is 0.6.

# Calibration of Posterior Mean at Stopping for Efficacy

When stopping early because of evidence that μ &gt; 0, the sample mean will overestimate the true mean. But with the Bayesian analysis, where the prior favors smaller treatment effects, the posterior mean/median/mode is pulled back by a perfect amount, as shown in the plot below.

``` r
plot(0, 0, xlab='Estimated Efficacy',
     ylab='True Efficacy', type='n', xlim=c(-2, 4), ylim=c(-2, 4))
abline(a=0, b=1, col=gray(.9), lwd=4)
lines(supsmu(cmean, mu))
lines(supsmu(postmean, mu), col='blue')
text(2, .4, 'Sample mean')
text(-1, .8, 'Posterior mean', col='blue')
```

<img src="/post/bayes-seq_files/figure-html/estmu-1.png" width="672" />

# Example Text to Communicate Study Design Overview to a Sponsor

It is always the case that estimated a single fixed sample size is problematic, because a number of assumptions must be made, and the veracity of those assumptions is not known until the study is completed. A sequential Bayesian approach allows for a lower expected sample size if some allowance can be made for the possibility that if the study gets to a certain landmark, the results are equivocal, and the study can be extended. The idea is to compute the (Bayesian) probability of efficacy as often as desired. The study could be terminated early for futility or harm, and less likely, for efficacy. Such early termination would save more resources than one would spend to extend a promising but equivocal study, on the average. The intended sample size would be set. At that point, if results are equivocal but promising (e.g. Bayesian posterior probability of efficacy is &gt; 0.8), the sponsor would have the option to decide to extend the study by adding more patients, perhaps in blocks of 50.

<!---
# Useful References
Berry[@ber87int], Edwards, Lindman and Savage[@edw63bay]
-->

# Other Resources

-   [Interim analysis for early stopping during the study](https://davidbock.netlify.app/post/2021/07/29/interim-analysis-for-early-stopping-during-the-study) by David Bock
-   [Five Bayesian intuitions for the stopping rule principle](https://www.bayesianspectacles.org/preprint-five-bayesian-intuitions-for-the-stopping-rule-principle) by EJ Wagenmakers, Q Gronau, J Vandekerckhove
-   [“We continuously increased the number of animals until statistical significance was reached to support our conclusions”](https://andrewgelman.com/2018/09/04/continuously-increased-number-animals-statistical-significance-reached-support-conclusions-think-not-bad-actually-2) by Andrew Gelman
-   [Is Bayesian A/B testing immune to peeking? Not exactly](http://varianceexplained.org/r/bayesian-ab-testing) by David Robinson
-   [The P value line dance: When does the music stop?](https://www.jmir.org/2020/8/e21345) by Marcus Bendtsen

# Computing Environment

<pre>
 R version 4.1.0 (2021-05-18)
 Platform: x86_64-pc-linux-gnu (64-bit)
 Running under: Pop!_OS 21.04
 
 Matrix products: default
 BLAS:   /usr/lib/x86_64-linux-gnu/blas/libblas.so.3.9.0
 LAPACK: /usr/lib/x86_64-linux-gnu/lapack/liblapack.so.3.9.0
 
 attached base packages:
 [1] stats     graphics  grDevices utils     datasets  methods   base     
 
 other attached packages:
 [1] rms_6.2-1       SparseM_1.81    Hmisc_4.6-0     ggplot2_3.3.3  
 [5] Formula_1.2-4   survival_3.2-11 lattice_0.20-44
 </pre>
 To cite R in publications use:
 <p>R Core Team (2021).
 <em>R: A Language and Environment for Statistical Computing</em>.
 R Foundation for Statistical Computing, Vienna, Austria.
 <a href="https://www.R-project.org/">https://www.R-project.org/</a>. 
 </p>
 
To cite the <tt>Hmisc</tt> package in publications use:

 <p>Harrell Jr F (2021).
 <em>Hmisc: Harrell Miscellaneous</em>.
 R package version 4.6-0, <a href="https://hbiostat.org/R/Hmisc/">https://hbiostat.org/R/Hmisc/</a>. 
 </p>
 
To cite the <tt>rms</tt> package in publications use:

 <p>Harrell Jr FE (2021).
 <em>rms: Regression Modeling Strategies</em>.
 https://hbiostat.org/R/rms/, https://github.com/harrelfe/rms. 
 </p>
 
To cite the <tt>ggplot2</tt> package in publications use:

 <p>Wickham H (2016).
 <em>ggplot2: Elegant Graphics for Data Analysis</em>.
 Springer-Verlag New York.
 ISBN 978-3-319-24277-4, <a href="https://ggplot2.tidyverse.org">https://ggplot2.tidyverse.org</a>. 
 </p>
 
To cite the <tt>survival</tt> package in publications use:

 <p>Therneau T (2021).
 <em>A Package for Survival Analysis in R</em>.
 R package version 3.2-11, <a href="https://CRAN.R-project.org/package=survival">https://CRAN.R-project.org/package=survival</a>. 
 </p>
