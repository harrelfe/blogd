--- 
title: Assessing the Proportional Odds Assumption and Its Impact
author: Frank Harrell
date: '2022-03-09'
modified: ''
slug: impactpo
tags:
  - 2022
  - accuracy-score
  - dichotomization
  - endpoints
  - ordinal
link-citations: yes
summary: 'This article demonstrates how the proportional odds assumption and its impact can be assessed.'
header:
  caption: ''
  image: ''
---

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

# Introduction

Reviewers who do not seem to worry about the proportional hazards assumption in a Cox model or the equal variance assumption in a `\(t\)`-test seem to worry a good deal about the proportional odds (PO) assumption in a semiparametric ordinal logistic regression model. This in spite of the fact that proportional hazards and equal variance are exact analogies to the PO assumption in other models. Furthermore, when there is no covariate adjustment, the PO model is equivalent to the Wilcoxon test, and reviewers do not typically criticize the Wilcoxon test or realize that it has optimum power only under the PO assumption.

The purpose of this report is to (1) demonstrate examinations of the PO assumption for a treatment effect in a two-treatment observational comparison, and (2) discuss various issues around PO model analysis and alternative analyses using cutpoints on the outcome variable. It is shown that exercises such as comparing predicted vs. observed values can be misleading when the sample size is not very large.

# Dataset

The dataset, taken from a real observational study, consists of a 7-level ordinal outcome variable `y` having values 0-6, a treatment variable `trt`, and a strong baseline variable `baseline` defined by a disease scale that is related to `y` but with more resolution. This is a dominating covariate, and failure to adjust for it will result in a weaker treatment comparison. `trt` levels are A and B, with 48 patients given treatment B and 100 given treatment A.

``` r
getHdata(txpo)
d <- txpo
dd <- datadist(d); options(datadist='dd')
html(describe(d))
```

<meta http-equiv="Content-Type" content="text/html; charset=utf-8" /> 
<script type="text/javascript">
<!--
    function expand_collapse(id) {
       var e = document.getElementById(id);
       var f = document.getElementById(id+"_earrows");
       if(e.style.display == 'none'){
          e.style.display = 'block';
          f.innerHTML = '&#9650';
       }
       else {
          e.style.display = 'none';
          f.innerHTML = '&#9660';
       }
    }
//-->
</script>
<style>
.earrows {color:silver;font-size:11px;}

fcap {
 font-family: Verdana;
 font-size: 12px;
 color: MidnightBlue
 }

smg {
 font-family: Verdana;
 font-size: 10px;
 color: &#808080;
}

hr.thinhr { margin-top: 0.15em; margin-bottom: 0.15em; }

