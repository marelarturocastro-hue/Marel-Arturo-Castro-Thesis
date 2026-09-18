# Comparing Smoothing Procedures for Conditional Density Estimation in Factor-Augmented Quantile Regressions

Master Thesis (TFM), Master Degree in Statistics for Data Science, Universidad Carlos III de Madrid, 2025-2026.

- Author: Marel Arturo Castro
- Supervisor: Esther Ruiz Ortega, Ph.D.
- Final thesis: [`thesis/memoria.pdf`](thesis/memoria.pdf)

## Summary

This thesis compares two procedures for converting the conditional quantiles of a factor-augmented quantile regression into a full conditional density: the parametric skew-t fit of Adrian, Boyarchenko, and Giannone (2019) and the piecewise-linear procedure of Mitchell, Poon, and Zhu (2024). In a Monte Carlo study with Gaussian, skew-t, and bimodal data-generating processes, the piecewise-linear smoother reduces the mean integrated squared error by roughly 38 percent when the true density is bimodal, while the skew-t is more accurate on MISE under the Gaussian benchmark. In an application to one-quarter-ahead US GDP growth with factors extracted from the FRED-QD database, the two smoothers give similar densities in 2006 Q2 and differ most clearly in 2008 Q4, where the piecewise-linear density is multimodal. The skew-t fit reaches a parameter bound at 47 percent of the dates in-sample and 50 percent out-of-sample. In an out-of-sample evaluation over 143 quarters, neither smoother is significantly more accurate on any variant of the continuous ranked probability score.

## Repository structure

```
analysis/
  Draft.Rmd            R Markdown file with all the code (Monte Carlo study, factor extraction,
                       quantile regressions, both smoothers, out-of-sample evaluation) and a draft of the text
  2026-03-QD.csv       FRED-QD data, March 2026 vintage (input data)
  oos_results.rds      saved out-of-sample forecasts and scores (written by Draft.Rmd)
thesis/
  memoria.pdf          final thesis in the UC3M template
  memoria.tex          LaTeX source of the final thesis
  referencias.bib      bibliography (biblatex, APA style)
  output.xmpdata       PDF/A metadata used by the pdfx package
  imagenes/            figures (300 dpi PNG exports of the R figures) and cover logos
```

`Draft.Rmd` is where the results are computed. The text in `thesis/memoria.tex` is the same as in `Draft.Rmd`, formatted in the UC3M template with biblatex citations and chapter numbering. The numbers in the LaTeX version are the ones produced by `Draft.Rmd`.

## Reproducing the results

The analysis was run with R 4.5.1 and these package versions:

| Package | Version |
|---|---|
| rmarkdown | 2.31 |
| knitr | 1.51 |
| quantreg | 6.1 |
| sn | 2.1.3 |
| dplyr | 1.2.0 |
| tidyr | 1.3.2 |
| ggplot2 | 4.0.2 |
| gridExtra | 2.3.1 |
| readxl | 1.4.5 |

Install the packages:

```r
install.packages(c("rmarkdown", "knitr", "quantreg", "sn", "dplyr", "tidyr", "ggplot2", "gridExtra", "readxl"))
```

Then knit `analysis/Draft.Rmd`, either with the Knit button in RStudio or from the repository folder with:

```r
rmarkdown::render("analysis/Draft.Rmd")
```

The code reads `2026-03-QD.csv` and writes `oos_results.rds` in the `analysis` folder.

Knitting to PDF also needs a LaTeX distribution (for example TinyTeX, `tinytex::install_tinytex()`).

Notes:

- Seeds are fixed (`set.seed(8675309)` for the Monte Carlo study, `set.seed(42)` for the MPZ draws, `set.seed(314159)` for the out-of-sample exercise), so the results are reproducible. Two separate knits gave identical numbers.
- The full knit takes a while. The out-of-sample loop alone (143 expanding-window re-estimations) took about 7 minutes on the author's laptop.
- Console output (progress and diagnostics) is hidden in the PDF. It is visible when the chunks are run interactively.

## Building the thesis PDF

From the `thesis` folder:

```
pdflatex memoria.tex
biber memoria
pdflatex memoria.tex
pdflatex memoria.tex
```

The template uses the `pdfx` package for PDF/A output, which reads the metadata file named after the job. On Overleaf the job name is `output`, so `output.xmpdata` works as it is. For a local build, copy it to `memoria.xmpdata` first.

The R figures were exported to PNG and flattened onto a white background, because PDF/A-1b does not allow transparency or non-embedded fonts in included images.

## Data

The data are the FRED-QD quarterly database maintained by the Federal Reserve Bank of St. Louis, March 2026 vintage (McCracken and Ng, 2020). The file is included unchanged so that the results can be reproduced, since later vintages contain revised data. The first two rows of the file are metadata: the first flags the series intended for factor estimation and the second gives the recommended transformation code for each series.

If you use the data, cite:

McCracken, M. W., and S. Ng (2020). "FRED-QD: A Quarterly Database for Macroeconomic Research." NBER Working Paper 26872.

## Main references

- Adrian, T., N. Boyarchenko, and D. Giannone (2019). "Vulnerable Growth." *American Economic Review* 109(4): 1263–1289.
- Mitchell, J., A. Poon, and D. Zhu (2024). "Constructing Density Forecasts from Quantile Regressions: Multimodality in Macrofinancial Dynamics." *Journal of Applied Econometrics* 39(5): 790–812.

The full reference list is in the thesis.

## License

The thesis document is licensed under Creative Commons Attribution–NonCommercial–NoDerivatives, as stated on its cover.

## Citation

Castro, M. A. (2026). *Comparing Smoothing Procedures for Conditional Density Estimation in Factor-Augmented Quantile Regressions*. Master Thesis, Universidad Carlos III de Madrid.
