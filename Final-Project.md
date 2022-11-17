Final Project
================
Soobin Choi
2022-10-01

- <a href="#developmental-process-of-l2---english-and-korean"
  id="toc-developmental-process-of-l2---english-and-korean">Developmental
  process of L2 - English and Korean</a>
  - <a href="#korean-learners-corpus-klc"
    id="toc-korean-learners-corpus-klc">Korean Learners’ Corpus (KLC)</a>
    - <a href="#data-import" id="toc-data-import">Data Import</a>
    - <a href="#data-manipulation" id="toc-data-manipulation">Data
      Manipulation</a>
    - <a href="#tokenization" id="toc-tokenization">Tokenization</a>
  - <a href="#pelic" id="toc-pelic">PELIC</a>
    - <a href="#data-import-1" id="toc-data-import-1">Data Import</a>
    - <a href="#data-manipulation-1" id="toc-data-manipulation-1">Data
      manipulation</a>

# Developmental process of L2 - English and Korean

``` r
knitr::opts_chunk$set(echo=TRUE, include=TRUE, comment="")
library(tidyverse)
library(tidytext)
library(repurrrsive)
```

## Korean Learners’ Corpus (KLC)

### Data Import

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

### Data Manipulation

``` r
# change column names with clearer description
KLC <- KLC %>% 
  rename(ID = X1,
         Nationality = X2,
         Gender = X3,
         Topic = X4,
         Text = X5,
         Morphemes = X6,
         Level = X7,
         Score = X8)

# remove some values with error
KLC_clean <- KLC %>% 
  filter(Level %in% c("A1", "A2", "B1", "B2", "C1", "C2"))
```

``` r
# filter the data with nationality.
KLC_eng <- KLC_clean %>% 
  filter(Nationality %in% c("미국", "영국", "호주", "필리핀", "싱가포르", "인도", "르완다"))

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

``` r
KLC_token <- KLC_eng %>% 
  unnest_tokens(word, Text) %>% 
  count(ID, name = "token_num")

# 모르겠으면 F1을 눌러보자ㅎㅎ

KLC_inner <- KLC_clean %>% 
  filter(Nationality %in% c("미국", "영국", "호주"))
```

When filtering nationality of participants, I was not sure what sort of
criterium I should apply. I know a huge part of the population in
Thailand speaks English, then should I include them too?

So, I ended up sorting out the countries whose official language
includes English. It would have been easier for me to sort out if there
is a column about their first language, not their nationality.

### Tokenization

``` r
KLC_tok <- KLC_eng %>% 
  select(ID, Nationality, Text) %>% 
  unnest_tokens(word, Text) %>% 
  group_by(ID)

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

#### 1. Lexical Diversity

how to calculate lexical diversity: how many different words are used in
each person’s essay?; the number of *word type* divided by the number of
*tokens*