span.xscript {
position: relative;
}
span.xscript sub {
position: absolute;
left: 0.1em;
bottom: -1ex;
}
</style>
 <font color="MidnightBlue"><div align=center><span style="font-weight:bold">d <br><br> 3  Variables   148  Observations</span></div></font> <hr class="thinhr"> <span style="font-weight:bold">y</span><div style='float: right; text-align: right;'><img src="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAABYAAAANCAMAAACae25RAAAAXVBMVEUAAAAzMzM3Nzc7OztAQEBBQUFDQ0NJSUlNTU1YWFhhYWFnZ2dzc3OgoKCkpKSnp6esrKyvr6+ysrKzs7O0tLS2tra3t7e7u7vDw8PU1NTa2trl5eXq6urr6+v///+UIJEyAAAAaklEQVQYlZ3QyQqAMAwE0LG17q37Fo3//5nSiCClJ+cy5DGngCUzel8dpufGJdmw+lqxPfdvXpYoG/NhIqpLamFJKbJoSeLXuZJ1mr7rs4nyiCgPQHUEvJNnDAHrQthxlvCEjrWWnyQZO9xNDxe6yXp1BQAAAABJRU5ErkJggg==" alt="image" /></div> <style>
 .hmisctable778888 {
 border: none;
 font-size: 85%;
 }
 .hmisctable778888 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable778888 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable778888">
 <tr><th>n</th><th>missing</th><th>distinct</th><th>Info</th><th>Mean</th><th>Gmd</th></tr>
 <tr><td>148</td><td>0</td><td>7</td><td>0.954</td><td>3</td><td>1.759</td></tr>
 </table>
 <span style="font-size: 85%;"><font color="MidnightBlue">lowest</font> : 0 1 2 3 4 ,  <font color="MidnightBlue">highest</font>: 2 3 4 5 6</span> <pre style="font-size:85%;">
 Value          0     1     2     3     4     5     6
 Frequency      6    19    39    24    40     5    15
 Proportion 0.041 0.128 0.264 0.162 0.270 0.034 0.101
 </pre>
 <hr class="thinhr"> <span style="font-weight:bold">trt</span> <style>
 .hmisctable180930 {
 border: none;
 font-size: 85%;
 }
 .hmisctable180930 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable180930 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable180930">
 <tr><th>n</th><th>missing</th><th>distinct</th></tr>
 <tr><td>148</td><td>0</td><td>2</td></tr>
 </table>
 <pre style="font-size:85%;">
 Value          A     B
 Frequency    100    48
 Proportion 0.676 0.324
 </pre>
 <hr class="thinhr"> <span style="font-weight:bold">baseline</span><div style='float: right; text-align: right;'><img src="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAHAAAAANCAMAAABGghpmAAABdFBMVEUAAAABAQECAgIDAwMGBgYICAgLCwsMDAwNDQ0ODg4PDw8QEBARERESEhIUFBQZGRkbGxseHh4iIiIkJCQlJSUpKSksLCwxMTE4ODg6Ojo7Ozs+Pj5AQEBGRkZLS0tMTExNTU1PT09SUlJTU1NUVFRWVlZYWFhZWVlbW1tcXFxfX19gYGBhYWFiYmJkZGRnZ2doaGhqampra2tsbGxtbW1wcHBycnJzc3N4eHh5eXl6enp7e3t8fHx9fX1+fn6Dg4OGhoaHh4eNjY2Pj4+RkZGTk5OWlpaXl5eYmJibm5uhoaGioqKlpaWmpqapqamrq6uvr6+wsLCysrK3t7e5ubm9vb3Dw8PGxsbHx8fNzc3Ozs7R0dHV1dXW1tbY2Nja2trb29vc3Nze3t7f39/g4ODh4eHj4+Pk5OTl5eXn5+fo6Ojp6enq6urr6+vs7Ozu7u7w8PDy8vLz8/P29vb39/f4+Pj6+vr7+/v8/Pz9/f3+/v7////XCqOCAAABVElEQVQ4je3U6zMCARAA8FURhTxK5HBIkhzJK0nelLwfoceR9MCKpGj/edeZM/eFqT70wdgPO7s7s/Ob/bKAqHZgDQOIGuephvEP/gGQ5xsm+MrD46liqRTVXsgw8i4bL//CH0C/V97lbn4BowVy6aoAl/fkc3OXVBVtQVqr/yiVhejXJCwHURGgWW2FYMiSJ72V4sPP9PD4DUbf6XiOcrBKXihQPEsBBVIqQ2cQFsFwkTIpSoBPAt+uywQj27DjFsAjCN72DUhgum6fnK0CuDi0IIA6F/kgQUY7fwhbUwwTu4RT3m5MRGDFPK29f7Kf04YqT3cv4r7XL6TXWKmMFCXsZEYAhV9qBQ4G2zvMuAu25u5e8eWxBryCTeRaMAljYIU0aiZxHUKoZ5UcjKpMpqZx4JSsvtMCNhjR9PSDC5cgiW1Ocd/ACsndIKQLOJD+qEON+An34mFS/g0JYgAAAABJRU5ErkJggg==" alt="image" /></div> <style>
 .hmisctable415799 {
 border: none;
 font-size: 85%;
 }
 .hmisctable415799 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable415799 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable415799">
 <tr><th>n</th><th>missing</th><th>distinct</th><th>Info</th><th>Mean</th><th>Gmd</th><th>.05</th><th>.10</th><th>.25</th><th>.50</th><th>.75</th><th>.90</th><th>.95</th></tr>
 <tr><td>148</td><td>0</td><td>37</td><td>0.993</td><td>20.31</td><td>17.63</td><td> 0</td><td> 0</td><td> 4</td><td>20</td><td>32</td><td>38</td><td>42</td></tr>
 </table>
 <span style="font-size: 85%;"><font color="MidnightBlue">lowest</font> :  0  1  2  3  4 ,  <font color="MidnightBlue">highest</font>: 44 48 51 54 60</span> <hr class="thinhr">

``` r
with(d, pr(obj=table(trt, y)))
```

       y
    trt  0  1  2  3  4  5  6
      A  4  8 26 19 27  2 14
      B  2 11 13  5 13  3  1

# Proportional Odds Model

``` r
f <- lrm(y ~ trt + baseline, data=d)
f
```

<strong>Logistic Regression Model</strong>

<pre>
 lrm(formula = y ~ trt + baseline, data = d)
 </pre>

Frequencies of Responses

<pre>
  0  1  2  3  4  5  6 
  6 19 39 24 40  5 15 
 </pre>
