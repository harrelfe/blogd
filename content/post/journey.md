+++
title = "My Journey From Frequentist to Bayesian Statistics"
date = 2017-02-19
modified = 2022-04-06
tags = ["inference", "p-value", "likelihood", "RCT", "bayes", "multiplicity", "posterior", "drug-evaluation", "principles", "evidence", "hypothesis-testing", "2017"]
+++

|               |                |
| ---------:| ----------:|
|  | <small>**The difference between Bayesian and frequentist inference in a nutshell**:<br>With Bayes you start with a prior distribution for θ and given your data make an inference about the θ-driven process generating your data (whatever that process happened to be), to quantify evidence for every possible value of θ.  With frequentism, you make assumptions about the process that generated your data and infinitely many replications of them, and try to build evidence for what θ is not.</small> |
|  |  |
|  | <small>Frequentism is about the data generating process.  Bayes is about the θ generating process, and about the data generate<b>d</b>.</small> |
|  |  |
|  | <small>Far better an <b>approximate</b> answer to the <b>right question</b>, which is often vague, than the <b>exact answer</b> to the <b>wrong question</b>, which can always be made precise -- [John Tukey](https://projecteuclid.org/euclid.aoms/1177704711)</small><br><small><b>Approximate</b>: My prior distribution disagrees with yours, so my posterior probability that the treatment works is 0.02 less than yours (and BTW the p-value from the frequentist analysis is only accurate to ±0.015 in this non-normal case)</small></br><small><b>Right question</b>: Does the treament work more than a trivial amount?</small><br><small><b>Exact answer</b>: Under the simplifying assumption that H<sub>0</sub> is true we can actually compute exact p-values in <sup>1</sup>/<sub>10</sub> of the models we use</small><br><small><b>Wrong question</b>: How surprising is our result if the treatment has no effect at all?</small>
|  |  |
|  | <small>The Bayesian approach goes out on a limb in order to answer the original question (is an effect > <em>x</em>).  The frequentist approach stays close to home, not requiring quantification of prior knowledge, to answer an easier but almost irrelevant question (how strange are my data)</small> |
|  |  |
|  |  <small>Any frequentist criticizing the Bayesian paradigm for requiring one to choose a prior distribution must recognize that she has a possibly more daunting task: to completely specify the experimental design, sampling scheme, and data generating process that were **actually used** and would be infinitely replicated to allow p-values and confidence limits to be computed.</small> |
|  |  |
|  |  <small>Those who criticize Bayes for having to choose a prior must remember that the frequentist approach leads to different p-values on the <b>same data</b> depending on how intentions are handled (e.g., observing 6 heads out of 10 tosses vs. having to toss 10 times to observe 6 heads; accounting for earlier inconsequential data looks in sequential testing).</small> |
|  |  |
|  |  <small>When was the last time you observed a sampling distribution, empirically and objectively? When was the last time you saw someone justify their choice of a sampling distribution, or tested it? -- [Stephen Martin](https://twitter.com/smartin2018/status/1057500260782800898)</small> |
|  |  |
|  | <small>The subjectivist (i.e. Bayesian) states his judgements, whereas the objectivist sweeps them under the carpet by calling assumptions knowledge, and he basks in the glorious objectivity of science. --  IJ Good</small> |
|  |  |
|  | <small>Null hypothesis testing is simple because it kicks down the road the gymnastics needed to subjectively convert observations about data to evidence about parameters.</small> |
|  |  |
|  | <small>Subjective Bayesian priors are explicit.  Subjective frequentist interpretation of a rather meaningless probability is not and thus not subject to informed challenge. --[Paul Pharoah](https://twitter.com/paulpharoah/status/1313748731993391105)</small> |
|  |  |
|  | <small>Type I error for smoke detector: probability of alarm given no fire=0.05</small><br><small>Bayesian: probability of fire given current air characteristics</small><br><small>Frequentist smoke alarm designed as most research is done: Set the alarm trigger so as to have a 0.8 chance of detecting an inferno</small><br><small>**Advantage of actionable evidence quantification**:</small><br><small>Set the alarm to trigger when the posterior probability of a fire exceeds 0.02 while at home and at 0.01 while away</small> |
|  | <small>Reject a _specific_ null, and then argue for an _arbitrary_ alternative.  It's pretty remarkable that so few people see how absurd this procedure is. -- [JP de Ruiter](https://twitter.com/JPdeRuiter/status/963481008417988609)</small> |
|  |  |
|  | <small>If you tossed a coin 100 times resulting in 60 heads, would you rather know the probability of getting > 59 heads out of 100 tosses if the coin happened to be fair, or the probability it is fair given exactly 60 heads?  The frequentist approach is alluring because of the minimal work in carrying out a test of the null hypothesis θ=½.  But the Bayesian approach provides a direct answer to the second question, and requires you to think.  What is an "unfair" coin?  Is it θ outside of [0.49, 0.51]?  What is the world view of coins, e.g., is someone likely to provide a coin that is easily detectable as unfair because its θ=0.6? Was the coin chosen at random or handed to us?</small> |
|  |  |
|  | <small>The null-hypothesis significance test treats 'acceptance' or 'rejection' of a hypothesis as though these were decisions one makes. But a hypothesis is not something, like a piece of pie offered for dessert, which can be accepted or rejected by a voluntary physical action. Acceptance or rejection of a hypothesis is a cognitive process, a degree of believing or disbelieving which, if rational, is not a matter of choice but determined solely by how likely it is, given the evidence, that the hypothesis is true. -- WW Rozeboom, 1960, pp. 422-423 in [EJ Wagenmakers and Q Gronau](https://www.bayesianspectacles.org/redefine-statistical-significance-xvii-william-rozeboom-destroys-the-justify-your-own-alpha-argument-back-in-1960)</small>.|
|  |  |
|  |  <small>Some statisticians argue that the implied logic concerning a small P-value is compelling: “Either H₀ is true and a rare event has occurred, or H₀ is false.”  One could again argue against this reasoning as addressing the wrong question, but there is a more obvious major flaw: the “rare event” whose probability is being calculated under H₀ is not the event of observing the actual data x₀, but the event E = {possible data x: \|T(x)\| ≥ \|T(x₀)\|}.  The inclusion of all data “more extreme” than the actual x₀ is a curious step, and one which we have seen no remotely convincing justification. … the “logic of surprise” cannot differentiate between x₀ and E … -- [Berger and Delampady, 1987, Section 4.6](https://projecteuclid.org/journals/statistical-science/volume-2/issue-3/Testing-Precise-Hypotheses/10.1214/ss/1177013238.full)</small>|
|  |  |
|  | <small>The only way to make frequentist methods comprehensible is to obfuscate them, and the only way to be fully accurate is to make them incomprehensible.</small> |
|  |  |
|  | <small>The frequentist type I error is the probability of asserting an effect when there is <b>no effect</b>, and is independent of data.  One minus the Bayesian conditional probability of an effect given the data is the probability the treatment doesn't work <b>whether or not</b> you assert that it does.</small> |
|  |  |
|  | <small>Judging a Bayesian procedure by its type I probability α is equivalent to judging a gambler by the fraction of <em>potential</em> losses in which he <emph>would have</emph> placed a bet, instead of judging him by the fraction of bets that he won.</small> | 
|  |  |
|  | <small>Someone demanding that a Bayesian procedure preserve type I error, e.g. that P(posterior probability of positive effect > 0.95 given no effect) ≤ α, should be demanded to show their frequentist procedure yields decisons as good as those driven by the Bayesian P(effect > 0 given data, prior).</small> |
|  |  | 
|  |  <small>A paradigm that cannot <b>compute</b> the probability that a treatment works cannot <b>control</b> the probability of making a decision error, e.g., the probability that a treatment actually doesn't work when you act as though it does.</small> |
|  |  |
|  |  <small>The only agents that use frequentist statistics are frequentists statisticians when they practice frequentist statistics. In the real world, evolution rooted out those agents who willfully ignore prior knowledge to address irrelevant questions in an incoherent fashion. -- [EJ Wagenmakers](https://twitter.com/EJWagenmakers/status/1344922099765407744)</small> |
|  |  |
|  |  <small>In arguing for progressive (and pragmatic) attitudes in statistical theory and practice [in overcoming inertia moving beyond Frequentism and embracing Bayesianism] it may be well to point to how other fields, such as Economics and Physics, fields that describe how the world works, have ineluctably had major paradigm shifts. In every major field---if it is an intellectually honest one---there is a moment where we have to "un-learn" what we have been inculcated in, conculcate operative premises, and retool our paradigms. There is no reason to expect that Statistics is different. -- [Drew Levy](https://www.linkedin.com/in/drewglevy/)</small> |

If I had been taught Bayesian modeling before being taught the
frequentist paradigm, I'm sure I would have always been a Bayesian.  I
started becoming a Bayesian about 1994 because of an [influential
paper](https://www.zotero.org/groups/2199991/feh/items/itemKey/D9Y5R5JK/q/Spiegelhalter) by David
Spiegelhalter and because I worked in the same building at Duke
University as Don Berry.  Two other things strongly contributed to my
thinking: difficulties explaining p-values and confidence intervals
(especially the latter) to clinical researchers, and difficulty of
learning group sequential methods in clinical trials.  When I talked
with Don and learned about the flexibility of the Bayesian approach to
clinical trials, and saw Spiegelhalter's embrace of Bayesian methods
because of its problem-solving abilities, I was hooked.  [Note: I've
heard Don say that he became Bayesian after multiple attempts to teach
statistics students the exact definition of a confidence interval.  He
decided the concept was defective.]

At the time I was working on clinical trials at Duke and started to see
that multiplicity adjustments were arbitrary.  This started with a
clinical trial coordinated by Duke in which low dose and high dose of a
new drug were to be compared to placebo, using an alpha cutoff of 0.03
for each comparison to adjust for multiplicity.  The comparison of high
dose with placebo resulted in a p-value of 0.04 and the trial was
labeled completely "negative" which seemed problematic to me. [Note:
the p-value was two-sided and thus didn't give any special "credit" for
the treatment effect coming out in the right direction.]

I began to see that the hypothesis testing framework wasn't always the
best approach to science, and that in biomedical research the typical
hypothesis was an artificial construct designed to placate a reviewer
who believed that an NIH grant's specific aims must include null
hypotheses.  I saw the contortions that investigators went through to
achieve this, came to see that questions are more relevant than
hypotheses, and estimation was even more important than questions.

With Bayes, estimation is emphasized.  I very much like Bayesian
modeling instead of hypothesis testing.  I saw that a large number of
clinical trials were incorrectly interpreted when p>0.05 because the
investigators involved failed to realize that a p-value can only provide
evidence against a hypothesis. Investigators are motivated by "we spent
a lot of time and money and must have gained something from this
experiment." The classic "[absence of evidence is not evidence of
absence](http://www.bmj.com/content/311/7003/485)" error results,
whereas with Bayes it is easy to estimate the probability of similarity
of two treatments.  Investigators will be surprised to know how little
we have learned from clinical trials that are not huge when p>0.05.

I listened to many discussions of famous clinical trialists debating
what should be the primary endpoint in a trial, the co-primary endpoint,
the secondary endpoints, co-secondary endpoints, etc.  This was all
because of their paying attention to alpha-spending.  I realized this
was all a game.

I came to not believe in the possibility of infinitely many repetitions
of identical experiments, as required to be envisioned in the
frequentist paradigm.  When I looked more thoroughly into the
multiplicity problem, and sequential testing, and I looked at Bayesian
solutions, I became more of a believer in the approach.  I learned that
posterior probabilities have a simple interpretation independent of the
stopping rule and frequency of data looks.  I got involved in working
with the FDA and then consulting with pharmaceutical companies, and
started observing how multiple clinical endpoints were handled.  I saw a
closed testing procedures where a company was seeking a superiority
claim for a new drug, and if there was insufficient evidence for such a
claim, they wanted to seek a non-inferiority claim on another endpoint.
 They developed a closed testing procedure that when diagrammed truly
looked like a train wreck.  I felt there had to be a better approach, so
I sought to see how far posterior probabilities could be pushed.  I
found that with MCMC simulation of Bayesian posterior draws I could
quite simply compute probabilities such as P(any efficacy), P(efficacy
more than trivial), P(non-inferiority), P(efficacy on endpoint A and on
either endpoint B or endpoint C), and P(benefit on more than 2 of 5
endpoints).  I realized that frequentist multiplicity problems came from
the chances you give data to be more extreme, not from the chances you
give assertions to be true.

I enjoy the fact that posterior probabilities define their own error
probabilities, and that they count not only inefficacy but also harm.
 If P(efficacy)=0.97, P(no effect or harm)=0.03.  This is the
"regulator's regret", and type I error is not the error of major
interest (is it really even an 'error'?).  One minus a p-value is P(data
in general are less extreme than that observed if H<sub>0</sub> is true) which is
the probability of an event I'm not that interested in.

The extreme amount of time I spent analyzing data led me to understand
other problems with the frequentist approach.  Parameters are either in
a model or not in a model.  We test for interactions with treatment and
hope that the p-value is not between 0.02 and 0.2.  We either include
the interactions or exclude them, and the power for the interaction test
is modest.  Bayesians have a prior for the differential treatment effect
and can easily have interactions "half in" the model.  Dichotomous
irrevocable decisions are at the heart of many of the statistical
modeling problems we have today.  I really like penalized maximum
likelihood estimation (which is really empirical Bayes) but once we have
a penalized model all of our frequentist inferential framework fails us.
 No one can interpret a confidence interval for a biased (shrunken;
penalized) estimate.  On the other hand, the Bayesian posterior
probability density function, after shrinkage is accomplished using
skeptical priors, is just as easy to interpret as had the prior been
flat.  For another example, consider a categorical predictor variable
that we hope is predicting in an ordinal (monotonic) fashion.  We tend
to either model it as ordinal or as completely unordered (using k-1
indicator variables for k categories).  A Bayesian would say "let's use
a prior that favors monotonicity but allows larger sample sizes to
override this belief."

Now that adaptive and sequential experiments are becoming more popular,
and a formal mechanism is needed to use data from one experiment to
inform a later experiment (a good example being the use of adult
clinical trial data to inform clinical trials on children when it is
difficult to enroll a sufficient number of children for the child data
to stand on their own), Bayes is needed more than ever.  It took me a
while to realize something that is quite profound: A Bayesian solution
to a simple problem (e.g., 2-group comparison of means) can be embedded
into a complex design (e.g., adaptive clinical trial) **without
modification**.  Frequentist solutions require highly complex
modifications to work in the adaptive trial setting.

I met likelihoodist [Jeffrey Blume](https://biostat.app.vumc.org/wiki/Main/JeffreyBlume) in 2008 and
started to like the likelihood approach.  It is more Bayesian than
frequentist.  I plan to learn more about this paradigm.  Jeffrey has an excellent [web site](http://statisticalevidence.com).

Several readers have asked me how I could believe all this and publish a
frequentist-based book such as *Regression Modeling Strategies*.  There
are two primary reasons.  First, I started writing the book before I
knew much about Bayes.  Second, I performed a lot of simulation studies
that showed that purely empirical model-building had a low chance of
capturing clinical phenomena correctly and of validating on new
datasets.  I worked extensively with cardiologists such as Rob Califf,
Dan Mark, Mark Hlatky, David Prior, and Phil Harris who give me the
ideas for injecting clinical knowledge into model specification.  From
that experience I wrote *Regression Modeling Strategies* in the most
Bayesian way I could without actually using specific  Bayesian methods.
 I did this by emphasizing subject-matter-guided model specification.
 The section in the book about specification of interaction terms is
perhaps the best example.  When I teach the full-semester version of my
course I interject Bayesian counterparts to many of the techniques
covered.

There are challenges in moving more to a Bayesian approach.  The ones I
encounter most frequently are:

1.  Teaching clinical trialists to embrace Bayes when they already do in
    spirit but not operationally.  Unlearning things is much more
    difficult than learning things.
2.  How to work with sponsors, regulators, and NIH principal
    investigators to specify the (usually skeptical) prior up front, and
    to specify the amount of applicability assumed for previous data.
3.  What is a Bayesian version of the multiple degree of freedom "chunk
    test"?  Partitioning sums of squares or the log likelihood into
    components, e.g., combined test of interaction and combined test of
    nonlinearities, is very easy and natural in the frequentist setting.
4.  How do we specify priors for complex entities such as the degree of
    monotonicity of the effect of a continuous predictor in a regression
    model?  The Bayesian approach to this will ultimately be more
    satisfying, but operationalizing this is not easy.

With new tools such as [Stan](http://mc-stan.org/) and well written
accessible books such
as [Kruschke's](https://www.zotero.org/groups/2199991/feh/items/itemKey/RBW62KMN/q/kruschke) and [McElreath's](https://www.zotero.org/groups/2199991/feh/items/q/mcelreath)
it's
getting to be easier to be Bayesian each day.  For a longer list of suggested articles and books recommended for those without advanced statistics background see [this](http://hbiostat.org/bib/bayesteach.html).  See also Richard McElreath's [online lectures](https://www.youtube.com/playlist?list=PLDcUM9US4XdM9_N6XUUFrhghGJ4K25bFc) and [trialdesign.org](http://trialdesign.org).  The R
[brms](https://cran.r-project.org/web/packages/brms) package, which uses
Stan, makes a large class of regression models even more accessible.  A large number of R scripts illustrating Bayesian analysis are [here](https://github.com/avehtari/BDA_R_demos).

Another reason for moving from frequentism to Bayes is that frequentist
ideas are so confusing that even expert statisticians frequently
misunderstand them, and are tricked into dichotomous thinking because of
the adoption of null hypothesis significance testing (NHST).   The
[paper](http://www.blakemcshane.com/Papers/jasa_dichotomization.pdf) by
BB McShane and D Gal in JASA demonstrates alarming errors in
interpretation by many authors of JASA papers.  If those with a high
level of statistical training make frequent interpretation errors could
frequentist statistics be fundamentally flawed?  Yes!  In McShane and
Gal's paper they described two surveys sent to authors of JASA, as well
as to authors of articles not appearing in the statistical literature
(luckily for statisticians the non-statisticians fared a bit worse). 
 Some of their key findings are as follows.

1.  When a p-value is present, (primarily frequentist) statisticians
    confuse population vs. sample, especially if the p-value is large. 
    Even when directly asked whether patients *in this sample* fared
    batter on one treatment than the other, the respondents often
    answered according to whether or not p<0.05.  Dichotomous
    thinking crept in.
2.  When asked whether evidence from the data made it more or less
    likely that a drug is beneficial in the population, many
    statisticians again were swayed by the p-value and not tendencies
    indicated by the raw data.  The failed to understand that your
    chances are improved by "playing the odds", and gave different
    answers whether one was playing the odds for an unknown person vs.
    selecting treatment for themselves.
3.  In previous studies by the authors, they found that "applied
    researchers presented with not only a p-value but also with a
    posterior probability based on a noninformative prior were less
    likely to make dichotomization errors."

The authors also echoed Wasserstein, Lazar, and Cobb's concern that we
are setting researchers up for failure: "we teach NHST because that's
what the scientific community and journal editors use but they use NHST
because that's what we teach them.  Indeed, statistics at the
undergraduate level as well as at the graduate level in applied fields
is often taught in a rote and recipe-like manner that typically focuses
exclusively on the NHST paradigm."

Some of the problems with frequentist statistics are the way in which
its methods are misused, especially with regard to dichotomization.  But
an approach that is so easy to misuse and which sacrifices direct
inference in a [futile attempt at objectivity](/post/improve-research)  still has fundamental
problems.

I use the following slightly oversimplified equations to contrast
frequentist and Bayesian inference.

* Frequentist = subjectivity<sub>1</sub> + subjectivity<sub>2</sub> + objectivity + data + endless arguments about everything
* Bayesian = subjectivity<sub>1</sub> + subjectivity<sub>3</sub> + objectivity + data +
  endless arguments about one thing (the prior)

where

* subjectivity<sub>1</sub> = choice of the data model
* subjectivity<sub>2</sub> = sample space and how repetitions of the
  experiment are envisioned, choice of the stopping rule, 1-tailed
  vs. 2-tailed tests, multiplicity adjustments, ...
* subjectivity<sub>3</sub> = prior distribution

<a name="bayesfreq"></a>

**Aren't they the same anyway?**

I frequently see this argument: Bayes is a lot of trouble, and when
the prior is non-informative, the one-sided p-value equals the
posterior probability of inefficacy, so why not avoid trouble,
controversy, and having to learn new things by just sticking with
p-values?  [Nalborczyk, Bürkner, and Williams](https://online.ucpress.edu/collabra/article/5/1/13/112982/Pragmatism-should-Not-be-a-Substitute-for) explain why comparing the two approaches in this way is not a good idea.  But there are simpler reasons not to do so related to the fact that the two don't even arrive at the same calculated value in the first place:

* For exact agreement one must have an exact p-value.  Outside of simple Gaussian models and a few nonparametric tests, the p-values that are calculated are only approximate.  For example we usually use normal approximations with binary logistic models, and these approximations are not very good in many cases.  Bayesian calculations are exact and do not use normal approximations.
* The equivalence does not exist unless the sample size is fixed and there is exactly one look at the data.  The approaches diverge any time a multiplicity adjustment is used, for example when sequential testing is used or there are multiple endpoints.
* Most practitioners use two-sided p-values, not one-sided p-values.
* Using a non-informative prior on the main endpoint is usually silly, and helps to divorce clinical significance from statistical significance. In most medical treatment comparisons, for example, we know that the treatment is not a cure.  So there is no chance that the odds ratio or hazard ratio, for example, is 0.0.  It is easy to see the silliness of flat priors by running a clinical trial simulation where the prior for an odds ratio is nearly flat.  In simulating Bayesian power one might easily draw an odds ratio of 10,000.
* When one wants to go further than assessing evidence against a simple null hypothesis, it immediately becomes unclear what to compare.  For example one may want to compute the posterior probability that a treatment reduces the risk of heart attack by at least 20%, or the probability that the treatment either reduces mortality by any amount or reduces the risk of heart attack by at least 10%.  With which frequentist calculation would you compare this?

<a name="pwhat"></a>

**And What is a p-value?**

Many believe that p-values are useful and that there is a logic by which p-values provide evidence against the null hypothesis and that makes hypothesis tests useful in decision making.  So far I am not fully convinced.  What exactly can we say about a p-value that doesn't require complex arguments or a leap of logic?  Consider a treatment comparison with a proper design and properly chosen data model that yields p=0.06.  What do we really know directly?

* either the null hypothesis is false or we have just witnessed an event that is improbable (Which event? How improbable does it have to be for us to say this?)
* the probability of observing data **more extreme** that what we observed were the treatment to be ignorable is 0.06 (What does "observing" mean?  Why talk about "more extreme" instead of talking about exactly what we observed?  If the study is sequential are we only talking about observing the latest results?  Are we watching a series of identical study replications?  How could we possibly get funded to do identical replications?)
* Paraphrasing but requiring a bit of a logic leap: Under the supposition that the treatment has zero effect (and could not cause harm), our observed data are in the 0.94 quantile of data extremeness thus seem to be outliers.  We believe this is evidence against the null hypothesis, so we interpret this as evidence in favor of the alternative hypothesis that the treatment effect is nonzero.
* Reviewer: You had the opportunity to detect harm from the treatment and to write a paper claiming such harm.  You need to be penalized for this extra opportunity, so I'm using a p-value of 0.12 instead of 0.06.  Researcher: But we would just drop the treatment if it had an effect in the wrong direction.   No such formal claim of harm would be made.

I feel that the formal interpretations of the p-value has not sufficiently addressed the question of whether the treatment works.  The Bayesian approach is more direct, while embracing uncertainties.  A Bayesian conclusion when the posterior probability of treatment effectiveness is 0.93 may be "Under data model M and skeptical prior P, the treatment probably (0.93) works".  So if I act as if the treatment works, I have a probability of 0.07 of being wrong, under that prior.

------------------------------------------------------------------------
* Frank's [Plenary Session Podcast](https://soundcloud.com/plenarysession/206-heart-failure-choosing-a-specialty-the-bayesian-approach-with-dr-frank-harrell) on Bayesian thinking.
* Frank's [Learning Bayesian Statistics Podcast](https://www.learnbayesstats.com/episode/45-biostats-clinical-trial-design-frank-harrell) on Bayes, clinical trial design, and the journey to biostatistics and Bayes
* Go [here](https://news.ycombinator.com/item?id=13684429) for discussions about this article that are not on this blog.

--------------------------------------
## Other Resources

* [General resources](https://hbiostat.org/bayes)
* [Bernoulli's Fallacy](https://aubreyclayton.com/bernoulli) by Aubrey Clayton
* [Statistical Remedies for Medical Researchers](https://link.springer.com/book/10.1007%2F978-3-030-43714-5) by Peter Thall
* [Bayesian data analysis for newcomers](https://link.springer.com/article/10.3758/s13423-017-1272-1) by John Kruschke and Torrin Liddell
* [Bayesian estimation supersedes the t test](http://www.indiana.edu/~kruschke/articles/Kruschke2013JEPG.pdf) by John Kruschke
* [Bayesian t-tests](https://vuorre.netlify.app/post/2017/01/02/how-to-compare-two-groups-with-robust-bayesian-estimation-using-r-stan-and-brms)
* [Statistical Rethinking](http://xcelab.net/rm/statistical-rethinking) examples with [R brms](https://osf.io/97t6w)
* Michael Clark's [R and Stan](http://m-clark.github.io/documents) example code
* Stephen Martin's [A foray into Bayesian handling of missing data](http://srmart.in/a-foray-into-bayesian-handling-of-missing-data) with Stan
* [Thomas Bayes and the crisis in science](https://www.the-tls.co.uk/articles/public/thomas-bayes-science-crisis) by David Papineau
* [Bayesian theorists were little better than cranks](http://wmbriggs.com/post/24848) by William Briggs.
* [A student's guide to Bayesian statistics](https://youtu.be/P_og8H-VkIY) by Ben Lambert
* [Book for Lambert's _A Student's Guide to Bayesian Statistics_](http://ben-lambert.com/a-students-guide-to-bayesian-statistics)
* [Data analysis: A Bayesian tutorial](https://www.amazon.com/gp/product/0198568320/ref=ppx_yo_dt_b_asin_title_o08_s00?ie=UTF8&psc=1)
  by Devinderjit Sivia and John Skilling
* [Redefine Statistical Significance
  XVII](https://www.bayesianspectacles.org/redefine-statistical-significance-xvii-william-rozeboom-destroys-the-justify-your-own-alpha-argument-back-in-1960)
  by EJ Wagenmakers and Q Gronau
* [Steve Ruberg's Blog](https://analytixthinking.blog)
* [Dicing with the Unknown](https://rss.onlinelibrary.wiley.com/doi/full/10.1111/j.1740-9713.2004.00050.x) by Tony O'Hagan
* [Reasons confidence intervals and credible intervals may not be equated](https://online.ucpress.edu/collabra/article/5/1/13/112982/Pragmatism-should-Not-be-a-Substitute-for) by  Ladislas Nalborczyk, Paul-Christian Bürkner, Donald R. Williams
* [Language for communicating frequentist results about treatment effects](https://discourse.datamethods.org/t/language-for-communicating-frequentist-results-about-treatment-effects)
* [Five statistical fax pas](https://www.methodspace.com/top-5-statistical-fax-pas) by Andy Field
* [Against inferential statistics: How and why current statistics teaching gets It wrong](https://eric.ed.gov/?id=EJ1152511) by Patrick White and Stephen Gorard
----------------------------------------
## More Quotes
* Bayes: Distributional + prior assumption<br>Freq: Distributional + sampling dist assumption<br>You don't need a prior to be 'true', you need it to be defendable. "Given this prior uncertainty, what do the data suggest?"<br>Can you defend the existence of a sampling distribution? - [Stephen Martin](https://twitter.com/smartin2018/status/1025974302607388672)
* The thing is-Both frameworks can operate w/in counterfactual reasoning. "Assuming I am an extreme skeptic, this is what the data suggest", for example.
The nice thing about Bayes is that the counterfactual reasoning is immediate, rather than dependent on samples you'll never see. - [Stephen Martin](https://twitter.com/smartin2018/status/1025975277040623616)
