# Effect of Instrument, Harmonic Motion, and Voice Leading on Classical Music Classification

### Author: Myung Kyung (Rachel) Hyeon[^1]

### Date: December 11, 2022

[^1]: Department of Statistics and Data Science, Carnegie Mellon University

## Abstract

  This paper analyzed data collected by Ivan Jimenez, a composer and
  musicologist visiting the University of Pittsburgh, and student
  Vincent Rossi, for a study measuring the influence of instrument,
  harmonic motion, and voice leading on listeners' identification of
  musical stimuli as "classical" or "popular". Two research questions
  were investigated in this paper: (1) what experimental factor, or
  combinations of factors, has the strongest influence on rating a music
  as "classical", and (2) if there are differences in the way that
  musicians and non-musicians identify classical music. The raw data
  used for analysis contained 2520 observations and 28 variables, but
  only 1540 observations were complete. To answer the first research
  question, a final linear model was selected after performing model
  selection using information criteria, Analysis of Variance test, and
  backward selection of fixed effects. To answer the second research
  question, participants were dichotomized into two groups, musicians
  and non-musicians, and the interactions between the dichotomized
  variable and other predictors were assessed. It was found that
  listeners think piano and especially strings, sound more classical
  than guitar, the harmony I-V-VI have a strongest association with
  classical ratings among the four levels of harmonic motion, and
  contrary motion have the strongest association with classical ratings
  among the levels of voice leading. Also, musicians tend to rate
  harmony structure I-V-VI as classical differently from how
  non-musicians rate them. Limitations of the study include a large
  amount of missing data and a potential small sample bias affecting
  some coefficient estimates of the final linear model.


---

# Introduction

Although classical music is not limited to a single definition,
classical music generally refers to a form of formal European music of
the late 18th to early 19th century [@oxford-dict]. Classical music is
characterized by "harmony, balance, and adherence to established forms"
[@oxford-dict]. On the other hand, popular music generally refers to a
form of music that has a wide appeal to a general audience, and is
characterized by "lightly romantic or sentimental melodies"
[@collins-dict]. An example of a study that studied what factor
differentiates classical music from popular music is a study conducted
by De Clercq and Temperley [-@declercq-2011] where the researchers found
that popular music from five decades (50s to 90s) had different
distribution of chromatic roots than classical music from the 18th to
19th century. Many studies were conducted on assessing listeners'
ability to identify music based on song characteristics. Doll
[-@doll-2017] found that common chord progressions found in North
American and British popular music can be stored in the long-term
memories of listeners and be recognized as "stereotypical". Listeners
were able to identify a piece of music based on the pitch and rhythmic
features of the melody even when melody was presented in a different
instrument or tempo [@warren-1991; @andrews-1998; @dowling-2008]. A
study by Jimenez and Kuusi [-@Jimenez-2018] found that listeners were
able to identify music from chord progressions, and musicians performed
better than non-musicians at this task. The motivation for this study is
to examine the influence of three design variables, Harmony, Instrument,
and Voice on listeners' classification of music as classical.

In this paper, we will analyze the data collected by Ivan Jimenez, a
composer and musicologist visiting the University of Pittsburgh, and
student Vincent Rossi, for a study measuring the influence of
instrument, harmonic motion, and voice leading on listeners'
identification of a series of three-chord successions as "classical" or
"popular" [@Jimenez-presentation]. The researchers presented 36 musical
stimuli to 70 listeners, recruited from the population of undergraduates
at the University of Pittsburgh [@homework-9]. Listeners were asked to
rate the musical stimuli on a scale of 1 to 10 on how classical and how
popular the music sounds, with 1 being not at all, and 10 being very
classical or very popular. Listeners were told that a piece could be
rated as both classical and popular, neither classical nor popular, or
mostly classical and not popular (or vice versa), so that the scales
should have functioned more or less independently.

The 36 stimuli were chosen by completely crossing these factors:

-   Instrument: String Quartet, Piano, Electric Guitar

-   Harmonic Motion: I-V-VI, I-VI-V, I-V-IV, IV-I-V

-   Voice Leading: Contrary Motion, Parallel 3rds, Parallel 5ths

This paper will address the following research questions:

1.  What experimental factor, or combinations of factors, has the
    strongest influence on ratings?

    1.  Does Instrument exert the strongest influence among the three
        design factors (Instrument, Harmonic Motion, Voice Leading), as
        the researchers suspect?

    2.  Among the levels of Harmonic Motion does I-V-VI have a strong
        association (the strongest) with classical ratings? Does it seem
        to matter whether the respondent is familiar with one or the
        other (or both) of the Pachelbel rants/comedy bits?

    3.  Among the levels of Voice Leading, does contrary motion have a
        strong (the strongest) association with classical ratings?

2.  Are there differences in the way that musicians and non-musicians
    identify classical music?

# Data

The raw data provided by Dr. Jimenez consisted of 2520 observations of
28 variable each, fully balanced across 70 listeners/subjects and 36
experimental conditions (Appendix A.1). The data has two levels: level-1
observations are ratings of how Classical each musical stimulus sounds,
and level-2 groups are the set of stimuli that each listener rated. A
brief description of all variables in the data set is presented in Table 1.

From the raw data, we created two data sets for the analysis:

-   `fulldata` is a data set consisting of all of the original variables
    except for `X1stInstr`, `X2ndInstr`. `X1stInstr` and `X2ndInstr`
    were removed because 87% of `X2ndInstr` column and 60% of
    `X1stInstr` column were missing data (Appendix A.3). 27 rows from
    `fulldata` that had missing Classical and Popular response values
    were also removed (Appendix A.4). Furthermore, two responses of 19
    for Classical and Popular ratings were deleted as 19's were assumed
    to be keying errors when entering the data (Appendix A.2). After
    deletion, this data set has 2491 observations of 26 variables, and
    is nearly balanced across the 70 subjects and 36 experimental
    conditions (Appendix A.4). With the `fulldata` data set, I explored
    the influence of the three experimental factors, both fixed and
    random effects, on Classical ratings.

-   `nomissdata` is a subset of `fulldata` with no missing values. This
    data set has 1540 complete observations of 26 variables. It is
    reasonably well balanced across experimental conditions and
    subjects, but represents only 43 of the original 70 subjects
    (Appendix A.4). With the `nomissdata` data set, we did variable
    selection on the person covariate fixed effects and re-considered
    random effects in light of a final person covariates model.

The distributions of the variables in `fulldata` and `nomissdata` are
similar (Appendix A.5). Although three quantitative variables (`OMSI`,
`X16.minus.17`, and `NoClass`) exhibit noticable skewing, no
transformations were conducted to make the results as immediately
interpretable.

### Table 1: Description of 28 variables in the data set.


| **Variable**                 | **Variable Definition**                                                                                  |
|:-----------------------------|:---------------------------------------------------------------------------------------------------------|
| X                            | Line number in the data set (IGNORE.)                                                                    |
| Classical                    | How classical does the stimulus sound?                                                                   |
| Popular                      | How popular does the stimulus sound?                                                                     |
| Subject                      | Unique subject ID                                                                                        |
| Harmony                      | Harmonic Motion (4 levels)                                                                               |
| Instrument                   | Instrument (3 levels)                                                                                    |
| Voice                        | Voice Leading (3 levels)                                                                                 |
| Selfdeclare                  | Are you a musician? (1-6, 1=not at all)                                                                  |
| OMSI                         | Score on a test of musical knowledge                                                                     |
| X16.minus.17                 | Auxiliary measure of listener's ability to distinguish classical vs popular music                        |
| ConsInstr                    | How much did you concentrate on the instrument while listening (0-5, 0=not at all)                       |
| ConsNotes                    | How much did you concentrate on the notes while listening? (0-5, 0=not at all)                           |
| Instr.minus.Notes            | Difference between prev. two variable                                                                    |
| PachListen                   | How familiar are you with Pachelbel's Canon in D (0-5, 0=not at all)                                     |
| ClsListen                    | How much do you listen to classical music? (0-5, 0=not at all)                                           |
| KnowRob                      | Have you heard Rob Paravonian's Pachelbel Rant (0-5, 0=not at all)                                       |
| KnowAxis                     | Have you heard Axis of Evil's Comedy bit on the 4 Pachelbel chords in popular music? (0-5, 0=not at all) |
| X1990s2000s                  | How much do you listen to pop and rock from the 90's and 2000's? (0-5, 0=not at all)                     |
| X1990s2000s.minus.1960s1970s | Difference between prev variable and a similar variable referring to 60's and 70's pop and rock.         |
| CollegeMusic                 | Have you taken music classes in college (0=no, 1=yes)                                                    |
| NoClass                      | How many music classes have you taken?                                                                   |
| APTheory                     | Did you take AP Music Theory class in High School (0=no, 1=yes)                                          |
| Composing                    | Have you done any music composing (0-5, 0=not at all)                                                    |
| PianoPlay                    | Do you play piano (0-5, 0=not at all)                                                                    |
| GuitarPlay                   | Do you play guitar (0-5, 0=not at all)                                                                   |
| X1stInstr                    | How proficient are you at your first musical instrument (0-5, 0=not at all)                              |
| X2ndInstr                    | Same, for second musical instrument                                                                      |
| first12                      | In the experiment, which instrument was presented to the subject in the first 12 stimuli? (IGNORE.)      |