``` r
KLC_eng$Morphemes %>%
  map(~ str_split(., " (\\+)?")) %>% 
  head(3)
```

    [[1]]
    [[1]][[1]]
     [1] "우리/PRON"  "집/NNG"     "에/JKB"     "가/VV"      "았/EP"     
     [6] "습니다/EF"  "강아지/NNG" "하/XSV"     "고/EC"      "제/XPN"    
    [11] "집/NNG"     "에/JKB"     "가/VV"      "았/EP"      "습니다/EF" 
    [16] "우리/PRON"  "가/JKS"     "놀/VV"      "았/EP"      "습니다/EF" 
    [21] "저/PRON"    "는/JX"      "줌/NNG"     "을/JKO"     "자/VV"     
    [26] "지/EC"      "않/VX"      "았/EP"      "습니다/EF"  "집/NNG"    
    [31] "에/JKB"     "제침대/NNG" "를/JKO"     "있/VV"      "습니다/EF" 
    [36] "집/NNG"     "에/JKB"     "강아지/NNG" "침대/NNG"   "도/JX"     
    [41] "있/VV"      "습니다/EF"  "하지만/MAJ" "우리/PRON"  "가/JKS"    
    [46] "줌/NNG"     "을/JKO"     "안/MAG"     "자/VV"      "ㅂ니다/EF" 
    [51] "그리고/MAJ" "강아지/NNG" "하/XSV"     "고/EC"      "밥/NNG"    
    [56] "을/JKO"     "먹/VV"      "었/EP"      "습니다/EF"  "저/PRON"   
    [61] "는/JX"      "빵/NNG"     "을/JKO"     "먹/VV"      "었/EP"     
    [66] "습니다/EF"  "제/XPN"     "강아지/NNG" "가/JKS"     "동물/NNG"  
    [71] "음식/NNG"   "을/JKO"     "먹/VV"      "었/EP"      "습니다/EF" 
    [76] "모두/MAG"   "맛있/VA"    "었/EP"      "습니다/EF"  "저/PRON"   
    [81] "는/JX"      "재미있/VA"  "었/EP"      "습니다/EF" 


    [[2]]
    [[2]][[1]]
     [1] "저/PRON"    "는/JX"      "여행/NNG"   "을/JKO"     "좋아하/VV" 
     [6] "ㅂ니다/EF"  "십이월/NNG" "에/JKB"     "여행/NNG"   "을/JKO"    
    [11] "가/VV"      "고/EC"      "싶/VX"      "습니다/EF"  "일본/NNP"  
    [16] "에/JKB"     "가/VV"      "고/EC"      "싶/VX"      "습니다/EF" 
    [21] "저/PRON"    "하고/JKB"   "제/MM"      "친구/NNG"   "제니퍼/NNP"
    [26] "를/JKO"     "가/VV"      "ㄹ/ETM"     "것/NNB"     "이/VCP"    
    [31] "ㅂ니다/EF"  "우리/PRON"  "가/JKS"     "한국/NNP"   "에/JKB"    
    [36] "있/VV"      "아서/EC"    "비행기/NNG" "로/JKB"     "타/VV"     
    [41] "ㄹ/ETM"     "것/NNB"     "이/VCP"     "ㅂ니다/EF"  "일본/NNP"  
    [46] "에서/JKB"   "쇼핑/NNG"   "하/XSV"     "고/EC"      "먹/VV"     
    [51] "을/ETM"     "것/NNB"     "이/VCP"     "ㅂ니다/EF"  "저/PRON"   
    [56] "는/JX"      "웃/NNG"     "하/XSV"     "고/EC"      "화정펌/NNG"
    [61] "을/JKO"     "사고/NNG"   "싶/VX"      "습니다/EF"  "저/PRON"   
    [66] "는/JX"      "일본/NNP"   "음식/NNG"   "을/JKO"     "좋아하/VV" 
    [71] "니까/EC"    "많이/MAG"   "먹/VV"      "을/ETM"     "것/NNB"    
    [76] "이/VCP"     "ㅂ니다/EF"  "재미있/VA"  "을/ETM"     "것/NNB"    
    [81] "이/VCP"     "ㅂ니다/EF" 


    [[3]]
    [[3]][[1]]
      [1] "저/PRON"    "는/JX"      "방학/NNG"   "기간/NNG"   "에/JKB"    
      [6] "여행/NNG"   "을/JKO"     "가/VV"      "고/EC"      "싶/VX"     
     [11] "습니다/EF"  "시간/NNG"   "이/JKS"     "작/VA"      "아서/EC"   
     [16] "멀어/NNG"   "이/VCP"     "지/EC"      "않/VX"      "는/ETM"    
     [21] "일본/NNP"   "에/JKB"     "가/VV"      "ㄹ/ETM"     "것/NNB"    
     [26] "이/VCP"     "ㅂ니다/EF"  "동생/NNG"   "이/JKS"     "한국/NNP"  
     [31] "에/JKB"     "오/VV"      "아서/EC"    "같이/MAG"   "일본/NNP"  
     [36] "에/JKB"     "가/VV"      "ㄹ/ETM"     "것/NNB"     "이/VCP"    
     [41] "ㅂ니다/EF"  "한국/NNP"   "에서/JKB"   "일본/NNP"   "까지/JX"   
     [46] "비행기/NNG" "로/JKB"     "가/VV"      "ㄹ/ETM"     "것/NNB"    
     [51] "이/VCP"     "ㅂ니다/EF"  "아마/MAG"   "세/MM"      "시간/NNG"  
     [56] "정도/NNG"   "걸리/VV"    "ㅂ니다/EF"  "저/PRON"    "는/JX"     
     [61] "동생/NNG"   "과/JC"      "일본/NNP"   "음식/NNG"   "이/JKS"    
     [66] "아주/MAG"   "좋아하/VV"  "ㅂ니다/EF"  "동생/NNG"   "하고/JKB"  
     [71] "같이/MAG"   "일식/NNG"   "을/JKO"     "먹/VV"      "고/EC"     
     [76] "쇼핑/NNG"   "을/JKO"     "하/VV"      "ㄹ/ETM"     "것/NNB"    
     [81] "이/VCP"     "ㅂ니다/EF"  "그리고/MAJ" "구경/NNG"   "도/JX"     
     [86] "하/VV"      "고/EC"      "싶/VX"      "습니다/EF"  "일본/NNP"  
     [91] "에서/JKB"   "예쁘/VA"    "ㄴ/ETM"     "장소/NNG"   "가/JKS"    
     [96] "많/VA"      "습니다/EF"  "제/PRON"    "가/JKS"     "좋아하/VV" 
    [101] "는/ETM"     "나사/NNG"   "에/JKB"     "가/VV"      "ㄹ/ETM"    
    [106] "수/NNB"     "있/VV"      "어서/EC"    "기분/NNG"   "이/JKS"    
    [111] "좋/VA"      "을/ETM"     "것/NNB"     "이/VCP"     "ㅂ니다/EF" 

