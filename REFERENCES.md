# Methodological References

The pipeline reports **reliability evidence** and **preliminary validity evidence based on internal structure**. The references below support that terminology and the implemented decision process.

## Validity, measurement properties, and reporting

- American Educational Research Association, American Psychological Association, & National Council on Measurement in Education. (2014). *Standards for Educational and Psychological Testing*. AERA.
- Mokkink, L. B., Terwee, C. B., Patrick, D. L., Alonso, J., Stratford, P. W., Knol, D. L., Bouter, L. M., & de Vet, H. C. W. (2010). The COSMIN checklist for assessing the methodological quality of studies on measurement properties of health status measurement instruments: An international Delphi study. *Quality of Life Research, 19*, 539–549. https://doi.org/10.1007/s11136-010-9606-8
- Mokkink, L. B., Terwee, C. B., Knol, D. L., Stratford, P. W., Alonso, J., Patrick, D. L., Bouter, L. M., & de Vet, H. C. W. (2010). The COSMIN checklist for evaluating the methodological quality of studies on measurement properties: A clarification of its content. *BMC Medical Research Methodology, 10*, 22. https://doi.org/10.1186/1471-2288-10-22
- Worthington, R. L., & Whittaker, T. A. (2006). Scale development research: A content analysis and recommendations for best practices. *The Counseling Psychologist, 34*(6), 806–838. https://doi.org/10.1177/0011000006288127

## Exploratory factor analysis and factor retention

- Fabrigar, L. R., Wegener, D. T., MacCallum, R. C., & Strahan, E. J. (1999). Evaluating the use of exploratory factor analysis in psychological research. *Psychological Methods, 4*(3), 272–299. https://doi.org/10.1037/1082-989X.4.3.272
- Hayton, J. C., Allen, D. G., & Scarpello, V. (2004). Factor retention decisions in exploratory factor analysis: A tutorial on parallel analysis. *Organizational Research Methods, 7*(2), 191–205. https://doi.org/10.1177/1094428104263675
- MacCallum, R. C., Widaman, K. F., Zhang, S., & Hong, S. (1999). Sample size in factor analysis. *Psychological Methods, 4*(1), 84–99. https://doi.org/10.1037/1082-989X.4.1.84
- Garrido, L. E., Abad, F. J., & Ponsoda, V. (2013). A new look at Horn's parallel analysis with ordinal variables. *Psychological Methods, 18*(4), 454–474. https://doi.org/10.1037/a0030005

## Continuous vs. ordinal treatment of Likert items

The pipeline decides whether Pearson or polychoric correlations are the primary analysis based on response-category count, skewness, and polychoric estimation stability. The decision thresholds are aligned with the following literature.

- Rhemtulla, M., Brosseau-Liard, P. É., & Savalei, V. (2012). When can categorical variables be treated as continuous? A comparison of robust continuous and categorical SEM estimation methods under suboptimal conditions. *Psychological Methods, 17*(3), 354–373. https://doi.org/10.1037/a0029315
- Dolan, C. V. (1994). Factor analysis of variables with 2, 3, 5 and 7 response categories: A comparison of categorical variable estimators using simulated data. *British Journal of Mathematical and Statistical Psychology, 47*(2), 309–326. https://doi.org/10.1111/j.2044-8317.1994.tb01039.x
- Curran, P. J., West, S. G., & Finch, J. F. (1996). The robustness of test statistics to nonnormality and specification error in confirmatory factor analysis. *Psychological Methods, 1*(1), 16–29. https://doi.org/10.1037/1082-989X.1.1.16
- Finney, S. J., & DiStefano, C. (2006). Non-normal and categorical data in structural equation modeling. In G. R. Hancock & R. O. Mueller (Eds.), *Structural equation modeling: A second course* (pp. 269–314). Information Age Publishing.
- Finney, S. J., & DiStefano, C. (2013). Nonnormal and categorical data in structural equation modeling. In G. R. Hancock & R. O. Mueller (Eds.), *Structural equation modeling: A second course* (2nd ed., pp. 439–492). Information Age Publishing.

