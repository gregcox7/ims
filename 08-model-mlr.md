# Linear regression with multiple predictors {#model-mlr}



::: {.chapterintro data-latex=""}
Building on the ideas of one predictor variable in a linear regression model (from Chapter \@ref(model-slr)), a multiple linear regression model is now fit to two or more predictor variables.
By considering how different explanatory variables interact, we can uncover complicated relationships between the predictor variables and the response variable.
One challenge to working with multiple variables is that it is sometimes difficult to know which variables are most important to include in the model.
Model building is an extensive topic, and we scratch the surface here by defining and utilizing the adjusted $R^2$ value.
:::

Multiple regression extends single predictor variable regression to the case that still has one response but many predictors (denoted $x_1$, $x_2$, $x_3$, ...).
The method is motivated by scenarios where many variables may be simultaneously connected to an output.

We will consider data about loans from the peer-to-peer lender, Lending Club, which is a dataset we first encountered in Chapter \@ref(data-hello).
The loan data includes terms of the loan as well as information about the borrower.
The outcome variable we would like to better understand is the interest rate assigned to the loan.
For instance, all other characteristics held constant, does it matter how much debt someone already has?
Does it matter if their income has been verified?
Multiple regression will help us answer these and other questions.

The dataset includes results from 10,000 loans, and we'll be looking at a subset of the available variables, some of which will be new from those we saw in earlier chapters.
The first six observations in the dataset are shown in Table \@ref(tab:loans-data-matrix), and descriptions for each variable are shown in Table \@ref(tab:loans-variables).
Notice that the past bankruptcy variable (`bankruptcy`) is an indicator variable, where it takes the value 1 if the borrower had a past bankruptcy in their record and 0 if not.
Using an indicator variable in place of a category name allows for these variables to be directly used in regression.
Two of the other variables are categorical (`verified_income` and `issue_month`), each of which can take one of a few different non-numerical values; we'll discuss how these are handled in the model in Section \@ref(ind-and-cat-predictors).

::: {.data data-latex=""}
The [`loans_full_schema`](http://openintrostat.github.io/openintro/reference/loans_full_schema.html) data can be found in the [**openintro**](http://openintrostat.github.io/openintro) R package.
Based on the data in this dataset we have created two new variables: `credit_util` which is calculated as the total credit utilized divided by the total credit limit and `bankruptcy` which turns the number of bankruptcies to an indicator variable (0 for no bankruptcies and 1 for at least 1 bankruptcy).
We will refer to this modified dataset as `loans`.
:::

<table class="table table-striped table-condensed" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>(\#tab:loans-data-matrix)First six rows of the `loans` dataset.</caption>
 <thead>
  <tr>
   <th style="text-align:right;"> interest_rate </th>
   <th style="text-align:left;"> verified_income </th>
   <th style="text-align:right;"> debt_to_income </th>
   <th style="text-align:right;"> credit_util </th>
   <th style="text-align:right;"> bankruptcy </th>
   <th style="text-align:right;"> term </th>
   <th style="text-align:right;"> credit_checks </th>
   <th style="text-align:left;"> issue_month </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 14.07 </td>
   <td style="text-align:left;"> Verified </td>
   <td style="text-align:right;"> 18.01 </td>
   <td style="text-align:right;"> 0.548 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 60 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:left;"> Mar-2018 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 12.61 </td>
   <td style="text-align:left;"> Not Verified </td>
   <td style="text-align:right;"> 5.04 </td>
   <td style="text-align:right;"> 0.150 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;"> 36 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> Feb-2018 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 17.09 </td>
   <td style="text-align:left;"> Source Verified </td>
   <td style="text-align:right;"> 21.15 </td>
   <td style="text-align:right;"> 0.661 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 36 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> Feb-2018 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 6.72 </td>
   <td style="text-align:left;"> Not Verified </td>
   <td style="text-align:right;"> 10.16 </td>
   <td style="text-align:right;"> 0.197 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 36 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> Jan-2018 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 14.07 </td>
   <td style="text-align:left;"> Verified </td>
   <td style="text-align:right;"> 57.96 </td>
   <td style="text-align:right;"> 0.755 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 36 </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:left;"> Mar-2018 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 6.72 </td>
   <td style="text-align:left;"> Not Verified </td>
   <td style="text-align:right;"> 6.46 </td>
   <td style="text-align:right;"> 0.093 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 36 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:left;"> Jan-2018 </td>
  </tr>
</tbody>
</table>

<table class="table table-striped table-condensed" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:loans-variables)Variables and their descriptions for the `loans` dataset.</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Variable </th>
   <th style="text-align:left;"> Description </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;font-family: monospace;"> interest_rate </td>
   <td style="text-align:left;width: 30em; "> Interest rate on the loan, in an annual percentage. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> verified_income </td>
   <td style="text-align:left;width: 30em; "> Categorical variable describing whether the borrower's income source and amount have been verified, with levels `Verified`, `Source Verified`, and `Not Verified`. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> debt_to_income </td>
   <td style="text-align:left;width: 30em; "> Debt-to-income ratio, which is the percentage of total debt of the borrower divided by their total income. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> credit_util </td>
   <td style="text-align:left;width: 30em; "> Of all the credit available to the borrower, what fraction are they utilizing. For example, the credit utilization on a credit card would be the card's balance divided by the card's credit limit. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> bankruptcy </td>
   <td style="text-align:left;width: 30em; "> An indicator variable for whether the borrower has a past bankruptcy in their record. This variable takes a value of `1` if the answer is *yes* and `0` if the answer is *no*. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> term </td>
   <td style="text-align:left;width: 30em; "> The length of the loan, in months. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> issue_month </td>
   <td style="text-align:left;width: 30em; "> The month and year the loan was issued, which for these loans is always during the first quarter of 2018. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> credit_checks </td>
   <td style="text-align:left;width: 30em; "> Number of credit checks in the last 12 months. For example, when filing an application for a credit card, it is common for the company receiving the application to run a credit check. </td>
  </tr>
</tbody>
</table>

## Indicator and categorical predictors {#ind-and-cat-predictors}

Let's start by fitting a linear regression model for interest rate with a single predictor indicating whether or not a person has a bankruptcy in their record:

$$\widehat{\texttt{interest_rate}} = 12.34 + 0.74 \times \texttt{bankruptcy}$$

Results of this model are shown in Table \@ref(tab:int-rate-bankruptcy).

<table class="table table-striped table-condensed" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:int-rate-bankruptcy)Summary of a linear model for predicting `interest_rate` based on whether the borrower has a bankruptcy in their record. Degrees of freedom for this model is 9998.</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> term </th>
   <th style="text-align:right;"> estimate </th>
   <th style="text-align:right;"> std.error </th>
   <th style="text-align:right;"> statistic </th>
   <th style="text-align:right;"> p.value </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> (Intercept) </td>
   <td style="text-align:right;width: 5em; "> 12.34 </td>
   <td style="text-align:right;width: 5em; "> 0.05 </td>
   <td style="text-align:right;width: 5em; "> 231.49 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> bankruptcy1 </td>
   <td style="text-align:right;width: 5em; "> 0.74 </td>
   <td style="text-align:right;width: 5em; "> 0.15 </td>
   <td style="text-align:right;width: 5em; "> 4.82 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
</tbody>
</table>

::: {.workedexample data-latex=""}
Interpret the coefficient for the past bankruptcy variable in the model.

------------------------------------------------------------------------

The variable takes one of two values: 1 when the borrower has a bankruptcy in their history and 0 otherwise.
A slope of 0.74 means that the model predicts a 0.74% higher interest rate for those borrowers with a bankruptcy in their record.
(See Section \@ref(categorical-predictor-two-levels) for a review of the interpretation for two-level categorical predictor variables.)
:::

Suppose we had fit a model using a 3-level categorical variable, such as `verified_income`.
The output from software is shown in Table \@ref(tab:int-rate-ver-income).
This regression output provides multiple rows for the variable.
Each row represents the relative difference for each level of `verified_income`.
However, we are missing one of the levels: `Not Verified`.
The missing level is called the **reference level** and it represents the default level that other levels are measured against.



<table class="table table-striped table-condensed" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:int-rate-ver-income)Summary of a linear model for predicting `interest_rate` based on whether the borrower’s income source and amount has been verified. This predictor has three levels, which results in 2 rows in the regression output.</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> term </th>
   <th style="text-align:right;"> estimate </th>
   <th style="text-align:right;"> std.error </th>
   <th style="text-align:right;"> statistic </th>
   <th style="text-align:right;"> p.value </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> (Intercept) </td>
   <td style="text-align:right;width: 5em; "> 11.10 </td>
   <td style="text-align:right;width: 5em; "> 0.08 </td>
   <td style="text-align:right;width: 5em; "> 137.2 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> verified_incomeSource Verified </td>
   <td style="text-align:right;width: 5em; "> 1.42 </td>
   <td style="text-align:right;width: 5em; "> 0.11 </td>
   <td style="text-align:right;width: 5em; "> 12.8 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> verified_incomeVerified </td>
   <td style="text-align:right;width: 5em; "> 3.25 </td>
   <td style="text-align:right;width: 5em; "> 0.13 </td>
   <td style="text-align:right;width: 5em; "> 25.1 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
