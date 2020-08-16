[![Netlify Status](https://api.netlify.com/api/v1/badges/2afbdb92-43e5-415b-b3dd-61a4d9168b0f/deploy-status)](https://app.netlify.com/sites/openintro-ims/deploys)

# Introduction to Modern Statistics

**THIS IS A WORK IN PROGRESS!!!**

## Where did Intoduction to Statistics with Randomization and Simulation go?

As we're working on the 2nd edition of this book, we realized that we weren't too enamoured by the name, and decided to rename the book to "Introduction to Modern Statistics" to better reflect the content covered in the book, which features simulation-based inference but also many non-inference topics!

If you're looking for the source files for the 1st edition of OpenIntro - Introduction to Statistics with Randomization and Simulation, please download the zipped release [here](https://github.com/OpenIntroStat/randomization-and-simulation/releases).

## What's planned for Introduction to Modern Statistics?

Some restructuring, some reordering, and some new content with better treatment of randomization and simulation throughout the book.

Tentative outline:

### [Chp 1. Introduction to data](https://openintro-ims.netlify.app/intro-to-data.html)

*Complete*

- Case Study (capable of extending to MLR or 2 by 2 table)
- Data basics
- Sampling principles and strategies
- Experiments
- Chapter review

### Chp 2. Exploratory Data Analysis - Visual summaries and summary statistics

- Cat vs. cat - segmented plots / contingency tables
	- Conditional probability from contingency tables
	- Bayes Theorem (law of total probability?)
- Num vs. cat - side-by-side box plots / comparing distributions 
	- Mention univariate - center, skew, shape, spread
	- Mention conditional probabilities as well

### Chp 3. Correlation and Regression (descriptive)

*Awaiting completion*

- Visual summaries of data: scatterplot, side-by-side boxplots, histogram, density plot, box plot (lead out with multivariate, follow with univariate)
- Describing distributions: correlation, central tendency, variability, skew, modality
- Num vs. num - SLR
	- correlation
	- Line fitting, residuals, and correlation
	- Fitting a line by least squares regression
	- Types of outliers in linear regression

### Chp 4. Multiple Regression (descriptive)

*Awaiting completion*

- Num vs. whatever - MLR
	- Introduction to multiple regression
- Parallel slopes
- Hint at interaction, planes, and parallel planes but not quantify
	- Visualization of higher-dimensional models (rgl demo)
- Logistic regression
	- Binary vs. num/whatever
	- Three scales interpretation (e.g. probability, odds, log-odds)
	- “parallel” logistic curves? 

### Chp 5. Foundations of Inference

*Awaiting completion*

- Understanding inference through simulation
- Randomization case study: gender discrimination
- Randomization case study: opportunity cost
- Hypothesis testing
- Confidence intervals
- Simulation case studies

### Chp 6. Inference for categorical data

*Awaiting completion*

- Inference for a single proportion
	- Simulation
	- Exact (if we include course on probability)
	- CLT and Normal approximation
- Difference of two proportions
- Testing for goodness of fit using chi-square (special topic, include simulation version)
- Testing for independence in two-way tables (special topic)

### Chp 7. Inference for numerical data

*Awaiting completion*

- One-sample means
	- Bootstrap (for means, medians)
	- t-distribution
- Paired data
- Difference of two means
- Comparing many means with ANOVA (special topic, include simulation version)

### Chp 8. Inference for regression

*Awaiting completion*

- Inference for linear regression
	- Bootstrap for regression coefficients
	- t-distribution for regression coefficients
	- Model Comparison: Occam’s Razor and R^2 > R^2_adj
- Checking model assumptions using graphs
	- L-I-N-E
- Inference for multiple regression
	- residuals vs. fitted instead of residuals vs. x
- Inference for logistic regression

### Appendix: Probability

*Not included in preliminary edition, may be included in first edition*

---

Please note that this project is released with a [Contributor Code of Conduct](https://www.contributor-covenant.org/version/2/0/code_of_conduct/). By participating in this project you agree to abide by its terms.