Decision rules implemented: (1) items with 5 or more response categories that are not severely skewed may be treated as continuous (Pearson/ML), with negligible estimation bias (Rhemtulla et al., 2012; Dolan, 1994); (2) items with 4 or fewer categories, or with |skewness| ≥ 2 (and |kurtosis| ≥ 7), are analyzed with polychoric correlations, since normal-theory continuous estimation becomes unreliable under these conditions (Curran et al., 1996; Finney & DiStefano, 2006, 2013); (3) when the polychoric solution shows a high proportion of boundary or fallback estimates, or the sample is small with sparse response categories, the primary analysis reverts to Pearson correlations and the polychoric solution is retained only as a sensitivity check. These are pipeline decision rules calibrated to the cited thresholds, not universal statistical laws, and the non-primary correlation matrix is always reported alongside the primary one.

## Reliability

- McNeish, D. (2018). Thanks coefficient alpha, we'll take it from here. *Psychological Methods, 23*(3), 412–433. https://doi.org/10.1037/met0000144
- Sijtsma, K. (2009). On the use, the misuse, and the very limited usefulness of Cronbach's alpha. *Psychometrika, 74*, 107–120. https://doi.org/10.1007/s11336-008-9101-0
- Nunnally, J. C. (1978). *Psychometric theory* (2nd ed.). McGraw-Hill.
- Tavakol, M., & Dennick, R. (2011). Making sense of Cronbach's alpha. *International Journal of Medical Education, 2*, 53–55. https://doi.org/10.5116/ijme.4dfb.8dfd
- Streiner, D. L. (2003). Starting at the beginning: An introduction to coefficient alpha and internal consistency. *Journal of Personality Assessment, 80*(1), 99–103. https://doi.org/10.1207/S15327752JPA8001_18

**Interpreting alpha/omega magnitude.** Nunnally (1978) proposed a staged standard rather than a single cutoff: reliabilities of roughly .70 are treated as adequate for early-stage or exploratory scales, applied research settings are typically held to .80 or higher, and .90–.95 is reserved for contexts where decisions are made about individuals (e.g., clinical or high-stakes use) — not as a blanket target for every scale. Tavakol and Dennick (2011) summarize a practical range of .80–.95 as generally acceptable and note that values below .70 warrant caution. Values above roughly .90, and especially above .95, should prompt a check for item redundancy rather than be read as an unambiguous improvement: Streiner (2003) argues that very highly intercorrelated items usually indicate the scale is repeating the same narrow content rather than covering the construct's breadth, and recommends examining inter-item correlations and considering whether the scale can be shortened. This pipeline reports alpha/omega together with inter-item correlation ranges so that unusually high reliability can be flagged as a redundancy check rather than treated only as a success criterion.

## AVE, CR, and HTMT

- Fornell, C., & Larcker, D. F. (1981). Evaluating structural equation models with unobservable variables and measurement error. *Journal of Marketing Research, 18*(1), 39–50. https://doi.org/10.1177/002224378101800104
- Henseler, J., Ringle, C. M., & Sarstedt, M. (2015). A new criterion for assessing discriminant validity in variance-based structural equation modeling. *Journal of the Academy of Marketing Science, 43*(1), 115–135. https://doi.org/10.1007/s11747-014-0403-8

AVE, CR, and HTMT are included as exploratory diagnostics in this pipeline. Their strongest confirmatory interpretation requires a clearly specified measurement model and adequate sample size.

## Measurement error in correlation and regression

- Nicewander, W. A. (2018). Modifying Spearman's attenuation equation to yield partial corrections for measurement error. *Educational and Psychological Measurement, 78*(6), 1073–1084. https://doi.org/10.1177/0013164417728220
- Rutter, C. E., et al. (2023). Exploring regression dilution bias using repeat measurements of multiple risk factors. *International Journal of Epidemiology*.
- Saccenti, E., Hendriks, M. H. W. B., & Smilde, A. K. (2020). Corruption of the Pearson correlation coefficient by measurement error and its estimation, bias, and correction under different error models. *Scientific Reports, 10*, 438. https://doi.org/10.1038/s41598-019-57247-4

Measurement error may attenuate observed associations and reduce power. Multicollinearity is a separate problem concerning high dependence among predictors, although poor discriminant validity can make multicollinearity more likely when overlapping subscales are entered together.

## Confirmatory models

- Rosseel, Y. (2012). lavaan: An R package for structural equation modeling. *Journal of Statistical Software, 48*(2), 1–36. https://doi.org/10.18637/jss.v048.i02