</tbody>
</table>

::: {.workedexample data-latex=""}
How would we write an equation for this regression model?

------------------------------------------------------------------------

The equation for the regression model may be written as a model with two predictors:

$$
\begin{aligned}
\widehat{\texttt{interest_rate}} &= 11.10 \\
&+ 1.42 \times \texttt{verified_income}_{\texttt{Source Verified}} \\
&+ 3.25 \times \texttt{verified_income}_{\texttt{Verified}}
\end{aligned}
$$

We use the notation $\texttt{variable}_{\texttt{level}}$ to represent indicator variables for when the categorical variable takes a particular value.
For example, $\texttt{verified_income}_{\texttt{Source Verified}}$ would take a value of 1 if it was for a borrower that was source verified, and it would take a value of 0 otherwise.
Likewise, $\texttt{verified_income}_{\texttt{Verified}}$ would take a value of 1 if it was for a borrower that was verified, and 0 if it took any other value.
:::

The notation $\texttt{variable}_{\texttt{level}}$ may feel a bit confusing.
Let's figure out how to use the equation for each level of the `verified_income` variable.

::: {.workedexample data-latex=""}
Using the model for predicting interest rate from income verification type, compute the average interest rate for borrowers whose income source and amount are both *unverified*.

------------------------------------------------------------------------

When `verified_income` takes a value of `Not Verified`, then both indicator functions in the equation for the linear model are set to 0:

$$\widehat{\texttt{interest_rate}} = 11.10 + 1.42 \times 0 + 3.25 \times 0 = 11.10$$

The average interest rate for these borrowers is 11.1%.
Because the level does not have its own coefficient and it is the reference value, the indicators for the other levels for this variable all drop out.
:::

::: {.workedexample data-latex=""}
Using the model for predicting interest rate from income verification type, compute the average interest rate for borrowers whose income source and amount are both *source verified*.

------------------------------------------------------------------------

When `verified_income` takes a value of `Source Verified`, then the corresponding variable takes a value of 1 while the other is 0:

$$\widehat{\texttt{interest_rate}} = 11.10 + 1.42 \times 1 + 3.25 \times 0 = 12.52$$

The average interest rate for these borrowers is 12.52%.
:::

::: {.guidedpractice data-latex=""}
Compute the average interest rate for borrowers whose income source and amount are both verified.[^model-mlr-1]
:::

[^model-mlr-1]: When `verified_income` takes a value of `Verified`, then the corresponding variable takes a value of 1 while the other is 0: $11.10 + 1.42 \times 0 + 3.25 \times 1 = 14.35.$ The average interest rate for these borrowers is 14.35%.

::: {.important data-latex=""}
**Predictors with several categories.**

When fitting a regression model with a categorical variable that has $k$ levels where $k > 2$, software will provide a coefficient for $k - 1$ of those levels.
For the last level that does not receive a coefficient, this is the reference level, and the coefficients listed for the other levels are all considered relative to this reference level.
:::

::: {.guidedpractice data-latex=""}
Interpret the coefficients from the model above.[^model-mlr-2]
:::

[^model-mlr-2]: Each of the coefficients gives the incremental interest rate for the corresponding level relative to the `Not Verified` level, which is the reference level.
    For example, for a borrower whose income source and amount have been verified, the model predicts that they will have a 3.25% higher interest rate than a borrower who has not had their income source or amount verified.

The higher interest rate for borrowers who have verified their income source or amount is surprising.
Intuitively, we'd think that a loan would look *less* risky if the borrower's income has been verified.
However, note that the situation may be more complex, and there may be confounding variables that we didn't account for.
For example, perhaps lenders require borrowers with poor credit to verify their income.
That is, verifying income in our dataset might be a signal of some concerns about the borrower rather than a reassurance that the borrower will pay back the loan.
For this reason, the borrower could be deemed higher risk, resulting in a higher interest rate.
(What other confounding variables might explain this counter-intuitive relationship suggested by the model?)

::: {.guidedpractice data-latex=""}
How much larger of an interest rate would we expect for a borrower who has verified their income source and amount vs a borrower whose income source has only been verified?[^model-mlr-3]
:::

[^model-mlr-3]: Relative to the `Not Verified` category, the `Verified` category has an interest rate of 3.25% higher, while the `Source Verified` category is only 1.42% higher.
    Thus, `Verified` borrowers will tend to get an interest rate about $3.25% - 1.42% = 1.83%$ higher than `Source Verified` borrowers.

## Many predictors in a model

The world is complex, and it can be helpful to consider many factors at once in statistical modeling.
For example, we might like to use the full context of borrowers to predict the interest rate they receive rather than using a single variable.
This is the strategy used in **multiple regression**.
While we remain cautious about making any causal interpretations using multiple regression on observational data, such models are a common first step in gaining insights or providing some evidence of a causal connection.



We want to construct a model that accounts not only for any past bankruptcy or whether the borrower had their income source or amount verified, but simultaneously accounts for all the variables in the `loans` dataset: `verified_income`, `debt_to_income`, `credit_util`, `bankruptcy`, `term`, `issue_month`, and `credit_checks`.

$$\begin{aligned}
\widehat{\texttt{interest_rate}} &= b_0 \\
&+ b_1 \times \texttt{verified_income}_{\texttt{Source Verified}} \\
&+ b_2 \times \texttt{verified_income}_{\texttt{Verified}} \\
&+ b_3 \times \texttt{debt_to_income} \\
&+ b_4 \times \texttt{credit_util} \\
&+ b_5 \times \texttt{bankruptcy} \\
&+ b_6 \times \texttt{term} \\
&+ b_9 \times \texttt{credit_checks} \\
&+ b_7 \times \texttt{issue_month}_{\texttt{Jan-2018}} \\
&+ b_8 \times \texttt{issue_month}_{\texttt{Mar-2018}}
\end{aligned}$$

This equation represents a holistic approach for modeling all of the variables simultaneously.
Notice that there are two coefficients for `verified_income` and also two coefficients for `issue_month`, since both are 3-level categorical variables.

We calculate $b_0$, $b_1$, $b_2$, $\cdots$, $b_9$ the same way as we did in the case of a model with a single predictor -- we select values that minimize the sum of the squared residuals:

$$SSE = e_1^2 + e_2^2 + \dots + e_{10000}^2 = \sum_{i=1}^{10000} e_i^2 = \sum_{i=1}^{10000} \left(y_i - \hat{y}_i\right)^2$$

where $y_i$ and $\hat{y}_i$ represent the observed interest rates and their estimated values according to the model, respectively.
10,000 residuals are calculated, one for each observation.
Note that these values are sample statistics and in the case where the observed data is a random sample from a target population that we are interested in making inferences about, they are estimates of the population parameters $\beta_0$, $\beta_1$, $\beta_2$, $\cdots$, $\beta_9$.
We will discuss inference based on linear models in Chapter \@ref(inf-model-mlr), for now we will focus on calculating sample statistics $b_i$.

We typically use a computer to minimize the sum of squares and compute point estimates, as shown in the sample output in Table \@ref(tab:loans-full).
Using this output, we identify $b_i,$ just as we did in the one-predictor case.