<table class="gmisc_table" style="border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;">
<thead>
<tr>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;">
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;">
Model Likelihood<br>Ratio Test
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;">
Discrimination<br>Indexes
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;">
Rank Discrim.<br>Indexes
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;">
Obs 148
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
LR χ<sup>2</sup> 69.26
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
<i>R</i><sup>2</sup> 0.386
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
<i>C</i> 0.778
</td>
</tr>
<tr>
<td style="min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;">
max \|∂log <i>L</i>/∂β\| 4×10<sup>-13</sup>
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
d.f. 2
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
<i>g</i> 1.624
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
<i>D</i><sub>xy</sub> 0.556
</td>
</tr>
<tr>
<td style="min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;">
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
Pr(\>χ<sup>2</sup>) \<0.0001
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
<i>g</i><sub>r</sub> 5.071
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
γ 0.571
</td>
</tr>
<tr>
<td style="min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;">
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
<i>g</i><sub>p</sub> 0.310
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
τ<sub>a</sub> 0.449
</td>
</tr>
<tr>
<td style="min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;">
</td>
<td style="min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;">
</td>
<td style="min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;">
Brier 0.151
</td>
<td style="min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;">
</td>
</tr>
</tbody>
</table>
<table class="gmisc_table" style="border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;">
<thead>
<tr>
<th style="border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;">
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;">
β
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;">
S.E.
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;">
Wald <i>Z</i>
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;">
Pr(\>\|<i>Z</i>\|)
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="min-width: 7em; text-align: left;">
y≥1
</td>
<td style="min-width: 7em; text-align: right;">
  6.1565
</td>
<td style="min-width: 7em; text-align: right;">
 0.6167
</td>
<td style="min-width: 7em; text-align: right;">
9.98
</td>
<td style="min-width: 7em; text-align: right;">
\<0.0001
</td>
</tr>
<tr>
<td style="min-width: 7em; text-align: left;">
y≥2
</td>
<td style="min-width: 7em; text-align: right;">
  4.3821
</td>
<td style="min-width: 7em; text-align: right;">
 0.4718
</td>
<td style="min-width: 7em; text-align: right;">
9.29
</td>
<td style="min-width: 7em; text-align: right;">
\<0.0001
</td>
</tr>
<tr>
<td style="min-width: 7em; text-align: left;">
y≥3
</td>
<td style="min-width: 7em; text-align: right;">
  2.5139
</td>
<td style="min-width: 7em; text-align: right;">
 0.3600
</td>
<td style="min-width: 7em; text-align: right;">
6.98
</td>
<td style="min-width: 7em; text-align: right;">
\<0.0001
</td>
</tr>
<tr>
<td style="min-width: 7em; text-align: left;">
y≥4
</td>
<td style="min-width: 7em; text-align: right;">
  1.5520
</td>
<td style="min-width: 7em; text-align: right;">
 0.3174
</td>
<td style="min-width: 7em; text-align: right;">
4.89
</td>
<td style="min-width: 7em; text-align: right;">
\<0.0001
</td>
</tr>
<tr>
<td style="min-width: 7em; text-align: left;">
y≥5
</td>
<td style="min-width: 7em; text-align: right;">
 -0.3033
</td>
<td style="min-width: 7em; text-align: right;">
 0.3150
</td>
<td style="min-width: 7em; text-align: right;">
-0.96
</td>
<td style="min-width: 7em; text-align: right;">
0.3357
</td>
</tr>
<tr>
<td style="min-width: 7em; text-align: left;">
y≥6
</td>
<td style="min-width: 7em; text-align: right;">
 -0.6738
</td>
<td style="min-width: 7em; text-align: right;">
 0.3361
</td>
<td style="min-width: 7em; text-align: right;">
-2.00
</td>
<td style="min-width: 7em; text-align: right;">
0.0450
</td>
</tr>
<tr>
<td style="min-width: 7em; text-align: left;">
trt=B
</td>
<td style="min-width: 7em; text-align: right;">
 -1.1328
</td>
<td style="min-width: 7em; text-align: right;">
 0.3290
</td>
<td style="min-width: 7em; text-align: right;">
-3.44
</td>
<td style="min-width: 7em; text-align: right;">
0.0006
</td>
</tr>
<tr>
<td style="min-width: 7em; border-bottom: 2px solid grey; text-align: left;">
baseline
</td>
<td style="min-width: 7em; border-bottom: 2px solid grey; text-align: right;">
 -0.0888
</td>
<td style="min-width: 7em; border-bottom: 2px solid grey; text-align: right;">
 0.0121
</td>
<td style="min-width: 7em; border-bottom: 2px solid grey; text-align: right;">
-7.32
</td>
<td style="min-width: 7em; border-bottom: 2px solid grey; text-align: right;">
\<0.0001
</td>
</tr>
</tbody>
</table>

``` r
summary(f)
```