# Methods

All statistical analyses were conducted using R [@r].

## Statistical Analysis for First Research Question

To answer the first research question, "What experimental factor, or
combinations of factors, has the strongest influence on ratings?", we
developed a final model that accurately models the relationship between
Classical ratings and various experimental factors. Model selection was
conducted in four steps, which are listed below. The intermediate model
that was selected in the previous step was used as a basis when deciding
to add more experimental factors in the next step. Both the intermediate
models and the final model that was selected will be presented. The
final model obtained after completing step 4 will be used to answer the
first research question.

Model selection steps:

1.  Testing whether interaction term(s) between three design variables
    (`Harmony`, `Instrument`, `Voice`) are needed

2.  Testing whether including the random intercept is preferred by
    information criteria and ANOVA test

3.  Testing whether adding random slope(s) is/are preferred by
    information criteria

4.  Selecting fixed effects (person covariates) to include in the model
    using backward selection

During the model selection process, Analysis of Variance (ANOVA) test
and three information criterions (Akaike information criterion (AIC),
Bayesian information criterion (BIC), and Deviance information criterion
(DIC)), and backward selection of person covariates were used. Analysis
of Variance (ANOVA) test between regression models were performed using
the `anova()` function in R to compare between the nested models with
differing interactions and random effects [@r]. The `anova()` function
performs a Chi-square test between a reduced model and a full model
where the reduced model is nested within a full model to test the null
hypothesis that the coefficients are zero for all variables in the full
model that are not in the reduced model [@stack-1]. The alternative
hypothesis is that the coefficients for all variables in the full model
that are not in the reduced model are not zero. In this paper, we used a
significance threshold of 0.05. AIC, BIC, and DIC are information
criteria that are used for model selection. AIC, BIC, and DIC all use a
model's maximum likelihood estimation (log-likelihood) as a measure of
fit, but each of the three information criteria differ in what aspects
of the model it penalizes. AIC tends to pick larger models with lower
prediction error [@lecture-22]. BIC tends to pick simpler models that
are closer to the true model [@lecture-22]. DIC is a generalization of
AIC where it can compute score for hierarchical models. DIC is designed
to correctly count degrees of freedom when comparing models with random
effects. Therefore, DIC of a linear regression model is the same as AIC
of the same linear regression model [@lecture-22]. For all three
information criteria, a model with smaller information criterion score
compared to another model means that the model with smaller score is a
preferred model. After identifying which combinations of interaction
between three design variables and what random effects should be added,
person covariates were added to the final model after variable selection
using `fitLMER.fnc` from `LMERConvenienceFunctions` library
[@LMERConvenienceFunctions]. The function `fitLMER.fnc` did backwards
selection of fixed effects and then one more pass of backwards selection
of fixed effects using AIC and BIC.

## Statistical Analysis for Second Research Question

To answer the second research question, "Are there any differences in
the way that musicians and non-musicians identify classical music?", the
interactions between a new dichotomous variable called `Musician` and
all fixed effects in the final model were analyzed. The `Musician`
variable was created from dichotomizing the `Selfdeclare` variable so
that participants with self-declaration below or equal to a certain
threshold were assigned 0 as values for `Musician`, and the other
participants with self-declaration above a threshold were assigned 1.
The effect of dichotomization using different thresholds were analyzed
to determine if the result is sensitive to the dichotomization criteria.

