Project Plan_Soobin
================
Soobin Choi
2022-10-01

-   <a
    href="#comparing-english-speakers-korean-learning-development-with-korean-speakers-english-learning-development"
    id="toc-comparing-english-speakers-korean-learning-development-with-korean-speakers-english-learning-development">Comparing
    English speakers’ Korean learning development with Korean speakers’
    English learning development.</a>
    -   <a href="#brief-summary" id="toc-brief-summary">Brief summary</a>
    -   <a href="#data" id="toc-data">Data</a>
    -   <a href="#hypothesis" id="toc-hypothesis">Hypothesis</a>
    -   <a href="#r-packages-needed-for-analysis"
        id="toc-r-packages-needed-for-analysis">R packages needed for
        analysis</a>
    -   <a href="#presentation" id="toc-presentation">Presentation</a>

# Comparing English speakers’ Korean learning development with Korean speakers’ English learning development.

## Brief summary

I would like to compare the process of the development of L2 learners
with different background. According to Foreign Service Institute (FIS),
for English native speakers, Korean is one of the most difficult
languages to learn. Same thing would apply to Koreans; one of the most
difficult languages to learn for Koreans is English. So, I would like to
compare each group of speakers’ development process in terms of lexical
diversity and syntactic complexity based on their L2 level.

## Data

For this project, I will use two datasets, which are PELIC, and a data
set from an article about Korean L2 writing. Below is the article’s
title and its authors:

Lim, K., Song, J., & Park, J. (2022). Neural automated writing
evaluation for Korean L2 writing. Natural Language Engineering,
*Cambridge University Press, 1-23.*
<https://doi.org/10.1017/S1351324922000298>. Lim, K. and Song, J.
contributed equally.

Since both corpora contains student identifier code, their level of
target language, and the tokens of their essay, so they perfectly fit
into the purpose of this project.

## Hypothesis

Since Korean is a synthetic language and has a rich vocabulary pool of
honorifics which are reflected to morphosyntactic markings of verbs,
there are numerous morphemes that can be considered conjunctive
particles. Therefore, I assume that at the relatively lower level of L2
speakers, English learners have a higher degree of syntactic complexity.
However, for the upper level of L2 speakers, Korean learners shows
higher level of syntactic complexity. In terms of lexical diversity, I
assume no difference between two groups.

## R packages needed for analysis

Fortunately, the data I will be using for this project are already
organized and accessible to manipulate. In addition, they are all either
.csv or .tsv file, whch can be easily readable in RStudio. Therefore, I
believe (and hope) that `dyplr` and `tidyverse` would be more than
enough.

## Presentation

I am not sure at this moment which way would be the most efficient to
demonstrate what I will find, but I believe I will be comparing each
levels of each target language and their lexical diversity and syntactic
complexity, making graphs and plots with `tidyverse`.
