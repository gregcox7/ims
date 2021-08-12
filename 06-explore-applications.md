# Applications: Explore {#explore-applications}



## Case study: Effective communication of exploratory results {#case-study-effective-comms}

Graphs can powerfully communicate ideas directly and quickly.
We all know, after all, that "a picture is worth 1000 words." Unfortunately, however, there are times when an image conveys a message which is inaccurate or misleading.

This chapter focuses on how graphs can best be utilized to present data accurately and effectively.
Along with data modeling, creative visualization is somewhat of an art.
However, even with an art, there are recommended guiding principles.
We provide a few best practices for creating data visualizations.

### Keep it simple

When creating a graphic, keep in mind what it is that you'd like your reader to see.
Colors should be used to group items or differentiate levels in meaningful ways.
Colors can be distracting when they are only used to brighten up the plot.

Consider a manufacturing company that has summarized their costs into five different categories.
In the two graphics provided in Figure \@ref(fig:pie-to-bar), notice that the magnitudes in the pie chart are difficult for the eye to compare.
That is, can your eye tell how different "Buildings and administration" is from "Workplace materials" when looking at the slices of pie?
Additionally, the colors in the pie chart do not mean anything and are therefore distracting.
Lastly, the three-dimensional aspect of the image does not improve the reader's ability to understand the data presented.

As an alternative, a bar plot has been provided.
Notice how much easier it is to identify the magnitude of the differences across categories while not being distracted by other aspects of the image.
Typically, a bar plot will be easier for the reader to digest than a pie chart, especially if the categorical data being plotted has more than just a few levels.