# Results

## Exploratory Data Analysis

The exploratory data analysis revealed that many variables are
categorical variables with differing levels. `OMSI`, `ConsInstr`,
`NoClass`, `X16.minus.17`, `Instr.minus.Notes`, and
`X1990s2000s.minus.1960s1970s` were classified as continuous variables.
Out of the remaining 20 variables in Table 1 excluding the response variables `Classical` and `Popular`, nine variables (`PachListen`, `ClsListen`, `KnowRob`,
`KnowAxis`, `CollegeMusic`, `APTheory`, `Composing`, `PianoPlay`, and
`GuitarPlay`) were converted to factors before applying `fitLMER.fnc`
(Appendix C.1). Figure 5 shows the distribution of 20 categorical and
continuous variables that are numeric. `OMSI`, `X16.minus.17`,
`NoClass`, and `Selfdeclare` were right-skewed. From the distributions,
we can see that most study participants answered that they are very
familiar (rating of 5 out of 5) with Pachelbel's Canon in D, but were
"not-at-all" familiar (rating of 0 out of 5) with Rob Parvonian's
Pachelbel Rant and Axis of Evil's Comedy bit. Most study participants
answered that they listen to pop and rock from the 90's and 2000's
extensively (rating of 5 out of 5). Study participants seem to vary in
how much they listen to classical music, with some answering that they
hardly listen to classical music (rating of 0 or 1 out of 5), some
answering that they listen to some classical music (rating of 3 out of
5), and a few answering that they listen to classical music extensively
(rating of 4 or 5 out of 5).

Applying square-root and log transformation to most skewed continuous
variables, `OMSI`, `X16.minus.17` and `NoClass`, did not drastically
improve the distribution to warrant a transformation while sacrificing
interpretability (Appendix A.5). Therefore, no transformation was
applied to any variables as mentioned above in the Data section.

### Figure 1: Histograms of categorical and continuous variables

![Histograms of categorical and continuous variables
](images/000024.png)

![Histograms of categorical and continuous variables
](images/000025.png)

![Histograms of categorical and continuous variables
](images/000026.png)

![Histograms of categorical and continuous variables
](images/000027.png)

![Histograms of categorical and continuous variables
](images/000030.png)

## What experimental factor, or combinations of factors, has the strongest influence on ratings?

To answer the first research question, model selection was conducted in
four steps as mentioned in the Methods section above. Table 2 shows the different combinations of
experimental factors, interactions, and random effects that were
included in each of the 17 models that were compared throughout step 1
to 3. Models 1.0 to 1.4 were compared in step one, Models 2.0 to 2.4
were compared in step two, and Models 3.1 to 3.7 were compared in step
three. The final combination of design variables, interactions, and
random effects selected after completing step three was used as a basis
when selecting person covariates in step four of the model selection
process.

Table 3 shows the AIC, BIC, and DIC for each of the
17 models compared. Between Models 1.0 to 1.4, AIC was the lowest for
Model 1.2 (model with three design variables, `Harmony`, `Instrument`,
and `Voice` and interaction between `Harmony` and `Voice`) and BIC was
the lowest for Model 1.1 (additive model with only three design
variables). The result of the ANOVA test was that Model 1.2 was the most
preferred model out of Models 1.0 and 1.4 (Appendix B.1). Between Models
2.0 to 2.4, AIC was the lowest for Model 2.2 (model with 3 design
variables, interaction between `Voice` and `Harmony`, and random
intercept for each participant) and BIC was the lowest for Model 2.1
(model with 3 design variables and random intercept for each
participant). The result of the ANOVA test was that Model 2.2 was the
most preferred model out of Models 2.0 and 2.4. For step 3, Model 2.2
was used as a basis when adding different combinations of random slope
for three design variables. The correlation between the random slopes
for the three design variables were forced to be zero as the random
effects become more interpretable (Appendix B.3). Between Models 3.1 to
3.7, AIC and DIC were the lowest for Model 3.7 (model with 3 design
variables, interaction between `Voice` and `Harmony`, random intercept
for each participant, and random slopes for all three design variables),
and BIC was the lowest for Model 3.4 (model with 3 design variables,
interaction between `Voice` and `Harmony`, random intercept for each
participant, and random slopes for `Instrument` and `Harmony`). The
model chosen after completing steps 1 to 3 was Model 3.7, and the model
equation can be found in Appendix B.4.