``` r
KLC_clean <- KLC_eng %>%
  select(ID, Morphemes) %>% 
  map(~ str_split(., " (\\+)?")) %>% 
  as_data_frame() %>% 
  rename(num_token = Morphemes) %>% 
  unnest(ID) %>% 
  left_join(KLC_eng, .,  by="ID")
```

    Warning: `as_data_frame()` was deprecated in tibble 2.0.0.
    Please use `as_tibble()` instead.
    The signature and semantics have changed, see `?as_tibble`.
    This warning is displayed once every 8 hours.
    Call `lifecycle::last_lifecycle_warnings()` to see where this warning was generated.

``` r
nrow(unique(unnest(KLC_clean[1,9])))
```

    Warning: `cols` is now required when using unnest().
    Please use `cols = c(num_token)`

    [1] 37

``` r
unique(unnest(KLC_clean[2,9]))
```

    Warning: `cols` is now required when using unnest().
    Please use `cols = c(num_token)`

    # A tibble: 41 × 1
       num_token 
       <chr>     
     1 저/PRON   
     2 는/JX     
     3 여행/NNG  
     4 을/JKO    
     5 좋아하/VV 
     6 ㅂ니다/EF 
     7 십이월/NNG
     8 에/JKB    
     9 가/VV     
    10 고/EC     
    # … with 31 more rows

``` r
unique(unnest(KLC_clean[3,9]))
```

    Warning: `cols` is now required when using unnest().
    Please use `cols = c(num_token)`

    # A tibble: 61 × 1
       num_token
       <chr>    
     1 저/PRON  
     2 는/JX    
     3 방학/NNG 
     4 기간/NNG 
     5 에/JKB   
     6 여행/NNG 
     7 을/JKO   
     8 가/VV    
     9 고/EC    
    10 싶/VX    
    # … with 51 more rows

``` r
KLC_clean %>% 
  group_by(ID) %>% 
  select(num_token) %>% 
  unnest() %>% 
  unique() 
```

    Adding missing grouping variables: `ID`

    Warning: `cols` is now required when using unnest().
    Please use `cols = c(num_token)`

    # A tibble: 7,643 × 2
    # Groups:   ID [81]
       ID                    num_token 
       <chr>                 <chr>     
     1 A100003_v01.txt_1.txt 우리/PRON 
     2 A100003_v01.txt_1.txt 집/NNG    
     3 A100003_v01.txt_1.txt 에/JKB    
     4 A100003_v01.txt_1.txt 가/VV     
     5 A100003_v01.txt_1.txt 았/EP     
     6 A100003_v01.txt_1.txt 습니다/EF 
     7 A100003_v01.txt_1.txt 강아지/NNG
     8 A100003_v01.txt_1.txt 하/XSV    
     9 A100003_v01.txt_1.txt 고/EC     
    10 A100003_v01.txt_1.txt 제/XPN    
    # … with 7,633 more rows

