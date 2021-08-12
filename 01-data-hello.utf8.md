# (PART) Introduction to data {.unnumbered}



# Hello data {#data-hello}

::: {.chapterintro data-latex=""}
Scientists seek to answer questions using rigorous methods and careful observations.
These observations -- collected from the likes of field notes, surveys, and experiments -- form the backbone of a statistical investigation and are called **data**.
Statistics is the study of how best to collect, analyze, and draw conclusions from data.
In this first chapter, we focus on both the properties of data and on the collection of data.
:::



## Case study: Using stents to prevent strokes {#case-study-stents-strokes}

In this section we introduce a classic challenge in statistics: evaluating the efficacy of a medical treatment.
Terms in this section, and indeed much of this chapter, will all be revisited later in the text.
The plan for now is simply to get a sense of the role statistics can play in practice.

An experiment is designed to study the effectiveness of stents in treating patients at risk of stroke [@chimowitz2011stenting].
Stents are small mesh tubes that are placed inside narrow or weak arteries to assist in patient recovery after cardiac events and reduce the risk of an additional heart attack or death.

Many doctors have hoped that there would be similar benefits for patients at risk of stroke.
We start by writing the principal question the researchers hope to answer:

> Does the use of stents reduce the risk of stroke?

The researchers who asked this question conducted an experiment with 451 at-risk patients.
Each volunteer patient was randomly assigned to one of two groups:

-   **Treatment group**. Patients in the treatment group received a stent and medical management. The medical management included medications, management of risk factors, and help in lifestyle modification.
-   **Control group**. Patients in the control group received the same medical management as the treatment group, but they did not receive stents.

Researchers randomly assigned 224 patients to the treatment group and 227 to the control group.
In this study, the control group provides a reference point against which we can measure the medical impact of stents in the treatment group.

\clearpage

Researchers studied the effect of stents at two time points: 30 days after enrollment and 365 days after enrollment.
The results of 5 patients are summarized in Table \@ref(tab:stentStudyResultsDF).
Patient outcomes are recorded as `stroke` or `no event`, representing whether or not the patient had a stroke during that time period.

::: {.data data-latex=""}
The [`stent30`](http://openintrostat.github.io/openintro/reference/stent30.html) data and [`stent365`](http://openintrostat.github.io/openintro/reference/stent365.html) data can be found in the [**openintro**](http://openintrostat.github.io/openintro) R package.
:::

<table class="table table-striped table-condensed" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>(\#tab:stentStudyResultsDF)Results for five patients from the stent study.</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> patient </th>
   <th style="text-align:left;"> group </th>
   <th style="text-align:left;"> 30 days </th>
   <th style="text-align:left;"> 365 days </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;width: 8em; "> 1 </td>
   <td style="text-align:left;width: 8em; "> treatment </td>
   <td style="text-align:left;width: 8em; "> no event </td>
   <td style="text-align:left;width: 8em; "> no event </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 8em; "> 2 </td>
   <td style="text-align:left;width: 8em; "> treatment </td>
   <td style="text-align:left;width: 8em; "> stroke </td>
   <td style="text-align:left;width: 8em; "> stroke </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 8em; "> 3 </td>
   <td style="text-align:left;width: 8em; "> treatment </td>
   <td style="text-align:left;width: 8em; "> no event </td>
   <td style="text-align:left;width: 8em; "> no event </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 8em; "> 4 </td>
   <td style="text-align:left;width: 8em; "> treatment </td>
   <td style="text-align:left;width: 8em; "> no event </td>
   <td style="text-align:left;width: 8em; "> no event </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 8em; "> 5 </td>
   <td style="text-align:left;width: 8em; "> control </td>
   <td style="text-align:left;width: 8em; "> no event </td>
   <td style="text-align:left;width: 8em; "> no event </td>
  </tr>
</tbody>
</table>

It would be difficult to answer a question on the impact of stents on the occurrence of strokes for **all** of the study patients using these *individual* observations.
This question is better addressed by performing a statistical data analysis of *all* of the observations.
Table \@ref(tab:stentStudyResultsDFsummary) summarizes the raw data in a more helpful way.
In this table, we can quickly see what happened over the entire study.
For instance, to identify the number of patients in the treatment group who had a stroke within 30 days after the treatment, we look in the leftmost column (30 days), at the intersection of treatment and stroke: 33.
To identify the number of control patients who did not have a stroke after 365 days after receiving treatment, we look at the rightmost column (365 days), at the intersection of control and no event: 199.

<table class="table table-striped table-condensed" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:stentStudyResultsDFsummary)Descriptive statistics for the stent study.</caption>
 <thead>
<tr>
<th style="empty-cells: hide;border-bottom:hidden;" colspan="1"></th>
<th style="border-bottom:hidden;padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; border-bottom: 2px solid" colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">30 days</div></th>
<th style="border-bottom:hidden;padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; border-bottom: 2px solid" colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">365 days</div></th>
</tr>
  <tr>
   <th style="text-align:left;"> Group </th>
   <th style="text-align:right;"> Stroke </th>
   <th style="text-align:right;"> No event </th>
   <th style="text-align:right;"> Stroke </th>
   <th style="text-align:right;"> No event </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;border-top: 2px solid"> Control </td>
   <td style="text-align:right;border-top: 2px solid"> 13 </td>
   <td style="text-align:right;border-top: 2px solid"> 214 </td>
   <td style="text-align:right;border-top: 2px solid"> 28 </td>
   <td style="text-align:right;border-top: 2px solid"> 199 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:right;"> 33 </td>
   <td style="text-align:right;"> 191 </td>
   <td style="text-align:right;"> 45 </td>
   <td style="text-align:right;"> 179 </td>
  </tr>
  <tr>
   <td style="text-align:left;border-top: 2px solid"> Total </td>
   <td style="text-align:right;border-top: 2px solid"> 46 </td>
   <td style="text-align:right;border-top: 2px solid"> 405 </td>
   <td style="text-align:right;border-top: 2px solid"> 73 </td>
   <td style="text-align:right;border-top: 2px solid"> 378 </td>
  </tr>
</tbody>
</table>

::: {.guidedpractice data-latex=""}
Of the 224 patients in the treatment group, 45 had a stroke by the end of the first year.
Using these two numbers, compute the proportion of patients in the treatment group who had a stroke by the end of their first year.
(Note: answers to all Guided Practice exercises are provided in footnotes!)[^data-hello-1]
:::

[^data-hello-1]: The proportion of the 224 patients who had a stroke within 365 days: $45/224 = 0.20.$

We can compute summary statistics from the table to give us a better idea of how the impact of the stent treatment differed between the two groups.
A **summary statistic** is a single number summarizing data from a sample.
For instance, the primary results of the study after 1 year could be described by two summary statistics: the proportion of people who had a stroke in the treatment and control groups.



-   Proportion who had a stroke in the treatment (stent) group: $45/224 = 0.20 = 20\%.$
-   Proportion who had a stroke in the control group: $28/227 = 0.12 = 12\%.$

These two summary statistics are useful in looking for differences in the groups, and we are in for a surprise: an additional 8% of patients in the treatment group had a stroke!
This is important for two reasons.
First, it is contrary to what doctors expected, which was that stents would *reduce* the rate of strokes.
Second, it leads to a statistical question: do the data show a "real" difference between the groups?

This second question is subtle.
Suppose you flip a coin 100 times.
While the chance a coin lands heads in any given coin flip is 50%, we probably won't observe exactly 50 heads.
This type of variation is part of almost any type of data generating process.
It is possible that the 8% difference in the stent study is due to this natural variation.
However, the larger the difference we observe (for a particular sample size), the less believable it is that the difference is due to chance.
So what we are really asking is the following: if in fact stents have no effect, how likely is it that we observe such a large difference?

While we don't yet have statistical tools to fully address this question on our own, we can comprehend the conclusions of the published analysis: there was compelling evidence of harm by stents in this study of stroke patients.

**Be careful:** Do not generalize the results of this study to all patients and all stents.
This study looked at patients with very specific characteristics who volunteered to be a part of this study and who may not be representative of all stroke patients.
In addition, there are many types of stents and this study only considered the self-expanding Wingspan stent (Boston Scientific).
However, this study does leave us with an important lesson: we should keep our eyes open for surprises.

## Data basics {#data-basics}

Effective presentation and description of data is a first step in most analyses.
This section introduces one structure for organizing data as well as some terminology that will be used throughout this book.

### Observations, variables, and data matrices

Table \@ref(tab:loan50-df) displays six rows of a dataset for 50 randomly sampled loans offered through Lending Club, which is a peer-to-peer lending company.
This dataset will be referred to as `loan50`.

::: {.data data-latex=""}
The [`loan50`](http://openintrostat.github.io/openintro/reference/loans_full_schema.html) data can be found in the [**openintro**](http://openintrostat.github.io/openintro) R package.
:::

Each row in the table represents a single loan.
The formal name for a row is a \index{case}**case** or \index{unit of observation}**observational unit**.
The columns represent characteristics of each loan, where each column is referred to as a \index{variable}**variable**.
For example, the first row represents a loan of \$22,000 with an interest rate of 10.90%, where the borrower is based in New Jersey (NJ) and has an income of \$59,000.



::: {.guidedpractice data-latex=""}
What is the grade of the first loan in Table \@ref(tab:loan50-df)?
And what is the home ownership status of the borrower for that first loan?
Reminder: for these Guided Practice questions, you can check your answer in the footnote.[^data-hello-2]
:::

[^data-hello-2]: The loan's grade is B, and the borrower rents their residence.

In practice, it is especially important to ask clarifying questions to ensure important aspects of the data are understood.
For instance, it is always important to be sure we know what each variable means and its units of measurement.
Descriptions of the variables in the `loan50` dataset are given in Table \@ref(tab:loan-50-variables).

<table class="table table-striped table-condensed" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:loan50-df)Six observations from the `loan50` dataset</caption>
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;"> loan_amount </th>
   <th style="text-align:right;"> interest_rate </th>
   <th style="text-align:right;"> term </th>
   <th style="text-align:left;"> grade </th>
   <th style="text-align:left;"> state </th>
   <th style="text-align:right;"> total_income </th>
   <th style="text-align:left;"> homeownership </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:right;"> 22,000 </td>
   <td style="text-align:right;"> 10.90 </td>
   <td style="text-align:right;"> 60 </td>
   <td style="text-align:left;"> B </td>
   <td style="text-align:left;"> NJ </td>
   <td style="text-align:right;"> 59,000 </td>
   <td style="text-align:left;"> rent </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:right;"> 6,000 </td>
   <td style="text-align:right;"> 9.92 </td>
   <td style="text-align:right;"> 36 </td>
   <td style="text-align:left;"> B </td>
   <td style="text-align:left;"> CA </td>
   <td style="text-align:right;"> 60,000 </td>
   <td style="text-align:left;"> rent </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 3 </td>
   <td style="text-align:right;"> 25,000 </td>
   <td style="text-align:right;"> 26.30 </td>
   <td style="text-align:right;"> 36 </td>
   <td style="text-align:left;"> E </td>
   <td style="text-align:left;"> SC </td>
   <td style="text-align:right;"> 75,000 </td>
   <td style="text-align:left;"> mortgage </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 4 </td>
   <td style="text-align:right;"> 6,000 </td>
   <td style="text-align:right;"> 9.92 </td>
   <td style="text-align:right;"> 36 </td>
   <td style="text-align:left;"> B </td>
   <td style="text-align:left;"> CA </td>
   <td style="text-align:right;"> 75,000 </td>
   <td style="text-align:left;"> rent </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 5 </td>
   <td style="text-align:right;"> 25,000 </td>
   <td style="text-align:right;"> 9.43 </td>
   <td style="text-align:right;"> 60 </td>
   <td style="text-align:left;"> B </td>
   <td style="text-align:left;"> OH </td>
   <td style="text-align:right;"> 254,000 </td>
   <td style="text-align:left;"> mortgage </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 6 </td>
   <td style="text-align:right;"> 6,400 </td>
   <td style="text-align:right;"> 9.92 </td>
   <td style="text-align:right;"> 36 </td>
   <td style="text-align:left;"> B </td>
   <td style="text-align:left;"> IN </td>
   <td style="text-align:right;"> 67,000 </td>
   <td style="text-align:left;"> mortgage </td>
  </tr>
</tbody>
</table>

<table class="table table-striped table-condensed" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:loan-50-variables)Variables and their descriptions for the `loan50` dataset.</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Variable </th>
   <th style="text-align:left;"> Description </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;font-family: monospace;"> loan_amount </td>
   <td style="text-align:left;width: 30em; "> Amount of the loan received, in US dollars. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> interest_rate </td>
   <td style="text-align:left;width: 30em; "> Interest rate on the loan, in an annual percentage. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> term </td>
   <td style="text-align:left;width: 30em; "> The length of the loan, which is always set as a whole number of months. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> grade </td>
   <td style="text-align:left;width: 30em; "> Loan grade, which takes a values A through G and represents the quality of the loan and its likelihood of being repaid. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> state </td>
   <td style="text-align:left;width: 30em; "> US state where the borrower resides. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> total_income </td>
   <td style="text-align:left;width: 30em; "> Borrower's total income, including any second income, in US dollars. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> homeownership </td>
   <td style="text-align:left;width: 30em; "> Indicates whether the person owns, owns but has a mortgage, or rents. </td>
  </tr>
