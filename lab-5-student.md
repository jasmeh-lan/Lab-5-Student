Jasmine Lantaca

In this lab, we will be using the `dplyr` package to explore student
evaluations of teaching data.

**You are expected to use functions from `dplyr` to do your data
manipulation!**

# Part 1: GitHub Workflow

Now that you have the Lab 5 repository cloned, you need to make sure you
can successfully push to GitHub. To do this you need to:

- Open the `lab-5-student.qmd` file (in the lower right hand corner).
- Change the `author` line at the top of the document (in the YAML) to
  your name.
- Save your file either by clicking on the blue floppy disk or with a
  shortcut (command / control + s).
- Click the “Git” tab in upper right pane
- Check the “Staged” box for the `lab-5-student.qmd` file (the file you
  changed)
- Click “Commit”
- In the box that opens, type a message in “Commit message”, such as
  “Added my name”.
- Click “Commit”.
- Click the green “Push” button to send your local changes to GitHub.

RStudio will display something like:

    >>> /usr/bin/git push origin HEAD:refs/heads/main
    To https://github.com/atheobold/introduction-to-quarto-allison-theobold.git
       3a2171f..6d58539  HEAD -> main

Now you are ready to go! Remember, as you are going through the lab I
would strongly recommend rendering your HTML and committing your after
**every** question!

# Part 2: Some Words of Advice

Part of learning to program is learning from a variety of resources.
Thus, I expect you will use resources that you find on the internet.
There is, however, an important balance between copying someone else’s
code and *using their code to learn*.

Therefore, if you use external resources, I want to know about it.

- If you used Google, you are expected to “inform” me of any resources
  you used by **pasting the link to the resource in a code comment next
  to where you used that resource**.

- If you used ChatGPT, you are expected to “inform” me of the assistance
  you received by (1) indicating somewhere in the problem that you used
  ChatGPT (e.g., below the question prompt or as a code comment),
  and (2) downloading and including the `.txt` file containing your
  **entire** conversation with ChatGPT.

Additionally, you are permitted and encouraged to work with your peers
as you complete lab assignments, but **you are expected to do your own
work**. Copying from each other is cheating, and letting people copy
from you is also cheating. Please don’t do either of those things.

## Setting Up Your Code Chunks

- The first chunk of this Quarto document should be used to *declare
  your libraries* (probably only `tidyverse` for now).
- The second chunk of your Quarto document should be to *load in your
  data*.

## Save Regularly, Render Often

- Be sure to **save** your work regularly.
- Be sure to **render** your file every so often, to check for errors
  and make sure it looks nice.
  - Make sure your Quarto document does not contain `View(dataset)` or
    `install.packages("package")`, both of these will prevent rendering.
  - Check your Quarto document for occasions when you looked at the data
    by typing the name of the data frame. Leaving these in means the
    whole dataset will print out and this looks unprofessional. **Remove
    these!**
  - If all else fails, you can set your execution options to
    `error: true`, which will allow the file to render even if errors
    are present.

# Part 3: Let’s Start Working with the Data!

## The Data

The `teacher_evals` dataset contains student evaluations of reaching
(SET) collected from students at a University in Poland. There are SET
surveys from students in all fields and all levels of study offered by
the university.

The SET questionnaire that every student at this university completes is
as follows:

> Evaluation survey of the teaching staff of University of Poland.
> Please complete the following evaluation form, which aims to assess
> the lecturer’s performance. Only one answer should be indicated for
> each question. The answers are coded in the following way: 5 - I
> strongly agree; 4 - I agree; 3 - Neutral; 2 - I don’t agree; 1 - I
> strongly don’t agree.
>
> Question 1: I learned a lot during the course.
>
> Question 2: I think that the knowledge acquired during the course is
> very useful.
>
> Question 3: The professor used activities to make the class more
> engaging.
>
> Question 4: If it was possible, I would enroll for a course conducted
> by this lecturer again.
>
> Question 5: The classes started on time.
>
> Question 6: The lecturer always used time efficiently.
>
> Question 7: The lecturer delivered the class content in an
> understandable and efficient way.
>
> Question 8: The lecturer was available when we had doubts.
>
> Question 9. The lecturer treated all students equally regardless of
> their race, background and ethnicity.