``` r
x <- rerun(2, sample(4))
x
```

    [[1]]
    [1] 2 4 1 3

    [[2]]
    [1] 2 4 1 3

``` r
unlist(x)
```

    [1] 2 4 1 3 2 4 1 3

``` r
x %>% 
  as_tibble(.name_repair = "universal")
```

    New names:
    • `` -> `...1`
    • `` -> `...2`

    # A tibble: 4 × 2
       ...1  ...2
      <int> <int>
    1     2     2
    2     4     4
    3     1     1
    4     3     3

``` r
x %>% flatten() %>% 
  as_tibble(.name_repair = "universal")
```

    New names:
    • `` -> `...1`
    • `` -> `...2`
    • `` -> `...3`
    • `` -> `...4`
    • `` -> `...5`
    • `` -> `...6`
    • `` -> `...7`
    • `` -> `...8`

    # A tibble: 1 × 8
       ...1  ...2  ...3  ...4  ...5  ...6  ...7  ...8
      <int> <int> <int> <int> <int> <int> <int> <int>
    1     2     4     1     3     2     4     1     3

``` r
x %>% flatten_int()
```

    [1] 2 4 1 3 2 4 1 3

``` r
# You can use flatten in conjunction with map
x %>% map(1L) %>% flatten_int()
```

    [1] 2 2

``` r
# But it's more efficient to use the typed map instead.
x %>% map_int(1L)
```

    [1] 2 2

#### 2. Syntactic Complexity

how to calculate syntactic complexity: how many words are used in one
sentence and what are the mean of each essay?

``` r
KLC_small <- head(KLC_eng, 5)
KLC_small
```

    # A tibble: 5 × 8
      ID                    Nationality Gender Topic       Text  Morph…¹ Level Score
      <chr>                 <chr>       <chr>  <chr>       <chr> <chr>   <chr> <dbl>
    1 A100003_v01.txt_1.txt 미국        여자   주말 이야기 우리… 우리/P… A1       80
    2 A100004_v01.txt_1.txt 미국        여자   여행 계획   저는… 저/PRO… A1       70
    3 A100011_v03.txt_1.txt 영국        여자   여행 계획   저는… 저/PRO… A1       80
    4 A100074_v03.txt_1.txt 호주        남자   일기        어제… 어제/N… A1       70
    5 A100140_v02.txt_1.txt 미국        여자   주말 이야기 일요… 일요일… A1       70
    # … with abbreviated variable name ¹​Morphemes

``` r
a <- KLC_eng %>%
  select(ID, Morphemes) %>% 
  map(~ str_split(., " (\\+)?")) %>% 
  as_data_frame() %>% 
  rename(num_token = Morphemes) %>% 
  unnest(ID)  
  count(num_token)
```

    Error in count(num_token): 객체 'num_token'를 찾을 수 없습니다

``` r
a
```

    # A tibble: 81 × 2
       ID                    num_token  
       <chr>                 <list>     
     1 A100003_v01.txt_1.txt <chr [84]> 
     2 A100004_v01.txt_1.txt <chr [82]> 
     3 A100011_v03.txt_1.txt <chr [115]>
     4 A100074_v03.txt_1.txt <chr [75]> 
     5 A100140_v02.txt_1.txt <chr [69]> 
     6 A100148_v02.txt_1.txt <chr [89]> 
     7 A100154_v02.txt_1.txt <chr [53]> 
     8 A100166_v02.txt_1.txt <chr [93]> 
     9 A100234_v02.txt_1.txt <chr [69]> 
    10 A100235_v02.txt_1.txt <chr [90]> 
    # … with 71 more rows

``` r
KLC_eng %>%
  select(ID, Morphemes) %>% 
  map(~ str_split(., " (\\+)?")) %>% 
  as_data_frame() %>% 
  rename(num_token = Morphemes) %>% 
  unnest(ID)
```

    # A tibble: 81 × 2
       ID                    num_token  
       <chr>                 <list>     
     1 A100003_v01.txt_1.txt <chr [84]> 
     2 A100004_v01.txt_1.txt <chr [82]> 
     3 A100011_v03.txt_1.txt <chr [115]>
     4 A100074_v03.txt_1.txt <chr [75]> 
     5 A100140_v02.txt_1.txt <chr [69]> 
     6 A100148_v02.txt_1.txt <chr [89]> 
     7 A100154_v02.txt_1.txt <chr [53]> 
     8 A100166_v02.txt_1.txt <chr [93]> 
     9 A100234_v02.txt_1.txt <chr [69]> 
    10 A100235_v02.txt_1.txt <chr [90]> 
    # … with 71 more rows