</tbody>
</table>

The data in Table \@ref(tab:loan50-df) represent a \index{data frame}**data frame**, which is a convenient and common way to organize data, especially if collecting data in a spreadsheet.
A data frame where each row is a unique case (observational unit), each column is a variable, and each cell is a single value is commonly referred to as \index{tidy data}**tidy data** @wickham2014.



When recording data, use a tidy data frame unless you have a very good reason to use a different structure.
This structure allows new cases to be added as rows or new variables as new columns and facilitates visualization, summarization, and other statistical analyses.

::: {.guidedpractice data-latex=""}
The grades for assignments, quizzes, and exams in a course are often recorded in a gradebook that takes the form of a data frame.
How might you organize a course's grade data using a data frame?
Describe the observational units and variables.[^data-hello-3]
:::

[^data-hello-3]: There are multiple strategies that can be followed.
    One common strategy is to have each student represented by a row, and then add a column for each assignment, quiz, or exam.
    Under this setup, it is easy to review a single line to understand the grade history of a student.
    There should also be columns to include student information, such as one column to list student names.

::: {.guidedpractice data-latex=""}
We consider data for 3,142 counties in the United States, which includes the name of each county, the state where it resides, its population in 2017, the population change from 2010 to 2017, poverty rate, and nine additional characteristics.
How might these data be organized in a data frame?[^data-hello-4]
:::

[^data-hello-4]: Each county may be viewed as a case, and there are eleven pieces of information recorded for each case.
    A table with 3,142 rows and 14 columns could hold these data, where each row represents a county and each column represents a particular piece of information.

\clearpage

The data described in the Guided Practice above represents the `county` dataset, which is shown as a data frame in Table \@ref(tab:county-df).
The variables as well as the variables in the dataset that did not fit in Table \@ref(tab:county-df) are described in Table \@ref(tab:county-variables).

<table class="table table-striped table-condensed" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:county-df)Six observations and six variables from the `county` dataset.</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> name </th>
   <th style="text-align:left;"> state </th>
   <th style="text-align:right;"> pop2017 </th>
   <th style="text-align:right;"> pop_change </th>
   <th style="text-align:right;"> unemployment_rate </th>
   <th style="text-align:left;"> median_edu </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Autauga County </td>
   <td style="text-align:left;"> Alabama </td>
   <td style="text-align:right;"> 55,504 </td>
   <td style="text-align:right;"> 1.48 </td>
   <td style="text-align:right;"> 3.86 </td>
   <td style="text-align:left;"> some_college </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Baldwin County </td>
   <td style="text-align:left;"> Alabama </td>
   <td style="text-align:right;"> 212,628 </td>
   <td style="text-align:right;"> 9.19 </td>
   <td style="text-align:right;"> 3.99 </td>
   <td style="text-align:left;"> some_college </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Barbour County </td>
   <td style="text-align:left;"> Alabama </td>
   <td style="text-align:right;"> 25,270 </td>
   <td style="text-align:right;"> -6.22 </td>
   <td style="text-align:right;"> 5.90 </td>
   <td style="text-align:left;"> hs_diploma </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bibb County </td>
   <td style="text-align:left;"> Alabama </td>
   <td style="text-align:right;"> 22,668 </td>
   <td style="text-align:right;"> 0.73 </td>
   <td style="text-align:right;"> 4.39 </td>
   <td style="text-align:left;"> hs_diploma </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Blount County </td>
   <td style="text-align:left;"> Alabama </td>
   <td style="text-align:right;"> 58,013 </td>
   <td style="text-align:right;"> 0.68 </td>
   <td style="text-align:right;"> 4.02 </td>
   <td style="text-align:left;"> hs_diploma </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Bullock County </td>
   <td style="text-align:left;"> Alabama </td>
   <td style="text-align:right;"> 10,309 </td>
   <td style="text-align:right;"> -2.28 </td>
   <td style="text-align:right;"> 4.93 </td>
   <td style="text-align:left;"> hs_diploma </td>
  </tr>
</tbody>
</table>