These data are from the end of the winter semester of the 2020-2021
academic year. In the period of data collection, all university classes
were entirely online amid the COVID-19 pandemic. While expected learning
outcomes were not changed, the online mode of study could have affected
grading policies and could have implications for data.

**Average SET scores** were combined with many other variables,
including:

1.  **characteristics of the teacher** (degree, seniority, gender, SET
    scores in the past 6 semesters).
2.  **characteristics of the course** (time of day, day of the week,
    course type, course breadth, class duration, class size).
3.  **percentage of students providing SET feedback.**
4.  **course grades** (mean, standard deviation, percentage failed for
    the current course and previous 6 semesters).

This rich dataset allows us to **investigate many of the biases in
student evaluations of teaching** that have been reported in the
literature and to formulate new hypotheses.

Before tackling the problems below, study the description of each
variable included in the `teacher_evals_codebook.pdf`.

**1. Load the appropriate R packages for your analysis.**

``` r
# code chunk for loading packages
library(tidyverse)
```

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ✔ purrr     1.0.2     
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

**2. Load in the `teacher_evals` data.**

``` r
# code chunk for importing the data

evals <- read_csv("Data/Data-Raw/teacher_evals.csv")
```

    Rows: 8015 Columns: 22
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    chr  (5): course_id, weekday, time_of_day, academic_degree, gender
    dbl (17): teacher_id, question_no, no_participants, resp_share, SET_score_av...

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

### Data Inspection + Summary

**3. Provide a brief overview (~4 sentences) of the dataset.**

``` r
# you may want to use code to answer this question

glimpse(evals)
```

    Rows: 8,015
    Columns: 22
    $ course_id               <chr> "0000-BHP", "0000-BHP", "0000-BHP", "0000-BHP"…
    $ teacher_id              <dbl> 54655, 54655, 54655, 54655, 54655, 3432, 3432,…
    $ question_no             <dbl> 901, 903, 904, 905, 907, 902, 903, 906, 909, 9…
    $ no_participants         <dbl> 255, 255, 255, 255, 255, 998, 998, 998, 998, 1…
    $ resp_share              <dbl> 0.21176471, 0.20784314, 0.21176471, 0.20784314…
    $ SET_score_avg           <dbl> 4.222222, 3.679245, 3.740741, 3.301887, 3.6792…
    $ stud_grade_avg          <dbl> 3.000000, 3.000000, 3.000000, 3.000000, 3.0000…
    $ stud_grade_std          <dbl> 0.0000000, 0.0000000, 0.0000000, 0.0000000, 0.…
    $ stud_grade_var_coef     <dbl> 0.0000000, 0.0000000, 0.0000000, 0.0000000, 0.…
    $ percent_failed          <dbl> 0.000000000, 0.000000000, 0.000000000, 0.00000…
    $ stud_grade_avg_cur      <dbl> 3.000000, 3.000000, 3.000000, 3.000000, 3.0000…
    $ stud_grade_std_cur      <dbl> 0.0000000, 0.0000000, 0.0000000, 0.0000000, 0.…
    $ stud_grade_var_coef_cur <dbl> 0.0000000, 0.0000000, 0.0000000, 0.0000000, 0.…
    $ percent_failed_cur      <dbl> 0.000000000, 0.000000000, 0.000000000, 0.00000…
    $ class_duration          <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0…
    $ weekday                 <chr> "Friday", "Friday", "Friday", "Friday", "Frida…
    $ time_of_day             <chr> "<10", "<10", "<10", "<10", "<10", "14-18", "1…
    $ SET_score_1sem          <dbl> 3.820000, 3.820000, 3.820000, 3.820000, 3.8200…
    $ maximum_score           <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0…
    $ academic_degree         <chr> "no_dgr", "no_dgr", "no_dgr", "no_dgr", "no_dg…
    $ seniority               <dbl> 4, 4, 4, 4, 4, 11, 11, 11, 11, 11, 11, 11, 11,…
    $ gender                  <chr> "male", "male", "male", "male", "male", "male"…