``` r
count(unique(unnest(a$num_token)))
```

    Warning: `cols` is now required when using unnest().
    Please use `cols = c()`

    Error in UseMethod("unnest"): 클래스 "list"의 객체에 적용된 'unnest'에 사용할수 있는 메소드가 없습니다

``` r
KLC_eng %>%
  select(ID, Morphemes) %>% 
  map(~ str_split(., " (\\+)?")) %>% 
  as_data_frame() %>% 
  map()
```

    Error in as_mapper(.f, ...): 기본값이 없는 인수 ".f"가 누락되어 있습니다

## PELIC

### Data Import

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

### Data manipulation

\*\* sorting out columns needed

``` r
PELIC_scr1 <- PELIC_scr %>% 
  select(anon_id, MTELP_Conv_Score, Writing_Sample)

PELIC_crs1 <- PELIC_crs %>% 
  select(course_id, level_id)

PELIC_ans1 <- PELIC_ans %>%
  select(anon_id, course_id, text_len, text, tokens, tok_lem_POS)
  
PELIC_id1 <- PELIC_id %>% 
  select(anon_id, native_language)
```

- course_id: a unique identifier for each course - a 1-4 digit integer,
  e.g. 987

- level_id: a code to identify in which of the four levels the text was
  produced (5 the highest)

\*\* Joining all the columns in one dataframe

``` r
PELIC1 <- left_join(PELIC_ans1, PELIC_crs1, by = "course_id") %>%  
  relocate(c(level_id), .after = course_id)
PELIC2 <- left_join(PELIC1, PELIC_id1,by = "anon_id")
PELIC3 <- left_join(PELIC2, PELIC_scr1, by = "anon_id")

PELIC_clean <- PELIC3 %>% 
  filter(native_language == "Korean", text_len > 10, level_id == 5) %>% 
  relocate(c(text, tokens),.after = Writing_Sample)

head(PELIC_clean)
```

    # A tibble: 6 × 10
      anon_id course_id level…¹ text_…² tok_l…³ nativ…⁴ MTELP…⁵ Writi…⁶ text  tokens
      <chr>       <dbl>   <dbl>   <dbl> <chr>   <chr>     <dbl>   <dbl> <chr> <chr> 
    1 at8           118       5      93 "[('my… Korean       56     1   "my … "['my…
    2 az2           118       5     130 "[('Wh… Korean       71     4   "Whe… "['Wh…
    3 at8           118       5     104 "[('my… Korean       56     1   "my … "['my…
    4 at8           118       5     104 "[('my… Korean       56     1   "my … "['my…
    5 fn2           117       5     271 "[('1'… Korean       81     4   "1. … "['1'…
    6 dj0           117       5     299 "[('Th… Korean       53     2.8 "The… "['Th…
    # … with abbreviated variable names ¹​level_id, ²​text_len, ³​tok_lem_POS,
    #   ⁴​native_language, ⁵​MTELP_Conv_Score, ⁶​Writing_Sample

#### 1. Lexical Diversity

how to calculate lexical diversity: how many different words are used in
each person’s essay?; the number of *word type* divided by the number of
*tokens*

