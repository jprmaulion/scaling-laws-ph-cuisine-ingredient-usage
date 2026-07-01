# Scaling Laws in Philippine Cuisine Ingredient Usage
### Zipf's Law and Heaps' Law in a Corpus of Filipino Recipes

![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python&logoColor=white)
![scipy](https://img.shields.io/badge/scipy-1.11-blue)
![pandas](https://img.shields.io/badge/pandas-2.x-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)


## Summary

Cuisines behave like symbolic systems. In the same way that word frequencies in a
language and city sizes in a country follow predictable statistical patterns rather than
random distributions, ingredient usage in a large recipe corpus should, in principle,
obey the same regularities. This repository tests two of the most well established of
these patterns, Zipf's law and Heaps' law, on a corpus of 1,984 Filipino recipes scraped
from Panlasang Pinoy, and finds that both hold strongly.

Zipf's law predicts that if every ingredient is ranked from most to least frequently
used, frequency should fall off with rank as a power law. This holds closely in the
data, with an exponent of negative 0.892 and an R2 of 0.907. Heaps' law predicts that as
a corpus grows, its vocabulary, the count of distinct items it contains, grows
sublinearly, since early documents introduce most of the common vocabulary quickly while
later documents mostly repeat it. This also holds, with an exponent of 0.726 and an R2
of 0.999, an unusually clean fit for empirical data of any kind. A formal
Clauset-Shalizi-Newman test, the rigorous statistical standard for confirming power law
behavior, further confirms that the most frequently used ingredients follow a true power
law rather than a superficially similar lognormal distribution.

**Key Result:** Zipf's law, exponent **-0.892**, R2 = **0.907**. Heaps' law, exponent
**0.726**, R2 = **0.999**. A formal maximum likelihood test confirms true power law
behavior for the 149 most frequently used ingredients, alpha = **1.993**, with a Vuong
likelihood ratio test favoring the power law over a lognormal alternative at p < 0.0001.
Cooking oil, water, salt, and garlic anchor the head of the distribution, the same ingredients identified as network hubs in a separate ingredient co-occurrence network analysis, providing independent cross-validation across two separate methods.


## Graphical Abstract

<p align="center">
  <img src="outputs/graphical_abstract .png" alt="Graphical Abstract" width="900">
</p>
<p align="left"><em><strong>Figure.</strong> Three panel graphical abstract summarizing the scaling law analysis of ingredient usage in Philippine cuisine. <strong>(A)</strong> Zipf rank-frequency scaling across all 4,202 unique ingredients, exponent -0.892, R2 = 0.907. <strong>(B)</strong> Heaps' law vocabulary growth as recipes accumulate, exponent 0.726, R2 = 0.999. <strong>(C)</strong> Formal Clauset-Shalizi-Newman power law test on the frequency tail, alpha = 1.993, xmin = 19, confirming true power law behavior for the most frequently used ingredients. Data: 1,984 recipes from Panlasang Pinoy, scraped June 2026.</em></p>


## Dataset

| Property | Value |
| :--- | :--- |
| **Source** | Panlasang Pinoy (panlasangpinoy.com) |
| **Total recipes** | 1,984 |
| **Total ingredient mentions** | 20,844 |
| **Unique ingredients (normalized)** | 4,202 |
| **Scrape date** | June 2026 |


## Methods

### Zipf's Law

Every unique ingredient is ranked from most to least frequently used across the full
corpus. Frequency is then fitted against rank on logarithmic axes using ordinary least
squares regression, the standard first-pass approach for characterizing power law
relationships in empirical data.

### Heaps' Law

Recipe order is randomly shuffled with a fixed seed to remove any bias from the order
recipes happened to appear on the source website. Recipes are then processed one at a
time, tracking the running count of distinct ingredients encountered so far. This
cumulative vocabulary size is fitted against cumulative recipe count on logarithmic
axes, again using ordinary least squares regression.

### Formal Power Law Confirmation

An ordinary least squares fit on logarithmic axes is a reasonable first pass, but it
cannot, by itself, distinguish a true power law from a lognormal distribution that looks
similar over a limited range. The Clauset-Shalizi-Newman procedure addresses this
directly. It uses maximum likelihood estimation rather than linear regression, searches
for the threshold above which power law behavior actually holds, and statistically
compares the power law hypothesis against a lognormal alternative using a likelihood
ratio test.

| Step | Method |
| :--- | :--- |
| Maximum likelihood estimation | Discrete power law estimator for alpha at each candidate threshold |
| Threshold selection | Minimum Kolmogorov-Smirnov distance across candidate thresholds |
| Alternative model comparison | Lognormal fit on the same tail, Vuong likelihood ratio test |


## Results

### Zipf's Law

| Parameter | Value |
| :--- | :--- |
| Exponent | **-0.892** |
| R2 | 0.907 |
| p-value | < 0.0001 |
| Most frequent ingredient | cooking oil (1,041 uses) |

### Heaps' Law

| Parameter | Value |
| :--- | :--- |
| Exponent | **0.726** |
| R2 | 0.999 |
| p-value | < 0.0001 |
| Final vocabulary at N = 1,984 | 4,202 unique ingredients |

### Formal Power Law Confirmation

| Parameter | Value |
| :--- | :--- |
| Threshold (xmin) | 19 |
| Tail size | 149 ingredients |
| Maximum likelihood alpha | 1.993 |
| Vuong z-score versus lognormal | 5.092 |
| p-value | < 0.0001 |
| Conclusion | Power law favored over lognormal |


## Discussion

The strength of both results is notable on its own terms. An R2 above 0.9 for Zipf's law
and above 0.99 for Heaps' law places Filipino ingredient usage well within the range
reported for natural language corpora and for ingredient usage in other national
cuisines studied in the computational gastronomy literature. The formal
Clauset-Shalizi-Newman test adds a further layer of confidence beyond the regression fit
alone, since it rules out the most common alternative explanation, a lognormal
distribution that merely resembles a power law over a limited range.

The convergence with the ingredient co-occurrence network built separately in this
series is also worth noting. Cooking oil, water, salt, and garlic top the frequency
ranking here, and the same ingredients emerge independently as the highest degree hub
nodes in the network analysis. Two different methods, one based on raw usage counts and
one based on co-occurrence structure, arrive at the same short list of structurally
central ingredients. That convergence is a meaningful form of internal validation for
both analyses.


## Known Limitations

- This analysis is based on a single recipe website. Cross site validation with a
  second major Filipino recipe source is planned.
- The Clauset-Shalizi-Newman test applies specifically to the frequency tail above the
  identified threshold. The full-range Zipf fit and the tail-restricted formal test
  describe complementary aspects of the same distribution rather than identical claims.
- A third literature grounded candidate, the Menzerath-Altmann relation linking unit
  count to average unit length, has not yet been tested. Given the strength of the Zipf
  and Heaps results, this is treated as an optional completeness check.


## References

[1] G. Bagler, G. K. Tewari, A. R. Yadav, A. Singh, P. Bansal, U. Dargar, M. Goel, and M. K. Sinha, "Universal statistical laws governing culinary design," arXiv:2604.28021, 2026.

[2] O. Kinouchi, R. W. Diez-Garcia, A. J. Holanda, P. Zambianchi, and A. C. Roque, "The non-equilibrium nature of culinary evolution," arXiv:0802.4393.

[3] A. Clauset, C. R. Shalizi, and M. E. J. Newman, "Power-law distributions in empirical data," SIAM Review, vol. 51, no. 4, pp. 661 to 703, 2009. doi: 10.1137/070710111

[4] M. E. J. Newman, "Power laws, Pareto distributions and Zipf's law," Contemporary Physics, vol. 46, no. 5, pp. 323 to 351, 2005. doi: 10.1080/00107510500052444

[5] H. S. Heaps, Information Retrieval: Computational and Theoretical Aspects. Academic Press, 1978.

[6] Y.-Y. Ahn, S. E. Ahnert, J. P. Bagrow, and A.-L. Barabasi, "Flavor network and the principles of food pairing," Scientific Reports, vol. 1, p. 196, 2011. doi: 10.1038/srep00196



## Requirements

```
pandas>=2.0
numpy>=1.24
matplotlib>=3.7
scipy>=1.11
```


## License

This repository is under MIT License.


## Questions?

For questions, kindly send then in at jprmaulion[at]gmail[dot]com.