``` r
dim(evals)
```

    [1] 8015   22

> There is a total of 8015 rows of data with 22 columns. The data is
> collected from students at Universirty of Poland from all sorts of
> courses and studies offered by the university whose rating the
> teaching staff of based on 9 question with answers from 1 to 5. Other
> data information includes the time of day with day of the week and
> class duration of the course the teacher teach, the gender of teacher,
> the degree and senority of the teacher, and data fo the students like
> average grade/standard deviation, and percentage of failed students.

**4. What is the unit of observation (i.e. a single row in the dataset)
identified by?**

``` r
# you may want to use code to answer this question

summary(evals)
```

      course_id           teacher_id      question_no  no_participants  
     Length:8015        Min.   :  3432   Min.   :901   Min.   :   1.00  
     Class :character   1st Qu.: 38606   1st Qu.:903   1st Qu.:  15.00  
     Mode  :character   Median : 50752   Median :905   Median :  27.00  
                        Mean   : 49964   Mean   :905   Mean   :  46.95  
                        3rd Qu.: 66076   3rd Qu.:907   3rd Qu.:  50.00  
                        Max.   :110292   Max.   :909   Max.   :1003.00  
                                                       NA's   :34       
       resp_share      SET_score_avg   stud_grade_avg  stud_grade_std  
     Min.   :0.00348   Min.   :1.000   Min.   :2.375   Min.   :0.0000  
     1st Qu.:0.06061   1st Qu.:4.000   1st Qu.:3.583   1st Qu.:0.6369  
     Median :0.10000   Median :4.667   Median :4.015   Median :0.8562  
     Mean   :0.14020   Mean   :4.392   Mean   :3.962   Mean   :0.8078  
     3rd Qu.:0.16667   3rd Qu.:5.000   3rd Qu.:4.392   3rd Qu.:1.0100  
     Max.   :1.00000   Max.   :5.000   Max.   :5.000   Max.   :1.7678  
     NA's   :34                        NA's   :41      NA's   :41      
     stud_grade_var_coef percent_failed    stud_grade_avg_cur stud_grade_std_cur
     Min.   :0.0000      Min.   :0.00000   Min.   :2.375      Min.   :0.0000    
     1st Qu.:0.1483      1st Qu.:0.00000   1st Qu.:3.583      1st Qu.:0.5632    
     Median :0.2150      Median :0.07692   Median :4.048      Median :0.8311    
     Mean   :0.2120      Mean   :0.12490   Mean   :3.990      Mean   :0.7907    
     3rd Qu.:0.2777      3rd Qu.:0.19048   3rd Qu.:4.450      3rd Qu.:1.0278    
     Max.   :0.5439      Max.   :0.75000   Max.   :5.000      Max.   :1.7678    
     NA's   :41          NA's   :41        NA's   :41         NA's   :41        
     stud_grade_var_coef_cur percent_failed_cur class_duration    weekday         
     Min.   :0.0000          Min.   :0.0000     Min.   :0.000   Length:8015       
     1st Qu.:0.1325          1st Qu.:0.0000     1st Qu.:1.000   Class :character  
     Median :0.2107          Median :0.0723     Median :2.000   Mode  :character  
     Mean   :0.2073          Mean   :0.1274     Mean   :1.564                     
     3rd Qu.:0.2795          3rd Qu.:0.2000     3rd Qu.:2.000                     
     Max.   :0.5439          Max.   :0.7500     Max.   :3.000                     
     NA's   :41              NA's   :41         NA's   :914                       
     time_of_day        SET_score_1sem  maximum_score    academic_degree   
     Length:8015        Min.   :1.220   Min.   :0.0000   Length:8015       
     Class :character   1st Qu.:4.119   1st Qu.:0.0000   Class :character  
     Mode  :character   Median :4.471   Median :0.0000   Mode  :character  
                        Mean   :4.388   Mean   :0.4131                     
                        3rd Qu.:4.685   3rd Qu.:1.0000                     
                        Max.   :5.000   Max.   :1.0000                     
                        NA's   :697                                        
       seniority         gender         
     Min.   : 1.000   Length:8015       
     1st Qu.: 2.000   Class :character  
     Median : 6.000   Mode  :character  
     Mean   : 5.496                     
     3rd Qu.: 8.000                     
     Max.   :11.000                     
                                        