<table class="table table-striped table-condensed" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:loans-full)Output for the regression model, where interest rate is the outcome and the variables listed are the predictors. Degrees of freedom for this model is 9990.</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> term </th>
   <th style="text-align:right;"> estimate </th>
   <th style="text-align:right;"> std.error </th>
   <th style="text-align:right;"> statistic </th>
   <th style="text-align:right;"> p.value </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> (Intercept) </td>
   <td style="text-align:right;width: 5em; "> 1.89 </td>
   <td style="text-align:right;width: 5em; "> 0.21 </td>
   <td style="text-align:right;width: 5em; "> 9.01 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> verified_incomeSource Verified </td>
   <td style="text-align:right;width: 5em; "> 1.00 </td>
   <td style="text-align:right;width: 5em; "> 0.10 </td>
   <td style="text-align:right;width: 5em; "> 10.06 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> verified_incomeVerified </td>
   <td style="text-align:right;width: 5em; "> 2.56 </td>
   <td style="text-align:right;width: 5em; "> 0.12 </td>
   <td style="text-align:right;width: 5em; "> 21.87 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> debt_to_income </td>
   <td style="text-align:right;width: 5em; "> 0.02 </td>
   <td style="text-align:right;width: 5em; "> 0.00 </td>
   <td style="text-align:right;width: 5em; "> 7.43 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> credit_util </td>
   <td style="text-align:right;width: 5em; "> 4.90 </td>
   <td style="text-align:right;width: 5em; "> 0.16 </td>
   <td style="text-align:right;width: 5em; "> 30.25 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> bankruptcy1 </td>
   <td style="text-align:right;width: 5em; "> 0.39 </td>
   <td style="text-align:right;width: 5em; "> 0.13 </td>
   <td style="text-align:right;width: 5em; "> 2.96 </td>
   <td style="text-align:right;width: 5em; "> 0.0031 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> term </td>
   <td style="text-align:right;width: 5em; "> 0.15 </td>
   <td style="text-align:right;width: 5em; "> 0.00 </td>
   <td style="text-align:right;width: 5em; "> 38.89 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> credit_checks </td>
   <td style="text-align:right;width: 5em; "> 0.23 </td>
   <td style="text-align:right;width: 5em; "> 0.02 </td>
   <td style="text-align:right;width: 5em; "> 12.52 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> issue_monthJan-2018 </td>
   <td style="text-align:right;width: 5em; "> 0.05 </td>
   <td style="text-align:right;width: 5em; "> 0.11 </td>
   <td style="text-align:right;width: 5em; "> 0.42 </td>
   <td style="text-align:right;width: 5em; "> 0.6736 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> issue_monthMar-2018 </td>
   <td style="text-align:right;width: 5em; "> -0.04 </td>
   <td style="text-align:right;width: 5em; "> 0.11 </td>
   <td style="text-align:right;width: 5em; "> -0.39 </td>
   <td style="text-align:right;width: 5em; "> 0.696 </td>
  </tr>
</tbody>
</table>

::: {.important data-latex=""}
**Multiple regression model.**

A multiple regression model is a linear model with many predictors.
In general, we write the model as

$$\hat{y} = b_0 + b_1 x_1 + b_2 x_2 + \cdots + b_k x_k$$

when there are $k$ predictors.
We always calculate $b_i$ using statistical software.
:::

::: {.workedexample data-latex=""}
Write out the regression model using the regression output from Table \@ref(tab:loans-full).
How many predictors are there in this model?

------------------------------------------------------------------------

The fitted model for the interest rate is given by:

$$
\begin{aligned}
\widehat{\texttt{interest_rate}} &= 1.89 \\
&+ 1.00 \times \texttt{verified_income}_{\texttt{Source Verified}} \\
&+ 2.56 \times \texttt{verified_income}_{\texttt{Verified}} \\
&+ 0.02 \times \texttt{debt_to_income} \\
&+ 4.90 \times \texttt{credit_util} \\
&+ 0.39 \times \texttt{bankruptcy} \\
&+ 0.15 \times \texttt{term} \\
&+ 0.23 \times \texttt{credit_checks} \\
&+ 0.05 \times \texttt{issue_month}_{\texttt{Jan-2018}} \\
&- 0.04 \times \texttt{issue_month}_{\texttt{Mar-2018}}
\end{aligned}
$$

If we count up the number of predictor coefficients, we get the *effective* number of predictors in the model; there are nine of those.
Notice that the categorical predictor counts as two, once for each of the two levels shown in the model.
In general, a categorical predictor with $p$ different levels will be represented by $p - 1$ terms in a multiple regression model.
A total of seven variables were used as predictors to fit this model: `verified_income`, `debt_to_income`, `credit_util`, `bankruptcy`, `term`, `credit_checks`, `issue_month`.
:::

::: {.guidedpractice data-latex=""}
Interpret the coefficient of the variable `credit_checks`.[^model-mlr-4]
:::

[^model-mlr-4]: All else held constant, for each additional inquiry into the applicant's credit during the last 12 months, we would expect the interest rate for the loan to be higher, on average, by 0.23 points.

::: {.guidedpractice data-latex=""}
Compute the residual of the first observation in Table \@ref(tab:loans-data-matrix) on page using the full model.[^model-mlr-5]
:::

