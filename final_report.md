final_report.md
================
Soobin Choi
2022-12-12

- <a href="#introduction" id="toc-introduction">Introduction</a>
- <a href="#methodology-and-data"
  id="toc-methodology-and-data">Methodology and Data</a>
- <a href="#analysis" id="toc-analysis">Analysis</a>
  - <a href="#intragroup-analysis" id="toc-intragroup-analysis">Intragroup
    analysis</a>
    - <a href="#korean-learner-corpus" id="toc-korean-learner-corpus">Korean
      Learner Corpus</a>
  - <a href="#intergroup-analysis" id="toc-intergroup-analysis">Intergroup
    analysis</a>
- <a href="#conclusion" id="toc-conclusion">Conclusion</a>

# Introduction

The Foreign Service Institute (FSI) reports that Korean is one of the
most difficult languages to learn for English-speaking students. In
order to obtain a certain degree of proficiency in the Korean language,
FSI claims that it takes 88 weeks for those whose first language is
English. This can be interpreted the other way around: English is one of
the most difficult languages to learn for those whose first language is
Korean. Based on this fact, this project seeks to determine if there is
any difference in language learning development between each group of L2
learners due to the typological difference between English and Korean.

# Methodology and Data

To evaluate the level of language proficiency and provide a comparison
within each group of L2 learners, two indicators are used: syntactic
complexity and lexical diversity. Syntax complexity refers to the length
of sentences. It is used to measure the ability to construct complex and
long sentences in a target language. The concept of lexical diversity
relates to words. It provides information regarding the vocabulary range
of learners. For the purpose of the project, two corpora are selected:
Korean Learner Corpus (Park 2016), and the University of Pittsburgh
English Language Institute Corpus (PELIC) (Juffs, Han and Naismith
2020). Both corpora fit into this project in that they contain the
information of the level of proficiency of each speaker with the same
criteria (Common European Framework of Reference for Languages (CEFR)),
and the raw data (essays of each speaker). Due to the differences in
language typology, different tokenization methodologies are deployed for
each language. English language, as an analytic language, can be
tokenized by white space because it is generally the case that each unit
divided by white space is a word. Korean language, however, as a
synthetic language, cannot be tokenized in the same way as English
because one word segment often contains more than one word. Therefore,
in Korean NLP, words are tokenized by morpheme boundaries.

# Analysis

## Intragroup analysis

### Korean Learner Corpus

1.  Syntactic complexity

![Korean Learners’ Syntactic Complexity by
Level](https://github.com/Data-Sci-2022/Learning-Curve-Comparison/raw/main/visual_aids/KLC_SC.pdf)

## Intergroup analysis

# Conclusion