<table class="gmisc_table" style="border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;">
<thead>
<tr>
<td colspan="8" style="text-align: left;">
Effects   Response: <code>y</code>
</td>
</tr>
<tr>
<th style="border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;">
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;">
Low
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;">
High
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;">
Δ
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;">
Effect
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;">
S.E.
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;">
Lower 0.95
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;">
Upper 0.95
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">
baseline
</td>
<td style="padding-left:4ex; text-align: right;">
4
</td>
<td style="padding-left:4ex; text-align: right;">
32
</td>
<td style="padding-left:4ex; text-align: right;">
28
</td>
<td style="padding-left:4ex; text-align: right;">
-2.48800
</td>
<td style="padding-left:4ex; text-align: right;">
0.3397
</td>
<td style="padding-left:4ex; text-align: right;">
-3.15300
</td>
<td style="padding-left:4ex; text-align: right;">
-1.8220
</td>
</tr>
<tr>
<td style="text-align: left;">
 <em>Odds Ratio</em>
</td>
<td style="padding-left:4ex; text-align: right;">
4
</td>
<td style="padding-left:4ex; text-align: right;">
32
</td>
<td style="padding-left:4ex; text-align: right;">
28
</td>
<td style="padding-left:4ex; text-align: right;">
0.08311
</td>
<td style="padding-left:4ex; text-align: right;">
</td>
<td style="padding-left:4ex; text-align: right;">
0.04271
</td>
<td style="padding-left:4ex; text-align: right;">
0.1617
</td>
</tr>
<tr>
<td style="text-align: left;">
trt — B:A
</td>
<td style="padding-left:4ex; text-align: right;">
1
</td>
<td style="padding-left:4ex; text-align: right;">
2
</td>
<td style="padding-left:4ex; text-align: right;">
</td>
<td style="padding-left:4ex; text-align: right;">
-1.13300
</td>
<td style="padding-left:4ex; text-align: right;">
0.3290
</td>
<td style="padding-left:4ex; text-align: right;">
-1.77800
</td>
<td style="padding-left:4ex; text-align: right;">
-0.4880
</td>
</tr>
<tr>
<td style="border-bottom: 2px solid grey; text-align: left;">
 <em>Odds Ratio</em>
</td>
<td style="padding-left:4ex; border-bottom: 2px solid grey; text-align: right;">
1
</td>
<td style="padding-left:4ex; border-bottom: 2px solid grey; text-align: right;">
2
</td>
<td style="padding-left:4ex; border-bottom: 2px solid grey; text-align: right;">
</td>
<td style="padding-left:4ex; border-bottom: 2px solid grey; text-align: right;">
0.32210
</td>
<td style="padding-left:4ex; border-bottom: 2px solid grey; text-align: right;">
</td>
<td style="padding-left:4ex; border-bottom: 2px solid grey; text-align: right;">
0.16900
</td>
<td style="padding-left:4ex; border-bottom: 2px solid grey; text-align: right;">
0.6138
</td>
</tr>
</tbody>
</table>

``` r
anova(f)
```

<table class="gmisc_table" style="border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;">
<thead>
<tr>
<td colspan="4" style="text-align: left;">
Wald Statistics for <code style="font-size:0.8em">y</code>
</td>
</tr>
<tr>
<th style="border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;">
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;">
<i>χ<sup>2</sup></i>
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;">
d.f.
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;">
<i>P</i>
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;">
trt
</td>
<td style="padding-left:3ex; text-align: right;">
11.86
</td>
<td style="padding-left:3ex; text-align: right;">
1
</td>
<td style="padding-left:3ex; text-align: right;">
0.0006
</td>
</tr>
<tr>
<td style="text-align: left;">
baseline
</td>
<td style="padding-left:3ex; text-align: right;">
53.63
</td>
<td style="padding-left:3ex; text-align: right;">
1
</td>
<td style="padding-left:3ex; text-align: right;">
\<0.0001
</td>
</tr>
<tr>
<td style="border-bottom: 2px solid grey; text-align: left;">
TOTAL
</td>
<td style="padding-left:3ex; border-bottom: 2px solid grey; text-align: right;">
56.57
</td>
<td style="padding-left:3ex; border-bottom: 2px solid grey; text-align: right;">
2
</td>
<td style="padding-left:3ex; border-bottom: 2px solid grey; text-align: right;">
\<0.0001
</td>
</tr>
</tbody>
</table>

# Volatility of ORs Using Different Cutoffs

Even when the data generating mechanism is exactly proportional odds for treatment, different cutoffs of the response variable Y can lead to much different ORs when the sample size is not in the thousands. This is just the play of chance (sampling variation). To illustrate this point, consider the observed proportions of Y for `trt=A` as population probabilities for A. Apply an odds ratio of 0.3 to get the population distribution of Y for treated patients. For 10 simulated trials, sample from these two multinomial distributions and compute sample ORs for all Y cutoffs.

