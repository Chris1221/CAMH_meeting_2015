coR-ge | Investigation of sFDR in Environments of Complex Correlation
========================================================
author: Christopher Cole
date: July 29th 2015
transition: rotate
 Supervisor: Dr. Jo Knight

Genome Wide Association Studies
========================================================

- Agnostic search of the genome for significant associations
- Hypothesis free
- Many finding, but huge missing heritability
- Moving towards hypothesis driven GWAS (GWAS-HD)
  - Single SNP association with re-prioritization based on biological hypothesis
  - Stratified FDR

Missing Heritability
========

Maher B (Nature, 2008)

1. **Right under everyone's nose** - better tagging (Imputation and sequencing)
2. **Out of sight** - Rare variants
3. **In the architecture** - CNVs
4. **Underground networks** - Systems Biology
5. **The great beyond** - Epigenetics
6. **Lost in diagnosis** - Phenotype Definition

Statistical Challeneges and Opportunities
========
- SNP genotype calling algorithms
- Imputation
- Single vs. multi SNP / trait analysis
- Sample heterogeneity and population stratification
- Meta versus mega analysis
- Family data

>- **Assesing statistical significance of findings**

- *et cetera*

Multiple Testing Correction Primer
=====

> **Theorum 1**: If $H_0$ is true, $P(\phi \leq |\phi_{\alpha}|) = 1-\alpha$
 - i.e. if "significant" $P$-value is $0.05$, given $H_0$, chance of correcting failing to reject the null is $0.95$

> **Theorum 2**: Given $A$ and $B$ are independent, $P(A \cup B) = P(A) \times P(B)$
 - i.e. probability of two events occuring together is the product of their individual probabilities (assuming independence)

Multiple Testing Correction Primer
=======

 Probability of *at least 1* false positive = $1-(1-\alpha)^M$ given $M$ independant tests at $\alpha$ rejection level

- For 3 tests, probability of at least one false positive
 - $1-(1-0.05)^3 = 0.1426$
- For 10 tests $= 0.40$
- As $M \rightarrow \infty$, $P(FP>1) \rightarrow 1$

Correction Methods
=======

- As $M$ increases, expect more false positives under the null
- **Family Wise Error Rate**:
 - Control for the probability of *even one* false positive $\leq \alpha$
 - $\alpha_{control} = 1-(1-\alpha)^{\frac{1}{M}} \sim \frac{\alpha}{M}$
- **False Disovery Rate**:
 - Allow some false positives but control the proportion | $E[\frac{V}{R}] \leq \alpha$ given $V$ false positves and $R$ total positive signals.
 - **A comprimise**: no-adjustment (too liberal) $\leftrightarrow$ FWER (too stringent)

Stratefied FDR
=====
- Lei Sun, 2006
- "Priority Group"
- Lots of questions
 - Effect of genome-like complex correlation
![Gone fishing](pres-figure/fish.png)

Introducing coR-ge
=====

![coR-ge](pres-figure/corge_quickstart.png)

- Software for the Examination of Multiple Correction Methodologies in Accurate Genomic Environments
- Permutation testing of correction methodologies in different settings

Introducing coR-ge
=====

<img src="pres-figure/flow.png" alt="Logic Flow"  align="right" style="width: 500px;"/>

- ~5k lines R, Py, Sh
- Released under GPL
- Requires
 - Cluster
 - Reference Data
 - Causal SNPs
 - etc.

Disease Model
=====

- Heritability
 - 0.45
- Variability
 - 0.55
- Disease SNPs or loci
- Region of the genome
 - Chromosome 1
- Reference Panel
 - CEU 1000G

Resampling
=====

<img src="pres-figure/ld2.jpg" alt="Conservation of LD patterns" align="middle" style="width: 650px;"/>

- **HapGen2**
 - Take haplotypes from reference panel
 - Estimate fine-scale recombination rate

Phenotype Generation
===========
- Effect size for each $i$ of $N$ SNPs $\beta = \delta\sqrt{\frac{\theta^2_i}{2p(1-p)}}$
 - $\beta$ effect size in s.d., $\theta^2_i$ variance, $p = MAF$, $\delta = \pm 1$
- Risk score $WAS = \sum_{i=1}^{N} \beta_i SNP_i -2\beta_i p_i$
 - $i$ of $N$ SNPs, $\beta$ E.S., $p=MAF$
- Overall $z$-score $= WAS + N(0,1-h^2)$
 - $WAS$ weighted allele score, Gaussian noise $\mu = 0$, $\theta^2 = 1-heritability$

Analysis
========

- Standard GWAS with `plink`, data manipulation in `gtool` and R
- Algorithmic analysis of each permutation
- Reports $TP_m$, $FP_m$, $TN_m$, and $FN_m$ given $m$ methodology
- Permuted to establish trends
 - $n = 100$

Results
======

- https://plot.ly/~ChrisCole/58/number-of-true-positives-identified/
- https://plot.ly/~ChrisCole/70/true-positive-improvement-with-sfdr/
- https://plot.ly/~ChrisCole/85/difference-in-false-positives-identified/
- https://plot.ly/~ChrisCole/40/average-difference-in-true-positives-with-maf/
- https://plot.ly/~ChrisCole/75/false-positive-inflation-vs-pathway-enrichment/

Discussion
==========
- We have demonstated sFDR to displaye less false positives with more true positives than aggregated FDR.
- Even in realistic case, more $TP$, less $FP$.  
- Inflation of $FP$ in strata is a function of pathway enrichment
- Novel methedology requires a hard second look as a contender
- Lends credibility to the use of sFDR in HD-GWAS

Future Directions
===========
- Mathematical concequences of genomic dependency on multiplicity control
 - Dr. Lei Sun
- Specific use cases
 - Use same reference panel and set of SNPs
- Improve documentation of **coR-ge**
- Larger and more complex disease models.


Acknowledgements
============
- Dr. Jo Knight
- Students / mentors from CAMH
- Hartmut Shmieder 
- Institute of Medical Science SURP program
- University of Ottawa Co-op Program
- 1000 Genomes / rOpenSci / R core team 
 - etc.
 
This presentation
=============

- Slides
 - RMarkdown -> Markdown / PanDocs -> Knitr -> ioslides 
 - MathJax / LaTeX 
 - Hosted on github
- Plots
 - D3.js -> htmlwidgets -> ggplot2 / ggvis -> plot.ly -> iframes 
 - API and server provided by plot.ly 