> In a single observation, the data consist of 22 elements: course id,
> teacher id, the type of question answered, the total number of
> participants for teacher/course, proportion of survey response to the
> all, evaluation average of teacher, grade average of student in
> teacher class of both the past semesters and current semester, the
> standard deviation of student grade in teacher class for both current
> semester and past semesters, teacher’s degree, senority, gender,
> course information (time of day, day of the week, duration of class),
> and percentage of student’s failed in both past and current semester.

**5. Use *one* `dplyr` pipeline to clean the data by:**

- **renaming the `gender` variable `sex`**
- **removing all courses with fewer than 10 respondents**
- **changing data types in whichever way you see fit (e.g., is the
  instructor ID really a numeric data type?)**
- **only keeping the columns we will use – `course_id`, `teacher_id`,
  `question_no`, `no_participants`, `resp_share`, `SET_score_avg`,
  `percent_failed_cur`, `academic_degree`, `seniority`, and `sex`**

**Assign your cleaned data to a new variable named `teacher_evals_clean`
–- use these data going forward. Save the data as
`teacher_evals_clean.csv` in the `data-clean` folder.**

``` r
# code chunk for Q4

evals_clean <- evals |>
  rename(sex = gender) |>
  filter(no_participants > 10) |>
  mutate(across(c(teacher_id, question_no), as.factor)) |>
  select(c(course_id, teacher_id, question_no, no_participants, resp_share, SET_score_avg, percent_failed_cur, academic_degree, seniority, sex))
  

glimpse(evals_clean)
```

    Rows: 6,663
    Columns: 10
    $ course_id          <chr> "0000-BHP", "0000-BHP", "0000-BHP", "0000-BHP", "00…
    $ teacher_id         <fct> 54655, 54655, 54655, 54655, 54655, 3432, 3432, 3432…
    $ question_no        <fct> 901, 903, 904, 905, 907, 902, 903, 906, 909, 901, 9…
    $ no_participants    <dbl> 255, 255, 255, 255, 255, 998, 998, 998, 998, 1003, …
    $ resp_share         <dbl> 0.21176471, 0.20784314, 0.21176471, 0.20784314, 0.2…
    $ SET_score_avg      <dbl> 4.222222, 3.679245, 3.740741, 3.301887, 3.679245, 4…
    $ percent_failed_cur <dbl> 0.000000000, 0.000000000, 0.000000000, 0.000000000,…
    $ academic_degree    <chr> "no_dgr", "no_dgr", "no_dgr", "no_dgr", "no_dgr", "…
    $ seniority          <dbl> 4, 4, 4, 4, 4, 11, 11, 11, 11, 11, 11, 11, 11, 11, …
    $ sex                <chr> "male", "male", "male", "male", "male", "male", "ma…

``` r
write_csv(evals_clean, "Data/Data-Clean/teacher_evals_clean.csv")
```

**6. How many unique instructors and unique courses are present in the
cleaned dataset?**

``` r
# code chunk for Q5


n_distinct(evals_clean$course_id) #number of unique courses
```

    [1] 921

``` r
n_distinct(evals_clean$teacher_id) # number of teachers
```

    [1] 294

> There are 921 unique courses while 294 teachers.

**7. One teacher-course combination has some missing values, coded as
`NA`. Which instructor has these missing values? Which course? What
variable are the missing values in?**

