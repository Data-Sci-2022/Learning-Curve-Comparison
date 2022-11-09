Project Plan_Soobin
================
Soobin Choi
2022-10-01

``` r
knitr::opts_chunk$set(echo=TRUE, include=TRUE, comment="")
library(tidyverse)
library(dplyr)
library(tidytext)
library(tidyr)
```

``` r
KLC <- read_tsv(file = "https://github.com/jungyeul/korean-learner-corpus/raw/main/data/kyunghee_v2.tsv", locale(encoding = "UTF-8")) 
```

    Warning: One or more parsing issues, see `problems()` for details

    Rows: 4094 Columns: 8
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: "\t"
    chr (7): X1, X2, X3, X4, X5, X6, X7
    dbl (1): X8

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
KLC <- KLC %>% 
  rename(ID = X1,
         Nationality = X2,
         Gender = X3,
         Topic = X4,
         Text = X5,
         Morphemes = X6,
         Level = X7,
         Score = X8)
KLC_clean <- KLC %>% 
  filter(Level %in% c("A1", "A2", "B1", "B2", "C1", "C2"))

KLC_clean %>% 
  filter(Level == "C2")
```

    # A tibble: 289 × 8
       ID                    Nationality Gender Topic      Text  Morph…¹ Level Score
       <chr>                 <chr>       <chr>  <chr>      <chr> <chr>   <chr> <dbl>
     1 C200000_v01.txt_1.txt 홍콩        여자   공공장소   2011… +년/NN… C2       80
     2 C200000_v01.txt_2.txt 홍콩        여자   문화       홍콩… 홍콩/N… C2       70
     3 C200000_v02.txt_1.txt 홍콩        여자   건강       건강… 건강/N… C2       70
     4 C200000_v02.txt_2.txt 홍콩        여자   안락사     안락… 안락사… C2       80
     5 C200001_v01.txt_1.txt 중국        여자   친구 한 …  제 …  제/MM … C2       60
     6 C200001_v01.txt_2.txt 중국        여자   이 시대에… 이 …  이/MM … C2       70
     7 C200001_v02.txt_1.txt 일본        여자   건강       음식… 음식/N… C2       90
     8 C200001_v02.txt_2.txt 일본        여자   안락사     안락… 안락사… C2      100
     9 C200002_v01.txt_1.txt 콜롬비아    여자   친구 한 …  그 …  그/MM … C2       90
    10 C200002_v01.txt_2.txt 콜롬비아    여자   이 시대에… 이 …  이/MM … C2       90
    # … with 279 more rows, and abbreviated variable name ¹​Morphemes

``` r
KLC_eng <- KLC_clean %>% 
  filter(Nationality == "미국"
        | Nationality == "영국" 
        | Nationality == "호주"
        | Nationality == "필리핀"
        | Nationality == "싱가포르"
        | Nationality == "인도"
        | Nationality == "르완다")

KLC_eng %>% 
  select(ID, Nationality, Text) %>% 
  unnest_tokens(word, Text) %>% 
  group_by(ID)
```

    # A tibble: 7,751 × 3
    # Groups:   ID [81]
       ID                    Nationality word      
       <chr>                 <chr>       <chr>     
     1 A100003_v01.txt_1.txt 미국        우리      
     2 A100003_v01.txt_1.txt 미국        집에      
     3 A100003_v01.txt_1.txt 미국        갔습니다  
     4 A100003_v01.txt_1.txt 미국        강아지하고
     5 A100003_v01.txt_1.txt 미국        제        
     6 A100003_v01.txt_1.txt 미국        집에      
     7 A100003_v01.txt_1.txt 미국        갔습니다  
     8 A100003_v01.txt_1.txt 미국        우리가    
     9 A100003_v01.txt_1.txt 미국        놀았습니다
    10 A100003_v01.txt_1.txt 미국        저는      
    # … with 7,741 more rows

``` r
KLC_eng %>% 
  select(ID,Nationality, Text)