``` r
p <- table(d$y[d$trt == 'A'])
p <- p / sum(p)
p   # probabilities for SOC
```


       0    1    2    3    4    5    6 
    0.04 0.08 0.26 0.19 0.27 0.02 0.14 

``` r
set.seed(1)
round(simPOcuts(n=210, odds.ratio=0.3, p=p), 2)
```

                  y>=1 y>=2 y>=3 y>=4 y>=5 y>=6
    Simulation 1  0.13 0.25 0.33 0.28 0.14 0.16
    Simulation 2  0.39 0.26 0.41 0.41 1.00 1.00
    Simulation 3  0.39 0.37 0.44 0.47 0.90 0.78
    Simulation 4  0.18 0.27 0.30 0.41 0.32 0.23
    Simulation 5  0.43 0.31 0.24 0.17 0.43 0.33
    Simulation 6  0.17 0.37 0.37 0.39 0.36 0.30
    Simulation 7  0.14 0.24 0.22 0.17 0.30 0.33
    Simulation 8  0.37 0.50 0.28 0.29 0.26 0.26
    Simulation 9  0.85 0.45 0.36 0.40 0.54 0.61
    Simulation 10 0.36 0.34 0.31 0.26 0.21 0.22

# Examining the PO Assumption

For discrete Y we are interested in checking the impact of
the PO assumption on predicted probabilities for all of the Y
categories, while also allowing for covariate adjustment. This can be
done using the following steps:

-   Select a set of covariate settings over which to evaluate accuracy of predictions
-   Vary at least one of the predictors, i.e., the one for which you want to assess the impact of the PO assumption
-   Fit a PO model the usual way
-   Fit models that relaxes the PO assumption
    -   to relax the PO assumption for all predictors fit a multinomial logistic model
    -   to relax the PO assumption for a subset of predictors fit a partial PO (PPO) model
-   For all the covariate combinations evaluate predicted probabilities for all levels of Y using the PO model and the relaxed assumption models
-   Use the bootstrap to compute confidence intervals for the differences in predicted values betwween a PO model and a relaxed model. This will put the differences in the right context by accounting for uncertainties. This guards against over-emphasis of differences when the sample size does not support estimation, especially for the relaxed model with more parameters. Note that the same problem occurs when comparing predicted unadjusted probabilities to observed proportions, as observed proportions can be noisy.

Level 5 of `y` has only 5 patients so we combine it with level 6 for fitting the two relaxed models that depend on individual cell frequencies. Similarly, level 0 has only 6 patients, so we combine it with level 1. The PPO model is fitted with the `VGAM` R package, and the `nonpo` argument below signifies that the PO assumption is only being relaxed for the treatment effect. The multinomial model allows not only non-PO for `trt` but also for `baseline`.

``` r
nd <- data.frame(trt=levels(d$trt), baseline=4)
d$y5 <- with(d, pmin(pmax(y, 1), 5))
w <- impactPO(y5 ~ trt + baseline, nonpo = ~ trt,
              data=d, newdata=nd, B=300)
```

``` r
w
```

                              PO     PPO    Multinomial
    Deviance                  395.58 393.10 388.36     
    d.f.                       6      9     12         
    AIC                       407.58 411.10 412.36     
    p                         2      5      8          
    LR chi^2                  69.41  71.89  76.63      
    LR - p                    67.41  66.89  68.63      
    LR chi^2 test for PO             2.48   7.22       
      d.f.                            3      6         
      Pr(>chi^2)                     0.4792 0.3013     
    Cox-Snell R2              0.374  0.385  0.404      
    Cox-Snell R2 adj          0.366  0.364  0.371      
    McFadden R2               0.149  0.155  0.165      
    McFadden R2 adj           0.141  0.133  0.130      
    Mean |difference| from PO        0.021  0.042      

    Covariate combination-specific mean |difference| in predicted probabilities

            method trt baseline Mean |difference|
    1          PPO   A        4             0.010
    2          PPO   B        4             0.033
    11 Multinomial   A        4             0.032
    21 Multinomial   B        4             0.052

    Bootstrap 0.95 confidence intervals for differences in model predicted
    probabilities based on 300 bootstraps


      trt baseline
    1   A        4

    PO - PPO probability estimates

               1      2      3      4      5
    Lower -0.004 -0.017 -0.058 -0.055 -0.042
    Upper  0.008  0.018  0.008  0.081  0.058

    PO - Multinomial probability estimates

              1      2      3      4      5
    Lower 0.002 -0.017 -0.152 -0.105 -0.037
    Upper 0.020  0.071 -0.006  0.107  0.133

      trt baseline
    2   B        4

    PO - PPO probability estimates

               1      2      3      4      5
    Lower -0.043 -0.077 -0.025 -0.191 -0.101
    Upper  0.013  0.083  0.197  0.065  0.095

    PO - Multinomial probability estimates

               1      2      3      4      5
    Lower -0.050 -0.025 -0.051 -0.272 -0.143
    Upper  0.035  0.147  0.194  0.041  0.095