``` r
# code chunk for Q6

evals_clean |>
  filter(if_any(.col = everything(), is.na))
```

    # A tibble: 7 × 10
      course_id   teacher_id question_no no_participants resp_share SET_score_avg
      <chr>       <fct>      <fct>                 <dbl>      <dbl>         <dbl>
    1 PAB3SE004PA 56347      901                      32     0.0312             5
    2 PAB3SE004PA 56347      902                      32     0.0312             5
    3 PAB3SE004PA 56347      903                      32     0.0312             4
    4 PAB3SE004PA 56347      905                      32     0.0312             4
    5 PAB3SE004PA 56347      906                      32     0.0312             3
    6 PAB3SE004PA 56347      907                      32     0.0312             3
    7 PAB3SE004PA 56347      908                      32     0.0312             5
    # ℹ 4 more variables: percent_failed_cur <dbl>, academic_degree <chr>,
    #   seniority <dbl>, sex <chr>

> The teacher-course combination that has missing value is course
> PAB3SE004PA and teacher 56347. The variable that is missing is the
> percentage failed in the current semester.

**8. What are the demographics of the instructors in this study?
Investigate the variables `academic_degree`, `seniority`, and `sex` and
summarize your findings in ~3 complete sentences.**

``` r
# code chunk for Q7

evals_clean |>
  select(sex, seniority, academic_degree) |>
  count(sex)
```

    # A tibble: 2 × 2
      sex        n
      <chr>  <int>
    1 female  3199
    2 male    3464

``` r
evals_clean |>
  select(sex, seniority, academic_degree) |>
  count(seniority) |>
  arrange(desc(n))
```

    # A tibble: 11 × 2
       seniority     n
           <dbl> <int>
     1         2  1639
     2         6   856
     3         8   778
     4        11   636
     5         4   552
     6         1   405
     7        10   402
     8         7   396
     9         3   367
    10         9   352
    11         5   280

``` r
evals_clean |>
  select(sex, seniority, academic_degree) |>
  count(academic_degree)
```

    # A tibble: 4 × 2
      academic_degree     n
      <chr>           <int>
    1 dr               4270
    2 ma               1808
    3 no_dgr            467
    4 prof              118

> Looking at the demographics of the instructors, there are more males
> (3464) than female (3199) professors/teaching staffs. Majority of the
> professors have been working in the university at least 2 years. the
> second closest majority senority is 6 years, and third cloest majority
> is 8 years; this data shows that majority of teaching staffs are
> people with decent years of teaching in the university. In terms of
> academic degree, majority of teachers hold a doctorate degree compared
> to the rest.

**9. Each course seems to have used a different subset of the nine
evaluation questions. How many teacher-course combinations asked all
nine questions?**

``` r
# code chunk for Q8

evals_clean |>
  count(course_id, teacher_id) |> 
  filter(n == 9)
```

    # A tibble: 48 × 3
       course_id       teacher_id     n
       <chr>           <fct>      <int>
     1 0000-SEM-SP     38335          9
     2 ARI3SE001AR-PDW 50752          9
     3 ARI3SP17AR-Z18  69447          9
     4 CII2SE02CI-L19  3662           9
     5 CII3SP12CI-Z17  3521           9
     6 CII7SE05CI-Z19  57632          9
     7 CII7SP02CI-Z20  54309          9
     8 CII7SP08CI-Z19  44665          9
     9 CIM3SE01SJO-Z19 59070          9
    10 ECB3NP02EC-Z20  38588          9
    # ℹ 38 more rows

> There are 48 teacher-course combinations that asked all nine
> questions.

## Rate my Professor

**10. Which instructors had the highest and lowest average rating for
Question 1 (I learnt a lot during the course.) across all their
courses?**