```

    # A tibble: 81 × 3
       ID                    Nationality Text                                       
       <chr>                 <chr>       <chr>                                      
     1 A100003_v01.txt_1.txt 미국        우리 집에 갔습니다. 강아지하고 제 집에 갔… 
     2 A100004_v01.txt_1.txt 미국        저는 여행을 좋아합니다. 십이월에 여행을 가…
     3 A100011_v03.txt_1.txt 영국        저는 방학 기간에 여행을 가고 싶습니다. 시… 
     4 A100074_v03.txt_1.txt 호주        어제가 부산에서 갔어요. 11시에 부신에서 도…
     5 A100140_v02.txt_1.txt 미국        일요일에 명동에 갔습니다. 제 친구하고 명동…
     6 A100148_v02.txt_1.txt 미국        이번 주말에 저는 영화을 가고 싶습니다. 토… 
     7 A100154_v02.txt_1.txt 필리핀      저는 필리핀에서 갔습니다. 저 친구하고 갔습…
     8 A100166_v02.txt_1.txt 필리핀      저는 여행에 부산에 갈 겁니다. 일요일 다음 …
     9 A100234_v02.txt_1.txt 필리핀      지난 주말에 인사동에 갔습니다. 택시 정류소…
    10 A100235_v02.txt_1.txt 필리핀      다음 주에 여행이 하고 싶습니다. 우리는 부… 
    # … with 71 more rows

When filtering nationality of participants, I was not sure what sort of
criterium I should apply. I know a huge part of the population in
Thailand speaks English, then should I include them too?

So, I ended up sorting out the countries whose official language
includes English. It would have been easier for me to sort out if there
is a column about their first language, not their nationality.

\*\* Importing Data

``` r
PELIC_ans <- read_csv("https://github.com/ELI-Data-Mining-Group/PELIC-dataset/raw/master/corpus_files/answer.csv")
```

    Rows: 46204 Columns: 10
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    chr  (4): anon_id, text, tokens, tok_lem_POS
    dbl  (5): answer_id, question_id, course_id, version, text_len
    dttm (1): created_date

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
PELIC_crs <- read_csv("https://github.com/ELI-Data-Mining-Group/PELIC-dataset/raw/master/corpus_files/course.csv")
```

    Rows: 1066 Columns: 5
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    chr (3): class_id, semester, section
    dbl (2): course_id, level_id

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
PELIC_id <- read_csv("https://github.com/ELI-Data-Mining-Group/PELIC-dataset/raw/master/corpus_files/student_information.csv")
```

    Rows: 1313 Columns: 21
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    chr (19): anon_id, gender, native_language, language_used_at_home, non_nativ...
    dbl  (2): birth_year, age

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
PELIC_scr <- read_csv("https://github.com/ELI-Data-Mining-Group/PELIC-dataset/raw/master/corpus_files/test_scores.csv")
```

    Rows: 1141 Columns: 10
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    chr (3): anon_id, semester, MTELP_Form
    dbl (7): LCT_Form, LCT_Score, MTELP_I, MTELP_II, MTELP_III, MTELP_Conv_Score...

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

\*\* sorting out columns needed

``` r
PELIC_scr1 <- PELIC_scr %>% 
  select(anon_id, MTELP_Conv_Score, Writing_Sample)

PELIC_crs1 <- PELIC_crs %>% 
  select(course_id, level_id)

PELIC_ans1 <- PELIC_ans %>%
  select(anon_id, course_id, text_len, text, tokens)
  
PELIC_id1 <- PELIC_id %>% 
  select(anon_id, native_language)
```

\*\* Joining all the columns in one dataframe

``` r
PELIC1 <- full_join(PELIC_ans1, PELIC_crs1, by = "course_id")

PELIC2 <- full_join(PELIC_ans1, PELIC_scr1, PELIC_id1, by = "anon_id")

PELIC1 <- PELIC1 %>%
  select(anon_id, level_id)

PELIC_clean <- full_join(PELIC1,PELIC2, by = "anon_id")


PELIC_clean
```

    # A tibble: 3,843,499 × 8
       anon_id level_id course_id text_len text               tokens MTELP…¹ Writi…²
       <chr>      <dbl>     <dbl>    <dbl> <chr>              <chr>    <dbl>   <dbl>
     1 eq0            4       149      177 "I met my friend … "['I'…      28       1
     2 eq0            4       113       48 "My favorite reci… "['My…      28       1
     3 eq0            4       149      113 "Welcome! I'm ANO… "['We…      28       1
     4 eq0            4       101       24 "commercial:\nTod… "['co…      28       1
     5 eq0            4       101       46 "commercial:\nTod… "['co…      28       1
     6 eq0            4       149      119 "I feel good when… "['I'…      28       1
     7 eq0            4       149       36 "2. Elena Garro, … "['2'…      28       1
     8 eq0            4       101      207 "1. \n- Word: res… "['1'…      28       1
     9 eq0            4       113      275 "ANON_NAME_0's Re… "['AN…      28       1
    10 eq0            4       176      364 "The Best Way to … "['Th…      28       1
    # … with 3,843,489 more rows, and abbreviated variable names ¹​MTELP_Conv_Score,
    #   ²​Writing_Sample