``` r
head(PELIC_clean, 10) %>% 
  select(anon_id, tok_lem_POS) %>% 
  mutate(tok_lem_POS = tok_lem_POS %>% 
           str_remove_all("\\[") %>% 
           str_remove_all("\\]") %>% 
           str_split("(\\))+,")) %>% # I can't figure out how to split the string in the correct way. I wanna leave ')' in each value.
  unnest(tok_lem_POS)
```

    # A tibble: 1,581 × 2
       anon_id tok_lem_POS                            
       <chr>   <chr>                                  
     1 at8     "('my', 'my', 'PRP$'"                  
     2 at8     " ('friend', 'friend', 'NN'"           
     3 at8     " ('is', 'be', 'VBZ'"                  
     4 at8     " ('ANON_NAME_0', 'ANON_NAME_0', 'NNP'"
     5 at8     " ('.', '.', '.'"                      
     6 at8     " ('she', 'she', 'PRP'"                
     7 at8     " ('is', 'be', 'VBZ'"                  
     8 at8     " ('a', 'a', 'DT'"                     
     9 at8     " ('my', 'my', 'PRP$'"                 
    10 at8     " ('ELI', 'ELI', 'NNP'"                
    # … with 1,571 more rows

``` r
  mutate(lemma = map(tok_lem_POS,2),
         POS = map(tok_lem_POS,3)) %>% 
  unnest(lemma, POS)
```

    Error in map(tok_lem_POS, 2): 객체 'tok_lem_POS'를 찾을 수 없습니다

#### 2. Syntactic Complexity

how to calculate syntactic complexity: how many words are used in one
sentence and what are the mean of each essay?

``` r
PELIC_clean %>% 
  select
```

    # A tibble: 2,404 × 0

##### codes needed for my project from Dan (Thanks!!!)

``` r
PELIC_ans %>% 
  mutate(tokens = tokens %>% 
           str_remove_all("\\['|'\\]|\\s") %>% 
           str_split("','")) %>% 
  unnest(tokens)
```

    # A tibble: 4,709,950 × 10
       answer_id questio…¹ anon_id cours…² version created_date        text_…³ text 
           <dbl>     <dbl> <chr>     <dbl>   <dbl> <dttm>                <dbl> <chr>
     1         1         5 eq0         149       1 2006-09-20 16:11:08     177 I me…
     2         1         5 eq0         149       1 2006-09-20 16:11:08     177 I me…
     3         1         5 eq0         149       1 2006-09-20 16:11:08     177 I me…
     4         1         5 eq0         149       1 2006-09-20 16:11:08     177 I me…
     5         1         5 eq0         149       1 2006-09-20 16:11:08     177 I me…
     6         1         5 eq0         149       1 2006-09-20 16:11:08     177 I me…
     7         1         5 eq0         149       1 2006-09-20 16:11:08     177 I me…
     8         1         5 eq0         149       1 2006-09-20 16:11:08     177 I me…
     9         1         5 eq0         149       1 2006-09-20 16:11:08     177 I me…
    10         1         5 eq0         149       1 2006-09-20 16:11:08     177 I me…
    # … with 4,709,940 more rows, 2 more variables: tokens <chr>,
    #   tok_lem_POS <chr>, and abbreviated variable names ¹​question_id, ²​course_id,
    #   ³​text_len

``` r
tibble(col1 = list(c('I', 'I', 'PRP'),
                   c('met', 'meet', 'VBD'),
                   c('my', 'my', 'PRP$'))) %>%
  mutate(Word = map(col1, 1),
         Lemma = map(col1, 2),
         POSTag = map(col1, 3)) %>% 
  unnest(Word:POSTag)
```

    # A tibble: 3 × 4
      col1      Word  Lemma POSTag
      <list>    <chr> <chr> <chr> 
    1 <chr [3]> I     I     PRP   
    2 <chr [3]> met   meet  VBD   
    3 <chr [3]> my    my    PRP$  

``` r
KLC_clean$Morphemes[1] %>% 
  str_split("\\s(?!\\+)") %>% 
  flatten_chr() %>%
  str_split(" \\+") %>% 
  head(3)
```

    [[1]]
    [1] "우리/PRON"

    [[2]]
    [1] "집/NNG" "에/JKB"

    [[3]]
    [1] "가/VV"     "았/EP"     "습니다/EF"

``` r
PELIC_ans1 %>% 
  unnest_tokens(word, text) %>% 
  left_join(PELIC_ans %>% select(), by="")
```

    Error in `left_join()`:
    ! Join columns must be present in data.
    ✖ Problem with ``.

``` r
# 이렇게하면 unnest하느라 없어졌던 열들을 다시 갖다 붙일 수 있음!

# %>% mutate(number = str_extract(colname, "regex"))
# 특정 열에서 특정 표현만 추출.
```