### Table 2: Experimental factors, interactions, and random effects present in Models 1.0 to 1.4, Models 2.0 to 2.4, and Models 3.1 to 3.7.

| Model | 3 Design Variables | Interactions      | Random Intercept | Random Slope for Instrument | Random Slope for Harmony | Random Slope for Voice |
|-------|--------------------|-------------------|------------------|-----------------------------|--------------------------|------------------------|
| 1.0   | N                  | X                 | N                | N                           | N                        | N                      |
| 1.1   | Y                  | X                 | N                | N                           | N                        | N                      |
| 1.2   | Y                  | Voice and Harmony | N                | N                           | N                        | N                      |
| 1.3   | Y                  | All two-way       | N                | N                           | N                        | N                      |
| 1.4   | Y                  | All three-way     | N                | N                           | N                        | N                      |
| 2.0   | N                  | X                 | Y                | N                           | N                        | N                      |
| 2.1   | Y                  | X                 | Y                | N                           | N                        | N                      |
| 2.2   | Y                  | Voice and Harmony | Y                | N                           | N                        | N                      |
| 2.3   | Y                  | All two-way       | Y                | N                           | N                        | N                      |
| 2.4   | Y                  | All three-way     | Y                | N                           | N                        | N                      |
| 3.1   | Y                  | Voice and Harmony | Y                | Y                           | N                        | N                      |
| 3.2   | Y                  | Voice and Harmony | Y                | N                           | Y                        | N                      |
| 3.3   | Y                  | Voice and Harmony | Y                | N                           | N                        | Y                      |
| 3.4   | Y                  | Voice and Harmony | Y                | Y                           | Y                        | N                      |
| 3.5   | Y                  | Voice and Harmony | Y                | Y                           | N                        | Y                      |
| 3.6   | Y                  | Voice and Harmony | Y                | N                           | Y                        | Y                      |
| 3.7   | Y                  | Voice and Harmony | Y                | Y                           | Y                        | Y                      |

### Table 3: AIC, BIC, and DIC values for Models 1, 2 and 3.

| Model | AIC   | BIC   | DIC   |
|-------|-------|-------|-------|
| 1.0   | 11918 | 11930 | 11918 |
| 1.1   | 11198 | 11251 | 11198 |
| 1.2   | 11193 | 11281 | 11193 |
| 1.3   | 11209 | 11281 | 11209 |
| 1.4   | 11221 | 11437 | 11221 |
| 2.0   | 11430 | 11448 | 11424 |
| 2.1   | 10431 | 10489 | 10411 |
| 2.2   | 10418 | 10511 | 10386 |
| 2.3   | 10432 | 10583 | 10380 |
| 2.4   | 10438 | 10659 | 10362 |
| 3.1   | 10044 | 10172 | 10000 |
| 3.2   | 10334 | 10485 | 10282 |
| 3.3   | 10430 | 10558 | 10386 |
| 3.4   | 9902  | 10089 | 9838  |
| 3.5   | 10056 | 10219 | 10000 |
| 3.6   | 10346 | 10532 | 10282 |
| 3.7   | 9901  | 10122 | 9825  |

### Table 4: Model after selection of person covariates
 