[^model-mlr-5]: To compute the residual, we first need the predicted value, which we compute by plugging values into the equation from earlier.
    For example, $\texttt{verified_income}_{\texttt{Source Verified}}$ takes a value of 0, $\texttt{verified_income}_{\texttt{Verified}}$ takes a value of 1 (since the borrower's income source and amount were verified), was 18.01, and so on.
    This leads to a prediction of $\widehat{\texttt{interest_rate}}_1 = 18.09$.
    The observed interest rate was 14.07%, which leads to a residual of $e_1 = 14.07 - 18.09 = -4.02$.

::: {.workedexample data-latex=""}
We calculated a slope coefficient of 0.74 for `bankruptcy` in Section \@ref(ind-and-cat-predictors) while the coefficient is 0.39 here.
Why is there a difference between the coefficient values between the models with single and multiple predictors?

------------------------------------------------------------------------

If we examined the data carefully, we would see that some predictors are correlated.
For instance, when we modeled the relationship of the outcome `interest_rate` and predictor `bankruptcy` using linear regression, we were unable to control for other variables like whether the borrower had their income verified, the borrower's debt-to-income ratio, and other variables.
That original model was constructed in a vacuum and did not consider the full context of everything that is considered when an interest rate is decided.
When we include all of the variables, underlying and unintentional bias that was missed by not including these other variables is reduced or eliminated.
Of course, bias can still exist from other confounding variables.
:::

The previous example describes a common issue in multiple regression: correlation among predictor variables.
We say the two predictor variables are collinear (pronounced as *co-linear*) when they are correlated, and this **multicollinearity** complicates model estimation.
While it is impossible to prevent multicollinearity from arising in observational data, experiments are usually designed to prevent predictors from being multicollinear.



::: {.guidedpractice data-latex=""}
The estimated value of the intercept is 1.89, and one might be tempted to make some interpretation of this coefficient, such as, it is the model's predicted interest rate when each of the variables take value zero: income source is not verified, the borrower has no debt (debt-to-income and credit utilization are zero), and so on.
Is this reasonable?
Is there any value gained by making this interpretation?[^model-mlr-6]
:::

[^model-mlr-6]: Many of the variables do take a value 0 for at least one data point, and for those variables, it is reasonable.
    However, one variable never takes a value of zero: `term`, which describes the length of the loan, in months.
    If `term` is set to zero, then the loan must be paid back immediately; the borrower must give the money back as soon as they receive it, which means it is not a real loan.
    Ultimately, the interpretation of the intercept in this setting is not insightful.

## Adjusted R-squared

We first used $R^2$ in Section \@ref(r-squared) to determine the amount of variability in the response that was explained by the model: $$
R^2 = 1 - \frac{\text{variability in residuals}}{\text{variability in the outcome}}
    = 1 - \frac{Var(e_i)}{Var(y_i)}
$$where $e_i$ represents the residuals of the model and $y_i$ the outcomes.
This equation remains valid in the multiple regression framework, but a small enhancement can make it even more informative when comparing models.

::: {.guidedpractice data-latex=""}
The variance of the residuals for the model given in the earlier Guided Practice is 18.53, and the variance of the total price in all the auctions is 25.01.
Calculate $R^2$ for this model.[^model-mlr-7]
:::

[^model-mlr-7]: $R^2 = 1 - \frac{18.53}{25.01} = 0.2591$.

This strategy for estimating $R^2$ is acceptable when there is just a single variable.
However, it becomes less helpful when there are many variables.
The regular $R^2$ is a biased estimate of the amount of variability explained by the model when applied to model with more than one predictor.
To get a better estimate, we use the adjusted $R^2$.

::: {.important data-latex=""}
**Adjusted R-squared as a tool for model assessment.**

The **adjusted R-squared** is computed as

$$
\begin{aligned}
  R_{adj}^{2}
    &= 1 - \frac{s_{\text{residuals}}^2 / (n-k-1)}
        {s_{\text{outcome}}^2 / (n-1)} \\
    &= 1 - \frac{s_{\text{residuals}}^2}{s_{\text{outcome}}^2}
        \times \frac{n-1}{n-k-1}
\end{aligned}
$$

where $n$ is the number of observations used to fit the model and $k$ is the number of predictor variables in the model.
Remember that a categorical predictor with $p$ levels will contribute $p - 1$ to the number of variables in the model.
:::



Because $k$ is never negative, the adjusted $R^2$ will be smaller -- often times just a little smaller -- than the unadjusted $R^2$.
The reasoning behind the adjusted $R^2$ lies in the **degrees of freedom** associated with each variance, which is equal to $n - k - 1$ in the multiple regression context.
If we were to make predictions for *new data* using our current model, we would find that the unadjusted $R^2$ would tend to be slightly overly optimistic, while the adjusted $R^2$ formula helps correct this bias.



::: {.guidedpractice data-latex=""}
There were n = 10,000 auctions in the dataset and $k=9$ predictor variables in the model.
Use $n$, $k$, and the variances from the earlier Guided Practice to calculate $R_{adj}^2$ for the interest rate model.[^model-mlr-8]
:::

[^model-mlr-8]: $R_{adj}^2 = 1 - \frac{18.53}{25.01}\times \frac{10000-1}{1000-9-1} = 0.2584$.
    While the difference is very small, it will be important when we fine tune the model in the next section.

::: {.guidedpractice data-latex=""}
Suppose you added another predictor to the model, but the variance of the errors $Var(e_i)$ didn't go down.
What would happen to the $R^2$?
What would happen to the adjusted $R^2$?[^model-mlr-9]
:::

[^model-mlr-9]: The unadjusted $R^2$ would stay the same and the adjusted $R^2$ would go down.

Adjusted $R^2$ could also have been used in Chapter \@ref(model-slr) where we introduced regression models with a single predictor.
However, when there is only $k = 1$ predictors, adjusted $R^2$ is very close to regular $R^2$, so this nuance isn't typically important when the model has only one predictor.

## Model selection {#model-selection}

The best model is not always the most complicated.
Sometimes including variables that are not evidently important can actually reduce the accuracy of predictions.
In this section, we discuss model selection strategies, which will help us eliminate variables from the model that are found to be less important.
It's common (and hip, at least in the statistical world) to refer to models that have undergone such variable pruning as **parsimonious**.



In practice, the model that includes all available predictors is often referred to as the **full model**.
The full model may not be the best model, and if it isn't, we want to identify a smaller model that is preferable.



### Stepwise selection

Two common strategies for adding or removing variables in a multiple regression model are called backward elimination and forward selection.
These techniques are often referred to as **stepwise selection** strategies, because they add or delete one variable at a time as they "step" through the candidate predictors.



**Backward elimination** starts with the full model (the model that includes all potential predictor variables. Variables are eliminated one-at-a-time from the model until we cannot improve the model any further.

**Forward selection** is the reverse of the backward elimination technique.
Instead of eliminating variables one-at-a-time, we add variables one-at-a-time until we cannot find any variables that improve the model any further.



An important consideration in implementing either of these stepwise selection strategies is the criterion used to decide whether to eliminate or add a variable.
One commonly used decision criterion is adjusted $R^2$.
When using adjusted $R^2$ as the decision criterion, we seek to eliminate or add variables depending on whether they lead to the largest improvement in adjusted $R^2$ and we stop when adding or elimination of another variable does not lead to further improvement in adjusted $R^2$.

Adjusted $R^2$ describes the strength of a model fit, and it is a useful tool for evaluating which predictors are adding value to the model, where *adding value* means they are (likely) improving the accuracy in predicting future outcomes.

Let's consider two models, which are shown in Table \@ref(tab:loans-full-for-model-selection) and Table \@ref(tab:loans-full-except-issue-month).
The first table summarizes the full model since it includes all predictors, while the second does not include the `issue_month` variable.

<table class="table table-striped table-condensed" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:loans-full-for-model-selection)The fit for the full regression model, including the adjusted $R^2$.</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> term </th>
   <th style="text-align:right;"> estimate </th>
   <th style="text-align:right;"> std.error </th>
   <th style="text-align:right;"> statistic </th>
   <th style="text-align:right;"> p.value </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> (Intercept) </td>
   <td style="text-align:right;width: 5em; "> 1.89 </td>
   <td style="text-align:right;width: 5em; "> 0.21 </td>
   <td style="text-align:right;width: 5em; "> 9.01 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> verified_incomeSource Verified </td>
   <td style="text-align:right;width: 5em; "> 1.00 </td>
   <td style="text-align:right;width: 5em; "> 0.10 </td>
   <td style="text-align:right;width: 5em; "> 10.06 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> verified_incomeVerified </td>
   <td style="text-align:right;width: 5em; "> 2.56 </td>
   <td style="text-align:right;width: 5em; "> 0.12 </td>
   <td style="text-align:right;width: 5em; "> 21.87 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> debt_to_income </td>
   <td style="text-align:right;width: 5em; "> 0.02 </td>
   <td style="text-align:right;width: 5em; "> 0.00 </td>
   <td style="text-align:right;width: 5em; "> 7.43 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> credit_util </td>
   <td style="text-align:right;width: 5em; "> 4.90 </td>
   <td style="text-align:right;width: 5em; "> 0.16 </td>
   <td style="text-align:right;width: 5em; "> 30.25 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> bankruptcy1 </td>
   <td style="text-align:right;width: 5em; "> 0.39 </td>
   <td style="text-align:right;width: 5em; "> 0.13 </td>
   <td style="text-align:right;width: 5em; "> 2.96 </td>
   <td style="text-align:right;width: 5em; "> 0.0031 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> term </td>
   <td style="text-align:right;width: 5em; "> 0.15 </td>
   <td style="text-align:right;width: 5em; "> 0.00 </td>
   <td style="text-align:right;width: 5em; "> 38.89 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> credit_checks </td>
   <td style="text-align:right;width: 5em; "> 0.23 </td>
   <td style="text-align:right;width: 5em; "> 0.02 </td>
   <td style="text-align:right;width: 5em; "> 12.52 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> issue_monthJan-2018 </td>
   <td style="text-align:right;width: 5em; "> 0.05 </td>
   <td style="text-align:right;width: 5em; "> 0.11 </td>
   <td style="text-align:right;width: 5em; "> 0.42 </td>
   <td style="text-align:right;width: 5em; "> 0.6736 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> issue_monthMar-2018 </td>
   <td style="text-align:right;width: 5em; "> -0.04 </td>
   <td style="text-align:right;width: 5em; "> 0.11 </td>
   <td style="text-align:right;width: 5em; "> -0.39 </td>
   <td style="text-align:right;width: 5em; "> 0.696 </td>
  </tr>
  <tr grouplength="2"><td colspan="5" style="border-bottom: 1px solid;"><strong></strong></td></tr>
<tr>
   <td style="text-align:left;width: 17em; padding-left: 4em;font-style: italic;" indentlevel="2"> Adjusted R-sq = 0.2597 </td>
   <td style="text-align:right;width: 5em; font-style: italic;">  </td>
   <td style="text-align:right;width: 5em; font-style: italic;">  </td>
   <td style="text-align:right;width: 5em; font-style: italic;">  </td>
   <td style="text-align:right;width: 5em; font-style: italic;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; padding-left: 4em;font-style: italic;" indentlevel="2"> df = 9964 </td>
   <td style="text-align:right;width: 5em; font-style: italic;">  </td>
   <td style="text-align:right;width: 5em; font-style: italic;">  </td>
   <td style="text-align:right;width: 5em; font-style: italic;">  </td>
   <td style="text-align:right;width: 5em; font-style: italic;">  </td>
  </tr>
</tbody>
</table>

<table class="table table-striped table-condensed" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:loans-full-except-issue-month)The fit for the regression model after dropping issue month, including the adjusted $R^2$.</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> term </th>
   <th style="text-align:right;"> estimate </th>
   <th style="text-align:right;"> std.error </th>
   <th style="text-align:right;"> statistic </th>
   <th style="text-align:right;"> p.value </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> (Intercept) </td>
   <td style="text-align:right;width: 5em; "> 1.90 </td>
   <td style="text-align:right;width: 5em; "> 0.20 </td>
   <td style="text-align:right;width: 5em; "> 9.56 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> verified_incomeSource Verified </td>
   <td style="text-align:right;width: 5em; "> 1.00 </td>
   <td style="text-align:right;width: 5em; "> 0.10 </td>
   <td style="text-align:right;width: 5em; "> 10.05 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> verified_incomeVerified </td>
   <td style="text-align:right;width: 5em; "> 2.56 </td>
   <td style="text-align:right;width: 5em; "> 0.12 </td>
   <td style="text-align:right;width: 5em; "> 21.86 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> debt_to_income </td>
   <td style="text-align:right;width: 5em; "> 0.02 </td>
   <td style="text-align:right;width: 5em; "> 0.00 </td>
   <td style="text-align:right;width: 5em; "> 7.44 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> credit_util </td>
   <td style="text-align:right;width: 5em; "> 4.90 </td>
   <td style="text-align:right;width: 5em; "> 0.16 </td>
   <td style="text-align:right;width: 5em; "> 30.25 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> bankruptcy1 </td>
   <td style="text-align:right;width: 5em; "> 0.39 </td>
   <td style="text-align:right;width: 5em; "> 0.13 </td>
   <td style="text-align:right;width: 5em; "> 2.96 </td>
   <td style="text-align:right;width: 5em; "> 0.0031 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> term </td>
   <td style="text-align:right;width: 5em; "> 0.15 </td>
   <td style="text-align:right;width: 5em; "> 0.00 </td>
   <td style="text-align:right;width: 5em; "> 38.89 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; font-family: monospace;"> credit_checks </td>
   <td style="text-align:right;width: 5em; "> 0.23 </td>
   <td style="text-align:right;width: 5em; "> 0.02 </td>
   <td style="text-align:right;width: 5em; "> 12.52 </td>
   <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
  </tr>
  <tr grouplength="2"><td colspan="5" style="border-bottom: 1px solid;"><strong></strong></td></tr>
