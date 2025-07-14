# Statistical Fragility

This repository explores two key concepts used to assess the **fragility** of results from 2×2 contingency tables: the **Unit Fragility Index (UFI)** and the **Robustness Index (RI)**.


## Unit Fragility Index (UFI)

Originally proposed by **Feinstein (1990)**, the Unit Fragility Index (UFI) addresses a critical issue: data sets may produce identical p-values, yet differ significantly in how susceptible they are to small errors or misclassifications.

**What it does:**
  UFI evaluates how many individual data points (unit changes) would need to be reclassified (moved between groups in a 2×2 table) to change a statistically significant result into a non-significant one (i.e., flip the p-value across the threshold) or vise versa.

**Why it's useful:**
  The more changes required, the more **numerically stable** and **replicable** the finding is likely to be.

**UFI Calcualtion:**
UFI is calculated as described by Feinstein & Heston using the fisher.test function from the stats package in R.
UFI is derived by transferring a single sample from one group to another (+1 and -1 adjustments), with simultaneous adjustments to maintain the marginal totals. The original p-value determined by Fisher’s exact test defines which cells to modify. If the initial p-value is ≤ 0.05 (indicating significance), the smallest observed outcome is incrementally increased by one unit at each iteration until the p-value becomes non-significant. If the original p-value is > 0.05, the cell with the largest value is increased incrementally by one unit until significance (p ≤ 0.05) is achieved. 
Thus, UFI is defined as the number of iterations required to change the p-value from significant to non-significant, or vice versa. 
It has been suggested that a UFI value of > 2 indicates numeric stability for a research study




## Robustness Index (RI)

The Robustness Index (RI) was recently introduced by **Heston**, providing a different approach to evaluating result fragility:

* **What it does:**
  RI modifies the sample size of all four cells in a 2×2 contingency table **proportionally** using small scaling factors (multiplication/division),  until the p-value flips.

* **Why it's useful:**
  RI maintains the internal distribution of the data while testing how robust the result is to proportional changes—offering a more **distribution-preserving** measure of fragility.

**RI Calcualtion:**
RI was calculated as described by Heston T.F.13 using the chisq.test function from the R stats package in R. The original and subsequent p-values were calculated using the Chi-square test, as it can handle decimal values.14 Before performing the Chi-square test the Haldane correction, which involves adding 0.5 to all cells of the contingency table, if any of the cell values is zero, was used. 
If the initial p-value is ≤ 0.05, each value of the matrix is divided by a decimal value until the p-value was > 0.05 . Thus, in this case RI is the smallest divisor, which could flip the p-value from ≤ 0.05 to > 0.05. 
If initial p- value is > 0.05, each matrix value is multiplied by a decimal value until the p-value was < 0.05. Thus, in this case the RI is the smallest multiplier, which could flip the p-value from > 0.05 to ≤ 0.05. 
When evaluating the smallest multiplier or divisor, we tested decimal values above 1 that differed by an increment of 0.0000000001. Lower increments did not have greater precision, because they provided identical RI values. 
It has been suggested that a RI > 2 indicates a robust finding.


**Reference**
Feinstein, A. R. (1990). The unit fragility index: an additional appraisal of "statistical significance" for a contrast of two proportions. J Clin Epidemiol, 43, 201–209
Walsh, M., Srinathan, S. K., McAuley, D. F., Mrkobrada, M., Levine, O., Ribic, C., Molnar, A. O., Dattani, N. D., Burke, A., Guyatt, G., Thabane, L., Walter, S. D., Pogue, J. and Devereaux, P. J. (2014). The statistical significance of randomized controlled trial results is frequently fragile: a case for a Fragility Index. J Clin Epidemiol, 67, 622–628.
Heston, T. F. (2023). Statistical Significance Versus Clinical Relevance: A Head-to-Head Comparison of the Fragility Index and Relative Risk Index. Cureus, 15, e47741,
Heston, T. F. (2023). The Robustness Index: Going Beyond Statistical Significance by Quantifying Fragility. Cureus, 15, e44397,
Heston, T. F. (2024). Redefining Significance: Robustness and Percent Fragility Indices in Biomedical Research. Stats, 7, 537–548,