|                                                  | Additive Fixed Effects |
|--------------------------------------------------|------------------------|
| (Intercept)                                      | 4.95 (0.93)            |
| HarmonyI-V-IV                                    | 0.21 (0.20)            |
| HarmonyI-V-VI                                    | 1.27 (0.28)            |
| HarmonyIV-I-V                                    | -0.30 (0.20)           |
| Instrumentpiano                                  | 1.65 (0.24)            |
| Instrumentstring                                 | 3.59 (0.31)            |
| Voicepar3rd                                      | -0.31 (0.20)           |
| Voicepar5th                                      | -0.20 (0.20)           |
| X16.minus.17                                     | -0.15 (0.05)           |
| ConsNotes                                        | -0.35 (0.07)           |
| PachListen3                                      | -1.80 (0.66)           |
| PachListen4                                      | 1.56 (0.93)            |
| PachListen5                                      | -0.95 (0.52)           |
| ClsListen1                                       | -0.28 (0.35)           |
| ClsListen3                                       | 0.38 (0.42)            |
| ClsListen4                                       | 6.25 (1.10)            |
| ClsListen5                                       | -0.06 (0.54)           |
| KnowRob1                                         | -0.54 (0.48)           |
| KnowRob5                                         | 1.34 (0.43)            |
| KnowAxis1                                        | -6.05 (1.37)           |
| KnowAxis5                                        | -0.47 (0.33)           |
| X1990s2000s                                      | -0.15 (0.12)           |
| X1990s2000s.minus.1960s1970s                     | 0.25 (0.09)            |
| NoClass                                          | 0.67 (0.18)            |
| APTheory1                                        | 2.61 (0.38)            |
| Composing1                                       | -0.47 (0.31)           |
| Composing2                                       | 1.01 (0.48)            |
| Composing3                                       | 0.41 (0.43)            |
| Composing4                                       | 0.79 (0.55)            |
| Composing5                                       | -1.84 (1.24)           |
| GuitarPlay1                                      | 0.43 (0.60)            |
| GuitarPlay2                                      | 3.48 (0.56)            |
| GuitarPlay4                                      | 2.23 (0.99)            |
| GuitarPlay5                                      | -4.50 (0.76)           |
| HarmonyI-V-IV:Voicepar3rd                        | -0.42 (0.28)           |
| HarmonyI-V-VI:Voicepar3rd                        | -0.71 (0.28)           |
| HarmonyIV-I-V:Voicepar3rd                        | 0.75 (0.28)            |
| HarmonyI-V-IV:Voicepar5th                        | -0.21 (0.28)           |
| HarmonyI-V-VI:Voicepar5th                        | -0.53 (0.28)           |
| HarmonyIV-I-V:Voicepar5th                        | 0.33 (0.28)            |
| AIC                                              | 6200.94                |
| BIC                                              | 6542.67                |
| Log Likelihood                                   | -3036.47               |
| Num. obs.                                        | 1540                   |
| Num. groups: Subject                             | 43                     |
| Var: Subject (Intercept)                         | 0.00                   |
| Var: Subject.1 Instrumentguitar                  | 0.81                   |
| Var: Subject.1 Instrumentpiano                   | 1.98                   |
| Var: Subject.1 Instrumentstring                  | 1.35                   |
| Cov: Subject.1 Instrumentguitar Instrumentpiano  | 0.42                   |
| Cov: Subject.1 Instrumentguitar Instrumentstring | -0.81                  |
| Cov: Subject.1 Instrumentpiano Instrumentstring  | 0.48                   |
| Var: Subject.2 Voicecontrary                     | 0.00                   |
| Var: Subject.2 Voicepar3rd                       | 0.12                   |
| Var: Subject.2 Voicepar5th                       | 0.04                   |
| Cov: Subject.2 Voicecontrary Voicepar3rd         | 0.00                   |
| Cov: Subject.2 Voicecontrary Voicepar5th         | 0.00                   |
| Cov: Subject.2 Voicepar3rd Voicepar5th           | 0.07                   |
| Var: Subject.3 HarmonyI-IV-V                     | 0.00                   |
| Var: Subject.3 HarmonyI-V-IV                     | 0.13                   |
| Var: Subject.3 HarmonyI-V-VI                     | 1.68                   |
| Var: Subject.3 HarmonyIV-I-V                     | 0.04                   |
| Cov: Subject.3 HarmonyI-IV-V HarmonyI-V-IV       | 0.00                   |
| Cov: Subject.3 HarmonyI-IV-V HarmonyI-V-VI       | 0.00                   |
| Cov: Subject.3 HarmonyI-IV-V HarmonyIV-I-V       | 0.00                   |
| Cov: Subject.3 HarmonyI-V-IV HarmonyI-V-VI       | 0.16                   |
| Cov: Subject.3 HarmonyI-V-IV HarmonyIV-I-V       | -0.05                  |
| Cov: Subject.3 HarmonyI-V-VI HarmonyIV-I-V       | 0.09                   |
| Var: Residual                                    | 2.44                   |

