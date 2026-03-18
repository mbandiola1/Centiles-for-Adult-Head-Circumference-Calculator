Adult Head Circumference Centile Tool


An interactive, web-based clinical reference tool for calculating and plotting head circumference centiles in adults, based on the regression model published by Bushby et al. (1992).


🔗 Live Tool




⚠️ Disclaimer: This tool is intended for reference purposes only and does not provide a definitive clinical diagnosis. All clinical decisions should be made by a qualified healthcare professional.





Background & Motivation


Standard paediatric head circumference charts (e.g. Tanner charts) do not extend beyond age 16. This creates a practical gap in clinical genetics practice, where head circumference measurement in adults is routinely used to identify conditions associated with macrocephaly or microcephaly, such as:




Neurofibromatosis type 1


PTEN hamartoma tumour syndrome


Fragile X syndrome


Sotos syndrome




Bushby et al. (1992) addressed this gap by measuring head circumference and height in 354 adults across two British centres, constructing gender-specific centile charts that account for the relationship between head circumference and stature.


This tool digitises and automates that methodology, replacing manual plotting against the paper chart with an interactive, reproducible calculator.



The Bushby et al. (1992) Paper


Full citation:




Bushby KM, Cole T, Matthews JN, Goodship JA. Centiles for adult head circumference. Arch Dis Child. 1992 Oct;67(10):1286–1287. doi: 10.1136/adc.67.10.1286




Key findings:




Adult head circumference is significantly correlated with height


Gender-specific linear regression models were derived


A constant standard deviation (σ = 1.41 cm) was justified by the authors' analysis


The mean head circumference of a male of average height exceeds the 97th centile on Tanner paediatric charts, confirming paediatric references are inappropriate for adult males





Methodology


This tool implements Model 1 from Bushby et al. (1992), using a constant standard deviation as justified by the authors.


Step 1 — Calculate Expected Mean Head Circumference (μ)


Gender-specific linear regression equations:




Gender
Equation




Male
μ = 42.4 + (0.08673 × height in cm)


Female
μ = 41.02 + (0.08673 × height in cm)




Step 2 — Calculate Z-Score


$$Z = \frac{\text{Measured HC} - \mu}{\sigma}, \quad \sigma = 1.41 \text{ cm}$$


Step 3 — Calculate Centile Rank


The centile rank is derived using the cumulative distribution function (Φ) of the standard normal distribution:


$$P = \Phi(Z) \times 100$$


Implemented in JavaScript using the jStat library (jStat.normal.cdf).


Interpretation Thresholds


Standard clinical ±2.0 SD criteria are applied:




Z-Score
Interpretation




Z ≥ +2.0 SD
Significantly large HC for stature (Macrocephaly)


Z ≤ −2.0 SD
Significantly small HC for stature (Microcephaly)


−2.0 SD < Z < +2.0 SD
Within expected range (Normocephaly)





Validation


Worked Examples


The following examples can be used to manually verify the tool's outputs.


Example 1 — Male, 175 cm, HC 57.5 cm




Step
Calculation
Result




Expected Mean (μ)
42.4 + (0.08673 × 175)
57.578 cm


Z-Score
(57.5 − 57.578) / 1.41
−0.055 SD


Centile
Φ(−0.055) × 100
~47.8th centile


Interpretation
Within ±2.0 SD
Normocephaly ✅




Example 2 — Female, 162 cm, HC 56.0 cm




Step
Calculation
Result




Expected Mean (μ)
41.02 + (0.08673 × 162)
55.070 cm


Z-Score
(56.0 − 55.070) / 1.41
+0.659 SD


Centile
Φ(0.659) × 100
~74.5th centile


Interpretation
Within ±2.0 SD
Normocephaly ✅




Example 3 — Macrocephaly boundary check, Male, 175 cm, HC 61.5 cm




Step
Calculation
Result




Expected Mean (μ)
42.4 + (0.08673 × 175)
57.578 cm


Z-Score
(61.5 − 57.578) / 1.41
+2.78 SD


Centile
Φ(2.78) × 100
~99.7th centile


Interpretation
Z ≥ +2.0 SD
Macrocephaly ✅





Centile Line Verification


For a male of 175 cm (μ = 57.578 cm), the centile lines fall at the following head circumference values:




Centile
Z-Score
Expected HC (cm)
Back-calculated centile
Match