<tr>
   <td style="text-align:left;width: 17em; padding-left: 4em;font-style: italic;" indentlevel="2"> Adjusted R-sq = 0.2598 </td>
   <td style="text-align:right;width: 5em; font-style: italic;">  </td>
   <td style="text-align:right;width: 5em; font-style: italic;">  </td>
   <td style="text-align:right;width: 5em; font-style: italic;">  </td>
   <td style="text-align:right;width: 5em; font-style: italic;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 17em; padding-left: 4em;font-style: italic;" indentlevel="2"> df = 9966 </td>
   <td style="text-align:right;width: 5em; font-style: italic;">  </td>
   <td style="text-align:right;width: 5em; font-style: italic;">  </td>
   <td style="text-align:right;width: 5em; font-style: italic;">  </td>
   <td style="text-align:right;width: 5em; font-style: italic;">  </td>
  </tr>
</tbody>
</table>

::: {.workedexample data-latex=""}
Which of the two models is better?

------------------------------------------------------------------------

We compare the adjusted $R^2$ of each model to determine which to choose.
Since the second model has a higher $R^2_{adj}$ compared to the first model, we prefer the second model to the first.
:::

Will the model without `issue_month` be better than the model with `issue_month`?
We cannot know for sure, but based on the adjusted $R^2$, this is our best assessment.

::: {.workedexample data-latex=""}
Results corresponding to the full model for the `loans` data are shown in Table \@ref(tab:loans-full-for-model-selection).
How should we proceed under the backward elimination strategy?

------------------------------------------------------------------------

Our baseline adjusted $R^2$ from the full model is 0.2597, and we need to determine whether dropping a predictor will improve the adjusted $R^2$.
To check, we fit models that each drop a different predictor, and we record the adjusted $R^2$:

-   Excluding `verified_income`: 0.2238
-   Excluding `debt_to_income`: 0.2557
-   Excluding `credit_util`: 0.1916
-   Excluding `bankruptcy`: 0.2589
-   Excluding `term`: 0.1468
-   Excluding `credit_checks`: 0.2484
-   Excluding `issue_month`: 0.2598

The model without `issue_month` has the highest adjusted $R^2$ of 0.2598, higher than the adjusted $R^2$ for the full model.
Because eliminating `issue_month` leads to a model with a higher adjusted $R^2$, we drop `issue_month` from the model.

Since we eliminated a predictor from the model in the first step, we see whether we should eliminate any additional predictors.
Our baseline adjusted $R^2$ is now $R^2_{adj} = 0.2598$.
We now fit new models, which consider eliminating each of the remaining predictors in addition to `issue_month`:

-   Excluding `issue_month` and `verified_income`: 0.22395
-   Excluding `issue_month` and `debt_to_income`: 0.25579
-   Excluding `issue_month` and `credit_util`: 0.19174
-   Excluding `issue_month` and `bankruptcy`: 0.25898
-   Excluding `issue_month` and `term`: 0.14692
-   Excluding `issue_month` and `credit_checks`: 0.24801

None of these models lead to an improvement in adjusted $R^2$, so we do not eliminate any of the remaining predictors.
That is, after backward elimination, we are left with the model that keeps all predictors except `issue_month`, which we can summarize using the coefficients from Table \@ref(tab:loans-full-except-issue-month).

$$
\begin{aligned}
\widehat{\texttt{interest_rate}} &= 1.90 \\
&+ 1.00 \times \texttt{verified_income}_\texttt{Source only} \\
&+ 2.56 \times \texttt{verified_income}_\texttt{Verified} \\
&+ 0.02 \times \texttt{debt_to_income} \\
&+ 4.90 \times \texttt{credit_util} \\
&+ 0.39 \times \texttt{bankruptcy} \\
&+ 0.15 \times \texttt{term} \\
&+ 0.23 \times \texttt{credit_check}
\end{aligned}
$$
:::

::: {.workedexample data-latex=""}
Construct a model for predicting `interest_rate` from the `loans` data using forward selection.

------------------------------------------------------------------------

We start with the model that includes no predictors.
Then we fit each of the possible models with just one predictor.
Then we examine the adjusted $R^2$ for each of these models:

-   Including `verified_income`: 0.05926
-   Including `debt_to_income`: 0.01946
-   Including `credit_util`: 0.06452
-   Including `bankruptcy`: 0.00222
-   Including `term`: 0.12855
-   Including `credit_checks`: -0.0001
-   Including `issue_month`: 0.01711

In this first step, we compare the adjusted $R^2$ against a baseline model that has no predictors.
The no-predictors model always has $R_{adj}^2 = 0$.
The model with one predictor that has the largest adjusted $R^2$ is the model with the `term` predictor, and because this adjusted $R^2$ is larger than the adjusted $R^2$ from the model with no predictors ($R_{adj}^2 = 0$), we will add this variable to our model.

We repeat the process again, this time considering 2-predictor models where one of the predictors is `term` and with a new baseline of $R^2_{adj} = 0.12855:$

-   Including `term` and `verified_income`: 0.16851
-   Including `term` and `debt_to_income`: 0.14368
-   Including `term` and `credit_util`: 0.20046
-   Including `term` and `bankruptcy`: 0.13070
-   Including `term` and `credit_checks`: 0.12840
-   Including `term` and `issue_month`: 0.14294

Adding `credit_util` yields the highest increase in adjusted $R^2$ and has a higher adjusted $R^2$ (0.20046) than the baseline (0.12855).
Thus, we will also add `credit_util` to the model as a predictor.

Since we have again added a predictor to the model, we have a new baseline adjusted $R^2$ of 0.20046.
We can continue on and see whether it would be beneficial to add a third predictor:

-   Including `term`, `credit_util, and verified_income`: 0.24183
-   Including `term`, `credit_util, and debt_to_income`: 0.20810
-   Including `term`, `credit_util, and bankruptcy`: 0.20169
-   Including `term`, `credit_util, and credit_checks`: 0.20031
-   Including `term`, `credit_util, and issue_month`: 0.21629

The model including `verified_income` has the largest increase in adjusted $R^2$ (0.24183) from the baseline (0.20046), so we add `verified_income` to the model as a predictor as well.

We continue on in this way, next adding `debt_to_income`, then `credit_checks`, and `bankruptcy`.
At this point, we come again to the `issue_month` variable: adding this as a predictor leads to $R_{adj}^2 = 0.25843$, while keeping all the other predictors but excluding `issue_month` has a higher $R_{adj}^2 = 0.25854$.
This means we do not add `issue_month` to the model as a predictor.
In this example, we have arrived at the same model that we identified from backward elimination.
:::

::: {.important data-latex=""}
**Stepwise selection strategies.**

Backward elimination begins with the model having the largest number of predictors and eliminates variables one-by-one until we are satisfied that all remaining variables are important to the model.
Forward selection starts with no variables included in the model, then it adds in variables according to their importance until no other important variables are found.
Notice that, for both methods, we have always chosen to retain the model with the largest adjusted $R^2$ value, even if the difference is less than half a percent (e.g., 0.2597 versus 0.2598).
One could argue that the difference between these two models is negligible, as they both explain nearly the same amount of variability in the `interest_rate`.
These negligible differences are an important aspect to model selection.
It is highly advised that *before* you begin the model selection process, you decide what a "meaningful" difference in adjusted $R^2$ is for the context of your data.
Maybe this difference is 1% or maybe it is 5%.
This "threshold" is what you will then use to decide if one model is "better" than another model.
Using meaningful thresholds in model selection requires more critical thinking about what the adjusted $R^2$ values mean.

Additionally, backward elimination and forward selection sometimes arrive at different final models.
This is because the decision for whether to include a given variable or not depends on the other variables that are already in the model.
With forward selection, you start with a model that includes no variables and add variables one at a time.
In backward elimination, you start with a model that includes all of the variables and remove variables one at a time.
How much a given variable changes the percentage of the variability in the outcome that is explained by the model depends on what other variables are in the model.
This is especially important if the predictor variables are correlated with each other.

There is no "one size fits all" model selection strategy, which is why there are so many different model selection methods.
We hope you walk away from this exploration understanding how stepwise selection is carried out and the considerations that should be made when using stepwise selection with regression models.
:::

### Other model selection strategies

Stepwise selection using adjusted $R^2$ as the decision criteria is one of many commonly used model selection strategies.
Stepwise selection can also be carried out with decision criteria other than adjusted $R^2$, such as p-values, which you'll learn about in Chapters \@ref(inf-model-slr) onwards, or AIC (Akaike information criterion) or BIC (Bayesian information criterion), which you might learn about in more advanced courses.

Alternatively, one could choose to include or exclude variables from a model based on expert opinion or due to research focus.
In fact, many statisticians discourage the use of stepwise regression alone for model selection and advocate, instead, for a more thoughtful approach that carefully considers the research focus and features of the data.