In addition to the three design variables, interaction between Voice and
Harmony, random intercept for each participant and three random slopes
for the three design variables for each participant (Model 3.7), 12
additional person covariates were selected by two passes of backwards
selection of fixed effects using AIC and BIC by `fitLMER.fnc` in step 4
of model selection (Appendix C.1). Random effects selection using DIC
resulted in all three random slopes for the three design variables being
kept in the model (Appendix C.2). The residual plots for the final model
showed that our model seems to satisfy the assumptions of normality and
constant variance of both level-1 and level-2 residuals well (Appendix
C.3). Seven fixed effects have variance inflation factor (VIF) greater
than 10, which is a common threshold for serious collinearity between
predictors in the model [@lecture-07]. The coefficient estimates for the
fixed effects with their standard error, the coefficient estimates for
random effects with their variances and covariances for the final model
are shown in Table 4. Out of all three design variables and 12
person covariates in the model, all predictors have statistically
significant levels except for the variable `X1990s2000s`. Intrepretation
of all fixed effects can be found in Appendix C.4.

Based on the results presented in Table 4, we can answer the first research question,
"What experimental factor, or combinations of factors, has the strongest
influence on ratings?" which consist of three sub-questions:

-   **Does Instrument exert the strongest influence among the three
    design factors (Instrument, Harmonic Motion, Voice Leading), as the
    researchers suspect?** Yes, listeners think piano and especially
    strings, sound more classical than guitar. The instrument "piano"
    tends to add more than 1.65 points to the Classical rating compared
    to the baseline instrument guitar. The instrument "string" tends to
    add more than 3.5 points to the Classical rating compared to guitar.

-   **Among the levels of Harmonic Motion does I-V-VI have a strong
    association (the strongest) with classical ratings? Does it seem to
    matter whether the respondent is familiar with one or the other (or
    both) of the Pachelbel rants/comedy bits?** The harmony I-V-VI has
    the strongest association out of the four harmonic motions with
    Classical rating because it tends to add nearly one point on average
    to the Classical rating compared to the baseline harmony I-IV-V and
    adds the most to classical ratings compared to other two harmonies.
    Relative to baseline category 0 ("not at all"), listeners who
    express modest familiarity with the Rob Paravonian's Pachelbel Rant
    have lower Classical ratings by about 0.5 points, whereas those with
    modest familiarity with the Axis of Awesome bit, reduce their
    classical ratings by more than six points. The giant jump
    ($\hat\beta_{KnowAxis=1}-6.05$) may be mainly a small-sample bias
    issue, since only one listener scored in category 1 on this variable
    (Appendix C.4) Having the highest familiarity with Pachelbel Rant
    adds 1.3 points to the Classical rating, while having the highest
    familiarity with the Axis of Awesome bit reduces Classical rating by
    about 0.5 points.

-   **Among the levels of Voice Leading, does contrary motion have a
    strong (the strongest) association with classical ratings?** Yes,
    contrary motion have the strongest association with Classical rating
    among the levels of Voice Leading. Compared to contrary motion,
    having the Voice Leading pattern "parallel 3rds" tends to reduce
    Classical rating by 0.3 points. Compared to contrary motion, having
    the voice-leading pattern "parallel 5ths" tends to reduce Classical
    rating by 0.2 points.

## Are there differences in the way that musicians and non-musicians identify classical music?

A new dichotomized variable called `Musician` was created by
dichotomizing `Selfdeclare` variable. Statistical significance of the
interaction term between `Musician` and other predictors were analyzed.
In other words, `Musician` variable was 0 if the value of `Selfdeclare`
was less than a certain threshold, $c$, where $c$ is a value between 1
and 6 corresponding to the levels of the `Selfdeclare` variable.