3rd
−1.881
54.926
3.00%
✅


10th
−1.282
55.770
9.99%
✅


25th
−0.674
56.627
25.02%
✅


50th
0.000
57.578
50.00%
✅


75th
+0.674
58.528
74.98%
✅


90th
+1.282
59.385
90.01%
✅


97th
+1.881
60.230
97.00%
✅





Computational Simulation — 600 Subjects


To validate distributional correctness, a Monte Carlo simulation was conducted using Python (numpy, scipy.stats), replicating the tool's calculation pipeline independently.


Simulation design:




600 subjects: 300 male (height mean 175 cm, SD 7 cm) and 300 female (height mean 162 cm, SD 6.5 cm)


Heights drawn from normal distributions and clipped to the valid range (140–200 cm)


Head circumferences generated from the Bushby model: HC ~ N(μ, σ) where σ = 1.41 cm


Each subject's Z-score and centile were calculated independently via two separate code paths and compared for agreement




Results:




Metric
Observed
Expected
Result




Validation agreement
600/600 (100%)
100%
✅


Z-Score mean
+0.039
~0.000
✅


Z-Score SD
0.986
~1.000
✅


Centile mean
51.18%
~50.00%
✅


Normocephaly
569 (94.8%)
~95.0%
✅


Macrocephaly
16 (2.7%)
~2.5%
✅


Microcephaly
15 (2.5%)
~2.5%
✅




Gender breakdown:




Gender
Z-Score mean
Z-Score SD
Macrocephaly
Microcephaly




Male (n=300)
−0.022
0.960
7 (2.3%)
7 (2.3%)


Female (n=300)
+0.099
1.008
9 (3.0%)
8 (2.7%)




Normality tests on simulated Z-scores:




Test
Statistic
p-value
Result




Shapiro-Wilk (n=300)
W = 0.9965
p = 0.755
Normal distribution confirmed ✅


Kolmogorov-Smirnov (n=600)
D = 0.0361
p = 0.404
Normal distribution confirmed ✅




The simulation confirms that the tool's implementation is mathematically faithful to the Bushby (1992) model, producing the expected distributional properties across a representative range of adult heights and head circumferences.



Known Limitations


1. Sample Size


The Bushby et al. (1992) study measured 354 adults across two British centres. This is a relatively modest sample for the derivation of population reference ranges.


2. Potential Sampling Bias


The paper does not specify whether participants were recruited from general population settings or clinical genetics centres. If recruitment included individuals attending genetics clinics, the sample may over-represent people with atypically large or small head circumferences, which could affect the centile distribution. This is a recognised methodological consideration when applying these reference ranges in a clinical genetics context.


3. Population Generalisability


The study population was British. These centile charts may not be fully generalisable to populations of different ethnic backgrounds, for whom separate normative data may be more appropriate.


4. Height Range


The tool is validated for heights between 140–200 cm, consistent with the range studied by Bushby et al. Inputs outside this range are not accepted.


5. Binary Sex Classification


The tool uses the binary male/female classification as defined in the original paper. It does not account for intersex variation or transgender individuals, for whom clinical judgement should guide appropriate reference range selection.



Technologies Used




Library
Purpose




Tailwind CSS
Styling and responsive layout


D3.js v7
Interactive centile chart rendering


jStat
Cumulative normal distribution function


MathJax
LaTeX equation rendering in methodology section





Usage




Select gender


Enter height in feet/inches or centimetres (auto-converts between units)


Enter head circumference in centimetres


Click Calculate & Plot




The tool will display:




Expected mean head circumference for the given height and gender


Calculated Z-score


Centile rank


Clinical interpretation (Normocephaly / Macrocephaly / Microcephaly)


An interactive centile chart with the subject's measurement plotted





Suggested Citation


If referencing the underlying methodology, please cite the original paper:




Bushby KM, Cole T, Matthews JN, Goodship JA. Centiles for adult head circumference. Arch Dis Child. 1992;67(10):1286–1287.





Author


M. Bandiola
Tool developed to digitise and operationalise the Bushby et al. (1992) reference model for use in clinical genetics practice.



Contributing


Contributions, corrections, and suggestions are welcome. Please open an issue or submit a pull request.



License


This tool is provided for reference and educational purposes. It is not a registered medical device and should not be used as a substitute for clinical judgement.
