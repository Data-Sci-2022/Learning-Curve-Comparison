1st Progress Report
================
Soobin Choi
3 November 2022

- <a href="#progress-report" id="toc-progress-report">Progress Report</a>
  - <a href="#progress-in-general" id="toc-progress-in-general">Progress in
    general</a>
  - <a href="#sharing-plan" id="toc-sharing-plan">sharing plan</a>
  - <a href="#updates" id="toc-updates">Updates</a>
    - <a href="#progress-report-1-11-09-2022"
      id="toc-progress-report-1-11-09-2022">Progress report 1 (11-09-2022)</a>
    - <a href="#progress-report-2-11-14-2022"
      id="toc-progress-report-2-11-14-2022">Progress report 2 (11-14-2022)</a>

# Progress Report

## Progress in general

I was able to import both korean-learner-corpus (KLC) and PELIC data
from github and finish sorting out what I need for my project as in the
file ‘interim check’.

Also, I tokenized the korean-learner-corpus text, but the problem that I
face here is, I am not really sure how I can count the tokens in one
text. I am thinking about using count() and then sum the number of
tokens with the same user id, but I haven’t tried yet. Maybe I will get
a lot of errors or unwanted result. Also, I am not sure how I can count
the morphemes in KLC. I believe I can just count the number of
slashes(/) in the corpus since they marked morpheme boundaries with
slash, but I haven’t figured this out yet.

Luckily, for PELIC, the text is already tokenized and even the number of
tokens of each text is in the data. So, it will definitely facilitate my
project. The problem I faced when mutating the PELIC data is that the
information I need is quite scattered in different data sets. So I had
to import almost all of the data set and mutate then using join()
function.

After I figure out how to count the tokens in KLC, I believe (and hope)
there would not be many issues.

## sharing plan

Regarding Korean learner corpus, I am not very sure at this moment
because the data owner does not specify the lisence in the repository.
Since the repository is in public domain, it might be okay to use the
data and share the result, but I think it would be a better approach to
reach out to the owner and asks his permission.

## Updates

### Progress report 1 (11-09-2022)

#### KLC

sorting out the data based on the nationality counting tokens using
`unnest_tokes()`

#### PELIC

merging datasets with the necessary columns only filtering data based on
text length (larger than 10) sorting based on the native language (only
korean)

### Progress report 2 (11-14-2022)

I am struggling with collecting word types from the tokens. I am not
sure how I can use `unique()` or `duplicate()` function on the tokens.
Because of this, I could not move further. Also, in the case of PELIC, I
need to make another column that only contains lemma and POS, which I am
still figuring out. I need Dan’s help here.

#### KLC

tokenize and count the tokens for each essay. I’m sort of stuck here
because I do not know how to count word type. How can I use unique() or
distinct function to the values whose type is list?

#### PELIC

Changing `full_join()` to `left_join()` to make the data smaller