$$\text{Musician}_i = \left\{\begin{array}{rl}
                  0, & \text{if Selfdeclare $\leq c$}\\
                  1, & \text{otherwise}
                  \end{array}\right.$$

Dichotomizing `Selfdeclare` variable so that about half the participants
are categorized as self-declared musicians and creating a new dummy
variable called `Musician` produced significant interaction between the
`Musician` variable and Harmony (Appendix D). The significant
interaction only involved one level of Harmony, which was I-V-VI. There
are some coefficient estimates at the order of 100 or 1000 and many
interactions are dropped. This means that Musicians tend to rate certain
harmony structures as classical differently from how non-musicians rate
them. The interactions with Musician change depending on where we
dichotomize the `Selfdeclare` variable. Appendix D shows that:

-   Dichotomizing between 1 and 2 ($c=1$) produced no significant
    interaction between `Musician` variable and other predictors.

-   Dichotomizing between 2 and 3 ($c=2$) produced significant
    interaction between `Musician` and `Harmony`, with
    $\hat{\beta}_{HarmonyI-V-VI:Musician} = 1.26$.

-   Dichotomizing between 3 and 4 ($c=3$) produced significant
    interaction between `Musician` and `Harmony`, and `Musician` and
    `OMSI`, with $\hat{\beta}_{HarmonyI-V-VI:Musician} = 1.41$ and
    $\hat{\beta}_{OMSI:Musician} = -0.03$.

-   Dichotomizing between 4 and 5 ($c=4$) produced significant
    interaction between `Musician` and `Harmony` with
    $\hat{\beta}_{HarmonyI-V-VI:Musician} = 1.95$.

-   Dichotomizing between 5 and 6 ($c=5$) produced no significant
    interaction between `Musician` variable and other predictors.

Therefore, the presence or absence of interactions of the dichotomized
`Musician` variable with design variables or with other person
covariates depends strongly on the value of $c$ in the above equation. A
final determination of the question of interactions of a dichotomized
version of the `Selfdeclare` with design factors and other terms in the
model should be determined after a discussion with the client.

# Discussion

In this report, I analyzed the influence of different experimental
factors and random effects on listeners' identification of music as
"classical".

## The influence of the three main experimental factors (Instrument, Harmony & Voice)

Choice of instrument seems to have an impact on listeners'
identification of music as "classical". Listeners tend to think that
piano and especially strings sound more classical than guitar. Piano and
strings add about 1.65 and 3.5 points to the Classical rating,
respectively, compared to musical stimuli with guitar as the instrument.
Among the levels of Harmonic Motion, the harmonic motion I-V-VI has the
strongest association with increasing classical rating out of the four
harmonic motions tested. Compared to the baseline harmony I-IV-V,
harmony I-V-VI, I-V-VI, and IV-I-V influence the classical rating by
0.2, 1.3, and -0.3 points on average, respectively. Among the levels of
Voice Leading, contrary motion have the strongest association with
increasing classical ratings. Compared to contrary motion, having the
voice leading patterns "parallel 3rds" and "parallel 5ths" tend to
reduce Classical rating by 0.3 and 0.2 points on average, respectively.

## Differences in the way that musicians and non-musicians identify classical music

There seems to be a difference in a way that musicians and non-musicians
identify classical music, but the result is sensitive to where the
dichotomization of participants into musicians and non-musicians occur.
If the dichotomizations of participants based on self-declaration to the
question to the question "Are you a musician" occur on ratings between 2
to 3, 3 to 4, and 4 to 5, then there seems to be a difference in how
musicians and non-musicians perceive harmonic motion I-V-VI as
classical. In particular, musicians seem to think that the harmonic
motion I-V-VI are more classical, adding over 1 points to classical
rating. If the dichotomizations occur on ratings between 1 and 2, and 5
and 6, there seems to be no difference in how musicians and
non-musicians perceive harmonic motion.

## Strengths and Weaknesses

One of the strengths of this study is that the study design was well
balanced across experimental conditions and subjects even after deletion
of incomplete observations. Another strength of this study is that we
explored many experimental factors and person covariates that could
influence participants' classification of music as classical. One of the
weaknesses of this study is that there are many missing data. Only 61%
of the observations were complete, and the number of subjects
represented were reduced from 70 to 43 subjects after removing
incomplete observations. Another weakness of the study is that the
coefficient estimates of the final multi-level model may have been
impacted by small-sample bias as some of the levels of the categorical
variables involved only one or two participants.

## Future works

In the future, verifying that the results are replicable in different
groups of listeners would be necessary. The participants were all
sampled from the undergraduate student population at the University of
Pittsburgh, and there might have been hidden confounders unique to the
group selected that influenced Classical ratings. Also, investigating
the interaction among person covariates could be a possibility for a
future work. Only the interaction between the person covariates and the
Musician variable was examined in this study, but other interactions
between person covariates might be influential to Classical rating.
Finally, it would be interesting to repeat the analysis for Popular
rating as the response variable, and compare the results to see which
experimental factors influence the classification of music as
"classical" or "popular".