``` r
# code chunk for Q9

evals_clean |>
  filter(question_no == 901) |>
  arrange(SET_score_avg) |>
  select(teacher_id, SET_score_avg) |>
  slice_min(SET_score_avg) # looking at instructors with the lowest SET score average
```

    # A tibble: 6 × 2
      teacher_id SET_score_avg
      <fct>              <dbl>
    1 100132                 1
    2 3577                   1
    3 40937                  1
    4 37035                  1
    5 54201                  1
    6 37011                  1

``` r
evals_clean |>
  filter(question_no == 901) |>
  arrange(SET_score_avg) |>
  select(teacher_id, SET_score_avg) |>
  slice_max(SET_score_avg) # looking at instructors with the highest SET score average
```

    # A tibble: 361 × 2
       teacher_id SET_score_avg
       <fct>              <dbl>
     1 40903                  5
     2 84689                  5
     3 38238                  5
     4 38335                  5
     5 40763                  5
     6 76398                  5
     7 79745                  5
     8 76398                  5
     9 66287                  5
    10 79490                  5
    # ℹ 351 more rows

> The instructors that have the lowest SET score average for question 1
> are 100132, 3577, 40937, 37035, 54201, and 37011 while the
> instructores there are 361 teachers that have the highest SET score
> average for quesstion 1(ex: 40903, 84689, 38238, 38335, etc).

**11. Which instructors with one year of experience had the highest and
lowest average percentage of students failing in the current semester
across all their courses?**

``` r
# code chunk for Q10

evals_clean |>
  filter(seniority == 1) |>
  count(teacher_id, percent_failed_cur) |>
  select(teacher_id, percent_failed_cur) |>
  arrange(percent_failed_cur) |>
  slice_min(percent_failed_cur) # the instructors that have the lowest average percent of students failing in current semester
```

    # A tibble: 9 × 2
      teacher_id percent_failed_cur
      <fct>                   <dbl>
    1 80974                       0
    2 84688                       0
    3 84689                       0
    4 86222                       0
    5 98650                       0
    6 98651                       0
    7 102379                      0
    8 103092                      0
    9 106126                      0

``` r
evals_clean |>
  filter(seniority == 1) |>
  count(teacher_id, percent_failed_cur) |>
  select(teacher_id, percent_failed_cur) |>
  arrange(percent_failed_cur) |>
  slice_max(percent_failed_cur) # the instructors that have the highest average percent of students failing in current semester
```

    # A tibble: 1 × 2
      teacher_id percent_failed_cur
      <fct>                   <dbl>
    1 104362                  0.745

> There are 9 instructors with the lowest average percentage of
> student’s failing in the currect semester and they are 80974, 84688,
> 84689, 86222, 98650, 98651, 102379, 103092, and 106126. There is one
> instructure with the highest average percentage of student’s failing
> in the current semester and they are 104362.

**12. Which female instructors with either a doctorate or professional
degree had the highest and lowest average percent of students responding
to the evaluation across all their courses?**

``` r
# code chunk for Q11

evals_clean |>
  filter(sex == "female" & academic_degree == "dr" | sex == "female" & academic_degree == "prof") |>
  arrange(resp_share) |>
  count(teacher_id, resp_share) |>
  select(teacher_id, resp_share) |>
  slice_min(resp_share) # looking at which female instructor who's either a dr or prof degree that has the lowest average in student responding
```

    # A tibble: 1 × 2
      teacher_id resp_share
      <fct>           <dbl>
    1 3485           0.0112

``` r
evals_clean |>
  filter(sex == "female" & academic_degree == "dr" | sex == "female" & academic_degree == "prof") |>
  arrange(resp_share) |>
  count(teacher_id, resp_share) |>
  select(teacher_id, resp_share) |>
  slice_max(resp_share) # looking at which female instructor who's either a dr or prof degree that has the highest average in student responding
```

    # A tibble: 1 × 2
      teacher_id resp_share
      <fct>           <dbl>
    1 101508          0.522

> The female instructor with either a doctorate or professor degree that
> has the lowest average percent of student’s responding is 3485 while
> the female instructor with the highest average percentage of student’s
> repsonding with the same conditions is 101508.