<div class="figure" style="text-align: center">
<img src="images/pie-3d.jpg" alt="A pie chart (with added irrelevant features) as compared to a simple bar plot." width="50%" /><img src="06-explore-applications_files/figure-html/pie-to-bar-2.png" alt="A pie chart (with added irrelevant features) as compared to a simple bar plot." width="50%" />
<p class="caption">(\#fig:pie-to-bar)A pie chart (with added irrelevant features) as compared to a simple bar plot.</p>
</div>

### Use color to draw attention

There are many reasons why you might choose to add **color** to your plots.
An important principle to keep in mind is to use color to draw attention.
Of course, you should still think about how visually pleasing your visualization is, and if you're adding color for making it visually pleasing without drawing attention to a particular feature, that might be fine.
However you should be critical of default coloring and explicitly decide whether to include color and how.
Notice that in Plot B in Figure \@ref(fig:red-bar) the coloring is done in such a way to draw the reader's attention to one particular piece of information.
The default coloring in Plot A can be distracting and makes the reader question, for example, is there something similar about the red and purple bars?
Also note that not everyone sees color the same way, often it's useful to add color and one more feature (e.g., pattern) so that you can refer to the features you're drawing attention to in multiple ways.

<div class="figure" style="text-align: center">
<img src="06-explore-applications_files/figure-html/red-bar-1.png" alt="The default coloring in the first bar plot does nothing for the understanding of the data. In the second plot, the color draws attention directly to the bar on Buildings and Administration." width="90%" />
<p class="caption">(\#fig:red-bar)The default coloring in the first bar plot does nothing for the understanding of the data. In the second plot, the color draws attention directly to the bar on Buildings and Administration.</p>
</div>



### Tell a story

For many graphs, an important aspect is the inclusion of information which is not provided in the dataset that is being plotted.
The external information serves to contextualize the data and helps communicate the narrative of the research.
In Figure \@ref(fig:duke-hires), the graph on the right is **annotated** with information about the start of the university's fiscal year which contextualizes the information provided by the data.
Sometimes the additional information may be a diagonal line given by $y = x$, points above the line quickly show the reader which values have a $y$ coordinate larger than the $x$ coordinate; points below the line show the opposite.



<div class="figure" style="text-align: center">
<img src="images/time-series-story.png" alt="Credit: Angela Zoss and Eric Monson, Duke Data Visualization Services" width="100%" />
<p class="caption">(\#fig:duke-hires)Credit: Angela Zoss and Eric Monson, Duke Data Visualization Services</p>
</div>

### Order matters

Most software programs have built in methods for some of the plot details.
For example, the default option for the software program used in this text, R, is to order the bars in a bar plot alphabetically.
As seen in Figure \@ref(fig:brexit-bars), the alphabetical ordering isn't particularly meaningful for describing the data.
Sometimes it makes sense to **order** the bars from tallest to shortest (or vice versa).
But in this case, the best ordering is probably the one in which the questions were asked.
An ordering which doesn't make sense in the context of the problem (e.g., alphabetically here), can mislead the reader who might take a quick glance at the axes and not read the bar labels carefully.



In September 2019, YouGov survey asked 1,639 Great Britain adults the following question[^explore-applications-1]:

[^explore-applications-1]: Source: [YouGov Survey Results](https://d25d2506sfb94s.cloudfront.net/cumulus_uploads/document/x0msmggx08/YouGov%20-%20Brexit%20and%202019%20election.pdf), retrieved Oct 7, 2019.

> How well or badly do you think the government are doing at handling Britain's exit from the European Union?
>
> -   Very well
> -   Fairly well
> -   Fairly badly
> -   Very badly
> -   Don't know



<div class="figure" style="text-align: center">
<img src="06-explore-applications_files/figure-html/brexit-bars-1.png" alt="Bar plot three different ways. Plot A: Alphabetic ordering of levels, Plot B: Bars ordered in descending order of frequency, Plot C: Bars ordered in the same order as they were presented in the survey question." width="100%" />
<p class="caption">(\#fig:brexit-bars)Bar plot three different ways. Plot A: Alphabetic ordering of levels, Plot B: Bars ordered in descending order of frequency, Plot C: Bars ordered in the same order as they were presented in the survey question.</p>
</div>

### Make the labels as easy to read as possible

The Brexit survey results were additionally broken down by region in Great Britain.
The stacked bar plot allows for comparison of Brexit opinion across the five regions.
In Figure \@ref(fig:brexit-region) the bars are vertical in Plot A and horizontal in Plot B. While the quantitative information in the two graphics is identical, flipping the graph and creating horizontal bars provides more space for the **axis labels**.
The easier the categories are to read, the more the reader will learn from the visualization.
Remember, the goal is to convey as much information as possible in a succinct and clear manner.

<div class="figure" style="text-align: center">
<img src="06-explore-applications_files/figure-html/brexit-region-1.png" alt="Stacked bar plots vertically and horizontally. The horizontal orientation makes the region labels easier to read." width="100%" />
<p class="caption">(\#fig:brexit-region)Stacked bar plots vertically and horizontally. The horizontal orientation makes the region labels easier to read.</p>
</div>



### Pick a purpose

Every graphical decision should be made with a **purpose**.
As previously mentioned, sticking with default options is not always best for conveying the narrative of your data story.
Stacked bar plots tell one part of a story.
Depending on your research question, they may not tell the part of the story most important to the research.
Figure \@ref(fig:seg-three-ways) provides three different ways of representing the same information.
If the most important comparison across regions is proportion, you might prefer Plot A. If the most important comparison across regions also considers the total number of individuals in the region, you might prefer Plot B. If a separate bar plot for each region makes the point you'd like, use Plot C, which has been **faceted** by region.



Plot C in Figure \@ref(fig:seg-three-ways) also provides full titles and a succinct URL with the data source.
Other deliberate decisions to consider include using informative labels and avoiding redundancy.

<div class="figure" style="text-align: center">
<img src="06-explore-applications_files/figure-html/seg-three-ways-1.png" alt="Three different representations of the two variables including survey opinion and region. Use the graphic that best conveys the data narrative at hand." width="90%" />
<p class="caption">(\#fig:seg-three-ways)Three different representations of the two variables including survey opinion and region. Use the graphic that best conveys the data narrative at hand.</p>
</div>



### Select meaningful colors

<!-- An example with an ordinal variable with more levels would be better. -->

One last consideration for building graphs is to consider color choices.
Default or rainbow colors are not always the choice which will best distinguish the level of your variables.
Much research has been done to find color combinations which are distinct and also which are clear for differently sighted individuals.
The cividis scale works well with ordinal data.
[@Nunez:2018] Figure \@ref(fig:brexit-viridis) shows the same plot with two different colorings.

<div class="figure" style="text-align: center">
<img src="06-explore-applications_files/figure-html/brexit-viridis-1.png" alt="Identical bar plots with two different coloring options. Plot A uses a default color scale, Plot B uses colors from the cividis scale." width="90%" />
<p class="caption">(\#fig:brexit-viridis)Identical bar plots with two different coloring options. Plot A uses a default color scale, Plot B uses colors from the cividis scale.</p>
</div>

In this chapter different representations are contrasted to demonstrate best practices in creating graphs.
The fundamental principle is that your graph should provide maximal information succinctly and clearly.
Labels should be clear and oriented horizontally for the reader.
Don't forget titles and, if possible, include the source of the data.

\clearpage

## Interactive R tutorials {#explore-tutorials}

Navigate the concepts you've learned in this chapter in R using the following self-paced tutorials.
All you need is your browser to get started!

::: {.alltutorials data-latex=""}
[Tutorial 2: Exploratory data analysis](https://openintrostat.github.io/ims-tutorials/02-explore/)\

:::

::: {.singletutorial data-latex=""}
[Tutorial 2 - Lesson 1: Visualizing categorical data](https://openintro.shinyapps.io/ims-02-explore-01/)\

:::

::: {.singletutorial data-latex=""}
[Tutorial 2 - Lesson 2: Visualizing numerical data](https://openintro.shinyapps.io/ims-02-explore-02/)\

:::

::: {.singletutorial data-latex=""}
[Tutorial 2 - Lesson 3: Summarizing with statistics](https://openintro.shinyapps.io/ims-02-explore-03/)\

:::

::: {.singletutorial data-latex=""}
[Tutorial 2 - Lesson 4: Case study](https://openintro.shinyapps.io/ims-02-explore-04/)\

:::



You can also access the full list of tutorials supporting this book [here](https://openintrostat.github.io/ims-tutorials).

## R labs {#explore-labs}

Further apply the concepts you've learned in this part in R with computational labs that walk you through a data analysis case study.

::: {.singlelab data-latex=""}
[Intro to data - Flight delays](https://www.openintro.org/go?id=ims-r-lab-intro-to-data)\

:::



You can also access the full list of labs supporting this book [here](https://www.openintro.org/go?id=ims-r-labs).