Comparisons of the PO model fit with models that relax the PO assumption above can be summarized as follows.

-   By AIC, the model that is most likely to have the best cross-validation performance is the fully PO model (the lower the AIC the better)
-   There is no evidence for non-PO, either when judging against a model that relaxes the PO assumption for treatment (P=0.48) or against a multinomial logistic model that does not assume PO for any variables (P=0.30).
-   The McFadden adjusted `\(R^2\)` index, in line with AIC, indicates the best fit is from the PO model
-   The Cox-Snell adjusted `\(R^2\)` indicates the PO model is competitive. See [this](https://hbiostat.org/bib/r2.html) for information about general adjusted `\(R^2\)` measures.
-   Nonparametric bootstrap percentile confidence intervals for the difference in predicted values between the PO model and one of the relaxed models take into account uncertainties and correlations of both sets of estimates. In all cases the confidence intervals are quite wide and include 0 (except for one case, where the lower confidence limit is 0.002), which is very much in line with apparent differences being clouded by overfitting (high number of parameters in non-PO models).

These assessments must be kept in mind when interpreting the inter-model agreement between probabilities of all levels of the ordinal outcome in the graphic that follows. According to AIC and adjusted `\(R^2\)`, the estimates from the partial PO model and especially those from the multinomial model are overfitted. This is related to the issue that odds ratios computed from oversimplifying an ordinal response by dichotomizing it are noisy (also see the next to last section below).

``` r
revo <- function(z) {
  z <- as.factor(z)
  factor(z, levels=rev(levels(as.factor(z))))
}
ggplot(w$estimates, aes(x=method, y=Probability, fill=revo(y))) +
  facet_wrap(~ trt) + geom_col() +
  xlab('') + guides(fill=guide_legend(title=''))
```

<img src="/post/impactpo_files/figure-html/plimpactpo-1.png" width="672" />

AIC is essentially a forecast of what is likely to happen were the accuracy of two competing models be computed on a new dataset not used to fit the model. Had the observational study’s sample size been much larger, we could have randomly split the data into training and test samples and had a head-to-head comparison of the predictive accuracy of a PO model vs. a non-PO (multinomial or partial PO) model in the test sample. Non-PO models will be more unbiased but pay a significant price in terms of variance of estimates. The AIC and adjusted `\(R^2\)` analyses above suggest that the PO model will have lower mean squared errors of outcome probability estimates due to the strong reduction in variance (also see below).

# Efficiency of Analyses Using Cutpoints

<p class="rquote">
Clearly, the dependence of the proportional odds model on the assumption of proportionality can be over-stressed. Suppose that two different statisticians would cut the same three-point scale at different cut points. It is hard to see how anybody who could accept either dichotomy could object to the compromise answer produced by the proportional odds model. — <a href="https://onlinelibrary.wiley.com/doi/abs/10.1002/sim.3603">Stephen Senn</a>
</p>

Above I considered evidence in favor of making the PO assumption. Now consider the cost of **not** making the assumption. What is the efficiency of using a dichotomous endpoint? Efficiency can be captured by comparing the variance of an inefficient estimate to the variance of the most efficient estimate (which comes from the PO model by using the full information in all levels of the outcome variable). We don’t know the true variances of estimated treatment effects so instead use the estimated variances from fitted PO and binary logistic models.

``` r
vtrt <- function(fit) vcov(fit)['trt=B', 'trt=B']
vpo <- vtrt(f)
w <- NULL
for(cutoff in 2 : 6) {
  h <- lrm(y >= cutoff ~ trt + baseline, data=d)
  eff <- vpo / vtrt(h)
  w <- rbind(w, data.frame(Cutoff=paste0('y≥', cutoff),
                           Efficiency=round(eff, 2),
                           `Sample Size Ratio`=round(1/eff, 1),
                           check.names=FALSE))
  }
w
```

      Cutoff Efficiency Sample Size Ratio
    1    y≥2       0.38               2.6
    2    y≥3       0.44               2.3
    3    y≥4       0.56               1.8
    4    y≥5       0.29               3.5
    5    y≥6       0.09              10.6

Under PO the odds ratio from the PO model estimates the same quantity as the odds ratio from any dichotomization of the outcome. The relative efficiency of a dichotomized analysis is the variance of the most efficient (PO model) model’s log odds ratio for treatment divided by the variance of the log odds ratio from a binary logistic model using the dichomization.
The optimal cutoff (mainly due to being a middle value in the frequency distribution) is y≥4. For this dichotomization the effiency is 0.56 (i.e., analyzing y≥4 vs. y is equivalent to discarding 44% of the sample) and the variance of the treatment log odds ratio is `\(1.8 \times\)` greater than the variance of the log odds ratio from the proportional odds model without binning. This means that the study would have to be `\(1.8 \times\)` larger to have the same power when dichotomizing the outcome as a smaller study that did not dichotomize it. Other dichotomizations result in even worse efficiency.

For more examples of relative efficiencies for various outcome configurations see [Information Gain From Using Ordinal Instead of Binary Outcomes](https://www.fharrell.com/post/ordinal-info).

# PO Model Results are Meaningful Even When PO is Violated

Putting aside covariate adjustment, the PO model is equivalent to a Wilcoxon-Mann-Whitney two-sample rank-sum test statistic. The normalized Wilcoxon statistic (concordance probability; also called probability index) is to within a high degree of approximation a simple function of the estimated odds ratio from a PO model fit. Over a wide variety of datasets satisfying and violating PO, the `\(R^2\)` for predicting the log odds ratio from the logit of the scaled Wilcoxon statistic is 0.996, and the mean absolute error in predicting the concordance probability from the log odds ratio is 0.002. See [Violation of Proportional Odds is Not Fatal](https://www.fharrell.com/post/po) and [If You Like the Wilcoxon Test You Must Like the Proportional Odds Model](https://www.fharrell.com/post/wpo).

Let’s compare the actual Wilcoxon concordance probability with the concordance probability estimated from the odds ratio without covariate adjustment, `\(\frac{\mathrm{OR}^{0.66}}{1 + \mathrm{OR}^{0.66}}\)`.

``` r
w <- wilcox.test(y ~ trt, data=d)
w
```


        Wilcoxon rank sum test with continuity correction

    data:  y by trt
    W = 2881, p-value = 0.04395
    alternative hypothesis: true location shift is not equal to 0

``` r
W <- w$statistic
concord <- W / prod(table(d$trt))
```

``` r
u <- lrm(y ~ trt, data=d)
u
```

<strong>Logistic Regression Model</strong>

<pre>
 lrm(formula = y ~ trt, data = d)
 </pre>

Frequencies of Responses

<pre>
  0  1  2  3  4  5  6 
  6 19 39 24 40  5 15 
 </pre>
<table class="gmisc_table" style="border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;">
<thead>
<tr>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;">
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;">
Model Likelihood<br>Ratio Test
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;">
Discrimination<br>Indexes
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; border-right: 1px solid black; text-align: center;">
Rank Discrim.<br>Indexes
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;">
Obs 148
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
LR χ<sup>2</sup> 4.18
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
<i>R</i><sup>2</sup> 0.029
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
<i>C</i> 0.555
</td>
</tr>
<tr>
<td style="min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;">
max \|∂log <i>L</i>/∂β\| 2×10<sup>-7</sup>
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
d.f. 1
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
<i>g</i> 0.285
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
<i>D</i><sub>xy</sub> 0.110
</td>
</tr>
<tr>
<td style="min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;">
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
Pr(\>χ<sup>2</sup>) 0.0409
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
<i>g</i><sub>r</sub> 1.330
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
γ 0.247
</td>
</tr>
<tr>
<td style="min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;">
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
<i>g</i><sub>p</sub> 0.070
</td>
<td style="min-width: 9em; border-right: 1px solid black; text-align: center;">
τ<sub>a</sub> 0.088
</td>
</tr>
<tr>
<td style="min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;">
</td>
<td style="min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;">
</td>
<td style="min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;">
Brier 0.240
</td>
<td style="min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;">
</td>
</tr>
</tbody>
</table>
<table class="gmisc_table" style="border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;">
<thead>
<tr>
<th style="border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; min-width: 7em; text-align: center;">
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;">
β
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;">
S.E.
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;">
Wald <i>Z</i>
</th>
<th style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;">
Pr(\>\|<i>Z</i>\|)
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="min-width: 7em; text-align: left;">
y≥1
</td>
<td style="min-width: 7em; text-align: right;">
  3.4217
</td>
<td style="min-width: 7em; text-align: right;">
 0.4390
</td>
<td style="min-width: 7em; text-align: right;">
7.79
</td>
<td style="min-width: 7em; text-align: right;">
\<0.0001
</td>
</tr>
<tr>
<td style="min-width: 7em; text-align: left;">
y≥2
</td>
<td style="min-width: 7em; text-align: right;">
  1.8302
</td>
<td style="min-width: 7em; text-align: right;">
 0.2524
</td>
<td style="min-width: 7em; text-align: right;">
7.25
</td>
<td style="min-width: 7em; text-align: right;">
\<0.0001
</td>
</tr>
<tr>
<td style="min-width: 7em; text-align: left;">
y≥3
</td>
<td style="min-width: 7em; text-align: right;">
  0.4742
</td>
<td style="min-width: 7em; text-align: right;">
 0.1948
</td>
<td style="min-width: 7em; text-align: right;">
2.43
</td>
<td style="min-width: 7em; text-align: right;">
0.0149
</td>
</tr>
<tr>
<td style="min-width: 7em; text-align: left;">
y≥4
</td>
<td style="min-width: 7em; text-align: right;">
 -0.1890
</td>
<td style="min-width: 7em; text-align: right;">
 0.1929
</td>
<td style="min-width: 7em; text-align: right;">
-0.98
</td>
<td style="min-width: 7em; text-align: right;">
0.3272
</td>
</tr>
<tr>
<td style="min-width: 7em; text-align: left;">
y≥5
</td>
<td style="min-width: 7em; text-align: right;">
 -1.6691
</td>
<td style="min-width: 7em; text-align: right;">
 0.2561
</td>
<td style="min-width: 7em; text-align: right;">
-6.52
</td>
<td style="min-width: 7em; text-align: right;">
\<0.0001
</td>
</tr>
<tr>
<td style="min-width: 7em; text-align: left;">
y≥6
</td>
<td style="min-width: 7em; text-align: right;">
 -1.9983
</td>
<td style="min-width: 7em; text-align: right;">
 0.2858
</td>
<td style="min-width: 7em; text-align: right;">
-6.99
</td>
<td style="min-width: 7em; text-align: right;">
\<0.0001
</td>
</tr>
<tr>
<td style="min-width: 7em; border-bottom: 2px solid grey; text-align: left;">
trt=B
</td>
<td style="min-width: 7em; border-bottom: 2px solid grey; text-align: right;">
 -0.6456
</td>
<td style="min-width: 7em; border-bottom: 2px solid grey; text-align: right;">
 0.3174
</td>
<td style="min-width: 7em; border-bottom: 2px solid grey; text-align: right;">
-2.03
</td>
<td style="min-width: 7em; border-bottom: 2px solid grey; text-align: right;">
0.0420
</td>
</tr>
</tbody>
</table>

Note that the `\(C\)` statistic in the above table handles ties differently than the concordance probability we are interested in here.

``` r
or <- exp(- coef(u)['trt=B'])
cat('Concordance probability from Wilcoxon statistic: ', concord, '\n',
    'Concordance probability estimated from OR: ',
    or ^ 0.66 / (1 + or ^ 0.66), '\n', sep='')
```

    Concordance probability from Wilcoxon statistic: 0.6002083
    Concordance probability estimated from OR: 0.604937

# Computing Environment

<pre>
 R version 4.1.2 (2021-11-01)
 Platform: x86_64-pc-linux-gnu (64-bit)
 Running under: Pop!_OS 21.10
 
 Matrix products: default
 BLAS:   /usr/lib/x86_64-linux-gnu/blas/libblas.so.3.9.0
 LAPACK: /usr/lib/x86_64-linux-gnu/lapack/liblapack.so.3.9.0
 
 attached base packages:
 [1] stats     graphics  grDevices utils     datasets  methods   base     
 
 other attached packages:
 [1] rms_6.2-1       SparseM_1.81    Hmisc_4.6-1     ggplot2_3.3.3  
 [5] Formula_1.2-4   survival_3.2-13 lattice_0.20-45
 </pre>
 To cite R in publications use:
 <p>R Core Team (2021).
 <em>R: A Language and Environment for Statistical Computing</em>.
 R Foundation for Statistical Computing, Vienna, Austria.
 <a href="https://www.R-project.org/">https://www.R-project.org/</a>. 
 </p>
 
To cite the <tt>Hmisc</tt> package in publications use:

 <p>Harrell Jr F (2022).
 <em>Hmisc: Harrell Miscellaneous</em>.
 R package version 4.6-1, <a href="https://hbiostat.org/R/Hmisc/">https://hbiostat.org/R/Hmisc/</a>. 
 </p>
 
To cite the <tt>rms</tt> package in publications use:

 <p>Harrell Jr FE (2022).
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
 R package version 3.2-13, <a href="https://CRAN.R-project.org/package=survival">https://CRAN.R-project.org/package=survival</a>. 
 </p>