<table class="table table-striped table-condensed" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:county-variables)Variables and their descriptions for the `county` dataset.</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Variable </th>
   <th style="text-align:left;"> Description </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;font-family: monospace;"> name </td>
   <td style="text-align:left;width: 30em; "> Name of county. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> state </td>
   <td style="text-align:left;width: 30em; "> Name of state. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> pop2000 </td>
   <td style="text-align:left;width: 30em; "> Population in 2000. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> pop2010 </td>
   <td style="text-align:left;width: 30em; "> Population in 2010. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> pop2017 </td>
   <td style="text-align:left;width: 30em; "> Population in 2017. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> pop_change </td>
   <td style="text-align:left;width: 30em; "> Population change from 2010 to 2017 (in percent). </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> poverty </td>
   <td style="text-align:left;width: 30em; "> Percent of population in poverty in 2017. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> homeownership </td>
   <td style="text-align:left;width: 30em; "> Homeownership rate, 2006-2010. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> multi_unit </td>
   <td style="text-align:left;width: 30em; "> Multi-unit rate: percent of housing units that are in multi-unit structures, 2006-2010. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> unemployment_rate </td>
   <td style="text-align:left;width: 30em; "> Unemployment rate in 2017. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> metro </td>
   <td style="text-align:left;width: 30em; "> Whether the county contains a metropolitan area, taking one of the values yes or no. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> median_edu </td>
   <td style="text-align:left;width: 30em; "> Median education level (2013-2017), taking one of the values below_hs, hs_diploma, some_college, or bachelors. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> per_capita_income </td>
   <td style="text-align:left;width: 30em; "> Per capita (per person) income (2013-2017). </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> median_hh_income </td>
   <td style="text-align:left;width: 30em; "> Median household income. </td>
  </tr>
  <tr>
   <td style="text-align:left;font-family: monospace;"> smoking_ban </td>
   <td style="text-align:left;width: 30em; "> Describes the type of county-level smoking ban in place in 2010, taking one of the values none, partial, or comprehensive. </td>
  </tr>
</tbody>
</table>

