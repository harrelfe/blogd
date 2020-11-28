---
title: "Implementation of the PATH Statement"
author: "Ewout Steyerberg"
date: "2020-11-24"
slug: path
tags:
  - RCT
  - drug-evaluation
  - generalizability
  - medicine
  - personalized-medicine
  - prediction
  - subgroup
  - 2020
summary: 'The recent  PATH (Predictive Approaches to Treatment effect Heterogeneity) Statement outlines principles, criteria, and key considerations for applying predictive approaches to clinical trials to provide patient-centered evidence in support of decision making. Here challenges in implementing the PATH Statement are addressed with the GUSTO-I trial as a case study.'
header:
  caption: ''
  image: ''
output:
  blogdown::html_page:
    fig_width: 7
    fig_height: 5
    dev: "svglite"
---



Ewout Steyerberg<br><small><tt>e.w.steyerberg@lumc.nl</tt></small><br><small><tt> [Twitter: ESteyerberg](https://twitter.com/ESteyerberg) </tt></small><br><small><tt> [Google scholar](https://scholar.google.com/citations?user=_75LDyMAAAAJ&hl=nl) </tt></small><br><small><tt> [ORCID](https://orcid.org/0000-0002-7787-0122) </tt></small>

>    
>    **_Evidence is derived from groups while most medical decisions are made for individual patients_**  
>    *(Kent et al, PATH statement)*  
>    

Heterogeneity of treatment effect (**_HTE_**) refers to the nonrandom variation in the magnitude of the absolute treatment effect (**_treatment benefit_**) across individual patients. The recent  **_[PATH](https://www.ncbi.nlm.nih.gov/pubmed/31711134)_** (Predictive Approaches to Treatment effect Heterogeneity) Statement outlines principles, criteria, and key considerations for applying predictive approaches to clinical trials to provide patient-centered evidence in support of decision making. The focus of PATH is on modeling of **_HTE_** across individual patients.  

The PATH statement lists a number of principles and guidelines. A first principle is to establish **_overall treatment effect_**. In another [blog](/post/covadj), I summarized the arguments in favor of covariate adjustment as the primary analysis of a RCT. Illustration was in the GUSTO-I trial. Here I continue that illustration, also following the blog by **Frank Harrell** on **_[examining HTE](/post/varyor)_**.

## Illustration in the GUSTO-I trial
Let's analyze the data from 30,510 patients with an acute myocardial infarction as included in the GUSTO-I trial. In GUSTO-I, 10,348 patients were randomized to receive tissue plasminogen activator (tPA), while 20,162 were randomzied to Streptokinase (SK) and had 30-day mortality status known.


```
Input object size:	 4042624 bytes;	 29 variables	 30510 observations
Kept variables	day30,tx,age,Killip,sysbp,pulse,pmi,miloc,sex
New object size:	1350344 bytes;	9 variables	30510 observations
```

<!--html_preserve--><div style="width: 100ex; overflow: auto; height: 25ex;"> <meta http-equiv="Content-Type" content="text/html; charset=utf-8" /> 
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
 <font color="MidnightBlue"><div align=center><span style="font-weight:bold">gusto <br><br> 9  Variables   30510  Observations</span></div></font> <hr class="thinhr"> <span style="font-weight:bold">day30</span> <style>
 .hmisctable306431 {
 border: none;
 font-size: 85%;
 }
 .hmisctable306431 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable306431 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable306431">
 <tr><th>n</th><th>missing</th><th>distinct</th><th>Info</th><th>Sum</th><th>Mean</th><th>Gmd</th></tr>
 <tr><td>30510</td><td>0</td><td>2</td><td>0.195</td><td>2128</td><td>0.06975</td><td>0.1298</td></tr>
 </table>
 <hr class="thinhr"> <span style="font-weight:bold">sex</span>: Sex <style>
 .hmisctable204390 {
 border: none;
 font-size: 85%;
 }
 .hmisctable204390 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable204390 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable204390">
 <tr><th>n</th><th>missing</th><th>distinct</th></tr>
 <tr><td>30510</td><td>0</td><td>2</td></tr>
 </table>
 <pre style="font-size:85%;">
 Value        male female
 Frequency   22795   7715
 Proportion  0.747  0.253
 </pre>
 <hr class="thinhr"> <span style="font-weight:bold">Killip</span>: Killip Class<div style='float: right; text-align: right;'><img src="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAA0AAAANCAMAAABFNRROAAAAOVBMVEUAAAAdHR02NjZlZWWioqKwsLC+vr6/v7/KysrOzs7U1NTZ2dna2trl5eXm5ubr6+vs7Ozz8/P///+qr/yoAAAAMElEQVQImWNg4OZHAAYGASEEIJ/HwYMApJnCyyyIxGNl4ITzuPlZGNj4uRjZ+fmYAJouCcAuq6zZAAAAAElFTkSuQmCC" alt="image" /></div> <style>
 .hmisctable569678 {
 border: none;
 font-size: 85%;
 }
 .hmisctable569678 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable569678 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable569678">
 <tr><th>n</th><th>missing</th><th>distinct</th></tr>
 <tr><td>30510</td><td>0</td><td>4</td></tr>
 </table>
 <pre style="font-size:85%;">
 Value          I    II   III    IV
 Frequency  26007  3857   417   229
 Proportion 0.852 0.126 0.014 0.008
 </pre>
 <hr class="thinhr"> <span style="font-weight:bold">age</span><div style='float: right; text-align: right;'><img src="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAJcAAAANCAMAAACTvAxuAAAB2lBMVEUAAAABAQEDAwMEBAQFBQUGBgYHBwcICAgKCgoLCwsMDAwODg4QEBARERESEhIUFBQWFhYXFxcYGBgaGhobGxscHBwdHR0eHh4fHx8gICAhISEiIiIjIyMkJCQmJiYnJycpKSkqKiorKystLS0uLi4vLy8wMDAxMTEzMzM0NDQ1NTU2NjY3Nzc4ODg5OTk6Ojo7Ozs8PDw9PT0+Pj4/Pz9AQEBBQUFCQkJDQ0NERERFRUVGRkZHR0dISEhJSUlKSkpLS0tMTExNTU1PT09QUFBRUVFSUlJUVFRWVlZXV1dYWFhZWVlbW1teXl5gYGBhYWFkZGRlZWVnZ2doaGhqampra2ttbW1vb29wcHBycnJzc3N1dXV3d3d4eHh6enp7e3t8fHx/f3+BgYGGhoaHh4eIiIiJiYmKioqLi4uMjIyPj4+RkZGWlpaXl5eZmZmdnZ2enp6hoaGioqKjo6OkpKSlpaWmpqanp6erq6usrKywsLCysrKzs7O4uLi8vLy9vb3AwMDCwsLGxsbJycnKysrPz8/S0tLV1dXY2NjZ2dna2tre3t7j4+Pk5OTl5eXp6enq6urr6+vt7e3u7u7v7+/x8fHy8vL29vb4+Pj5+fn7+/v8/Pz+/v7///80JPtyAAAB6klEQVQ4jb3V2VPTUBQG8K9BWlooW62lpbSFUAyQdEuapDVpEURQy+KGC4IoVXBXXKAosqiAoEIQVMD+r17HF8fRMdNov5d7Zu6dc39zXg404/mQ41px7a6WPdB1wTE3Z53IPn9rtCfyhvLFc+9OMINWFknXwQycCUQ5KFUlo1PG+uYNuHY2Rx5fR9pUpYBhkXAi/d3FMsSFDqx+Krrrc+jhjZnFq3EFkhcp/M6VAn26eC5tL7/5xj12FqqlPA06AvHPLm/vzfX1orj2t+2j2XYVfCNx2dS/uSRI3MlHY4W61vRmachJCfCwaGsAby6LwceAc0GAPYqmIMK1pKyNINiEmB08XBwYP2Jll3OvdH/xU/TOa/8d1WmnUvAJeuclIkJDtVmOlW4XMi89j5aXz2SSUApzqRiYXPkPLm1t0q8g1FK4q1HE7MrG13/o+shPP7AdR51szMVDbi4dir/eMOja3VlYffb05aXhQSQd5A/jLsGHFFV9bvzJ7PzWe10usov6ctrFCe3WeXG85+hIffcpSzdtUkw1IloOI16DBA7xaKMhVlAyvGGEfJDNVgkBFlE3ua0UEWQgOEjpEMA0Q6wkpTsKNgDJapbhDyHcAJmqkEC344jfM5zsv/9jDZ548ctevH1Fy/WR8xvGqUTi2QsCnAAAAABJRU5ErkJggg==" alt="image" /></div> <style>
 .hmisctable721035 {
 border: none;
 font-size: 85%;
 }
 .hmisctable721035 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable721035 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable721035">
 <tr><th>n</th><th>missing</th><th>distinct</th><th>Info</th><th>Mean</th><th>Gmd</th><th>.05</th><th>.10</th><th>.25</th><th>.50</th><th>.75</th><th>.90</th><th>.95</th></tr>
 <tr><td>30510</td><td>0</td><td>5342</td><td>1</td><td>60.91</td><td>13.58</td><td>40.92</td><td>44.73</td><td>52.11</td><td>61.58</td><td>69.84</td><td>76.19</td><td>79.42</td></tr>
 </table>
 <span style="font-size: 85%;"><font color="MidnightBlue">lowest</font> :  19.0  20.8  21.0  21.0  21.4 ,  <font color="MidnightBlue">highest</font>:  91.9  92.3  96.5 108.0 110.0</span> <hr class="thinhr"> <span style="font-weight:bold">pulse</span>: Heart Rate <span style='font-family:Verdana;font-size:75%;'>beats/min</span><div style='float: right; text-align: right;'><img src="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAJcAAAANCAMAAACTvAxuAAAB2lBMVEUAAAABAQECAgIDAwMEBAQGBgYHBwcICAgJCQkKCgoLCwsMDAwQEBARERESEhIUFBQVFRUWFhYXFxcYGBgZGRkaGhobGxscHBwdHR0eHh4fHx8gICAhISEiIiIjIyMkJCQlJSUmJiYnJycoKCgpKSkqKiorKyssLCwtLS0wMDAyMjIzMzM0NDQ2NjY4ODg5OTk6Ojo7Ozs+Pj4/Pz9BQUFCQkJERERFRUVISEhLS0tMTExNTU1OTk5PT09QUFBRUVFTU1NVVVVWVlZXV1dZWVlaWlpbW1tcXFxdXV1eXl5fX19gYGBiYmJjY2NkZGRlZWVmZmZnZ2doaGhpaWlqampsbGxtbW1zc3N1dXV6enp8fHx9fX1/f3+BgYGDg4OEhISGhoaHh4eIiIiJiYmKioqLi4uRkZGSkpKTk5OUlJSYmJiZmZmcnJyenp6ioqKlpaWmpqaqqqqtra2urq6xsbGysrKzs7O4uLi5ubm9vb3BwcHCwsLIyMjLy8vMzMzOzs7Pz8/R0dHS0tLU1NTY2Nja2trb29ve3t7g4ODj4+Pl5eXm5ubn5+fo6Ojp6enq6urr6+vs7Ozu7u7w8PDy8vL19fX29vb39/f4+Pj5+fn6+vr9/f3+/v7///9/ur6QAAABy0lEQVQ4jc3V61cSQRgG8AeyDIoUS2xdL7u6qOQFyCVkJRUrNSowFG+oSJamVoB3NCw1i66sFZmX/V8dxNMX+7DeeT7MmTkz75nfeT/MQJSdoiZx9d+iwia/8CiBJDvsg/eI7s1+TUuGBvmFR8mhXAuIfFjYrpkex890cz1i/2JoDPF0cW2spFxtVOO+K3ZlKQ1coxmbKVcO9l1RzJ6ma01eerBM1wfxpiEL6O7HW+7OFCZ6TTKrDx95/Vr6M4zEwX55si2BVSkhxT6ffL/kHPqtGE+6KmnMJ12+Z6CK91waXPDNXFq3COfh2o7cxMgwGmnuMqhbxFVWAlB8J0acGih9rxEzW8/e9VHylgNV1cAN4lJzxMUUEFepEtpy4uoaxKzR+k29KG1+PTNX6F0EU/Y8oJD5j+uanrj0NBRFxg6MDtDwLH7xb/n7pO875GE5pov8Ra1h8UdTZyD1MbWHorZPz10Wm8tZZdYaaGSW5AJ0IZDDZkLFZhNkPqBjlNCwV6FkdEA+2dVxKqgMDK7rOXOG1Z53sbr5Llcn3OZrH7t40z1PC3/f3R53dDy1C+6Xa8Lyq4eCw9snigMvyKVBt/gkLMYdc0nBpJMMuyhT82Twv+sbAAAAAElFTkSuQmCC" alt="image" /></div> <style>
 .hmisctable308517 {
 border: none;
 font-size: 85%;
 }
 .hmisctable308517 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable308517 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable308517">
 <tr><th>n</th><th>missing</th><th>distinct</th><th>Info</th><th>Mean</th><th>Gmd</th><th>.05</th><th>.10</th><th>.25</th><th>.50</th><th>.75</th><th>.90</th><th>.95</th></tr>
 <tr><td>30510</td><td>0</td><td>157</td><td>0.999</td><td>75.38</td><td>19.5</td><td> 50</td><td> 55</td><td> 62</td><td> 73</td><td> 86</td><td> 98</td><td>107</td></tr>
 </table>
 <span style="font-size: 85%;"><font color="MidnightBlue">lowest</font> :   0   1   6   9  20 ,  <font color="MidnightBlue">highest</font>: 191 200 205 210 220</span> <hr class="thinhr"> <span style="font-weight:bold">sysbp</span>: Systolic Blood Pressure <span style='font-family:Verdana;font-size:75%;'>mmHg</span><div style='float: right; text-align: right;'><img src="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAJcAAAANCAMAAACTvAxuAAABxVBMVEUAAAACAgIDAwMEBAQFBQUGBgYICAgJCQkLCwsMDAwNDQ0ODg4PDw8QEBARERESEhIVFRUWFhYXFxcYGBgZGRkaGhobGxsdHR0eHh4fHx8gICAhISEiIiIjIyMkJCQlJSUmJiYnJycoKCgpKSkrKysvLy8wMDAxMTEyMjI0NDQ3Nzc5OTk7Ozs8PDw9PT0+Pj4/Pz9AQEBBQUFCQkJDQ0NERERGRkZHR0dISEhKSkpLS0tNTU1OTk5PT09RUVFSUlJTU1NUVFRVVVVYWFhZWVlbW1tcXFxeXl5hYWFiYmJjY2NmZmZoaGhpaWltbW1ubm5wcHBxcXFzc3N0dHR2dnZ5eXl8fHyAgICBgYGHh4eIiIiJiYmKioqMjIyOjo6RkZGWlpaXl5eZmZmampqbm5ucnJydnZ2fn5+goKChoaGjo6Ompqapqamqqqqrq6usrKyvr6+xsbGysrKzs7O0tLS6urq7u7u+vr6/v7/BwcHFxcXGxsbKysrLy8vQ0NDU1NTW1tbX19fZ2dna2trb29vf39/h4eHo6Ojp6enq6urr6+vs7Ozu7u7w8PD09PT29vb4+Pj5+fn6+vr7+/v9/f3+/v7///8kMR30AAABrklEQVQ4jc3VV1PCQBQF4KugQKIoIvaKBaQYEOzYG9h77AULiopdUVHUiCX2wv5eV9GxPUmwnJ3sZHaSO9+clwWWY7rh7PV1h+usdwHEMTR4EToZfkDIBS6uw94SGNcsHCPkBGcgRL5wcXlv0YvLBp5/5OpRoX/pMsejLy7v9EVgXIxfcczjrVKGt1ZwrzMD4GQYO9gZZguG8GGRzb+xb/Gzr3y9r68JNQ1jvGsreA61S099ecCGSwOac1/+/Wagei2nlRHlXSIahsHOB/kcWGFJefDnLpPSIiakPtcoAIxjlxUWq7CLfXh2HZ3/qqusH7X1PblqSULSIFBAEtRgVxsYoRHrQLQsmALafYnSW/DXp2c/68Ljb1DF6uTeZmxcc1ZGWnRMpJQkhMKQYHifFPwU4NUg7myPLD7c3S8sub9DVyffd+G7qH7h8+3kMK9UbxgNOhWVS2kliiyNnJeoIFKDYgSSMIIMf04YSYgEofyPrtfw+KRQRMqi4mSx8uTsBGmOJlMty1PrVUpKl6vTUsbtIp3eYFivWTM7WuwdplLTCDtY2suyMyVN83VY8AjUEtiNCUnkQwAAAABJRU5ErkJggg==" alt="image" /></div> <style>
 .hmisctable493027 {
 border: none;
 font-size: 74%;
 }
 .hmisctable493027 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable493027 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable493027">
 <tr><th>n</th><th>missing</th><th>distinct</th><th>Info</th><th>Mean</th><th>Gmd</th><th>.05</th><th>.10</th><th>.25</th><th>.50</th><th>.75</th><th>.90</th><th>.95</th></tr>
 <tr><td>30510</td><td>0</td><td>196</td><td>0.999</td><td>129</td><td>26.58</td><td> 92.0</td><td>100.0</td><td>112.0</td><td>129.5</td><td>144.0</td><td>160.0</td><td>170.0</td></tr>
 </table>
 <span style="font-size: 85%;"><font color="MidnightBlue">lowest</font> :   0  36  40  43  46 ,  <font color="MidnightBlue">highest</font>: 266 274 275 276 280</span> <hr class="thinhr"> <span style="font-weight:bold">miloc</span>: MI Location<div style='float: right; text-align: right;'><img src="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAAoAAAANCAMAAACn6Q83AAAAGFBMVEUAAAABAQE8PDyampq5ubna2trr6+v///8e6JE1AAAALUlEQVQImWNgYIMBBgZ2GMDKZEYwGRFMBgYGVihgQBElhsnCAmECbWdiAjkBAM1mAsMRV8USAAAAAElFTkSuQmCC" alt="image" /></div> <style>
 .hmisctable303050 {
 border: none;
 font-size: 85%;
 }
 .hmisctable303050 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable303050 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable303050">
 <tr><th>n</th><th>missing</th><th>distinct</th></tr>
 <tr><td>30510</td><td>0</td><td>3</td></tr>
 </table>
 <pre style="font-size:85%;">
 Value      Inferior    Other Anterior
 Frequency     17582     1062    11866
 Proportion    0.576    0.035    0.389
 </pre>
 <hr class="thinhr"> <span style="font-weight:bold">pmi</span>: Previous MI <style>
 .hmisctable975608 {
 border: none;
 font-size: 85%;
 }
 .hmisctable975608 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable975608 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable975608">
 <tr><th>n</th><th>missing</th><th>distinct</th></tr>
 <tr><td>30510</td><td>0</td><td>2</td></tr>
 </table>
 <pre style="font-size:85%;">
 Value         no   yes
 Frequency  25452  5058
 Proportion 0.834 0.166
 </pre>
 <hr class="thinhr"> <span style="font-weight:bold">tx</span>: Tx in 3 groups <style>
 .hmisctable502173 {
 border: none;
 font-size: 85%;
 }
 .hmisctable502173 td {
 text-align: center;
 padding: 0 1ex 0 1ex;
 }
 .hmisctable502173 th {
 color: MidnightBlue;
 text-align: center;
 padding: 0 1ex 0 1ex;
 font-weight: normal;
 }
 </style>
 <table class="hmisctable502173">
 <tr><th>n</th><th>missing</th><th>distinct</th></tr>
 <tr><td>30510</td><td>0</td><td>2</td></tr>
 </table>
 <pre style="font-size:85%;">
 Value         SK   tPA
 Frequency  20162 10348
 Proportion 0.661 0.339
 </pre>
 <hr class="thinhr"> </div><!--/html_preserve-->

### Overall treatment effect
The primary outcome was 30-day mortality. Among the tPA group, the 30-day mortality was 653/10,348 = 6.3% vs 1475/20,162 = 7.3% in the SK group. This an absolute difference of 1.0%, or an odds ratio of 0.85 [0.78-0.94].

<!--html_preserve--><div class="Rtable1"><table class="Rtable1">
<thead>
<tr>
<th class='rowlabel firstrow lastrow'></th>
<th class='firstrow lastrow'><span class='stratlabel'>SK<br><span class='stratn'>(N=20162)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>tPA<br><span class='stratn'>(N=10348)</span></span></th>
<th class='firstrow lastrow'><span class='stratlabel'>Overall<br><span class='stratn'>(N=30510)</span></span></th>
</tr>
</thead>
<tbody>
<tr>
<td class='rowlabel firstrow'><span class='varlabel'>as.factor(day30)</span></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>0</td>
<td>18687 (92.7%)</td>
<td>9695 (93.7%)</td>
<td>28382 (93.0%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>1</td>
<td class='lastrow'>1475 (7.3%)</td>
<td class='lastrow'>653 (6.3%)</td>
<td class='lastrow'>2128 (7.0%)</td>
</tr>
</tbody>
</table>
</div><!--/html_preserve--><table class="table" style="width: auto !important; ">
 <thead>
  <tr>
   <th style="text-align:right;"> Odds Ratio </th>
   <th style="text-align:right;"> Lower CI </th>
   <th style="text-align:right;"> Upper CI </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 0.853 </td>
   <td style="text-align:right;"> 0.776 </td>
   <td style="text-align:right;"> 0.939 </td>
  </tr>
</tbody>
</table>

<table class="table" style="width: auto !important; ">
 <thead>
  <tr>
   <th style="text-align:right;"> Absolute difference </th>
   <th style="text-align:right;"> Lower CI </th>
   <th style="text-align:right;"> Upper CI </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 0.01 </td>
   <td style="text-align:right;"> 0.004 </td>
   <td style="text-align:right;"> 0.016 </td>
  </tr>
</tbody>
</table>

***

### Adjustment for baseline covariates
The unadjusted odds ratio of 0.853 is a marginal estimate. As explained in the other blog, a lot can be said in favor of *conditional estimates*, where we adjust for prognostically important baseline characteristics.  
In line with [Califf 1997](https://www.sciencedirect.com/science/article/pii/S0002870397701649) and [Steyerberg 2000](https://www.sciencedirect.com/science/article/pii/S0002870300900012), we consider a prediction model with 6 baseline covariates, including age and Killip class (a measure for ventricular function). Pulse rate is modeled using a linear spline with a knot at 50 beats/minute.

<!--html_preserve-->
 <strong>Logistic Regression Model</strong>
 
 <pre>
 lrm(formula = day30 ~ tpa + age + Killip + pmin(sysbp, 120) + 
     lsp(pulse, 50) + pmi + miloc, data = gusto, x = T, maxit = 99)
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
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>Obs 30510</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>LR χ<sup>2</sup> 2991.95</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>R</i><sup>2</sup> 0.235</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>C</i> 0.815</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 0 28382</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>d.f. 11</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i> 1.381</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>D</i><sub>xy</sub> 0.631</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'> 1 2128</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>Pr(>χ<sup>2</sup>) <0.0001</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>r</sub> 3.979</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>γ 0.631</td>
</tr>
<tr>
<td style='min-width: 9em; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'>max |∂log <i>L</i>/∂β| 6×10<sup>-10</sup></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'><i>g</i><sub>p</sub> 0.080</td>
<td style='min-width: 9em; border-right: 1px solid black; text-align: center;'>τ<sub>a</sub> 0.082</td>
</tr>
<tr>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-left: 1px solid black; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'></td>
<td style='min-width: 9em; border-bottom: 2px solid grey; border-right: 1px solid black; text-align: center;'>Brier 0.056</td>
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
<td style='min-width: 7em; text-align: left;'>Intercept</td>
<td style='min-width: 7em; text-align: right;'> -3.0203</td>
<td style='min-width: 7em; text-align: right;'> 0.7973</td>
<td style='min-width: 7em; text-align: right;'> -3.79</td>
<td style='min-width: 7em; text-align: right;'>0.0002</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>tpa</td>
<td style='min-width: 7em; text-align: right;'> -0.2080</td>
<td style='min-width: 7em; text-align: right;'> 0.0529</td>
<td style='min-width: 7em; text-align: right;'> -3.93</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>age</td>
<td style='min-width: 7em; text-align: right;'>  0.0769</td>
<td style='min-width: 7em; text-align: right;'> 0.0025</td>
<td style='min-width: 7em; text-align: right;'> 31.28</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>Killip=II</td>
<td style='min-width: 7em; text-align: right;'>  0.6137</td>
<td style='min-width: 7em; text-align: right;'> 0.0589</td>
<td style='min-width: 7em; text-align: right;'> 10.42</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>Killip=III</td>
<td style='min-width: 7em; text-align: right;'>  1.1610</td>
<td style='min-width: 7em; text-align: right;'> 0.1214</td>
<td style='min-width: 7em; text-align: right;'>  9.57</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>Killip=IV</td>
<td style='min-width: 7em; text-align: right;'>  1.9213</td>
<td style='min-width: 7em; text-align: right;'> 0.1618</td>
<td style='min-width: 7em; text-align: right;'> 11.87</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>sysbp</td>
<td style='min-width: 7em; text-align: right;'> -0.0392</td>
<td style='min-width: 7em; text-align: right;'> 0.0019</td>
<td style='min-width: 7em; text-align: right;'>-20.33</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>pulse</td>
<td style='min-width: 7em; text-align: right;'> -0.0242</td>
<td style='min-width: 7em; text-align: right;'> 0.0159</td>
<td style='min-width: 7em; text-align: right;'> -1.52</td>
<td style='min-width: 7em; text-align: right;'>0.1282</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>pulse'</td>
<td style='min-width: 7em; text-align: right;'>  0.0433</td>
<td style='min-width: 7em; text-align: right;'> 0.0162</td>
<td style='min-width: 7em; text-align: right;'>  2.67</td>
<td style='min-width: 7em; text-align: right;'>0.0075</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>pmi=yes</td>
<td style='min-width: 7em; text-align: right;'>  0.4472</td>
<td style='min-width: 7em; text-align: right;'> 0.0562</td>
<td style='min-width: 7em; text-align: right;'>  7.96</td>
<td style='min-width: 7em; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='min-width: 7em; text-align: left;'>miloc=Other</td>
<td style='min-width: 7em; text-align: right;'>  0.2863</td>
<td style='min-width: 7em; text-align: right;'> 0.1347</td>
<td style='min-width: 7em; text-align: right;'>  2.13</td>
<td style='min-width: 7em; text-align: right;'>0.0335</td>
</tr>
<tr>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: left;'>miloc=Anterior</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'>  0.5432</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 0.0511</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'> 10.62</td>
<td style='min-width: 7em; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>
<!--/html_preserve-->

So, we note that the **adjusted** regression coefficient for tPA was -0.208. The adjusted OR = 0.812.  
Let's check for statistical interaction with  
1.  Individual covariates  
2.  The linear predictor  


```r
## check for interaction
# 1. traditional approach
g <- lrm(day30 ~  tx * (age + Killip + pmin(sysbp, 120) + lsp(pulse, 50) + pmi + miloc), 
          data=gusto, maxit=100)
print(anova(g)) # tx interactions: 10 df, p=0.5720; based on LR test
```

<!--html_preserve--><table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">day30</code></td></tr>
<tr><th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>  23.48</td>
<td style='padding-left:3ex; text-align: right;'>11</td>
<td style='padding-left:3ex; text-align: right;'>0.0151</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>   8.53</td>
<td style='padding-left:3ex; text-align: right;'>10</td>
<td style='padding-left:3ex; text-align: right;'>0.5772</td>
</tr>
<tr>
<td style='text-align: left;'>age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'> 981.65</td>
<td style='padding-left:3ex; text-align: right;'> 2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>   1.60</td>
<td style='padding-left:3ex; text-align: right;'> 1</td>
<td style='padding-left:3ex; text-align: right;'>0.2065</td>
</tr>
<tr>
<td style='text-align: left;'>Killip  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'> 281.44</td>
<td style='padding-left:3ex; text-align: right;'> 6</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>   1.37</td>
<td style='padding-left:3ex; text-align: right;'> 3</td>
<td style='padding-left:3ex; text-align: right;'>0.7131</td>
</tr>
<tr>
<td style='text-align: left;'>sysbp  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'> 412.30</td>
<td style='padding-left:3ex; text-align: right;'> 2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>   0.11</td>
<td style='padding-left:3ex; text-align: right;'> 1</td>
<td style='padding-left:3ex; text-align: right;'>0.7369</td>
</tr>
<tr>
<td style='text-align: left;'>pulse  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'> 235.92</td>
<td style='padding-left:3ex; text-align: right;'> 4</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>   4.22</td>
<td style='padding-left:3ex; text-align: right;'> 2</td>
<td style='padding-left:3ex; text-align: right;'>0.1214</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear (Factor+Higher Order Factors)</i></td>
<td style='padding-left:3ex; text-align: right;'>   7.37</td>
<td style='padding-left:3ex; text-align: right;'> 2</td>
<td style='padding-left:3ex; text-align: right;'>0.0251</td>
</tr>
<tr>
<td style='text-align: left;'>pmi  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>  64.23</td>
<td style='padding-left:3ex; text-align: right;'> 2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>   0.05</td>
<td style='padding-left:3ex; text-align: right;'> 1</td>
<td style='padding-left:3ex; text-align: right;'>0.8233</td>
</tr>
<tr>
<td style='text-align: left;'>miloc  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'> 115.23</td>
<td style='padding-left:3ex; text-align: right;'> 4</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>   2.99</td>
<td style='padding-left:3ex; text-align: right;'> 2</td>
<td style='padding-left:3ex; text-align: right;'>0.2239</td>
</tr>
<tr>
<td style='text-align: left;'>tx × age  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>   1.60</td>
<td style='padding-left:3ex; text-align: right;'> 1</td>
<td style='padding-left:3ex; text-align: right;'>0.2065</td>
</tr>
<tr>
<td style='text-align: left;'>tx × Killip  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>   1.37</td>
<td style='padding-left:3ex; text-align: right;'> 3</td>
<td style='padding-left:3ex; text-align: right;'>0.7131</td>
</tr>
<tr>
<td style='text-align: left;'>tx × sysbp  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>   0.11</td>
<td style='padding-left:3ex; text-align: right;'> 1</td>
<td style='padding-left:3ex; text-align: right;'>0.7369</td>
</tr>
<tr>
<td style='text-align: left;'>tx × pulse  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>   4.22</td>
<td style='padding-left:3ex; text-align: right;'> 2</td>
<td style='padding-left:3ex; text-align: right;'>0.1214</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear</i></td>
<td style='padding-left:3ex; text-align: right;'>   0.08</td>
<td style='padding-left:3ex; text-align: right;'> 1</td>
<td style='padding-left:3ex; text-align: right;'>0.7784</td>
</tr>
<tr>
<td style='text-align: left;'> <i>Nonlinear Interaction : f(A,B) vs. AB</i></td>
<td style='padding-left:3ex; text-align: right;'>   0.08</td>
<td style='padding-left:3ex; text-align: right;'> 1</td>
<td style='padding-left:3ex; text-align: right;'>0.7784</td>
</tr>
<tr>
<td style='text-align: left;'>tx × pmi  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>   0.05</td>
<td style='padding-left:3ex; text-align: right;'> 1</td>
<td style='padding-left:3ex; text-align: right;'>0.8233</td>
</tr>
<tr>
<td style='text-align: left;'>tx × miloc  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>   2.99</td>
<td style='padding-left:3ex; text-align: right;'> 2</td>
<td style='padding-left:3ex; text-align: right;'>0.2239</td>
</tr>
<tr>
<td style='text-align: left;'>TOTAL NONLINEAR</td>
<td style='padding-left:3ex; text-align: right;'>   7.37</td>
<td style='padding-left:3ex; text-align: right;'> 2</td>
<td style='padding-left:3ex; text-align: right;'>0.0251</td>
</tr>
<tr>
<td style='text-align: left;'>TOTAL INTERACTION</td>
<td style='padding-left:3ex; text-align: right;'>   8.53</td>
<td style='padding-left:3ex; text-align: right;'>10</td>
<td style='padding-left:3ex; text-align: right;'>0.5772</td>
</tr>
<tr>
<td style='text-align: left;'>TOTAL NONLINEAR + INTERACTION</td>
<td style='padding-left:3ex; text-align: right;'>  15.57</td>
<td style='padding-left:3ex; text-align: right;'>11</td>
<td style='padding-left:3ex; text-align: right;'>0.1579</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>2343.20</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>21</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>
<!--/html_preserve-->

```r
# 2. PATH statement: linear interaction with linear predictor; baseline risk, so tx=ref, SK
lp.no.tx <- f$coefficients[1] + 0 * f$coefficients[2] + f$x[,-1] %*% f$coefficients[-(1:2)]
gusto$lp <- as.vector(lp.no.tx) # add lp to data frame
h <- lrm(day30 ~ tx * lp, data=gusto)
print(anova(h)) # tx interaction: 1 df, p=0.35; based on Wald statistics
```

<!--html_preserve--><table class='gmisc_table' style='border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;' >
<thead>
<tr><td colspan='4' style='text-align: left;'>
Wald Statistics for <code style="font-size:0.8em">day30</code></td></tr>
<tr><th style='border-bottom: 1px solid grey; font-weight: 900; border-top: 2px solid grey; text-align: center;'></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>χ<sup>2</sup></i></th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'>d.f.</th>
<th style='font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: right;'><i>P</i></th>
</tr>
</thead>
<tbody>
<tr>
<td style='text-align: left;'>tx  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>  16.24</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'>0.0003</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>   0.86</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.3526</td>
</tr>
<tr>
<td style='text-align: left;'>lp  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>2338.37</td>
<td style='padding-left:3ex; text-align: right;'>2</td>
<td style='padding-left:3ex; text-align: right;'><0.0001</td>
</tr>
<tr>
<td style='text-align: left;'> <i>All Interactions</i></td>
<td style='padding-left:3ex; text-align: right;'>   0.86</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.3526</td>
</tr>
<tr>
<td style='text-align: left;'>tx × lp  (Factor+Higher Order Factors)</td>
<td style='padding-left:3ex; text-align: right;'>   0.86</td>
<td style='padding-left:3ex; text-align: right;'>1</td>
<td style='padding-left:3ex; text-align: right;'>0.3526</td>
</tr>
<tr>
<td style='border-bottom: 2px solid grey; text-align: left;'>TOTAL</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>2342.20</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'>3</td>
<td style='padding-left:3ex; border-bottom: 2px solid grey; text-align: right;'><0.0001</td>
</tr>
</tbody>
</table>
<!--/html_preserve-->

So:   
1.  The overall test for interaction with the individual covariates is far from statistically significant (p>0.5).  
2.  Similarly, the test for interaction with the linear predictor is far from statistically significant (p>0.3).  

***

#### Conclusion: no interaction needed
We conclude that we may proceed by ignoring any interactions. We have no evidence against the assumption that the overall effect of treatment is applicable to all patients.  
The patients vary widely in risk, as can easily be seen in the histogram below.    
  
<img src="/post/path_files/figure-html/histogram.lp-1.svg" width="672" />

We note that many patients have baseline risks (tPA==0) below 5%. Obviously their maximum benefit is bounded by this risk estimate, even if tPA, hypothetically, would reduce the risk to Null.

***  

## PATH principle: perform risk-based subgrouping
**_[Fig 3](https://www.ncbi.nlm.nih.gov/pubmed/31711134)_** of the PATH Statement starts with:  

>    **Reporting RCT results stratified by a risk model is encouraged when overall trial results are positive 
> to better understand the distribution of effects across the trial population.**
    

How do we provide such reporting of RCT results?  

We can provide estimates of relative effects and absolute benefit  
1.  By group (e.g. quarters defined by quartiles)  
2.  By baseline risk (continuous, as in the histogram above)  


### 1a. Relative effects of treatment by risk-group 
The checks for interaction were far from statistically significant in GUSTO-I. We can further illustrate the relative effects in a forest plot. 
Many reports from RCTs include forest plots that show relative effects by single variables, such as men vs women; young vs old age; disease subtype; etc.  
The *PATH* statement encourages reporting by risk-based subgroup. How can such reporting be done?  

Let's do some data processing to make a better forest plot.

#### A PATH compatible forest plot
Let's expand the standard forest plot for subgroup effects with risk-based subgroups.  
Below subgroup effects are defined for 4 risk-based groups using `cut2(lp.no.tx, g=4)`,  
and for 3 classical subgroups (by sex, age, type of infarction).  


```r
## quantiles, suggest to use quarters
groups <- cut2(lp.no.tx, g=4)
group0 <- groups[gusto$tpa==0]  # SK gropup
group1 <- groups[gusto$tpa==1]  # tPA group

rate0 <- prop.table(table(group0, gusto$day30[gusto$tpa==0]),1 )[,2]
rate1 <- prop.table(table(group1, gusto$day30[gusto$tpa==1]),1 )[,2]
ratediff <- rate0-rate1 # benefit of tPA by group

# Make a data frame for the results
data.subgroups <- as.data.frame(matrix(nrow=(4+6+1), ncol=10))
colnames(data.subgroups) <- c("tevent", "tnoevent", "cevent", "cnoevent", 
                              "name", "type", "tn", "pt", "cn", "pc")

data.subgroups[11,1:4] <- table(gusto$tpa,gusto$day30)[4:1] # overall results
# define event and non-event numbers
events1   <- table(group0, gusto$day30[gusto$tpa==0])[,2]
nevents1  <- table(group0, gusto$day30[gusto$tpa==0])[,1]
events2   <- table(group1, gusto$day30[gusto$tpa==1])[,2]
nevents2  <- table(group1, gusto$day30[gusto$tpa==1])[,1]
n1      <- events1 + nevents1
n2      <- events2 + nevents2

data.subgroups[10:7,1:4] <- cbind(events2,nevents2,events1,nevents1)
```

Data for classic subgroups are managed below:    


```r
# Use `table`  to get the summary of cell numbers, by subgroup
# SEX
data.subgroups[5,1:4] <- table(1-gusto$day30,1-gusto$tpa, gusto$sex)[1:4]
data.subgroups[6,1:4] <- table(1-gusto$day30,1-gusto$tpa, gusto$sex)[5:8]
# AGE
data.subgroups[3,1:4] <- table(1-gusto$day30,1-gusto$tpa, gusto$age>=75)[1:4]
data.subgroups[4,1:4] <- table(1-gusto$day30,1-gusto$tpa, gusto$age>=75)[5:8]
# ANT
data.subgroups[1,1:4] <- table(1-gusto$day30,1-gusto$tpa, gusto$miloc=="Anterior")[1:4]
data.subgroups[2,1:4] <- table(1-gusto$day30,1-gusto$tpa, gusto$miloc=="Anterior")[5:8]

# Names
data.subgroups[11,5]   <- "Overall"
data.subgroups[10:7,5] <- paste("Quarter",1:4, sep=" ")
data.subgroups[5:6,5]  <- c("Male sex","Female sex")
data.subgroups[3:4,5]  <- c("Age <75","Age>=75")
data.subgroups[1:2,5]  <- c("Other MI","Anterior")

# Type of subgroup
data.subgroups[11,6]   <- ""
data.subgroups[10:7,6] <- c(rep("Risk-based subgroups", length(ratediff)))
data.subgroups[1:6,6] <- c(rep("Location",2), rep("Age",2), rep("Sex",2))

data.subgroups[,7] <- data.subgroups[,1] + data.subgroups[,2]
data.subgroups[,8] <- paste(round(100*data.subgroups[,1] / data.subgroups[,7] , 1),"%", sep="")
data.subgroups[,9] <- data.subgroups[,3] + data.subgroups[,4]
data.subgroups[,10] <- paste(round(100*data.subgroups[,3] / data.subgroups[,9] , 1),"%", sep="")

# Show the data
kable(as.data.frame((data.subgroups))) %>% kable_styling(full_width=F, position = "left")
```

<table class="table" style="width: auto !important; ">
 <thead>
  <tr>
   <th style="text-align:right;"> tevent </th>
   <th style="text-align:right;"> tnoevent </th>
   <th style="text-align:right;"> cevent </th>
   <th style="text-align:right;"> cnoevent </th>
   <th style="text-align:left;"> name </th>
   <th style="text-align:left;"> type </th>
   <th style="text-align:right;"> tn </th>
   <th style="text-align:left;"> pt </th>
   <th style="text-align:right;"> cn </th>
   <th style="text-align:left;"> pc </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 301 </td>
   <td style="text-align:right;"> 6025 </td>
   <td style="text-align:right;"> 649 </td>
   <td style="text-align:right;"> 11669 </td>
   <td style="text-align:left;"> Other MI </td>
   <td style="text-align:left;"> Location </td>
   <td style="text-align:right;"> 6326 </td>
   <td style="text-align:left;"> 4.8% </td>
   <td style="text-align:right;"> 12318 </td>
   <td style="text-align:left;"> 5.3% </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 352 </td>
   <td style="text-align:right;"> 3670 </td>
   <td style="text-align:right;"> 826 </td>
   <td style="text-align:right;"> 7018 </td>
   <td style="text-align:left;"> Anterior </td>
   <td style="text-align:left;"> Location </td>
   <td style="text-align:right;"> 4022 </td>
   <td style="text-align:left;"> 8.8% </td>
   <td style="text-align:right;"> 7844 </td>
   <td style="text-align:left;"> 10.5% </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 397 </td>
   <td style="text-align:right;"> 8622 </td>
   <td style="text-align:right;"> 981 </td>
   <td style="text-align:right;"> 16781 </td>
   <td style="text-align:left;"> Age &lt;75 </td>
   <td style="text-align:left;"> Age </td>
   <td style="text-align:right;"> 9019 </td>
   <td style="text-align:left;"> 4.4% </td>
   <td style="text-align:right;"> 17762 </td>
   <td style="text-align:left;"> 5.5% </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 256 </td>
   <td style="text-align:right;"> 1073 </td>
   <td style="text-align:right;"> 494 </td>
   <td style="text-align:right;"> 1906 </td>
   <td style="text-align:left;"> Age&gt;=75 </td>
   <td style="text-align:left;"> Age </td>
   <td style="text-align:right;"> 1329 </td>
   <td style="text-align:left;"> 19.3% </td>
   <td style="text-align:right;"> 2400 </td>
   <td style="text-align:left;"> 20.6% </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 383 </td>
   <td style="text-align:right;"> 7342 </td>
   <td style="text-align:right;"> 888 </td>
   <td style="text-align:right;"> 14182 </td>
   <td style="text-align:left;"> Male sex </td>
   <td style="text-align:left;"> Sex </td>
   <td style="text-align:right;"> 7725 </td>
   <td style="text-align:left;"> 5% </td>
   <td style="text-align:right;"> 15070 </td>
   <td style="text-align:left;"> 5.9% </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 270 </td>
   <td style="text-align:right;"> 2353 </td>
   <td style="text-align:right;"> 587 </td>
   <td style="text-align:right;"> 4505 </td>
   <td style="text-align:left;"> Female sex </td>
   <td style="text-align:left;"> Sex </td>
   <td style="text-align:right;"> 2623 </td>
   <td style="text-align:left;"> 10.3% </td>
   <td style="text-align:right;"> 5092 </td>
   <td style="text-align:left;"> 11.5% </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 455 </td>
   <td style="text-align:right;"> 2163 </td>
   <td style="text-align:right;"> 1000 </td>
   <td style="text-align:right;"> 4009 </td>
   <td style="text-align:left;"> Quarter 4 </td>
   <td style="text-align:left;"> Risk-based subgroups </td>
   <td style="text-align:right;"> 2618 </td>
   <td style="text-align:left;"> 17.4% </td>
   <td style="text-align:right;"> 5009 </td>
   <td style="text-align:left;"> 20% </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 130 </td>
   <td style="text-align:right;"> 2479 </td>
   <td style="text-align:right;"> 300 </td>
   <td style="text-align:right;"> 4719 </td>
   <td style="text-align:left;"> Quarter 3 </td>
   <td style="text-align:left;"> Risk-based subgroups </td>
   <td style="text-align:right;"> 2609 </td>
   <td style="text-align:left;"> 5% </td>
   <td style="text-align:right;"> 5019 </td>
   <td style="text-align:left;"> 6% </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 49 </td>
   <td style="text-align:right;"> 2481 </td>
   <td style="text-align:right;"> 130 </td>
   <td style="text-align:right;"> 4967 </td>
   <td style="text-align:left;"> Quarter 2 </td>
   <td style="text-align:left;"> Risk-based subgroups </td>
   <td style="text-align:right;"> 2530 </td>
   <td style="text-align:left;"> 1.9% </td>
   <td style="text-align:right;"> 5097 </td>
   <td style="text-align:left;"> 2.6% </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 2572 </td>
   <td style="text-align:right;"> 45 </td>
   <td style="text-align:right;"> 4992 </td>
   <td style="text-align:left;"> Quarter 1 </td>
   <td style="text-align:left;"> Risk-based subgroups </td>
   <td style="text-align:right;"> 2591 </td>
   <td style="text-align:left;"> 0.7% </td>
   <td style="text-align:right;"> 5037 </td>
   <td style="text-align:left;"> 0.9% </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 653 </td>
   <td style="text-align:right;"> 1475 </td>
   <td style="text-align:right;"> 9695 </td>
   <td style="text-align:right;"> 18687 </td>
   <td style="text-align:left;"> Overall </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2128 </td>
   <td style="text-align:left;"> 30.7% </td>
   <td style="text-align:right;"> 28382 </td>
   <td style="text-align:left;"> 34.2% </td>
  </tr>
</tbody>
</table>

In this table **tevent** means #events among treated; **cevent** means #events among non-treated; etc  

***  

Results can be plotted with `metafor` functions:  



```r
par(mar=c(4,4,1,2))
### fit random-effects model (use slab argument to define "study" labels)
res <- rma(ai=tevent, bi=tnoevent, ci=cevent, di=cnoevent, data=data.subgroups, measure="OR",
           slab=name, method="ML")

### set up forest plot (with 2x2 table counts added); rows argument is used
### to specify exactly in which rows the outcomes will be plotted)
forest(res, xlim=c(-8, 2.5), at=log(c(0.5, 1)), alim=c(log(0.2), log(2)), atransf=exp,
       ilab=cbind(data.subgroups$tn, data.subgroups$pt, data.subgroups$cn, data.subgroups$pc),
       ilab.xpos=c(-5,-4,-3,-2), adj=1,
       cex=.9, ylim=c(0, 19),
       rows=c(1:2, (4:5)-.5, 6:7, 10:13, 15),
       xlab="", mlab="", psize=.8, addfit=F)
# lines(x=c(-.15, -.15), y=c(0, 17)) ## could add a reference line of the overall treatment effect

text(c(-5,-4,-3,-2, 2.2), 18, c("n", "%mort", "n", "%mort", "OR    [95% CI]"), 
     font=2, adj=1, cex=.9)
text(-8, 18, c("GUSTO-I trial"), font=2, adj=0, cex=.9)
text(c(-4.5,-2.5),  19, c("tPA", "SK"), font=2, adj=1)

# This can be improved
```

<img src="/post/path_files/figure-html/subgroup.effects-1.svg" width="672" />

This forest plot shows the unadjusted overall effect of tPA vs SK treatment; risk-based subgroup effects; and traditional one at a time subgroup effects. The latter are to be interpreted with much caution; many false-positive findings may arise.  

>    Q: What R function can assist trialists in their reporting of risk-based subgroups  
>    together with classic subgroups?

*** 

We can also estimate the same subgroup effects, adjusted for baseline risk.


```r
# function for adjustment
subgroup.adj <- function(data=gusto, subgroup=gusto$sex) {
  coef.unadj <- by(data, subgroup, function(x)lrm.fit(y=x$day30, x=x$tpa)$coef[2])
  var.unadj  <- by(data, subgroup, function(x)lrm.fit(y=x$day30, x=x$tpa)$var[2,2])
  coef.adj   <- by(data, subgroup, function(x)lrm.fit(y=x$day30, x=x$tpa, offset=x$lp)$coef[2])
  var.adj    <- by(data, subgroup, function(x)lrm.fit(y=x$day30, x=x$tpa, offset=x$lp)$var[2,2])
  result <- cbind(coef.unadj, coef.adj, coef.ratio=coef.adj/coef.unadj, 
                  SEunadj=sqrt(var.unadj), SEadj=sqrt(var.adj), 
                  SEratio=sqrt(var.adj)/ sqrt(var.unadj)) 
  result} # end function

options(digits=3)
```


Overall trial result (un)adjusted 
<table class="table" style="width: auto !important; ">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;"> coef.unadj </th>
   <th style="text-align:right;"> coef.adj </th>
   <th style="text-align:right;"> coef.ratio </th>
   <th style="text-align:right;"> SEunadj </th>
   <th style="text-align:right;"> SEadj </th>
   <th style="text-align:right;"> SEratio </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> TRUE </td>
   <td style="text-align:right;"> -0.159 </td>
   <td style="text-align:right;"> -0.208 </td>
   <td style="text-align:right;"> 1.31 </td>
   <td style="text-align:right;"> 0.049 </td>
   <td style="text-align:right;"> 0.053 </td>
   <td style="text-align:right;"> 1.09 </td>
  </tr>
</tbody>
</table>

Risk-based subgroups  
<table class="table" style="width: auto !important; ">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;"> coef.unadj </th>
   <th style="text-align:right;"> coef.adj </th>
   <th style="text-align:right;"> coef.ratio </th>
   <th style="text-align:right;"> SEunadj </th>
   <th style="text-align:right;"> SEadj </th>
   <th style="text-align:right;"> SEratio </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> [-7.18,-4.01) </td>
   <td style="text-align:right;"> -0.199 </td>
   <td style="text-align:right;"> -0.198 </td>
   <td style="text-align:right;"> 0.993 </td>
   <td style="text-align:right;"> 0.275 </td>
   <td style="text-align:right;"> 0.275 </td>
   <td style="text-align:right;"> 1.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> [-4.01,-3.20) </td>
   <td style="text-align:right;"> -0.282 </td>
   <td style="text-align:right;"> -0.281 </td>
   <td style="text-align:right;"> 0.998 </td>
   <td style="text-align:right;"> 0.169 </td>
   <td style="text-align:right;"> 0.170 </td>
   <td style="text-align:right;"> 1.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> [-3.20,-2.36) </td>
   <td style="text-align:right;"> -0.193 </td>
   <td style="text-align:right;"> -0.187 </td>
   <td style="text-align:right;"> 0.971 </td>
   <td style="text-align:right;"> 0.108 </td>
   <td style="text-align:right;"> 0.108 </td>
   <td style="text-align:right;"> 1.00 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> [-2.36, 4.70] </td>
   <td style="text-align:right;"> -0.170 </td>
   <td style="text-align:right;"> -0.208 </td>
   <td style="text-align:right;"> 1.219 </td>
   <td style="text-align:right;"> 0.063 </td>
   <td style="text-align:right;"> 0.067 </td>
   <td style="text-align:right;"> 1.07 </td>
  </tr>
</tbody>
</table>

Classic subgroup: men vs women  
<table class="table" style="width: auto !important; ">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;"> coef.unadj </th>
   <th style="text-align:right;"> coef.adj </th>
   <th style="text-align:right;"> coef.ratio </th>
   <th style="text-align:right;"> SEunadj </th>
   <th style="text-align:right;"> SEadj </th>
   <th style="text-align:right;"> SEratio </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> male </td>
   <td style="text-align:right;"> -0.183 </td>
   <td style="text-align:right;"> -0.237 </td>
   <td style="text-align:right;"> 1.29 </td>
   <td style="text-align:right;"> 0.063 </td>
   <td style="text-align:right;"> 0.068 </td>
   <td style="text-align:right;"> 1.08 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> female </td>
   <td style="text-align:right;"> -0.127 </td>
   <td style="text-align:right;"> -0.164 </td>
   <td style="text-align:right;"> 1.29 </td>
   <td style="text-align:right;"> 0.078 </td>
   <td style="text-align:right;"> 0.085 </td>
   <td style="text-align:right;"> 1.10 </td>
  </tr>
</tbody>
</table>
Classic subgroup: old (>=75) vs younger age  
<table class="table" style="width: auto !important; ">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;"> coef.unadj </th>
   <th style="text-align:right;"> coef.adj </th>
   <th style="text-align:right;"> coef.ratio </th>
   <th style="text-align:right;"> SEunadj </th>
   <th style="text-align:right;"> SEadj </th>
   <th style="text-align:right;"> SEratio </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> FALSE </td>
   <td style="text-align:right;"> -0.239 </td>
   <td style="text-align:right;"> -0.262 </td>
   <td style="text-align:right;"> 1.10 </td>
   <td style="text-align:right;"> 0.061 </td>
   <td style="text-align:right;"> 0.065 </td>
   <td style="text-align:right;"> 1.06 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> TRUE </td>
   <td style="text-align:right;"> -0.083 </td>
   <td style="text-align:right;"> -0.100 </td>
   <td style="text-align:right;"> 1.21 </td>
   <td style="text-align:right;"> 0.086 </td>
   <td style="text-align:right;"> 0.093 </td>
   <td style="text-align:right;"> 1.08 </td>
  </tr>
</tbody>
</table>

The unadjusted and adjusted results are usually quite in line; only subtle differences is the estimates of the relative effects are noted. We might hypothesize that the unadjusted effect for women is confounded by the higher age of women (where higher age was associated with a somewhat weaker treatment effect); this was not confirmed. 

>    Q: What R function can be developed that extends the unadjusted forest plots 
>    to provide subgroup effects, adjusted for baseline characteristics?

*** 

### 1b. Absolute benefit of treatment by risk-group 


```r
# 95% CI 
CI      <- BinomDiffCI(x1 = events1, n1 = n1, x2 = events2, n2 = n2, method = "scorecc")

colnames(CI) <- c("Absolute difference", "Lower CI", "Upper CI")
rownames(CI) <- names(events1)

result <- round(CI, 3) # absolute difference with confidence interval
kable(as.data.frame(result)) %>% kable_styling(full_width=F, position = "left")
```

<table class="table" style="width: auto !important; ">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;"> Absolute difference </th>
   <th style="text-align:right;"> Lower CI </th>
   <th style="text-align:right;"> Upper CI </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> [-7.18,-4.01) </td>
   <td style="text-align:right;"> 0.002 </td>
   <td style="text-align:right;"> -0.003 </td>
   <td style="text-align:right;"> 0.006 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> [-4.01,-3.20) </td>
   <td style="text-align:right;"> 0.006 </td>
   <td style="text-align:right;"> -0.001 </td>
   <td style="text-align:right;"> 0.013 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> [-3.20,-2.36) </td>
   <td style="text-align:right;"> 0.010 </td>
   <td style="text-align:right;"> -0.001 </td>
   <td style="text-align:right;"> 0.020 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> [-2.36, 4.70] </td>
   <td style="text-align:right;"> 0.026 </td>
   <td style="text-align:right;"> 0.007 </td>
   <td style="text-align:right;"> 0.044 </td>
  </tr>
</tbody>
</table>

So, we see a substantial difference in absolute benefit. Low risk according to the linear predictor (lp<-2.36) implies low benefit (<1%); higher risk, higher benefit (>2%). As **Frank Harrell** would also emphasize, the grouping in quarters might primarily be considered for illustration. A better estimation of benefit avoids grouping, and conditions on the baseline risk.       

*** 

### 2a. Relative effects of treatment by baseline risk 
The checks for interaction with the linear predictor were far from statistically significant in GUSTO-I, as shown above; supporting the assumption that 1 single adjusted, relative effect applies across baseline risk.  

### 2b. Absolute benefit of treatment by baseline risk   
Estimation of absolute benefit can follow a parametric approach, i.e. following the no interaction, main effect model that includes baseline characteristics and a treatment effect; the model `f` considered above for the primary analysis of the trial.  
Further down we will consider relaxations of the proportionality of effect that is assumed in this model.


```r
# create baseline predictions: X-axis
xp <- seq(0.002,.5,by=0.001)
logxp0 <- log(xp/(1-xp))

# expected difference, if covariate adjusted model holds
p1exp <- plogis(logxp0) - plogis(logxp0+coef(f)[2]) # proportional effect assumed

plot(x=xp, y=p1exp, type='l', lty=2, lwd=3, xlim=c(0,.35), ylim=c(-0.007,.05), col="red",
     xlab="Baseline risk", ylab="Benefit by tPA", cex.lab=1.2, las=1, bty='l' )
# add horizontal line
lines(x=c(0,.5), y=c(0,0)) 
# distribution of predicted 
histSpike(plogis(lp.no.tx), add=T, side=1, nint=300, frac=.15) 

points(x=rate0, y=ratediff, pch=1, cex=2, lwd=2, col="blue")
arrows(x0=rate0, x1=rate0, y0=CI[,2], y1=CI[,3], angle=90, code=3,len=.1, col="blue")

legend("topleft", lty=c(2,NA), pch=c(NA,1), lwd=c(3,2), bty='n',col=c("red", "blue"), cex=1.2,
       legend=c("Expected with proportional effect", 
                "Grouped patients"))
```

<img src="/post/path_files/figure-html/plot.benefit-1.svg" width="672" />

This plot shows the benefit by tPA treatment over SK. The red line assumes a proportional effect of treatment, which may be quite reasonable here and in many other diseases. The quarters provide for a non-parametric confirmation of the benefit across baseline risk.  

***

### Relaxation of the proportional effect assumption
If we want to relax the proportional effect assumption, the blog by **Frank Harrell** on **_[examining HTE](https://www.fharrell.com/post/varyor/)_** provides an illustration with **penalized logistic regression**.
   
Another possible relaxation is by including **interaction with the linear predictor**. We consider linear interaction and a non-linear interaction (`rcs`, 2 *df* for non-linearity).  
And we could try a more non-paramteric approach as in [Califf 1997](https://www.sciencedirect.com/science/article/pii/S0002870397701649). There, `loess` smoothers were used for risk estimation the tPA (`day30~lp, subset=tpa==1`) and SK groups (`day30~lp, subset=tpa==0`).  
Benefit was the differences between these 2 risk groups conditional on baseline risk.



```r
h  <- lrm(day30 ~ tpa + tpa * lp, data=gusto, eps=0.005, maxit=30)
h2 <- lrm(day30 ~ tpa + rcs(lp,3)*tpa, data=gusto, eps=0.005, maxit=99)
# loess smoothing
l0 <- loess(day30 ~ lp, data=gusto, subset=tpa==0)
l1 <- loess(day30 ~ lp, data=gusto, subset=tpa==1)

# subtract predicted risks with from without tx
p1 <- plogis(Predict(h,  tpa=0, lp = logxp0)[,3]) - 
      plogis(Predict(h,  tpa=1, lp = logxp0)[,3])
p2 <- plogis(Predict(h2, tpa=0, lp = logxp0)[,3]) - 
      plogis(Predict(h2, tpa=1, lp = logxp0)[,3])
l  <- predict(l0, data.frame(lp = logxp0)) - 
      predict(l1, data.frame(lp = logxp0))

plot(x=xp, y=p1exp, type='l', lty=1, lwd=4, xlim=c(0,.35), ylim=c(-0.007,.05), col="red",
     xlab="Baseline risk", ylab="Benefit by tPA", cex.lab=1.2, las=1, bty='l' )
# benefit with interaction terms
lines(x=xp, y=p1, type='l', lty=2, lwd=3, col="dark blue") 
lines(x=xp, y=p2, type='l', lty=3, lwd=2, col="purple") 
lines(x=xp, y=l,  type='l', lty=1, lwd=3, col="black") 

# horizontal line
lines(x=c(0,.5), y=c(0,0)) 
# distribution of predicted 
histSpike(plogis(lp.no.tx), add=T, side=1, nint=300, frac=.1) 

legend("topleft", lty=c(1,2,3,1), pch=c(NA,NA,NA,NA), lwd=c(4,3,2,3), bty='n',
       col=c("red", "dark blue", "purple","black"), cex=1.2,
       legend=c("Expected with proportional effect", 
                "Linear interaction", "Spline smoothing, 2 df",
                "Loess"))
```

<img src="/post/path_files/figure-html/interaction.with.lp-1.svg" width="672" />

This plot confirms that all estimates of benefit by baseline risk are more or less similar, with benefit clearly increasing by baseline risk. For very low baseline risk, the `loess` estimates are implausible.   

*** 

We can also add the grouped observations by decile, as in [Califf 1997](https://www.sciencedirect.com/science/article/pii/S0002870397701649). The 95% confidence intervals show that the uncertainty per risk group is huge.

>    Q: How many risk-based groups should be used for illustration of benefit by risk?  
>    Default: use quartiles to define 4 quarters; perhaps 3 or only 2 groups in smaller trials?


<img src="/post/path_files/figure-html/interaction.with.lp.deciles-1.svg" width="672" />

<img src="/post/path_files/figure-html/interaction.with.lp.quarters-1.svg" width="672" />


***

## Conclusions
The GUSTO-I trial serves well to illustrate the impact of conditioning on baseline covariates when we consider relative and absolute effects of treatment on binary outcomes. The risk-adjusted estimate of the overall treatment effect has a different interpretation than the unadjusted estimate: the effect for *‘Patients with acute MI’* versus *‘A patient with a certain risk profile’*. 

### Implications for reporting of RCTs
RCTs typically report on: 

1.  an overall effect in the primary analysis;  
this analysis should condition on baseline covariates as argued in another blog.  
2.  effects stratified by single characteristics: one at a time subgroup analyses;  
these analyses should be regarded as secondary and exploratory.

Future RCT reports should include:  

1.  an adjusted estimate of the overall treatment effect as the primary analysis;  
2.  effects stratified by baseline risk;  
typically in 4 risk-based subgroups for illustration,  
and in an analysis with continuous baseline risk, typically plotted as benefit by baseline risk.  
3.  traditional subgroup analyses only as secondary and exploratory information; not to influence decision-making based on the current RCT, but to inform future studies and new RCTs. An exception may be the situation that strong prior hypotheses exist on effect modeification on the relative scale, as discussed in the [ICEMAN report](https://macsphere.mcmaster.ca/handle/11375/24375).

***

### References
***
#### GUSTO-I references
Gusto Investigators - New England Journal of Medicine, 1993   
[An international randomized trial comparing four thrombolytic strategies for acute myocardial infarction](https://www.nejm.org/doi/full/10.1056/NEJM199309023291001)

Califf R, …, ML Simoons, EJ Topol, GUSTO-I Investigators - American heart journal, 1997  
[Selection of thrombolytic therapy for individual patients: development of a clinical model](https://www.sciencedirect.com/science/article/pii/S0002870397701649)

EW Steyerberg, PMM Bossuyt, KL Lee - American heart journal, 2000  
[Clinical trials in acute myocardial infarction: should we adjust for baseline characteristics?](https://www.sciencedirect.com/science/article/pii/S0002870300900012)

***

#### PATH Statement references
**The Predictive Approaches to Treatment effect Heterogeneity
(PATH) Statement**  
David M. Kent, MD, MS; Jessica K. Paulus, ScD; David van Klaveren, PhD; Ralph D’Agostino, PhD;
Steve Goodman, MD, MHS, PhD; Rodney Hayward, MD; John P.A. Ioannidis, MD, DSc; Bray Patrick-Lake, MFS; Sally Morton, PhD;
Michael Pencina, PhD; Gowri Raman, MBBS, MS; Joseph S. Ross, MD, MHS; Harry P. Selker, MD, MSPH; Ravi Varadhan, PhD;
Andrew Vickers, PhD; John B. Wong, MD; and Ewout W. Steyerberg, PhD  
*Ann Intern Med. 2020;172:35-45.*  

[Annals of Internal Medicine, main text](https://annals.org/acp/content_public/journal/aim/938321/aime202001070-m183667.pdf?casa_token=rjMhlqehmZYAAAAA:s2gnJxSo---fGeU5d3BYBJ0s3S8UMxMWhH7RQXNlK8WmHH7WvghrEZJODK1fI_8KsvW1bPavUew)

[Annals of Internal Medicine, Explanation and  Elaboration](https://annals.org/acp/content_public/journal/aim/938321/aime202001070-m183668.pdf?casa_token=HSOwDdYuIfUAAAAA:LprkkDo9-n-qcHqcOC5IHsDWQtjd3CO_SXrSdF0SimqTuIG5I6rKEwhT24ySEZwxTr7GLpn56vM)

[Editorial by Localio et al, 2020](https://annals.org/aim/fullarticle/2755584/advancing-personalized-medicine-through-prediction)

***

#### Risks of traditional subgroup analyses 
SF Assmann, SJ Pocock, LE Enos, LE Kasten - The Lancet, 2000 [Subgroup analysis and other (mis) uses of baseline data in clinical trials](https://www.sciencedirect.com/science/article/pii/S0140673600020390)

PM Rothwell - The Lancet, 2005 [Subgroup analysis in randomised controlled trials: importance, indications, and interpretation](https://www.sciencedirect.com/science/article/pii/S0140673605177095)

RA Hayward, DM Kent, S Vijan… - BMC medical research nmethodology, 2006 [Multivariable risk prediction can greatly enhance the statistical power of clinical trial subgroup analysis](https://bmcmedresmethodol.biomedcentral.com/articles/10.1186/1471-2288-6-18)

AV Hernández, E Boersma, GD Murray… - American heart journal, 2006 [Subgroup analyses in therapeutic cardiovascular clinical trials: are most of them misleading?](https://www.sciencedirect.com/science/article/pii/S0002870305004394)

JD Wallach, …, KL Sainani, EW Steyerberg, JPA Ioannidis - JAMA internal medicine, 2017 [Evaluation of evidence of statistical support and corroboration of subgroup claims in randomized clinical trials](https://jamanetwork.com/journals/jamainternalmedicine/article-abstract/2601419)

JD Wallach, …, JF Trepanowski, EW Steyerberg, JPA Ioannidis - BMJ, 2016 [Sex based subgroup differences in randomized controlled trials: empirical evidence from Cochrane meta-analyses](https://www.bmj.com/content/355/bmj.i5826)

***

