---
title: "Report writing with R Markdown"
author:
   - name: Andrew Moles
     affiliation: Learning Developer, Digital Skills Lab
date: "05 September, 2022"
output: 
  html_document: 
    theme: united
    highlight: pygments
    keep_md: yes
    code_download: FALSE
    toc: TRUE
    toc_depth: 4
    toc_float: FALSE
---


```{=html}
<script>
  addKlippy('right', 'top', 'auto', '1', 'Copy code', 'Copied!');
</script>
```

## What is R Markdown and why use it?

R Markdown is a tool for making dynamic document with R which combines markdown, a lightweight markup language that is an easy-to-write plain text format, and sections or *chunks* of embedded R code. This is powerful as is allows you to write reports or presentations that contain your R code.

Markdown is a flexible and lightweight tool for document writing. Two good sources of picking up the basic syntax are the [markdown guide](https://www.markdownguide.org/basic-syntax) and the [markdown tutorial](https://commonmark.org/help/tutorial/). You can use these as points of reference throughout this tutorial. 

There are two main reasons for writing reports or presentations in R Markdown instead of LaTeX: simplicity, and the ability to include R code. Markdown is a very easy to read and write markup language (LaTeX and HTML or other markup languages), which makes your life easier when writing documents. You can supplement your documents with LaTeX or HTML if needed. The inclusion of R code chunks makes R Markdown very flexible, with a lot of people using it not just for reports but for there general analysis work as well; it brings together all elements of an analysis which is really valuable. 

There are very few disadvantages to using R Markdown over just LaTeX. One disadvantage is you need to use R and RStudio, which not everyone will have installed or feel comfortable using. 

We should also note that R Markdown is a *flavour* of Markdown. Markdown can work with a wide range of programming languages from R, Python, C++, JavaScript, Bash, and SQL; all of which also work within RStudio!  

## Outcomes

By the end of this workshop you will:

* Start using R Markdown in RStudio
* Create a document in R Markdown that includes
  - titles
  - sections
  - images
  - outputs from code
  - lists
  - tables
  - hyper links
  - references
* Change outputs of code cells to get desired output

It might feel like there is a lot to explore. However, we split it into separate sections to make it easier for you to work through. We hope you enjoy it!

## Requirements and setup

For this workshop you will need the following software and packages. 

#### Software

Make sure you have R and RStudio installed. If you haven’t, first install [R](https://www.r-project.org/) and then [RStudio](https://www.rstudio.com/products/rstudio/download/).

#### Packages

There are two main packages needed for this workshop, `rmarkdown` and `tinytex`. 

To install and load the packages copy the contents of this code chunk. 

```{.r .klippy}
install.packages("rmarkdown")
install.packages("tinytex")

library(rmarkdown)
library(tinytex)
```

To use LaTeX and render the output as pdf we will also need to install tinytex after installing and loading it. For more information on this [see the rmarkdown book](https://bookdown.org/yihui/rmarkdown/installation.html).

```{.r .klippy}
# to install tinytex
tinytex::install_tinytex()
```

## Introduction to R Markdown

When you open a R Markdown document, which has the extension of `.Rmd` you will see an interface like this. 
![R Markdown interface](https://raw.githubusercontent.com/andrewmoles2/rTrainIntroduction/main/r-fundamentals-1/Images/RMarkdown.png)
The grey areas are the code chunks. To run code cells you click on the green play button or use `control/command + enter`

The white space is where we write our text. We can use markdown formatting here. 

The blue lines are headers. One # means top header, two # means second header and so on. We use these to make sections in our document. 

The top part (the one that starts with title and ends with output between ---) is written in a so-called YAML language. Here we tell R Markdown what output we want, who we are and so on; this creates metadata that is used to compile your documents. 

## Useful resources to use during this session

* The [R Markdown book](https://bookdown.org/yihui/rmarkdown/)
* [R Markdown manuscript](http://svmiller.com/blog/2016/02/svm-r-markdown-manuscript/) has some very clear descriptions of YAML and Markdown syntax
* Google searching what you need - e.g. *footnotes r markdown*

### Extra resources for after the session

* Introduction to R Markdown [tutorial](https://ourcodingclub.github.io/tutorials/rmarkdown/)
* Coding club write your [dissertation in R Markdown](https://ourcodingclub.github.io/tutorials/rmarkdown-dissertation/)
* Write your thesis in R Markdown [short guide](https://rosannavanhespen.nl/thesis-in-rmarkdown/) which comes with [nice templates](https://github.com/rosannav/thesis_in_rmarkdown/tree/master/example_thesis)
* List of other [useful R Markdown resources](https://www.one-tab.com/page/d00HO6mxTTuqo2o7aGCffQ)

## Exercises

You recently converted a cool article about the gender pay gap in the UK to [LaTeX](https://andrewmoles2.github.io/Introduction-to-LaTeX/), which was really fun! 

The author of the article was feeling generous and also sent you the code and data used to make the figures and tables in the report. This made you wonder *'can I do this report all in one with the code and text?'*...then you remember that with R Markdown you can do just this! You decide it would be fun to convert the same article but this time use R Markdown to see the difference. Anyway, learning new skills is fun! 

Feeling organised you collate everything you need into a folder which contains:

* The pdf of the document to convert called `gender_pay_gap.pdf`
* R Markdown template called `report_template.Rmd`
* References file called `references.bib`
* An image called `pay_gap_bot.png`
* A folder called `analysis` that contains the following:
  - An image called `paygap.png`
  - Two R scripts called `paygap.R` and `sector_averages.R`
  - A data folder called `data` that contains:
    + A folder with UK grid map files (shape files) called `gb-grid` (this is used to make the `paygap.png` image)
    + A csv file called `paygap_sector_averages.csv`
    + A gpkg file called `postcode_polygons.gpkg`

  
[Click link to access the files](https://lsecloud.sharepoint.com/:f:/s/TEAM_APD-DSL-Digital-Skills-Trainers/EsWvNEn-6hNOlV_KprhhPO0BnaX9kGt818trdnJX58W7Xw?e=aa3f1X)

### Task 0 — load the template into RStudio

In RStudio go to File -> Open File and open the `report_template.Rmd` file.

You'll notice a few things. First is the section at the top that has three dashes and some text like output, author etc. This is the YAML (YAML Ain’t Markup Language) header. 

YAML in markdown is like the preamble section in LaTeX where we define what our document will look like. There are a lot of options and changes we can make. For now use this template and play around with it later! 

Before going onto the next task, press the **knit** button (or press shift+command/control+k) to compile your document so you can see what it looks like. 

### Task 1 — title page

We have a R Markdown template, first thing we want to do is to build up that title page! 

Using the `gender_pay_gap.pdf` file as your example:

* Add the title
* Add the authors
* Add the abstract

Hint: everything you change here is in the YAML header.

Note: the date will find today's date for you, which is handy. If you wanted to change it manually you can also do that. 

### Task 2 — contents

Our title page is looking good! 

Next we set up the contents page which should include a table of contents, list of figures, and list of tables. We might not have added tables or figures yet, but we will soon.

* Add a table of contents
* Add a list of figures
* Add a list of tables
* Make sure your contents page are on a separate page to the rest of the document

Do not hesitate to search for ways to do it online!

Hint: There are two ways of adding a table of contents, one is [using YAML](https://bookdown.org/yihui/rmarkdown/pdf-document.html) and the other is using LaTeX. If you get stuck check out the [LaTeX documentation via Overleaf](https://www.overleaf.com/learn/latex/Table_of_contents).

### Task 3 — organise your files

For the next tasks we will need to use the resources the author sent us. 

I recommend the following:

1) Create a new folder/directory for this workshop
2) Add your `report_template.Rmd` file and all the rest of the resources files there
3) You should see the files in the file explorer (bottom right window in Rstudio, Files tab)
4) You might get a message about file being moved or deleted. Don't worry, just open `report_template.Rmd` again and you're good to go

When you can see the files in your RStudio file viewer, click on one of them like the `pay_gap_bot.png` file to open it. If it opens, you're good to move on! 

### Task 4 — introduction

We are so organised! Now we can start adding the real content to the document. 

* Copy introduction text from the pdf into RStudio (make sure to paste in the introduction section)
* Add the links. Click on the links to get the urls
* Add the figure, which is the `pay_gap_bot.png image` file in your project
* Make sure you’ve added a caption and your image is in the centre of the page

Hint: You can use markdown syntax to add the image, but to add a caption I'd recommend using knitr for this. See [this page for an example](https://bookdown.org/yihui/rmarkdown/r-code.html#figures) about how to do this (last example with R markdown logo). The `out.width` and `fig.align` are known as *chunk options* and determine what the output of a code chunk looks like. 

Note: that to make the image the same as the example something like this in the chunk options will work: `out.width = '70%'`

#### Hold on, what are these code chunk options all about?

This section is ***strictly extra information*** which might be helpful in your future document writing endeavours. Feel free to skip over it and move on to [task 5](#methods).

R Markdown works as a combination of plain text, markdown, and code chunks. Code chunks are anything that has two sets of the three **\`\`\`**. This is true for all markdown types and is usually shown as a different colour such as grey. The cool thing about R Markdown is that is allows you to run these code chunks interactivity. To insert a new code chunk in RStudio use either Ctrl+Alt+I for Windows or Cmd+Option+I for Mac. 

The syntax for a code chunk is broken down as follows:

1) They are wrapped with \`\`\` above and below. You will see they are a different colour to the rest of the document
2) After the first \`\`\` you have curly brackets **\{\}** which contain the following:
    * Which programming language you are using which will usually be r. This is what you see by default on every code chunk
    * You can add a name of the code chunk which helps for referencing later
    * Then you have options like do you want to show the code and output, or just the output, or change the size of a output and so on
3) Then you have the actual code
4) Finally you close the code chunk with another ``` 

Below are two examples where we use some code to make a basic figure. One has chunk options, the other does not. Without chunk options we see everything. This is useful for showing collaborators, colleagues, and friends what you've done, but it is less good when you want a tidy report. 

```r
set.seed(19)
x <- runif(100)
y <- runif(100)
plot(
  x = x, y = y,
  col = ifelse(y > 0.6 | x < 0.3, '#2BB3F1','#F26B2C'),
  pch = 19,
  main = "Example plot"
)
```

![](Report_Writing_R_Markdown_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

With chunk options we can see how we can make the output more presentable if we were to use this in a report. We have added a caption, hidden the code, changed the size, and centred the output.  
<div class="figure" style="text-align: center">
<img src="Report_Writing_R_Markdown_files/figure-html/my-plot-1.png" alt="my interesting caption" width="50%" />
<p class="caption">my interesting caption</p>
</div>

The options used are listed below, which have to go within the curly brackets.

* I would first give the code chunk a name. Usually this is the first thing you add after the r. It will look something like `{r my-plot}`
* Next, we can set up some figure parameters. There are lots of options, that start with the prefix *fig.*. We've used align and cap. It looks like `{fig.align="center" and fig.cap="my interesting caption"}`
* Then we have changed the output, which starts with the *out.* prefix. We have used the width parameter which looks like `{out.width="50%"}`
* Finally we hide the code using the echo parameter, which takes true or false, and it looks like `{echo=FALSE}`

You end up with chunk options that look like: `{r my-plot, fig.align="center", fig.cap="my interesting caption", out.width="50%", echo=FALSE}`

A side note on the side note is that there are other ways to define chunk options. Rather than write them here, the author of R Markdown has written about these in his [excellent documentation](https://yihui.org/knitr/options/). 

### Task 5 — methods{#methods}

If you have not done so already knit your document again so we can see the changes we made! Remember we press the **knit** button (or press shift+command/control+k) to compile our document with R Markdown. You should see that the list of figures has appeared with an entry, yay!

The methods section has more new elements for us to try out such as equations and citations. 

* Make a new section called methods, like we have for introduction
* Add a new page between methods and introduction
* Copy the text from the methods in the pdf into your R Markdown document
* Add the url links
* Write the equations
* Add the references, all of which are in the references.bib file

Reference hint 1: there are two ways to cite in R Markdown using either `@author` or `[@author]`.

Reference hint 2: each reference in the references.bib file has a label which you use within the cite command like `@ggtext`.

Equation hint: you can use LaTeX or markdown syntax to write equations. 

#### General Note about references

The template, rather helpfully, has been set up to handle references and citations. When using R Markdown, to use references you just need to add the following two lines in the YAML header. 

```
bibliography: "references.bib"
biblio-style: apsr
```

As soon as you add in a reference, the references page will appear at the bottom of your document. 

You can change the referencing style to whatever you need and the [R Markdown book](https://bookdown.org/yihui/rmarkdown-cookbook/bibliography.html) provides a guide to this. 

### Task 6 — hyperlinks 

When you re-knit your document you should see a reference page at the bottom! 

However, you notice that the colour of the citations, and the links you added, look different to the pdf document we are trying to replicate. How do we fix this irregularity? 

Luckily this is straightforward in R Markdown and can be changed in the YAML header. Looking in the YAML header in the template you notice `urlcolor` and `linkcolor` which you suspect need to be changed. 

* Change url colours to blue
* Change link colours to red

### Task 7 — results 

Now our links are sorted, we can do the results section which has some new elements for us to think about including a table, footnotes and figure referencing. 

* Make a new section called results on a separate page from methods
* Copy the text from the results page in the pdf to your R Markdown document
* Add the footnote about tokenisation
* Add the two url links
* The copied table doesn’t work! You think writing tables from scratch in markdown seems like a lot of effort. After a Google search you find a [table convert tool](https://tableconvert.com/) which will do the hard work for you, great! Upload the `paygap_sector_average.csv` to the table converter to get your table
* Once your table is in your document make sure it has a caption; [this SO thread should help](https://stackoverflow.com/questions/33965560/r-markdown-table-with-a-caption)
* Add the figure, which is the `paygap.png` image file. Be sure to pay attention to the positioning of the figure, you might need to try different positions till it looks right
* Make sure your figure has a label
* Now you can add the figure labels using LaTeX code like `\ref{fig:chunk-label}`

Figure hint 1: the figure label in R Markdown will be the code chunk label if you use the `knitr::include_graphics` method. 

Figure hint 2: figure position is determined by a parameter in the chunk options, for example: `{r, chunk-label, out.width="80%, fig.pos = "p"}`

#### Table diversion

This section is ***strictly extra information*** which might be helpful in your future document writing endeavours. Feel free to skip over it and move on to [task 8](#dis). 

We have gone for the most basic and simple option here when adding a table, but there are many other options at our disposal because we are using R Markdown. We've listed these options below for you to look at in your own time. 

* A nice way to present statistical data, like regression outputs, is the [stargazer package](https://rpubs.com/ayyildiz/759651)
* To make tables with code there are two really good options:
  - [kableExtra](https://haozhu233.github.io/kableExtra/) which works well with pdf and HTML outputs, and is an extension of the kable function that comes with the `knitr` package
  - [gt](https://gt.rstudio.com/) and the extensions to gt called [gtExtras](https://jthomasmock.github.io/gtExtras/) and [gtsummary](https://www.danieldsjoberg.com/gtsummary/) can make very fancy tables but work best with HTML outputs

I've added two examples of what we can do using `kableExtra`, all code examples are based on the examples from the [kableExtra documentation](https://haozhu233.github.io/kableExtra/). 

This first example brings in the csv file we used for the table, then we display it using `kableExtra`. 

```{.r .klippy}
# load in kableExtra
library(kableExtra)

# load in csv
sector_avg <- read.csv("paygap_sector_averages.csv")

# make table with kableExtra
kbl(x = sector_avg, booktabs = T, 
    col.names = c("Sector", "% increase in mens wages compared to womens"),
    linesep = " ", align = "c",
    caption = "Table to show the sectors with, on average, the largest 
    percent gap in men’s wages compared to women’s wages.") %>% 
  kable_styling(latex_options = "striped") %>%
  column_spec(column = 2, 
              color = spec_color(sector_avg$percent_increase_in_mens_wages_compared_to_womens, 
                                 option = "A", 
                                 end = 0.8))
```

<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>Table to show the sectors with, on average, the largest 
    percent gap in men’s wages compared to women’s wages.</caption>
 <thead>
  <tr>
   <th style="text-align:center;"> Sector </th>
   <th style="text-align:center;"> % increase in mens wages compared to womens </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:center;"> primary </td>
   <td style="text-align:center;color: rgba(254, 159, 109, 1) !important;"> 0.2835 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> secondary </td>
   <td style="text-align:center;color: rgba(213, 68, 109, 1) !important;"> 0.2494 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> education </td>
   <td style="text-align:center;color: rgba(184, 55, 121, 1) !important;"> 0.2383 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> financial </td>
   <td style="text-align:center;color: rgba(110, 30, 129, 1) !important;"> 0.2112 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> construction </td>
   <td style="text-align:center;color: rgba(76, 17, 122, 1) !important;"> 0.1978 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> technology </td>
   <td style="text-align:center;color: rgba(63, 15, 114, 1) !important;"> 0.1932 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> information </td>
   <td style="text-align:center;color: rgba(51, 16, 104, 1) !important;"> 0.1892 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> head </td>
   <td style="text-align:center;color: rgba(2, 1, 9, 1) !important;"> 0.1640 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> offices </td>
   <td style="text-align:center;color: rgba(2, 1, 9, 1) !important;"> 0.1640 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> technical </td>
   <td style="text-align:center;color: rgba(0, 0, 4, 1) !important;"> 0.1615 </td>
  </tr>
</tbody>
</table>

This second example uses the code from the `sector_averages.R` script, then we display the output using `kableExtra`. I've included the code here but you can use chunk options to stop the code showing, such as using `echo=FALSE`. I've also included `message=FALSE` and `warning=FALSE` in as chunk options so we don't get loading messages and such. 

```{.r .klippy}
# load libraries
library(tidytext)
library(tidyverse)

# read in the data
paygap <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-06-28/paygap.csv')
uk_sic_codes <- read_csv("https://assets.publishing.service.gov.uk/government/uploads/system/uploads/attachment_data/file/527619/SIC07_CH_condensed_list_en.csv") %>%
  janitor::clean_names()

# clean the sic codes in pay gap, then join with our sic codes data
paygap_joined <- paygap %>%
  select(employer_name, diff_median_hourly_percent, sic_codes) %>%
  separate_rows(sic_codes, sep = ":") %>%
  left_join(uk_sic_codes, by = c("sic_codes" = "sic_code"))

# tokenise, and remove stop words
paygap_token <- paygap_joined %>%
  unnest_tokens(word, description) %>%
  anti_join(get_stopwords()) %>%
  na.omit()

# count tokens and pull out top 50
top_words <- paygap_token %>%
  count(word) %>%
  filter(!word %in% c("activities", "n.e.c", "general", "non")) %>%
  slice_max(n, n = 50) %>%
  pull(word)

# calculate wage differences
paygap_final <- paygap_token %>%
  filter(word %in% top_words) %>%
  transmute(
    diff_wage = diff_median_hourly_percent / 100, 
    word) %>%
  group_by(word) %>%
  summarise(diff_wage = round(mean(diff_wage), digits = 4)) %>%
  arrange(desc(diff_wage)) %>%
  rename(sector = word, percent_increase_in_mens_wages_compared_to_womens = diff_wage)

# just the top 10
paygap_top <- paygap_final %>% slice_head(n = 10)

# make kableExtra table
kbl(x = paygap_top, booktabs = T, 
    col.names = c("Sector", "% increase in mens wages compared to womens"),
    linesep = " ", align = "c",
    caption = "Table to show the sectors with, on average, the largest 
    percent gap in men’s wages compared to women’s wages.") %>% 
  kable_styling(latex_options = "striped") %>%
  column_spec(column = 2, 
              color = spec_color(sector_avg$percent_increase_in_mens_wages_compared_to_womens, 
                                 option = "A", 
                                 end = 0.8))
```

<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>Table to show the sectors with, on average, the largest 
    percent gap in men’s wages compared to women’s wages.</caption>
 <thead>
  <tr>
   <th style="text-align:center;"> Sector </th>
   <th style="text-align:center;"> % increase in mens wages compared to womens </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:center;"> primary </td>
   <td style="text-align:center;color: rgba(254, 159, 109, 1) !important;"> 0.2835 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> secondary </td>
   <td style="text-align:center;color: rgba(213, 68, 109, 1) !important;"> 0.2494 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> education </td>
   <td style="text-align:center;color: rgba(184, 55, 121, 1) !important;"> 0.2383 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> financial </td>
   <td style="text-align:center;color: rgba(110, 30, 129, 1) !important;"> 0.2112 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> construction </td>
   <td style="text-align:center;color: rgba(76, 17, 122, 1) !important;"> 0.1978 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> technology </td>
   <td style="text-align:center;color: rgba(63, 15, 114, 1) !important;"> 0.1932 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> information </td>
   <td style="text-align:center;color: rgba(51, 16, 104, 1) !important;"> 0.1892 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> head </td>
   <td style="text-align:center;color: rgba(2, 1, 9, 1) !important;"> 0.1640 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> offices </td>
   <td style="text-align:center;color: rgba(2, 1, 9, 1) !important;"> 0.1640 </td>
  </tr>
  <tr>
   <td style="text-align:center;"> technical </td>
   <td style="text-align:center;color: rgba(0, 0, 4, 1) !important;"> 0.1615 </td>
  </tr>
</tbody>
</table>

I should also note that the above idea of including your scripts in your document also applies to the `paygap.png` image we added which is generated from the `paygap.R` script. 

### Task 8 — discussion{#dis}

We are almost there, just the discussion left. Two new features here are a quote and a list.

* Make a new section called discussion on a separate page from results
* Copy the text from the discussion page in the pdf to your R Markdown document
* Format the quote properly using our recommended resources for help
* Add the footnote
* Format the list using our recommended resources for help

Wow, we’ve just taken a pdf document and converted it to R Markdown!

#### Word count tip

Last but not least you might be interested in either adding in or calculating your word count. This is not the most straightforward in R Markdown, but there is a nice workaround. 

Copy and paste this code and add it into your document. Make sure it is **not** in a code cell. This is known as *inline code* and produces the following: **Word count**: 4069. You will likely get a different word count!

```{.r .klippy}
**Word count**: `r as.integer(sub("(\\d+).+$","\\1",system(sprintf("wc -w %s", knitr::current_input()),intern=TRUE)))-20`
```

### Final task — take our survey

Link to survey here

## What's next?

Have a go at the take home challenge, or set your own challenge like converting a report or document you've previously written into R Markdown or LaTeX. You can try and use R Markdown or LaTeX for reports you write this year which is great opportunity to learn. 

You also might wonder if there are other options than LaTeX and R Markdown. For example, what if you are not primarily an R user but want to get all the great features of R Markdown? These days you can also write Python as well as other code in R (and R Markdown), for example to use Python you use the [reticulate package](https://rstudio.github.io/reticulate/). There are also other options available. 

This is where [Quarto](https://quarto.org/) comes in. Quarto® is an open-source scientific and technical publishing system built on Pandoc that was released on 2022. It is designed to work just like R Markdown, including all the great R Markdown features as well as lots of fun extras. It can build reports, websites, presentations and more with code from Python, R, Julia, and Observable (JavaScript). It also works with RStudio, Jupyter Notebooks, and VS code so it is very flexible. 

We should also mention that Jupyter Notebooks has the ability to write publication ready reports using the [Jupyter book extension](https://jupyterbook.org/en/stable/intro.html). If you generally prefer Jupyter Notebooks then it is worth having a look, but from my experience it is less user-friendly and flexible than R Markdown and Quarto. I'd recommend using Quarto with Jupyter Notebooks from within [Visual Studio Code](https://code.visualstudio.com/) (VS Code) to get the best possible experience. Check out the [VS Code Jupyter extension](https://code.visualstudio.com/docs/datascience/jupyter-notebooks) and how to use it [with Quarto](https://quarto.org/docs/get-started/hello/vscode.html); you can also do this with R and Julia. 

### Take home challenge

Write your CV using R Markdown with the [vitae](https://github.com/mitchelloharawild/vitae) package! This is a very convenient way of writing and keeping your CV up to date as it is *data driven*. What this basically means is you keep all the information about your work, skills, experiences and so on in an Excel file (or Google sheets), which you load into R Markdown to create a cool CV. 

There are lots of good examples on the vitae GitHub page, but two I'd recommend are from [Lorena Abad](https://github.com/loreabad6/R-CV) and [Shazia Ruybal-Pesántez](https://github.com/shaziaruybal/cv). 

My suggestion on getting started is the following:

1) Install vitae with `install.packages('vitae')`
2) Instead of using the examples with the package, use [Shazia Ruybal-Pesántez's](https://github.com/shaziaruybal/cv) CV as your template. To do this, click on `Code (green button) -> Download ZIP` which will download the files for you 
3) In the data folder you will find `cv_data.xlsx`. You can change this so it has your experience and skills in it
4) Load up the `cv-vitae.Rmd` file and change what you need, regularly compiling (knitting) to see what it looks like
5) To make your CV exciting this is where [Lorena Abad's](https://github.com/loreabad6/R-CV) CV comes in. She wrote some custom formatting options in the `awesome-cv.cls` file. You can copy the contents of the file on her GitHub and paste it into your local copy of the `awesome-cv.cls` file. Knit your report to see the difference. 
6) Now have a look through her [rmd file](https://github.com/loreabad6/R-CV/blob/master/CV.Rmd). She has icons with the headers which is a nice touch such as a briefcase with her work experience which looks like: `\faIcon{briefcase} Professional Experience`. Try and add some of these into your CV. *hint: You'll likely need the fontawesome package for this: https://github.com/rstudio/fontawesome*