\clearpage

## Chapter review {#chp8-review}

### Summary

With real data, there is often a need to describe how multiple variables can be modeled together.
In this chapter, we have presented one approach using multiple linear regression.
Each coefficient represents a one unit increase of that predictor variable on the response variable *given* the rest of the predictor variables in the model.
Working with and interpreting multivariable models can be tricky, especially when the predictor variables show multicollinearity.
There is often no perfect or "right" final model, but using the adjusted $R^2$ value is one way to identify important predictor variables for a final regression model.
In later chapters we will generalize multiple linear regression models to a larger population of interest from which the dataset was generated.

### Terms

We introduced the following terms in the chapter.
If you're not sure what some of these terms mean, we recommend you go back in the text and review their definitions.
We are purposefully presenting them in alphabetical order, instead of in order of appearance, so they will be a little more challenging to locate.
However you should be able to easily spot them as **bolded text**.

<table class="table table-striped table-condensed" style="margin-left: auto; margin-right: auto;">
<tbody>
  <tr>
   <td style="text-align:left;"> adjusted R-squared </td>
   <td style="text-align:left;"> full model </td>
   <td style="text-align:left;"> reference level </td>
  </tr>
  <tr>
   <td style="text-align:left;"> backward elimination </td>
   <td style="text-align:left;"> multicollinearity </td>
   <td style="text-align:left;"> stepwise selection </td>
  </tr>
  <tr>
   <td style="text-align:left;"> degrees of freedom </td>
   <td style="text-align:left;"> multiple regression </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> forward selection </td>
   <td style="text-align:left;"> parsimonious </td>
   <td style="text-align:left;">  </td>
  </tr>
</tbody>
</table>

\clearpage

## Exercises {#chp8-exercises}

Answers to odd numbered exercises can be found in Appendix \@ref(exercise-solutions-08).

::: {.exercises data-latex=""}

1. **High correlation, good or bad?**
Two friends, Frances and Annika, are in disagreement about whether high correlation values are *always* good in the context of regression.
Frances claims that it's desirable for all variables in the dataset to be highly correlated to each other when building linear models.
Annika claims that while it's desirable for each of the predictors to be highly correlated with the outcome, it is not desirable for the predictors to be highly correlated with each other.
Who is right: Frances, Annika, both, or neither? 
Explain your reasoning using appropriate terminology.

1. **Dealing with categorical predictors.**
Two friends, Elliott and Adrian, want to build a model predicting typing speed (average number of words typed per minute) from whether the person wears glasses or not.
Before building the model they want to conduct some exploratory analysis to evaluate the strength of the association between these two variables, but they're in disagreement about how to evaluate how strongly a categorical predictor is associated with a numerical outcome.
Elliott claims that it is not possible to calculate a correlation coefficient to summarize the relationship between a categorical predictor and a numerical outcome, however they're not sure what a better alternative is.
Adrian claims that you can recode a binary predictor as a 0/1 variable (assign one level to be 0 and the other to be 1), thus converting it to a numerical variable.
According to Adrian, you can then calculate the correlation coefficient between the predictor and the outcome.
Who is right: Elliott or Adrian?
If you pick Elliott, can you suggest a better alternative for evaluating the association between the categorical predictor and the numerical outcome?

1. **Training for the 5K.**
Nico signs up for a 5K (a 5,000 metre running race) 30 days prior to the race. 
They decide to run a 5K every day to train for it, and each day they record the following information: `days_since_start` (number of days since starting training), `days_till_race` (number of days left until the race), `mood` (poor, good, awesome), `tiredness` (1-not tired to 10-very tired), and `time` (time it takes to run 5K, recorded as mm:ss).
Top few rows of the data they collect is shown below.

    <table class="table table-striped table-condensed" style="width: auto !important; margin-left: auto; margin-right: auto;">
     <thead>
      <tr>
       <th style="text-align:right;"> days_since_start </th>
       <th style="text-align:right;"> days_till_race </th>
       <th style="text-align:left;"> mood </th>
       <th style="text-align:right;"> tiredness </th>
       <th style="text-align:right;"> time </th>
      </tr>
     </thead>
    <tbody>
      <tr>
       <td style="text-align:right;"> 1 </td>
       <td style="text-align:right;"> 29 </td>
       <td style="text-align:left;"> good </td>
       <td style="text-align:right;"> 3 </td>
       <td style="text-align:right;"> 25:45 </td>
      </tr>
      <tr>
       <td style="text-align:right;"> 2 </td>
       <td style="text-align:right;"> 28 </td>
       <td style="text-align:left;"> poor </td>
       <td style="text-align:right;"> 5 </td>
       <td style="text-align:right;"> 27:13 </td>
      </tr>
      <tr>
       <td style="text-align:right;"> 3 </td>
       <td style="text-align:right;"> 27 </td>
       <td style="text-align:left;"> awesome </td>
       <td style="text-align:right;"> 4 </td>
       <td style="text-align:right;"> 24:13 </td>
      </tr>
      <tr>
       <td style="text-align:right;"> ... </td>
       <td style="text-align:right;"> ... </td>
       <td style="text-align:left;"> ... </td>
       <td style="text-align:right;"> ... </td>
       <td style="text-align:right;"> ... </td>
      </tr>
    </tbody>
    </table>
    
    Using these data Nico wants to build a model predicting `time` from the other variables. Should they include all variables shown above in their model? Why or why not?

1. **Multiple regression fact checking.**
Determine which of the following statements are true and false.
For each statement that is false, explain why it is false.

    a. If predictors are collinear, then removing one variable will have no influence on the point estimate of another variable's coefficient.

    b. Suppose a numerical variable $x$ has a coefficient of $b_1 = 2.5$ in the multiple regression model. Suppose also that the first observation has $x_1 = 7.2$, the second observation has a value of $x_1 = 8.2$, and these two observations have the same values for all other predictors. Then the predicted value of the second observation will be 2.5 higher than the prediction of the first observation based on the multiple regression model.

    c. If a regression model's first variable has a coefficient of $b_1 = 5.7$, then if we are able to influence the data so that an observation will have its $x_1$ be 1 larger than it would otherwise, the value $y_1$ for this observation would increase by 5.7.
    
    \clearpage

