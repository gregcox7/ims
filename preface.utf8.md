# Preface {.unnumbered}

We hope readers will take away three ideas from this book in addition to forming a foundation of statistical thinking and methods.

1.  Statistics is an applied field with a wide range of practical applications.
2.  You don't have to be a math guru to learn from interesting, real data.
3.  Data are messy, and statistical tools are imperfect. However, when you understand the strengths and weaknesses of these tools, you can use them to learn interesting things about the world.

#### Textbook overview {.unnumbered}

-   **Part 1: Introduction to data.** Data structures, variables, summaries, graphics, and basic data collection and study design techniques.
-   **Part 2: Exploratory data analysis.** Data visualization and summarisation, with particular emphasis on multivariable relationships.
-   **Part 3: Regression modeling.** Modeling numerical and categorical outcomes with linear and logistic regression and using model results to describe relationships and made predictions.
-   **Part 4: Foundations for inference.** Case studies are used to introduce the ideas of statistical inference with randomization tests, bootstrap intervals, and mathematical models.
-   **Part 5: Statistical inference.** Further details of statistical inference using randomization tests, bootstrap intervals, and mathematical models for numerical and categorical data.
-   **Part 6: Inferential modeling.** Extending inference techniques presented thus-far to linear and logistic regression settings and evaluating model performance.

Each part contains multiple chapters and ends with a case study.
Building on the content covered in the part, the case study uses the tools and techniques to present a high level overview.

Each chapter ends with a review section which contains a chapter summary as well as a list of key terms introduced in the chapter.
If you're not sure what some of these terms mean, we recommend you go back in the text and review their definitions.
We purposefully present them in alphabetical order, instead of in order of appearance, so they will be a little more challenging to locate.
However you should be able to easily spot them as **bolded text**.

#### Examples and exercises {.unnumbered}

Examples are provided to establish an understanding of how to apply methods.

::: {.workedexample data-latex=""}
This is an example.
When a question is asked here, where can the answer be found?

------------------------------------------------------------------------

The answer can be found here, in the solution section of the example!
:::

When we think the reader is ready to try determining a solution on their own, we frame it as Guided Practice.

::: {.guidedpractice data-latex=""}
The reader may check or learn the answer to any Guided Practice problem by reviewing the full solution in a footnote.[^preface-1]
:::

[^preface-1]: Guided Practice problems are intended to stretch your thinking, and you can check yourself by reviewing the footnote solution for any Guided Practice.

Exercises are also provided at the end of each chapter.
Solutions are given for odd-numbered exercises in Appendix \@ref(exercise-solutions).

#### Datasets and their sources {.unnumbered}

A large majority of the datasets used in the book can be found in various R packages.
Each time a new dataset is introduced in the narrative, a reference to the package like the one below is provided.
Many of these datasets are in the [**openintro**](http://openintrostat.github.io/openintro) R package that contains datasets used in [OpenIntro](https://www.openintro.org/)'s open-source textbooks.[^preface-2]

[^preface-2]: Mine Çetinkaya-Rundel, David Diez, Andrew Bray, Albert Kim, Ben Baumer, Chester Ismay and Christopher Barr (2020).
    openintro: Data Sets and Supplemental Functions from 'OpenIntro' Textbooks and Labs.
    R package version 2.0.0.
    <https://github.com/OpenIntroStat/openintro>.

::: {.data data-latex=""}
The [`textbooks`](http://openintrostat.github.io/openintro/reference/textbooks.html) data can be found in the [**openintro**](http://openintrostat.github.io/openintro) R package.
:::

The datasets used throughout the book come from real sources like opinion polls and scientific articles, except for a handful of cases where we use toy data to highlight a particular feature or explain a particular concept.
References for the sources of the real data are provided at the end of the book.

#### Computing with R {.unnumbered}

The narrative and the exercises in the book are computing language agnostic, however while it's possible to learn about modern statistics without computing, it's not possible to apply it.
Therefore, we invite you to navigate the concepts you have learned in each part using the interactive R tutorials and the R labs that are included at the end of each part.

**Interactive R tutorials**

The self-paced and interactive R tutorials were developed using the [learnr](https://rstudio.github.io/learnr/index.html) R package, and only an internet browser is needed to complete them.

::: {.alltutorials data-latex=""}
Each part comes with a tutorial comprised of 4-10 lessons and listed like this.
:::

::: {.singletutorial data-latex=""}
Each of these lessons...
:::

::: {.singletutorial data-latex=""}
... is listed like this.
:::



You can access the full list of tutorials supporting this book [here](https://openintrostat.github.io/ims-tutorials).

\clearpage

**R labs**

Once you feel comfortable with the material in the tutorials, we also encourage you to apply what you've learned via the computational labs that are also linked at the end of each part.
The labs consist of data analysis case studies, and they require access to [R](https://cran.r-project.org/) and [RStudio](https://rstudio.com/products/rstudio/).
The first lab includes installation instructions.
If you'd rather not install the software locally, you can also try [RStudio Cloud](https://rstudio.cloud/) for free.

::: {.singlelab data-latex=""}
Labs for each part are listed like this.
:::



You can access the full list of labs supporting this book [here](https://www.openintro.org/go?id=ims-r-labs).

#### OpenIntro, online resources, and getting involved {.unnumbered}

OpenIntro is an organization focused on developing free and affordable education materials.

We encourage anyone learning or teaching statistics to visit [openintro.org](http://www.openintro.org) and to get involved.

All OpenIntro resources are free and anyone is welcomed to use these online tools and resources with or without this textbook as a companion.

We value your feedback.
If there is a part of the project you especially like or think needs improvement, we want to hear from you.
For feedback on this specific book, you can open an issue [on GitHub](https://github.com/openintrostat/ims/issues).
You can also provide feedback on this book or any other OpenIntro resource via our contact form at [openintro.org](https://www.openintro.org/form/?f=contact).

#### Acknowledgements {.unnumbered}

The *OpenIntro* project would not have been possible without the dedication and volunteer hours of all those involved, and we hope you will join us in extending a huge *thank you* to all those who volunteer with OpenIntro.

The authors would like to thank

-   David Diez and Christopher Barr for their work on the 1st Edition of this book,
-   Ben Baumer and Andrew Bray for their contribution rethinking how and which order we present this material as well as their work as original authors of the interactive tutorial content,
-   Yanina Bellini Saibene, Florencia D'Andrea, and Roxana Noelia Villafañe for their work on creating the interactive tutorials in learnr,
-   Will Gray for conceptual diagrams,
-   Allison Theobold, Melinda Yager, and Randy Prium for their valuable feedback and review of the book,
-   Colin Rundel for feedback on content and technical help with conversion from LaTeX to R Markdown,
-   Christophe Dervieux for help with multi-output bookdown issues, and
-   Müge Çetinkaya and Meenal Patel for their design vision.

We would like to also thank the developers of the open-source tools that make the development and authoring of this book possible, e.g., [bookdown](https://bookdown.org/), [tidyverse](https://tidyverse.org/), and [icons8](http://icons8.com/).

We are also grateful to the many teachers, students, and other readers who have helped improve OpenIntro resources through their feedback.