::: {.data data-latex=""}
The [`county`](http://openintrostat.github.io/usdata/reference/county.html) data can be found in the [**usdata**](http://openintrostat.github.io/usdata) R package.
:::

### Types of variables {#variable-types}

Examine the `unemployment_rate`, `pop2017`, `state`, and `median_edu` variables in the `county` dataset.
Each of these variables is inherently different from the other three, yet some share certain characteristics.

First consider `unemployment_rate`, which is said to be a \index{numerical variable}**numerical** variable since it can take a wide range of numerical values, and it is sensible to add, subtract, or take averages with those values.
On the other hand, we would not classify a variable reporting telephone area codes as numerical since the average, sum, and difference of area codes doesn't have any clear meaning.
Instead, we would consider area codes as a categorical variable.



The `pop2017` variable is also numerical, although it seems to be a little different than `unemployment_rate`.
This variable of the population count can only take whole non-negative numbers (0, 1, 2, ...).
For this reason, the population variable is said to be **discrete** since it can only take numerical values with jumps.
On the other hand, the unemployment rate variable is said to be **continuous**.



The variable `state` can take up to 51 values after accounting for Washington, DC: AL, AK, ..., and WY.
Because the responses themselves are categories, `state` is called a **categorical** variable, and the possible values (states) are called the variable's **levels** (e.g., DC, AL, AK, etc.) .



Finally, consider the `median_edu` variable, which describes the median education level of county residents and takes values `below_hs`, `hs_diploma`, `some_college`, or `bachelors` in each county.
This variable seems to be a hybrid: it is a categorical variable but the levels have a natural ordering.
A variable with these properties is called an **ordinal** variable, while a regular categorical variable without this type of special ordering is called a **nominal** variable.
To simplify analyses, any categorical variable in this book will be treated as a nominal (unordered) categorical variable.



<div class="figure" style="text-align: center">
<img src="01-data-hello_files/figure-html/variables-1.png" alt="Types of variables are broken down into numerical (which can be discrete or continuous) and categorical (which can be ordinal or nominal)." width="90%" />
<p class="caption">(\#fig:variables)Breakdown of variables into their respective types.</p>
</div>

::: {.workedexample data-latex=""}
Data were collected about students in a statistics course.
Three variables were recorded for each student: number of siblings, student height, and whether the student had previously taken a statistics course.
Classify each of the variables as continuous numerical, discrete numerical, or categorical.

------------------------------------------------------------------------

The number of siblings and student height represent numerical variables.
Because the number of siblings is a count, it is discrete.
Height varies continuously, so it is a continuous numerical variable.
The last variable classifies students into two categories -- those who have and those who have not taken a statistics course -- which makes this variable categorical.
:::

::: {.guidedpractice data-latex=""}
An experiment is evaluating the effectiveness of a new drug in treating migraines.
A `group` variable is used to indicate the experiment group for each patient: treatment or control.
The `num_migraines` variable represents the number of migraines the patient experienced during a 3-month period.
Classify each variable as either numerical or categorical?[^data-hello-5]
:::

[^data-hello-5]: The `group` variable can take just one of two group names, making it categorical.
    The `num_migraines` variable describes a count of the number of migraines, which is an outcome where basic arithmetic is sensible, which means this is a numerical outcome; more specifically, since it represents a count, `num_migraines` is a discrete numerical variable.

### Relationships between variables {#variable-relations}

Many analyses are motivated by a researcher looking for a relationship between two or more variables.
A social scientist may like to answer some of the following questions:

> Does a higher than average increase in county population tend to correspond to counties with higher or lower median household incomes?

> If homeownership in one county is lower than the national average, will the percent of housing units that are in multi-unit structures in that county tend to be above or below the national average?

> How much can the median education level explain the median household income for counties in the US?

To answer these questions, data must be collected, such as the `county` dataset shown in Table \@ref(tab:county-df).
Examining \index{summary statistic}**summary statistics** can provide numerical insights about the specifics of each of these questions.
Alternatively, graphs can be used to visually explore the data, potentially providing more insight than a summary statistic.

\index{scatterplot}**Scatterplots** are one type of graph used to study the relationship between two numerical variables.
Figure \@ref(fig:county-multi-unit-homeownership) displays the relationship between the variables `homeownership` and `multi_unit`, which is the percent of housing units that are in multi-unit structures (e.g., apartments, condos).
Each point on the plot represents a single county.
For instance, the highlighted dot corresponds to County 413 in the `county` dataset: Chattahoochee County, Georgia, which has 39.4% of housing units that are in multi-unit structures and a homeownership rate of 31.3%.
The scatterplot suggests a relationship between the two variables: counties with a higher rate of housing units that are in multi-unit structures tend to have lower homeownership rates.
We might brainstorm as to why this relationship exists and investigate each idea to determine which are the most reasonable explanations.

<div class="figure" style="text-align: center">
<img src="01-data-hello_files/figure-html/county-multi-unit-homeownership-1.png" alt="A scatterplot of homeownership versus the percent of housing units that are in multi-unit structures for US counties. The highlighted dot represents Chattahoochee County, Georgia, which has a multi-unit rate of 39.4\% and a homeownership rate of 31.3\%." width="90%" />
<p class="caption">(\#fig:county-multi-unit-homeownership)A scatterplot of homeownership versus the percent of housing units that are in multi-unit structures for US counties. The highlighted dot represents Chattahoochee County, Georgia, which has a multi-unit rate of 39.4\% and a homeownership rate of 31.3\%.</p>
</div>

The multi-unit and homeownership rates are said to be associated because the plot shows a discernible pattern.
When two variables show some connection with one another, they are called **associated** variables.



::: {.guidedpractice data-latex=""}
Examine the variables in the `loan50` dataset, which are described in Table \@ref(tab:loan-50-variables).
Create two questions about possible relationships between variables in `loan50` that are of interest to you.[^data-hello-6]
:::

[^data-hello-6]: Two example questions: (1) What is the relationship between loan amount and total income?
    (2) If someone's income is above the average, will their interest rate tend to be above or below the average?

::: {.workedexample data-latex=""}
This example examines the relationship between the percent change in population from 2010 to 2017 and median household income for counties, which is visualized as a scatterplot in Figure \@ref(fig:county-pop-change-med-hh-income).
Are these variables associated?

------------------------------------------------------------------------

The larger the median household income for a county, the higher the population growth observed for the county.
While it isn't true that every county with a higher median household income has a higher population growth, the trend in the plot is evident.
Since there is some relationship between the variables, they are associated.
:::

<div class="figure" style="text-align: center">
<img src="01-data-hello_files/figure-html/county-pop-change-med-hh-income-1.png" alt="A scatterplot showing population change against median household income. Owsley County of Kentucky, is highlighted, which lost 3.63\% of its population from 2010 to 2017 and had median household income of \$22,736." width="90%" />
<p class="caption">(\#fig:county-pop-change-med-hh-income)A scatterplot showing population change against median household income. Owsley County of Kentucky, is highlighted, which lost 3.63\% of its population from 2010 to 2017 and had median household income of \$22,736.</p>
</div>

Because there is a downward trend in Figure \@ref(fig:county-multi-unit-homeownership) -- counties with more housing units that are in multi-unit structures are associated with lower homeownership -- these variables are said to be **negatively associated**.
A **positive association** is shown in the relationship between the `median_hh_income` and `pop_change` variables in Figure \@ref(fig:county-pop-change-med-hh-income), where counties with higher median household income tend to have higher rates of population growth.



If two variables are not associated, then they are said to be **independent**.
That is, two variables are independent if there is no evident relationship between the two.



::: {.important data-latex=""}
**Associated or independent, not both.**

A pair of variables are either related in some way (associated) or not (independent).
No pair of variables is both associated and independent.
:::

### Explanatory and response variables

When we ask questions about the relationship between two variables, we sometimes also want to determine if the change in one variable causes a change in the other.
Consider the following rephrasing of an earlier question about the `county` dataset:

> If there is an increase in the median household income in a county, does this drive an increase in its population?

In this question, we are asking whether one variable affects another.
If this is our underlying belief, then *median household income* is the **explanatory variable** and the *population change* is the **response variable** in the hypothesized relationship.[^data-hello-7]

[^data-hello-7]: In some disciplines, it's customary to refer to the explanatory variable as the **independent variable** and the response variable as the **dependent variable**.
    However, this becomes confusing since a *pair* of variables might be independent or dependent, so we avoid this language.



::: {.important data-latex=""}
**Explanatory and response variables.**

When we suspect one variable might causally affect another, we label the first variable the explanatory variable and the second the response variable.
We also use the terms **explanatory** and **response** to describe variables where the **response** might be predicted using the **explanatory** even if there is no causal relationship.

<center>

explanatory variable $\rightarrow$ *might affect* $\rightarrow$ response variable

</center>

<br> For many pairs of variables, there is no hypothesized relationship, and these labels would not be applied to either variable in such cases.
:::

Bear in mind that the act of labeling the variables in this way does nothing to guarantee that a causal relationship exists.
A formal evaluation to check whether one variable causes a change in another requires an experiment.

### Observational studies and experiments

There are two primary types of data collection: experiments and observational studies.

When researchers want to evaluate the effect of particular traits, treatments, or conditions, they conduct an **experiment**.
For instance, we may suspect drinking a high-calorie energy drink will improve performance in a race.
To check if there really is a causal relationship between the explanatory variable (whether the runner drank an energy drink or not) and the response variable (the race time), researchers identify a sample of individuals and split them into groups.
The individuals in each group are *assigned* a treatment.
When individuals are randomly assigned to a group, the experiment is called a **randomized experiment**.
Random assignment organizes the participants in a study into groups that are roughly equal on all aspects, thus allowing us to control for any confounding variables that might affect the outcome (e.g., fitness level, racing experience, etc.).
For example, each runner in the experiment could be randomly assigned, perhaps by flipping a coin, into one of two groups: the first group receives a **placebo** (fake treatment, in this case a no-calorie drink) and the second group receives the high-calorie energy drink.
See the case study in Section \@ref(case-study-stents-strokes) for another example of an experiment, though that study did not employ a placebo.



Researchers perform an **observational study** when they collect data in a way that does not directly interfere with how the data arise.
For instance, researchers may collect information via surveys, review medical or company records, or follow a **cohort** of many similar individuals to form hypotheses about why certain diseases might develop.
In each of these situations, researchers merely observe the data that arise.
In general, observational studies can provide evidence of a naturally occurring association between variables, but they cannot by themselves show a causal connection as they don't offer a mechanism for controlling for confounding variables.



::: {.important data-latex=""}
**Association** $\neq$ **Causation.**

In general, association does not imply causation.
An advantage of a randomized experiment is that it is easier to establish causal relationships with such a study.
The main reason for this is that observational studies do not control for confounding variables, and hence establishing causal relationships with observational studies requires advanced statistical methods (that are beyond the scope of this book).
We will revisit this idea when we discuss experiments later in the book.
:::

\vspace{10mm}

## Chapter review {#chp1-review}

### Summary

This chapter introduced you to the world of data.
Data can be organized in many ways but tidy data, where each row represents an observation and each column represents a variable, lends itself most easily to statistical analysis.
Many of the ideas from this chapter will be seen as we move on to doing full data analyses.
In the next chapter you're going to learn about how we can design studies to collect the data we need to make conclusions with the desired scope of inference.

### Terms

We introduced the following terms in the chapter.
If you're not sure what some of these terms mean, we recommend you go back in the text and review their definitions.
We are purposefully presenting them in alphabetical order, instead of in order of appearance, so they will be a little more challenging to locate.
However you should be able to easily spot them as **bolded text**.

<table class="table table-striped table-condensed" style="margin-left: auto; margin-right: auto;">
<tbody>
  <tr>
   <td style="text-align:left;"> associated </td>
   <td style="text-align:left;"> experiment </td>
   <td style="text-align:left;"> ordinal </td>
  </tr>
  <tr>
   <td style="text-align:left;"> case </td>
   <td style="text-align:left;"> explanatory variable </td>
   <td style="text-align:left;"> placebo </td>
  </tr>
  <tr>
   <td style="text-align:left;"> categorical </td>
   <td style="text-align:left;"> independent </td>
   <td style="text-align:left;"> positive association </td>
  </tr>
  <tr>
   <td style="text-align:left;"> cohort </td>
   <td style="text-align:left;"> level </td>
   <td style="text-align:left;"> randomized experiment </td>
  </tr>
  <tr>
   <td style="text-align:left;"> continuous </td>
   <td style="text-align:left;"> negative association </td>
   <td style="text-align:left;"> response variable </td>
  </tr>
  <tr>
   <td style="text-align:left;"> data </td>
   <td style="text-align:left;"> nominal </td>
   <td style="text-align:left;"> summary statistic </td>
  </tr>
  <tr>
   <td style="text-align:left;"> data frame </td>
   <td style="text-align:left;"> numerical </td>
   <td style="text-align:left;"> tidy data </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dependent </td>
   <td style="text-align:left;"> observational study </td>
   <td style="text-align:left;"> variable </td>
  </tr>
  <tr>
   <td style="text-align:left;"> discrete </td>
   <td style="text-align:left;"> observational unit </td>
   <td style="text-align:left;">  </td>
  </tr>
</tbody>
</table>

\clearpage

## Exercises {#chp1-exercises}

Answers to odd numbered exercises can be found in Appendix \@ref(exercise-solutions-01).

::: {.exercises data-latex=""}

1.  **Marvel Cinematic Universe films.**
The data frame below contains information on Marvel Cinematic Universe films through the Infinity saga (a movie storyline spanning from Ironman in 2008 to Endgame in 2019). 
Box office totals are given in millions of US Dollars.
How many observations and how many variables does this data frame have?^[The [`mcu_films`](http://openintrostat.github.io/openintro/reference/mcu_films.html) data used in this exercise can be found in the [**openintro**](http://openintrostat.github.io/openintro) R package.]

    <table class="table table-striped table-condensed" style="margin-left: auto; margin-right: auto;">
     <thead>
    <tr>
    <th style="empty-cells: hide;border-bottom:hidden;" colspan="2"></th>
    <th style="border-bottom:hidden;padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">Length</div></th>
    <th style="empty-cells: hide;border-bottom:hidden;" colspan="2"></th>
    <th style="border-bottom:hidden;padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">Gross</div></th>
    </tr>
      <tr>
       <th style="text-align:center;">   </th>
       <th style="text-align:left;"> Title </th>
       <th style="text-align:center;"> Hrs </th>
       <th style="text-align:center;"> Mins </th>
       <th style="text-align:center;"> Release Date </th>
       <th style="text-align:center;"> Opening Wknd US </th>
       <th style="text-align:center;"> US </th>
       <th style="text-align:center;"> World </th>
      </tr>
     </thead>
    <tbody>
      <tr>
       <td style="text-align:center;"> 1 </td>
       <td style="text-align:left;width: 10em; "> Iron Man </td>
       <td style="text-align:center;"> 2 </td>
       <td style="text-align:center;"> 6 </td>
       <td style="text-align:center;"> 5/2/2008 </td>
       <td style="text-align:center;"> 98.62 </td>
       <td style="text-align:center;"> 319.03 </td>
       <td style="text-align:center;"> 585.8 </td>
      </tr>
      <tr>
       <td style="text-align:center;"> 2 </td>
       <td style="text-align:left;width: 10em; "> The Incredible Hulk </td>
       <td style="text-align:center;"> 1 </td>
       <td style="text-align:center;"> 52 </td>
       <td style="text-align:center;"> 6/12/2008 </td>
       <td style="text-align:center;"> 55.41 </td>
       <td style="text-align:center;"> 134.81 </td>
       <td style="text-align:center;"> 264.77 </td>
      </tr>
      <tr>
       <td style="text-align:center;"> 3 </td>
       <td style="text-align:left;width: 10em; "> Iron Man 2 </td>
       <td style="text-align:center;"> 2 </td>
       <td style="text-align:center;"> 4 </td>
       <td style="text-align:center;"> 5/7/2010 </td>
       <td style="text-align:center;"> 128.12 </td>
       <td style="text-align:center;"> 312.43 </td>
       <td style="text-align:center;"> 623.93 </td>
      </tr>
      <tr>
       <td style="text-align:center;"> 4 </td>
       <td style="text-align:left;width: 10em; "> Thor </td>
       <td style="text-align:center;"> 1 </td>
       <td style="text-align:center;"> 55 </td>
       <td style="text-align:center;"> 5/6/2011 </td>
       <td style="text-align:center;"> 65.72 </td>
       <td style="text-align:center;"> 181.03 </td>
       <td style="text-align:center;"> 449.33 </td>
      </tr>
      <tr>
       <td style="text-align:center;"> 5 </td>
       <td style="text-align:left;width: 10em; "> Captain America: The First Avenger </td>
       <td style="text-align:center;"> 2 </td>
       <td style="text-align:center;"> 4 </td>
       <td style="text-align:center;"> 7/22/2011 </td>
       <td style="text-align:center;"> 65.06 </td>
       <td style="text-align:center;"> 176.65 </td>
       <td style="text-align:center;"> 370.57 </td>
      </tr>
      <tr>
       <td style="text-align:center;"> ... </td>
       <td style="text-align:left;width: 10em; "> ... </td>
       <td style="text-align:center;"> ... </td>
       <td style="text-align:center;"> ... </td>
       <td style="text-align:center;"> ... </td>
       <td style="text-align:center;"> ... </td>
       <td style="text-align:center;"> ... </td>
       <td style="text-align:center;"> ... </td>
      </tr>
      <tr>
       <td style="text-align:center;"> 23 </td>
       <td style="text-align:left;width: 10em; "> Spiderman: Far from Home </td>
       <td style="text-align:center;"> 2 </td>
       <td style="text-align:center;"> 9 </td>
       <td style="text-align:center;"> 7/2/2019 </td>
       <td style="text-align:center;"> 92.58 </td>
       <td style="text-align:center;"> 390.53 </td>
       <td style="text-align:center;"> 1131.93 </td>
      </tr>
    </tbody>
    </table>

1. **Cherry Blossom Run.**
The data frame below contains information on runners in the 2017 Cherry Blossom Run, which is an annual road race that takes place in Washington, DC.
Most runners participate in a 10-mile run while a smaller fraction take part in a 5k run or walk.
How many observations and how many variables does this data frame have?^[The [`run17`](http://openintrostat.github.io/openintro/reference/run17.html) data used in this exercise can be found in the [**cherryblossom**](http://openintrostat.github.io/cherryblossom) R package.]

    <table class="table table-striped table-condensed" style="margin-left: auto; margin-right: auto;">
     <thead>
    <tr>
    <th style="empty-cells: hide;border-bottom:hidden;" colspan="6"></th>
    <th style="border-bottom:hidden;padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">Time</div></th>
    <th style="empty-cells: hide;border-bottom:hidden;" colspan="2"></th>
    </tr>
      <tr>
       <th style="text-align:left;">   </th>
       <th style="text-align:left;"> Bib </th>
       <th style="text-align:left;"> Name </th>
       <th style="text-align:left;"> Sex </th>
       <th style="text-align:center;"> Age </th>
       <th style="text-align:left;"> City / Country </th>
       <th style="text-align:center;"> Net </th>
       <th style="text-align:center;"> Clock </th>
       <th style="text-align:center;"> Pace </th>
       <th style="text-align:left;"> Event </th>
      </tr>
     </thead>
    <tbody>
      <tr>
       <td style="text-align:left;"> 1 </td>
       <td style="text-align:left;"> 6 </td>
       <td style="text-align:left;"> Hiwot G. </td>
       <td style="text-align:left;"> F </td>
       <td style="text-align:center;"> 21 </td>
       <td style="text-align:left;"> Ethiopia </td>
       <td style="text-align:center;"> 3217 </td>
       <td style="text-align:center;"> 3217 </td>
       <td style="text-align:center;"> 321 </td>
       <td style="text-align:left;"> 10 Mile </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 2 </td>
       <td style="text-align:left;"> 22 </td>
       <td style="text-align:left;"> Buze D. </td>
       <td style="text-align:left;"> F </td>
       <td style="text-align:center;"> 22 </td>
       <td style="text-align:left;"> Ethiopia </td>
       <td style="text-align:center;"> 3232 </td>
       <td style="text-align:center;"> 3232 </td>
       <td style="text-align:center;"> 323 </td>
       <td style="text-align:left;"> 10 Mile </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 3 </td>
       <td style="text-align:left;"> 16 </td>
       <td style="text-align:left;"> Gladys K. </td>
       <td style="text-align:left;"> F </td>
       <td style="text-align:center;"> 31 </td>
       <td style="text-align:left;"> Kenya </td>
       <td style="text-align:center;"> 3276 </td>
       <td style="text-align:center;"> 3276 </td>
       <td style="text-align:center;"> 327 </td>
       <td style="text-align:left;"> 10 Mile </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 4 </td>
       <td style="text-align:left;"> 4 </td>
       <td style="text-align:left;"> Mamitu D. </td>
       <td style="text-align:left;"> F </td>
       <td style="text-align:center;"> 33 </td>
       <td style="text-align:left;"> Ethiopia </td>
       <td style="text-align:center;"> 3285 </td>
       <td style="text-align:center;"> 3285 </td>
       <td style="text-align:center;"> 328 </td>
       <td style="text-align:left;"> 10 Mile </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 5 </td>
       <td style="text-align:left;"> 20 </td>
       <td style="text-align:left;"> Karolina N. </td>
       <td style="text-align:left;"> F </td>
       <td style="text-align:center;"> 35 </td>
       <td style="text-align:left;"> Poland </td>
       <td style="text-align:center;"> 3288 </td>
       <td style="text-align:center;"> 3288 </td>
       <td style="text-align:center;"> 328 </td>
       <td style="text-align:left;"> 10 Mile </td>
      </tr>
      <tr>
       <td style="text-align:left;"> ... </td>
       <td style="text-align:left;"> ... </td>
       <td style="text-align:left;"> ... </td>
       <td style="text-align:left;"> ... </td>
       <td style="text-align:center;"> ... </td>
       <td style="text-align:left;"> ... </td>
       <td style="text-align:center;"> ... </td>
       <td style="text-align:center;"> ... </td>
       <td style="text-align:center;"> ... </td>
       <td style="text-align:left;"> ... </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 19961 </td>
       <td style="text-align:left;"> 25153 </td>
       <td style="text-align:left;"> Andres E. </td>
       <td style="text-align:left;"> M </td>
       <td style="text-align:center;"> 33 </td>
       <td style="text-align:left;"> Woodbridge, VA </td>
       <td style="text-align:center;"> 5287 </td>
       <td style="text-align:center;"> 5334 </td>
       <td style="text-align:center;"> 1700 </td>
       <td style="text-align:left;"> 5K </td>
      </tr>
    </tbody>
    </table>

1.  **Air pollution and birth outcomes, study components.** 
Researchers collected data to examine the relationship between air pollutants and preterm births in Southern California.
During the study air pollution levels were measured by air quality monitoring stations.
Specifically, levels of carbon monoxide were recorded in parts per million, nitrogen dioxide and ozone in parts per hundred million, and coarse particulate matter (PM$_{10}$) in $\mu g/m^3$.
Length of gestation data were collected on 143,196 births between the years 1989 and 1993, and air pollution exposure during gestation was calculated for each birth.
The analysis suggested that increased ambient PM$_{10}$ and, to a lesser degree, CO concentrations may be associated with the occurrence of preterm births. [@Ritz+Yu+Chapa+Fruin:2000]

    a.  Identify the main research question of the study.

    b.  Who are the subjects in this study, and how many are included?

    c.  What are the variables in the study? Identify each variable as numerical or categorical. If numerical, state whether the variable is discrete or continuous. If categorical, state whether the variable is ordinal.

1.  **Cheaters, study components.** 
Researchers studying the relationship between honesty, age and self-control conducted an experiment on 160 children between the ages of 5 and 15. Participants reported their age, sex, and whether they were an only child or not. The researchers asked each child to toss a fair coin in private and to record the outcome (white or black) on a paper sheet, and said they would only reward children who report white. [@Bucciol:2011]

    a.  Identify the main research question of the study.

    b.  Who are the subjects in this study, and how many are included?

    c.  The study's findings can be summarized as follows: *"Half the students were explicitly told not to cheat and the others were not given any explicit instructions. In the no instruction group probability of cheating was found to be uniform across groups based on child's characteristics. In the group that was explicitly told to not cheat, girls were less likely to cheat, and while rate of cheating didn't vary by age for boys, it decreased with age for girls."* How many variables were recorded for each subject in the study in order to conclude these findings? State the variables and their types.

1. **Gamification and statistics, study components.** 
Gamification is the application of game-design elements and game principles in non-game contexts. 
In educational settings, gamification is often implemented as educational activities to solve problems by using characteristics of game elements.
Researchers investigating the effects of gamification on learning statistics conducted a study where they split college students in a statistics class into four groups: (1) no reading exercises and no gamification, (2) reading exercises but no gamification, (3) gamification but no reading exercises, and (4) gamification and reading exercises.
Students in all groups also attended lectures. 
Students in the class were from two majors: Electrical and Computer Engineering (n = 279) and Business Administration (n = 86). 
After their assigned learning experience, each student took a final evaluation comprised of 30 multiple choice question and their score was measured as the number of questions they answered correctly.
The researchers considered students' gender, level of studies (first through fourth year) and academic major. 
Other variables considered were expertise in the English language and use of personal computers and games, both of which were measured on a scale of 1 (beginner) to 5 (proficient). 
The study found that gamification had a positive effect on student learning compared to traditional teaching methods involving lectures and reading exercises.
They also found that the effect was larger for females and Engineering students. [@Legaki:2020]

    a.  Identify the main research question of the study.

    b.  Who were the subjects in this study, and how many were included?

    c.  What are the variables in the study? Identify each variable as numerical or categorical. If numerical, state whether the variable is discrete or continuous. If categorical, state whether the variable is ordinal.

1.  **Stealers, study components.** 
In a study of the relationship between socio-economic class and unethical behavior, 129 University of California undergraduates at Berkeley were asked to identify themselves as having low or high social-class by comparing themselves to others with the most (least) money, most (least) education, and most (least) respected jobs.
They were also presented with a jar of individually wrapped candies and informed that the candies were for children in a nearby laboratory, but that they could take some if they wanted.
After completing some unrelated tasks, participants reported the number of candies they had taken. [@Piff:2012]

    a.  Identify the main research question of the study.

    b.  Who were the subjects in this study, and how many were included?

    c.  The study found that students who were identified as upper-class took more candy than others. How many variables were recorded for each subject in the study in order to conclude these findings? State the variables and their types. 
    
    \clearpage

1. <img src="exercises/images/earacupuncture.png" alt="Figure from the original paper displaying the appropriate area (M) versus the inappropriate area (S) used in the treatment of migraine attacks." class="cover" width="30%"/> **Migraine and acupuncture.** A migraine is a particularly painful type of headache, which patients sometimes wish to treat with acupuncture. 
To determine whether acupuncture relieves migraine pain, researchers conducted a randomized controlled study where 89 individuals who identified as female diagnosed with migraine headaches were randomly assigned to one of two groups: treatment or control. 
Forty-three (43) patients in the treatment group received acupuncture that is specifically designed to treat migraines. 
Forty-six (46) patients in the control group received placebo acupuncture (needle insertion at non-acupoint locations). 
Twenty-four (24) hours after patients received acupuncture, they were asked if they were pain free. 
Results are summarized in the contingency table below. 
Also provided is a figure from the original paper displaying the appropriate area (M) versus the inappropriate area (S) used in the treatment of migraine attacks.^[The [`migraine`](http://openintrostat.github.io/openintro/reference/migraine.html) data used in this exercise can be found in the [**openintro**](http://openintrostat.github.io/openintro) R package.] [@Allais:2011]

    <table class="table table-striped table-condensed" style="width: auto !important; margin-left: auto; margin-right: auto;">
     <thead>
    <tr>
    <th style="empty-cells: hide;border-bottom:hidden;" colspan="1"></th>
    <th style="border-bottom:hidden;padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">Pain free?</div></th>
    </tr>
      <tr>
       <th style="text-align:left;"> Group </th>
       <th style="text-align:right;"> No </th>
       <th style="text-align:right;"> Yes </th>
      </tr>
     </thead>
    <tbody>
      <tr>
       <td style="text-align:left;width: 5em; "> Control </td>
       <td style="text-align:right;width: 5em; "> 44 </td>
       <td style="text-align:right;width: 5em; "> 2 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 5em; "> Treatment </td>
       <td style="text-align:right;width: 5em; "> 33 </td>
       <td style="text-align:right;width: 5em; "> 10 </td>
      </tr>
    </tbody>
    </table>

    a.  What percent of patients in the treatment group were pain free 24 hours after receiving acupuncture?

    b.  What percent were pain free in the control group?

    c.  In which group did a higher percent of patients become pain free 24 hours after receiving acupuncture?

    d.  Your findings so far might suggest that acupuncture is an effective treatment for migraines for all people who suffer from migraines.
        However this is not the only possible conclusion.
        What is one other possible explanation for the observed difference between the percentages of patients that are pain free 24 hours after receiving acupuncture in the two groups?
        
    e.  What are the explanatory and response variables in this study?

1. **Sinusitis and antibiotics.** 
Researchers studying the effect of antibiotic treatment for acute sinusitis compared to symptomatic treatments randomly assigned 166 adults diagnosed with acute sinusitis to one of two groups: treatment or control. 
Study participants received either a 10-day course of amoxicillin (an antibiotic) or a placebo similar in appearance and taste. 
The placebo consisted of symptomatic treatments such as acetaminophen, nasal decongestants, etc. 
At the end of the 10-day period, patients were asked if they experienced improvement in symptoms. 
The distribution of responses is summarized below.^[The [`sinusitis`](http://openintrostat.github.io/openintro/reference/sinusitis.html) data used in this exercise can be found in the [**openintro**](http://openintrostat.github.io/openintro) R package.] [@Garbutt:2012]

    <table class="table table-striped table-condensed" style="width: auto !important; margin-left: auto; margin-right: auto;">
     <thead>
    <tr>
    <th style="empty-cells: hide;border-bottom:hidden;" colspan="1"></th>
    <th style="border-bottom:hidden;padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">Improvement</div></th>
    </tr>
      <tr>
       <th style="text-align:left;"> Group </th>
       <th style="text-align:right;"> No </th>
       <th style="text-align:right;"> Yes </th>
      </tr>
     </thead>
    <tbody>
      <tr>
       <td style="text-align:left;width: 5em; "> Control </td>
       <td style="text-align:right;width: 5em; "> 16 </td>
       <td style="text-align:right;width: 5em; "> 65 </td>
      </tr>
      <tr>
       <td style="text-align:left;width: 5em; "> Treatment </td>
       <td style="text-align:right;width: 5em; "> 19 </td>
       <td style="text-align:right;width: 5em; "> 66 </td>
      </tr>
    </tbody>
    </table>

    a.  What percent of patients in the treatment group experienced improvement in symptoms?

    b.  What percent experienced improvement in symptoms in the control group?

    c.  In which group did a higher percentage of patients experience improvement in symptoms?

    d.  Your findings so far might suggest a real difference in the effectiveness of antibiotic and placebo treatments for improving symptoms of sinusitis. However this is not the only possible conclusion. What is one other possible explanation for the observed difference between the percentages patients who experienced improvement in symptoms?
    
    e.  What are the explanatory and response variables in this study?

1.  **Daycare fines, study components.** 
Researchers tested the deterrence hypothesis which predicts that the introduction of a penalty will reduce the occurrence of the behavior subject to the fine, with the condition that the fine leaves everything else unchanged by instituting a fine for late pickup at daycare centers. 
For this study, they worked with 10 volunteer daycare centers that did not originally impose a fine to parents for picking up their kids late. 
They randomly selected 6 of these daycare centers and instituted a monetary fine (of a considerable amount) for picking up children late and then removed it. 
In the remaining 4 daycare centers no fine was introduced. 
The study period was divided into four: before the fine (weeks 1–4), the first 4 weeks with the fine (weeks 5-8), the last 8 weeks with fine (weeks 9–16), and the after fine period (weeks 17-20).
Throughout the study, the number of kids who were picked up late was recorded each week for each daycare. 
The study found that the number of late-coming parents increased significantly when the fine was introduced, and no reduction occurred after the fine was removed.^[The [`daycare_fines`](http://openintrostat.github.io/openintro/reference/daycare_fines.html) data used in this exercise can be found in the [**openintro**](http://openintrostat.github.io/openintro) R package.] [@Gneezy:2000]

    <table class="table table-striped table-condensed" style="width: auto !important; margin-left: auto; margin-right: auto;">
     <thead>
      <tr>
       <th style="text-align:center;"> center </th>
       <th style="text-align:center;"> week </th>
       <th style="text-align:left;"> group </th>
       <th style="text-align:center;"> late_pickups </th>
       <th style="text-align:left;"> study_period </th>
      </tr>
     </thead>
    <tbody>
      <tr>
       <td style="text-align:center;"> 1 </td>
       <td style="text-align:center;"> 1 </td>
       <td style="text-align:left;"> test </td>
       <td style="text-align:center;"> 8 </td>
       <td style="text-align:left;"> before fine </td>
      </tr>
      <tr>
       <td style="text-align:center;"> 1 </td>
       <td style="text-align:center;"> 2 </td>
       <td style="text-align:left;"> test </td>
       <td style="text-align:center;"> 8 </td>
       <td style="text-align:left;"> before fine </td>
      </tr>
      <tr>
       <td style="text-align:center;"> 1 </td>
       <td style="text-align:center;"> 3 </td>
       <td style="text-align:left;"> test </td>
       <td style="text-align:center;"> 7 </td>
       <td style="text-align:left;"> before fine </td>
      </tr>
      <tr>
       <td style="text-align:center;"> 1 </td>
       <td style="text-align:center;"> 4 </td>
       <td style="text-align:left;"> test </td>
       <td style="text-align:center;"> 6 </td>
       <td style="text-align:left;"> before fine </td>
      </tr>
      <tr>
       <td style="text-align:center;"> 1 </td>
       <td style="text-align:center;"> 5 </td>
       <td style="text-align:left;"> test </td>
       <td style="text-align:center;"> 8 </td>
       <td style="text-align:left;"> first 4 weeks with fine </td>
      </tr>
      <tr>
       <td style="text-align:center;"> ... </td>
       <td style="text-align:center;"> ... </td>
       <td style="text-align:left;"> ... </td>
       <td style="text-align:center;"> ... </td>
       <td style="text-align:left;"> ... </td>
      </tr>
      <tr>
       <td style="text-align:center;"> 10 </td>
       <td style="text-align:center;"> 20 </td>
       <td style="text-align:left;"> control </td>
       <td style="text-align:center;"> 13 </td>
       <td style="text-align:left;"> after fine </td>
      </tr>
    </tbody>
    </table>

    a.  Is this an observational study or an experiment? Explain your reasoning.

    b.  What are the cases in this study and how many are included?

    c.  What is the response variable in the study and what type of variable is it?

    d.  What are the explanatory variables in the study and what types of variables are they?
    
    \vspace{5mm}

1. **Efficacy of COVID-19 vaccine on adolescents, study components.** 
Results of a Phase 3 trial announced in March 2021 show that the Pfizer-BioNTech COVID-19 vaccine demonstrated 100% efficacy and robust antibody responses on 12 to 15 years old adolescents with or without prior evidence of SARS-CoV-2 infection. In this trial 2,260 adolescents were randomly assigned to two groups: one group got the vaccine (n = 1,131) and the other got a placebo (n = 1,129). While 18 cases of COVID-19 were observed in the placebo group, none were observed in the vaccine group.^[The [`biontech_adolescents`](http://openintrostat.github.io/openintro/reference/biontech_adolescents.html) data used in this exercise can be found in the [**openintro**](http://openintrostat.github.io/openintro) R package.] [@Pfizer:2021]

    a.  Is this an observational study or an experiment? Explain your reasoning.

    b.  What are the cases in this study and how many are included?

    c.  What is the response variable in the study and what type of variable is it?

    d.  What are the explanatory variables in the study and what types of variables are they?
    
    \clearpage

1. **Palmer penguins.**
Data were collected on 344 penguins living on three islands (Torgersen, Biscoe, and Dream) in the Palmer Archipelago, Antarctica. 
In addition to which island each penguin lives on, the data contains information on the species of the penguin (*Adelie*, *Chinstrap*, or *Gentoo*), its bill length, bill depth, and flipper length (measured in millimeters), its body mass (measured in grams), and the sex of the penguin (female or male).^[The [`penguins`](https://allisonhorst.github.io/palmerpenguins/reference/penguins.html) data used in this exercise can be found in the [**palmerpenguins**](https://allisonhorst.github.io/palmerpenguins/) R package.] Bill length and depth are measured as shown in the image.^[Artwork by [Allison Horst](https://twitter.com/allison_horst).] [@palmerpenguins]

    <img src="exercises/images/culmen_depth.png" title="Bill length and depth marked on an illustration of a penguin head." alt="Bill length and depth marked on an illustration of a penguin head." width="40%" style="display: block; margin: auto;" />

    a.  How many cases were included in the data?
    b.  How many numerical variables are included in the data? Indicate what they are, and if they are continuous or discrete.
    c.  How many categorical variables are included in the data, and what are they? List the corresponding levels (categories) for each.

    \vspace{5mm}

1. **Smoking habits of UK residents.** 
A survey was conducted to study the smoking habits of 1,691 UK residents. Below is a data frame displaying a portion of the data collected in this survey. 
A blank cell indicates that data for that variable was not available for a given respondent.^[The [`smoking`](http://openintrostat.github.io/openintro/reference/smoking.html) data used in this exercise can be found in the [**openintro**](http://openintrostat.github.io/openintro) R package.] 

    <table class="table table-striped table-condensed" style="width: auto !important; margin-left: auto; margin-right: auto;">
     <thead>
    <tr>
    <th style="empty-cells: hide;border-bottom:hidden;" colspan="6"></th>
    <th style="border-bottom:hidden;padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">amount</div></th>
    </tr>
      <tr>
       <th style="text-align:left;">   </th>
       <th style="text-align:left;"> sex </th>
       <th style="text-align:left;"> age </th>
       <th style="text-align:left;"> marital_status </th>
       <th style="text-align:center;"> gross_income </th>
       <th style="text-align:left;"> smoke </th>
       <th style="text-align:center;"> weekend </th>
       <th style="text-align:center;"> weekday </th>
      </tr>
     </thead>
    <tbody>
      <tr>
       <td style="text-align:left;"> 1 </td>
       <td style="text-align:left;"> Female </td>
       <td style="text-align:left;"> 61 </td>
       <td style="text-align:left;"> Married </td>
       <td style="text-align:center;"> 2,600 to 5,200 </td>
       <td style="text-align:left;"> No </td>
       <td style="text-align:center;">  </td>
       <td style="text-align:center;">  </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 2 </td>
       <td style="text-align:left;"> Female </td>
       <td style="text-align:left;"> 61 </td>
       <td style="text-align:left;"> Divorced </td>
       <td style="text-align:center;"> 10,400 to 15,600 </td>
       <td style="text-align:left;"> Yes </td>
       <td style="text-align:center;"> 5 </td>
       <td style="text-align:center;"> 4 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 3 </td>
       <td style="text-align:left;"> Female </td>
       <td style="text-align:left;"> 69 </td>
       <td style="text-align:left;"> Widowed </td>
       <td style="text-align:center;"> 5,200 to 10,400 </td>
       <td style="text-align:left;"> No </td>
       <td style="text-align:center;">  </td>
       <td style="text-align:center;">  </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 4 </td>
       <td style="text-align:left;"> Female </td>
       <td style="text-align:left;"> 50 </td>
       <td style="text-align:left;"> Married </td>
       <td style="text-align:center;"> 5,200 to 10,400 </td>
       <td style="text-align:left;"> No </td>
       <td style="text-align:center;">  </td>
       <td style="text-align:center;">  </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 5 </td>
       <td style="text-align:left;"> Male </td>
       <td style="text-align:left;"> 31 </td>
       <td style="text-align:left;"> Single </td>
       <td style="text-align:center;"> 10,400 to 15,600 </td>
       <td style="text-align:left;"> Yes </td>
       <td style="text-align:center;"> 10 </td>
       <td style="text-align:center;"> 20 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> ... </td>
       <td style="text-align:left;"> ... </td>
       <td style="text-align:left;"> ... </td>
       <td style="text-align:left;"> ... </td>
       <td style="text-align:center;"> ... </td>
       <td style="text-align:left;"> ... </td>
       <td style="text-align:center;">  </td>
       <td style="text-align:center;">  </td>
      </tr>
      <tr>
       <td style="text-align:left;"> 1691 </td>
       <td style="text-align:left;"> Male </td>
       <td style="text-align:left;"> 49 </td>
       <td style="text-align:left;"> Divorced </td>
       <td style="text-align:center;"> Above 36,400 </td>
       <td style="text-align:left;"> Yes </td>
       <td style="text-align:center;"> 15 </td>
       <td style="text-align:center;"> 10 </td>
      </tr>
    </tbody>
    </table>

    a.  What does each row of the data frame represent?

    b.  How many participants were included in the survey?

    c.  Indicate whether each variable in the study is numerical or categorical. If numerical, identify as continuous or discrete. If categorical, indicate if the variable is ordinal.
    
    \clearpage

1.  **US Airports.** 
The visualization below shows the geographical distribution of airports in the contiguous United States and Washington, DC. 
This visualization was constructed based on a dataset where each observation is an airport.^[The [`usairports`](http://openintrostat.github.io/airports/reference/usairports.html) data used in this exercise can be found in the [**airports**](http://openintrostat.github.io/airports/) R package.]

    <img src="01-data-hello_files/figure-html/unnamed-chunk-24-1.png" width="90%" style="display: block; margin: auto;" />

    a.  List the variables you believe were necessary to create this visualization.

    b.  Indicate whether each variable in the study is numerical or categorical. If numerical, identify as continuous or discrete. If categorical, indicate if the variable is ordinal.
    
    \vspace{5mm}

1. **UN Votes.** 
The visualization below shows voting patterns in the United States, Canada, and Mexico in the United Nations General Assembly on a variety of issues. 
Specifically, for a given year between 1946 and 2019, it displays the percentage of roll calls in which the country voted yes for each issue. 
This visualization was constructed based on a dataset where each observation is a country/year pair.^[The data used in this exercise can be found in the [**unvotes**](https://cran.r-project.org/web/packages/unvotes/index.html) R package.]

    <img src="01-data-hello_files/figure-html/unnamed-chunk-25-1.png" width="90%" style="display: block; margin: auto;" />

    a.  List the variables used in creating this visualization.

    b.  Indicate whether each variable in the study is numerical or categorical. If numerical, identify as continuous or discrete. If categorical, indicate if the variable is ordinal.

1.  **UK baby names.** 
The visualization below shows the number of baby girls born in the United Kingdom (comprised of England & Wales, Northern Ireland, and Scotland) who were given the name "Fiona" over the years.^[The [`ukbabynames`](https://mine-cetinkaya-rundel.github.io/ukbabynames/reference/ukbabynames.html) data used in this exercise can be found in the [**ukbabynames**](https://mine-cetinkaya-rundel.github.io/ukbabynames/) R package.]

    <img src="01-data-hello_files/figure-html/unnamed-chunk-26-1.png" width="90%" style="display: block; margin: auto;" />

    a.  List the variables you believe were necessary to create this visualization.

    b.  Indicate whether each variable in the study is numerical or categorical. If numerical, identify as continuous or discrete. If categorical, indicate if the variable is ordinal.
    
    \vspace{5mm}

1. **Shows on Netflix.** 
The visualization below shows the distribution of ratings of TV shows on Netflix (a streaming entertainment service) based on the decade they were released in and the country they were produced in. In the dataset, each observation is a TV show.^[The [`netflix_titles`](https://github.com/rfordatascience/tidytuesday/blob/master/data/2021/2021-04-20/readme.md) data used in this exercise can be found in the [**tidytuesdayR**](https://cran.r-project.org/web/packages/tidytuesdayR/index.html) R package.]

    <img src="01-data-hello_files/figure-html/unnamed-chunk-27-1.png" width="90%" style="display: block; margin: auto;" />

    a.  List the variables you believe were necessary to create this visualization.

    b.  Indicate whether each variable in the study is numerical or categorical. If numerical, identify as continuous or discrete. If categorical, indicate if the variable is ordinal.
    
    \clearpage

1. **Stanford Open Policing.** 
The Stanford Open Policing project gathers, analyzes, and releases records from traffic stops by law enforcement agencies across the United States. Their goal is to help researchers, journalists, and policy makers investigate and improve interactions between police and the public. The following is an excerpt from a summary table created based off of the data collected as part of this project. [@pierson2020large]

    <table class="table table-striped table-condensed" style="width: auto !important; margin-left: auto; margin-right: auto;">
     <thead>
    <tr>
    <th style="empty-cells: hide;border-bottom:hidden;" colspan="2"></th>
    <th style="border-bottom:hidden;padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">Driver</div></th>
    <th style="border-bottom:hidden;padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">Car</div></th>
    </tr>
      <tr>
       <th style="text-align:left;"> County </th>
       <th style="text-align:left;"> State </th>
       <th style="text-align:left;"> Race / Ethnicity </th>
       <th style="text-align:center;"> Arrest rate </th>
       <th style="text-align:center;"> Stops / year </th>
       <th style="text-align:center;"> Search rate </th>
      </tr>
     </thead>
    <tbody>
      <tr>
       <td style="text-align:left;"> Apache County </td>
       <td style="text-align:left;"> AZ </td>
       <td style="text-align:left;"> Black </td>
       <td style="text-align:center;"> 0.016 </td>
       <td style="text-align:center;"> 266 </td>
       <td style="text-align:center;"> 0.077 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> Apache County </td>
       <td style="text-align:left;"> AZ </td>
       <td style="text-align:left;"> Hispanic </td>
       <td style="text-align:center;"> 0.018 </td>
       <td style="text-align:center;"> 1008 </td>
       <td style="text-align:center;"> 0.053 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> Apache County </td>
       <td style="text-align:left;"> AZ </td>
       <td style="text-align:left;"> White </td>
       <td style="text-align:center;"> 0.006 </td>
       <td style="text-align:center;"> 6322 </td>
       <td style="text-align:center;"> 0.017 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> Cochise County </td>
       <td style="text-align:left;"> AZ </td>
       <td style="text-align:left;"> Black </td>
       <td style="text-align:center;"> 0.015 </td>
       <td style="text-align:center;"> 1169 </td>
       <td style="text-align:center;"> 0.047 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> Cochise County </td>
       <td style="text-align:left;"> AZ </td>
       <td style="text-align:left;"> Hispanic </td>
       <td style="text-align:center;"> 0.01 </td>
       <td style="text-align:center;"> 9453 </td>
       <td style="text-align:center;"> 0.037 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> Cochise County </td>
       <td style="text-align:left;"> AZ </td>
       <td style="text-align:left;"> White </td>
       <td style="text-align:center;"> 0.008 </td>
       <td style="text-align:center;"> 10826 </td>
       <td style="text-align:center;"> 0.024 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> ... </td>
       <td style="text-align:left;"> ... </td>
       <td style="text-align:left;"> ... </td>
       <td style="text-align:center;"> ... </td>
       <td style="text-align:center;"> ... </td>
       <td style="text-align:center;"> ... </td>
      </tr>
      <tr>
       <td style="text-align:left;"> Wood County </td>
       <td style="text-align:left;"> WI </td>
       <td style="text-align:left;"> Black </td>
       <td style="text-align:center;"> 0.098 </td>
       <td style="text-align:center;"> 16 </td>
       <td style="text-align:center;"> 0.244 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> Wood County </td>
       <td style="text-align:left;"> WI </td>
       <td style="text-align:left;"> Hispanic </td>
       <td style="text-align:center;"> 0.029 </td>
       <td style="text-align:center;"> 27 </td>
       <td style="text-align:center;"> 0.036 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> Wood County </td>
       <td style="text-align:left;"> WI </td>
       <td style="text-align:left;"> White </td>
       <td style="text-align:center;"> 0.029 </td>
       <td style="text-align:center;"> 1157 </td>
       <td style="text-align:center;"> 0.033 </td>
      </tr>
    </tbody>
    </table>

    a.  What variables were collected on each individual traffic stop in order to create the summary table above?

    b.  State whether each variable is numerical or categorical. If numerical, state whether it is continuous or discrete. If categorical, state whether it is ordinal or not.

    c.  Suppose we wanted to evaluate whether vehicle search rates are different for drivers of different races. In this analysis, which variable would be the response variable and which variable would be the explanatory variable?
    
    \vspace{5mm}

1. **Space launches.** 
The following summary table shows the number of space launches in the US by the type of launching agency and the outcome of the launch (success or failure).^[The data used in this exercise comes from the [JSR Launch Vehicle Database, 2019 Feb 10 Edition](https://www.openintro.org/go?id=textbook-space-launches-data&referrer=ims0_html).]

    <table class="table table-striped table-condensed" style="width: auto !important; margin-left: auto; margin-right: auto;">
     <thead>
    <tr>
    <th style="empty-cells: hide;border-bottom:hidden;" colspan="1"></th>
    <th style="border-bottom:hidden;padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">1957 - 1999</div></th>
    <th style="border-bottom:hidden;padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">2000-2018</div></th>
    </tr>
      <tr>
       <th style="text-align:left;">  </th>
       <th style="text-align:right;"> Failure </th>
       <th style="text-align:right;"> Success </th>
       <th style="text-align:right;"> Failure </th>
       <th style="text-align:right;"> Success </th>
      </tr>
     </thead>
    <tbody>
      <tr>
       <td style="text-align:left;"> Private </td>
       <td style="text-align:right;"> 13 </td>
       <td style="text-align:right;"> 295 </td>
       <td style="text-align:right;"> 10 </td>
       <td style="text-align:right;"> 562 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> State </td>
       <td style="text-align:right;"> 281 </td>
       <td style="text-align:right;"> 3751 </td>
       <td style="text-align:right;"> 33 </td>
       <td style="text-align:right;"> 711 </td>
      </tr>
      <tr>
       <td style="text-align:left;"> Startup </td>
       <td style="text-align:right;"> 0 </td>
       <td style="text-align:right;"> 0 </td>
       <td style="text-align:right;"> 5 </td>
       <td style="text-align:right;"> 65 </td>
      </tr>
    </tbody>
    </table>

    a.  What variables were collected on each launch in order to create to the summary table above?

    b.  State whether each variable is numerical or categorical. If numerical, state whether it is continuous or discrete. If categorical, state whether it is ordinal or not.

    c.  Suppose we wanted to study how the success rate of launches vary between launching agencies and over time. In this analysis, which variable would be the response variable and which variable would be the explanatory variable?
    
    \clearpage

1.  **Pet names.** 
The city of Seattle, WA has an open data portal that includes pets registered in the city. For each registered pet, we have information on the pet's name and species. 
The following visualization plots the proportion of dogs with a given name versus the proportion of cats with the same name. The 20 most common cat and dog names are displayed. 
The diagonal line on the plot is the $x = y$ line; if a name appeared on this line, the name's popularity would be exactly the same for dogs and cats.^[The [`seattlepets`](http://openintrostat.github.io/openintro/reference/seattlepets.html) data used in this exercise can be found in the [**openintro**](http://openintrostat.github.io/openintro) R package.]

    <img src="01-data-hello_files/figure-html/unnamed-chunk-30-1.png" width="90%" style="display: block; margin: auto;" />

    a.  Are these data collected as part of an experiment or an observational study?

    b.  What is the most common dog name? What is the most common cat name?

    c.  What names are more common for cats than dogs?

    d.  Is the relationship between the two variables positive or negative? What does this mean in context of the data?
    
    \vspace{5mm}

1. **Stressed out in an elevator.** 
In a study evaluating the relationship between stress and muscle cramps, half the subjects are randomly assigned to be exposed to increased stress by being placed into an elevator that falls rapidly and stops abruptly and the other half are left at no or baseline stress.

    a.  What type of study is this?

    b.  Can this study be used to conclude a causal relationship between increased stress and muscle cramps?
:::