1. **Baby weights and smoking.** 
US Department of Health and Human Services, Centers for Disease Control and Prevention collect information on births recorded in the country.
The data used here are a random sample of 1,000 births from 2014.
Here, we study the relationship between smoking and weight of the baby. 
The variable `smoke` is coded 1 if the mother is a smoker, and 0 if not. 
The summary table below shows the results of a linear regression model for predicting the average birth weight of babies, measured in pounds, based on the smoking status of the mother.^[The [`births14`](http://openintrostat.github.io/openintro/reference/births14.html) data used in this exercise can be found in the [**openintro**](http://openintrostat.github.io/openintro) R package.] [@data:births14]

    <table class="table table-striped table-condensed" style="width: auto !important; margin-left: auto; margin-right: auto;">
     <thead>
      <tr>
       <th style="text-align:left;"> term </th>
       <th style="text-align:right;"> estimate </th>
       <th style="text-align:right;"> std.error </th>
       <th style="text-align:right;"> statistic </th>
       <th style="text-align:right;"> p.value </th>
      </tr>
     </thead>
    <tbody>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> (Intercept) </td>
       <td style="text-align:right;width: 5em; "> 7.270 </td>
       <td style="text-align:right;width: 5em; "> 0.0435 </td>
       <td style="text-align:right;width: 5em; "> 167.22 </td>
       <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> habitsmoker </td>
       <td style="text-align:right;width: 5em; "> -0.593 </td>
       <td style="text-align:right;width: 5em; "> 0.1275 </td>
       <td style="text-align:right;width: 5em; "> -4.65 </td>
       <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
      </tr>
    </tbody>
    </table>

    a.  Write the equation of the regression model.

    b.  Interpret the slope in this context, and calculate the predicted birth weight of babies born to smoker and non-smoker mothers.

1. **Baby weights and mature moms.**
The following is a model for predicting baby weight from whether the mom is classified as a `mature` mom (35 years or older at the time of pregnancy). [@data:births14]

    <table class="table table-striped table-condensed" style="width: auto !important; margin-left: auto; margin-right: auto;">
     <thead>
      <tr>
       <th style="text-align:left;"> term </th>
       <th style="text-align:right;"> estimate </th>
       <th style="text-align:right;"> std.error </th>
       <th style="text-align:right;"> statistic </th>
       <th style="text-align:right;"> p.value </th>
      </tr>
     </thead>
    <tbody>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> (Intercept) </td>
       <td style="text-align:right;width: 5em; "> 7.354 </td>
       <td style="text-align:right;width: 5em; "> 0.103 </td>
       <td style="text-align:right;width: 5em; "> 71.02 </td>
       <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> matureyounger mom </td>
       <td style="text-align:right;width: 5em; "> -0.185 </td>
       <td style="text-align:right;width: 5em; "> 0.113 </td>
       <td style="text-align:right;width: 5em; "> -1.64 </td>
       <td style="text-align:right;width: 5em; "> 0.102 </td>
      </tr>
    </tbody>
    </table>

    a.  Write the equation of the regression model.

    b.  Interpret the slope in this context, and calculate the predicted birth weight of babies born to mature and younger mothers.
    
1. **Movie returns, prediction.**
A model was fit to predict return-on-investment (ROI) on movies based on release year and genre (Adventure, Action, Drama, Horror, and Comedy). The model output is shown below. [@webpage:horrormovies]

    <table class="table table-striped table-condensed" style="width: auto !important; margin-left: auto; margin-right: auto;">
     <thead>
      <tr>
       <th style="text-align:left;"> term </th>
       <th style="text-align:right;"> estimate </th>
       <th style="text-align:right;"> std.error </th>
       <th style="text-align:right;"> statistic </th>
       <th style="text-align:right;"> p.value </th>
      </tr>
     </thead>
    <tbody>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> (Intercept) </td>
       <td style="text-align:right;width: 5em; "> -156.04 </td>
       <td style="text-align:right;width: 5em; "> 169.15 </td>
       <td style="text-align:right;width: 5em; "> -0.92 </td>
       <td style="text-align:right;width: 5em; "> 0.3565 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> release_year </td>
       <td style="text-align:right;width: 5em; "> 0.08 </td>
       <td style="text-align:right;width: 5em; "> 0.08 </td>
       <td style="text-align:right;width: 5em; "> 0.94 </td>
       <td style="text-align:right;width: 5em; "> 0.348 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> genreAdventure </td>
       <td style="text-align:right;width: 5em; "> 0.30 </td>
       <td style="text-align:right;width: 5em; "> 0.74 </td>
       <td style="text-align:right;width: 5em; "> 0.40 </td>
       <td style="text-align:right;width: 5em; "> 0.6914 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> genreComedy </td>
       <td style="text-align:right;width: 5em; "> 0.57 </td>
       <td style="text-align:right;width: 5em; "> 0.69 </td>
       <td style="text-align:right;width: 5em; "> 0.83 </td>
       <td style="text-align:right;width: 5em; "> 0.4091 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> genreDrama </td>
       <td style="text-align:right;width: 5em; "> 0.37 </td>
       <td style="text-align:right;width: 5em; "> 0.62 </td>
       <td style="text-align:right;width: 5em; "> 0.61 </td>
       <td style="text-align:right;width: 5em; "> 0.5438 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> genreHorror </td>
       <td style="text-align:right;width: 5em; "> 8.61 </td>
       <td style="text-align:right;width: 5em; "> 0.86 </td>
       <td style="text-align:right;width: 5em; "> 9.97 </td>
       <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
      </tr>
    </tbody>
    </table>
    
    a. For a given release year, which genre of movies are predicted, on average, to have the highest predicted return on investment?
    
    b. The adjusted $R^2$ of this model is 10.71%. Adding the production budget of the movie to the model increases the adjusted $R^2$ to 10.84%. Should production budget be added to the model?
    
    \clearpage

1. **Movie returns by genre.**
A model was fit to predict return-on-investment (ROI) on movies based on release year and genre (Adventure, Action, Drama, Horror, and Comedy). 
The plots below show the predicted ROI vs. actual ROI for each of the genres separately. Do these figures support the comment in the FiveThirtyEight.com article that states, "The return-on-investment potential for horror movies is absurd." Note that the x-axis range varies for each plot. [@webpage:horrormovies]

    <img src="08-model-mlr_files/figure-html/unnamed-chunk-16-1.png" width="90%" style="display: block; margin: auto;" />

1. **Predicting baby weights.**
A more realistic approach to modeling baby weights is to consider all possibly related variables at once. Other variables of interest include length of pregnancy in weeks (`weeks`), mother's age in years (`mage`), the sex of the baby (`sex`), smoking status of the mother (`habit`), and the number of hospital (`visits`) visits during pregnancy. Below are three observations from this data set.

    <table class="table table-striped table-condensed" style="width: auto !important; margin-left: auto; margin-right: auto;">
     <thead>
      <tr>
       <th style="text-align:center;"> weight </th>
       <th style="text-align:center;"> weeks </th>
       <th style="text-align:center;"> mage </th>
       <th style="text-align:center;"> sex </th>
       <th style="text-align:center;"> visits </th>
       <th style="text-align:center;"> habit </th>
      </tr>
     </thead>
    <tbody>
      <tr>
       <td style="text-align:center;width: 5em; "> 6.96 </td>
       <td style="text-align:center;width: 5em; "> 37 </td>
       <td style="text-align:center;width: 5em; "> 34 </td>
       <td style="text-align:center;width: 5em; "> male </td>
       <td style="text-align:center;width: 5em; "> 14 </td>
       <td style="text-align:center;"> nonsmoker </td>
      </tr>
      <tr>
       <td style="text-align:center;width: 5em; "> 8.86 </td>
       <td style="text-align:center;width: 5em; "> 41 </td>
       <td style="text-align:center;width: 5em; "> 31 </td>
       <td style="text-align:center;width: 5em; "> female </td>
       <td style="text-align:center;width: 5em; "> 12 </td>
       <td style="text-align:center;"> nonsmoker </td>
      </tr>
      <tr>
       <td style="text-align:center;width: 5em; "> 7.51 </td>
       <td style="text-align:center;width: 5em; "> 37 </td>
       <td style="text-align:center;width: 5em; "> 36 </td>
       <td style="text-align:center;width: 5em; "> female </td>
       <td style="text-align:center;width: 5em; "> 10 </td>
       <td style="text-align:center;"> nonsmoker </td>
      </tr>
    </tbody>
    </table>

    The summary table below shows the results of a regression model for predicting the average birth weight of babies based on all of the variables presented above.
    
    <table class="table table-striped table-condensed" style="width: auto !important; margin-left: auto; margin-right: auto;">
     <thead>
      <tr>
       <th style="text-align:left;"> term </th>
       <th style="text-align:right;"> estimate </th>
       <th style="text-align:right;"> std.error </th>
       <th style="text-align:right;"> statistic </th>
       <th style="text-align:right;"> p.value </th>
      </tr>
     </thead>
    <tbody>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> (Intercept) </td>
       <td style="text-align:right;width: 5em; "> -3.82 </td>
       <td style="text-align:right;width: 5em; "> 0.57 </td>
       <td style="text-align:right;width: 5em; "> -6.73 </td>
       <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> weeks </td>
       <td style="text-align:right;width: 5em; "> 0.26 </td>
       <td style="text-align:right;width: 5em; "> 0.01 </td>
       <td style="text-align:right;width: 5em; "> 18.93 </td>
       <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> mage </td>
       <td style="text-align:right;width: 5em; "> 0.02 </td>
       <td style="text-align:right;width: 5em; "> 0.01 </td>
       <td style="text-align:right;width: 5em; "> 2.53 </td>
       <td style="text-align:right;width: 5em; "> 0.0115 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> sexmale </td>
       <td style="text-align:right;width: 5em; "> 0.37 </td>
       <td style="text-align:right;width: 5em; "> 0.07 </td>
       <td style="text-align:right;width: 5em; "> 5.30 </td>
       <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> visits </td>
       <td style="text-align:right;width: 5em; "> 0.02 </td>
       <td style="text-align:right;width: 5em; "> 0.01 </td>
       <td style="text-align:right;width: 5em; "> 2.09 </td>
       <td style="text-align:right;width: 5em; "> 0.0373 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> habitsmoker </td>
       <td style="text-align:right;width: 5em; "> -0.43 </td>
       <td style="text-align:right;width: 5em; "> 0.13 </td>
       <td style="text-align:right;width: 5em; "> -3.41 </td>
       <td style="text-align:right;width: 5em; "> 7e-04 </td>
      </tr>
    </tbody>
    </table>

    a. Write the equation of the regression model that includes all of the variables.

    b. Interpret the slopes of `weeks` and `habit` in this context.

    c. If we fit a model predicting baby weight from only `habit` (whether the mom smokes), we observe a difference in the slope coefficient for `habit` in this small model and the slope coefficient for `habit` in the larger model. Why might there be a difference?

    d. Calculate the residual for the first observation in the data set.
    
    \clearpage

1. **Palmer penguins, predicting body mass.**
Researchers studying a community of Antarctic penguins collected body measurement (bill length, bill depth, and flipper length measured in millimeters and body mass, measured in grams), species (*Adelie*, *Chinstrap*, or *Gentoo*), and sex (female or male) data on 344 penguins living on three islands (Torgersen, Biscoe, and Dream) in the Palmer Archipelago, Antarctica.^[The [`penguins`](https://allisonhorst.github.io/palmerpenguins/reference/penguins.html) data used in this exercise can be found in the [**palmerpenguins**](https://allisonhorst.github.io/palmerpenguins/) R package.] The summary table below shows the results of a linear regression model for predicting body mass (which is more difficult to measure) from the other variables in the dataset. [@palmerpenguins] 
    
    <table class="table table-striped table-condensed" style="width: auto !important; margin-left: auto; margin-right: auto;">
     <thead>
      <tr>
       <th style="text-align:left;"> term </th>
       <th style="text-align:right;"> estimate </th>
       <th style="text-align:right;"> std.error </th>
       <th style="text-align:right;"> statistic </th>
       <th style="text-align:right;"> p.value </th>
      </tr>
     </thead>
    <tbody>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> (Intercept) </td>
       <td style="text-align:right;width: 5em; "> -1461.0 </td>
       <td style="text-align:right;width: 5em; "> 571.3 </td>
       <td style="text-align:right;width: 5em; "> -2.6 </td>
       <td style="text-align:right;width: 5em; "> 0.011 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> bill_length_mm </td>
       <td style="text-align:right;width: 5em; "> 18.2 </td>
       <td style="text-align:right;width: 5em; "> 7.1 </td>
       <td style="text-align:right;width: 5em; "> 2.6 </td>
       <td style="text-align:right;width: 5em; "> 0.0109 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> bill_depth_mm </td>
       <td style="text-align:right;width: 5em; "> 67.2 </td>
       <td style="text-align:right;width: 5em; "> 19.7 </td>
       <td style="text-align:right;width: 5em; "> 3.4 </td>
       <td style="text-align:right;width: 5em; "> 7e-04 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> flipper_length_mm </td>
       <td style="text-align:right;width: 5em; "> 16.0 </td>
       <td style="text-align:right;width: 5em; "> 2.9 </td>
       <td style="text-align:right;width: 5em; "> 5.5 </td>
       <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> sexmale </td>
       <td style="text-align:right;width: 5em; "> 389.9 </td>
       <td style="text-align:right;width: 5em; "> 47.8 </td>
       <td style="text-align:right;width: 5em; "> 8.1 </td>
       <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> speciesChinstrap </td>
       <td style="text-align:right;width: 5em; "> -251.5 </td>
       <td style="text-align:right;width: 5em; "> 81.1 </td>
       <td style="text-align:right;width: 5em; "> -3.1 </td>
       <td style="text-align:right;width: 5em; "> 0.0021 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> speciesGentoo </td>
       <td style="text-align:right;width: 5em; "> 1014.6 </td>
       <td style="text-align:right;width: 5em; "> 129.6 </td>
       <td style="text-align:right;width: 5em; "> 7.8 </td>
       <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
      </tr>
    </tbody>
    </table>

    a.  Write the equation of the regression model.

    a.  Interpret each one of the slopes in this context.

    b.  Calculate the residual for a male Adelie penguin that weighs 3750 grams with the following body measurements: `bill_length_mm` = 39.1, `bill_depth_mm` = 18.7, `flipper_length_mm` = 181. Does the model overpredict or underpredict this penguin's weight?
    
    c.  The $R^2$ of this model is 87.5%. Interpret this value in context of the data and the model.
    
    \vspace{5mm}

1.  **Baby weights, backwards elimination.**
Let's consider a model that predicts `weight` of newborns using several predictors: whether the mother is considered `mature`, number of `weeks` of gestation, number of hospital `visits` during pregnancy, weight `gained` by the mother during pregnancy, `sex` of the baby, and whether the mother smoke cigarettes during pregnancy (`habit`). [@data:births14]

    
    
    The adjusted $R^2$ of the full model is 0.326.
    We remove each variable one by one, refit the model, and record the adjusted $R^2$.
    Which, if any, variable should be removed from the model?
    
    
    
    -  Drop `mature`:  0.321
    -  Drop `weeks`:   0.061
    -  Drop `visits`:  0.326
    -  Drop `gained`:  0.327
    -  Drop `sex`:     0.301
    
    \clearpage

1. **Palmer penguins, backwards elimination.**
The following full model is built to predict the weights of three species (*Adelie*, *Chinstrap*, or *Gentoo*) of penguins living in the Palmer Archipelago, Antarctica. [@palmerpenguins]

    <table class="table table-striped table-condensed" style="width: auto !important; margin-left: auto; margin-right: auto;">
     <thead>
      <tr>
       <th style="text-align:left;"> term </th>
       <th style="text-align:right;"> estimate </th>
       <th style="text-align:right;"> std.error </th>
       <th style="text-align:right;"> statistic </th>
       <th style="text-align:right;"> p.value </th>
      </tr>
     </thead>
    <tbody>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> (Intercept) </td>
       <td style="text-align:right;width: 5em; "> -1461.0 </td>
       <td style="text-align:right;width: 5em; "> 571.3 </td>
       <td style="text-align:right;width: 5em; "> -2.6 </td>
       <td style="text-align:right;width: 5em; "> 0.011 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> bill_length_mm </td>
       <td style="text-align:right;width: 5em; "> 18.2 </td>
       <td style="text-align:right;width: 5em; "> 7.1 </td>
       <td style="text-align:right;width: 5em; "> 2.6 </td>
       <td style="text-align:right;width: 5em; "> 0.0109 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> bill_depth_mm </td>
       <td style="text-align:right;width: 5em; "> 67.2 </td>
       <td style="text-align:right;width: 5em; "> 19.7 </td>
       <td style="text-align:right;width: 5em; "> 3.4 </td>
       <td style="text-align:right;width: 5em; "> 7e-04 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> flipper_length_mm </td>
       <td style="text-align:right;width: 5em; "> 16.0 </td>
       <td style="text-align:right;width: 5em; "> 2.9 </td>
       <td style="text-align:right;width: 5em; "> 5.5 </td>
       <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> sexmale </td>
       <td style="text-align:right;width: 5em; "> 389.9 </td>
       <td style="text-align:right;width: 5em; "> 47.8 </td>
       <td style="text-align:right;width: 5em; "> 8.1 </td>
       <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> speciesChinstrap </td>
       <td style="text-align:right;width: 5em; "> -251.5 </td>
       <td style="text-align:right;width: 5em; "> 81.1 </td>
       <td style="text-align:right;width: 5em; "> -3.1 </td>
       <td style="text-align:right;width: 5em; "> 0.0021 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 10em; font-family: monospace;"> speciesGentoo </td>
       <td style="text-align:right;width: 5em; "> 1014.6 </td>
       <td style="text-align:right;width: 5em; "> 129.6 </td>
       <td style="text-align:right;width: 5em; "> 7.8 </td>
       <td style="text-align:right;width: 5em; "> &lt;0.0001 </td>
      </tr>
    </tbody>
    </table>
    
    The adjusted $R^2$ of the full model is 0.9. In order to evaluate whether any of the predictors can be dropped from the model without losing predictive performance of the model, the researchers dropped one variable at a time, refit the model, and recorded the adjusted $R^2$ of the smaller model. These values are given below.
    
    
    
    -  Drop `bill_length_mm`:    0.87
    -  Drop `bill_depth_mm`:     0.869
    -  Drop `flipper_length_mm`: 0.861
    -  Drop `sex`:               0.845
    -  Drop `species`:           0.821

    Which, if any, variable should be removed from the model first?

1.  **Baby weights, forward selection.**
Using information on the mother and the sex of the baby (which can be determined prior to birth), we want to build a model that predicts the birth weight of babies.
In order to do so, we will evaluate six candidate predictors: whether the mother is considered `mature`, number of `weeks` of gestation, number of hospital `visits` during pregnancy, weight `gained` by the mother during pregnancy, `sex` of the baby, and whether the mother smoke cigarettes during pregnancy (`habit`).
And we will make a decision about including them in the model using forward selection and adjusted $R^2$. 
Below are the six models we evaluate and their adjusted $R^2$ values. [@data:births14]

    
    -  Predict `weight` from `mature`: 0.002
    -  Predict `weight` from `weeks`:  0.3
    -  Predict `weight` from `visits`: 0.034
    -  Predict `weight` from `gained`: 0.021
    -  Predict `weight` from `sex`:    0.018
    -  Predict `weight` from `habit`:  0.021

    Which variable should be added to the model first?

1. **Palmer penguins, forward selection.**
Using body measurement and other relevant data on three species (*Adelie*, *Chinstrap*, or *Gentoo*) of penguins living in the Palmer Archipelago, Antarctica, we want to predict their body mass. In order to do so, we will evaluate five candidate predictors and make a decision about including them in the model using forward selection and adjusted $R^2$. Below are the five models we evaluate and their adjusted $R^2$ values:

    
    -  Predict body mass from `bill_length_mm`:    0.352
    -  Predict body mass from `bill_depth_mm`:     0.22
    -  Predict body mass from `flipper_length_mm`: 0.758
    -  Predict body mass from `sex`:               0.178
    -  Predict body mass from `species`:           0.668

    Which variable should be added to the model first?
:::
