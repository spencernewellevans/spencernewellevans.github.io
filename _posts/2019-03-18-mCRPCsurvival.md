---
title: "Overall Survival Prediction Model for mCRPC Patients"
date: 2019-03-18
tags: [machine learning, data science, prostate cancer, R]
header:
    #image: "images/mcrpc/image.jpg"
excerpt: "Machine Learning, Data Science, Prostate Cancer"
gallery:
  - url: /images/figures/ANALGESICS.png
    imagepath: /images/figures/ANALGESICS.png
    alt: "figure"
    title: "figure"
  - url: /images/figures/CA.png
    imagepath: /images/figures/CA.png
    alt: "figure"
    title: "figure"
  - url: /images/figures/COPD.png
    imagepath: /images/figures/COPD.png
    alt: "figure"
    title: "figure"
  - url: /images/figures/GI.png
    imagepath: /images/figures/GI.png
    alt: "figure"
    title: "figure"
  - url: /images/figures/GONA.png
    imagepath: /images/figures/GONA.png
    alt: "figure"
    title: "figure"
  - url: /images/figures/liver.png
    imagepath: /images/figures/liver.png
    alt: "figure"
    title: "figure"
  - url: /images/figures/lymph.png
    imagepath: /images/figures/lypmh.png
    alt: "figure"
    title: "figure"
---

## Problem definition

Prostate cancer is...

## Solution overview

Machine learning can be used to determine...

## LDA background

Linear discriminant analysis (LDA) is...

## Code snippets
```r
pcdata$DEATH <- as.factor(pcdata$DEATH)
train_control = trainControl(method = 'cv', number = 10)
fitC <- train(DEATH~., data = pcdata, trControl = train_control, method = 'lda')
```

## Cox PH background

Cox proportional hazards is...

## Code snippets
```r
survival_obj = Surv(time = pcdata$LKADT_P, event = pcdata$DEATH)
fit.coxph <- coxph(survival_obj ~ ALP + AST + CA + HB + LDH + PSA + TBILI + PROSTATE + LYMPH_NODES + LIVER +
                     KIDNEYS + PLEURA + CORTICOSTEROID + ANALGESICS + GLUCOCORTICOID + ANTI_ANDROGENS + GONADOTROPIN +
                     LYMPHADENECTOMY + SPINAL_CORD_SURGERY + HEAD_AND_NECK + TURP + GIBLEED + MI + COPD + DVT + MHSOCIAL +
                     MHSURG + MHNEOPLA + MHMUSCLE + MHNERV + Age1 + Age2 + Age3 + Asian + Black + White + OtherRace,
                   data = pcdata)
```
### TABLE WITH TOP HAZARD RATIOS
| Variable       | Hazard ratio   |
| :------------- | :------------- |
| LIVER          | 0.59           |
| GIBLEED        | 0.50           |
| PLEURA         | 0.49           |
| COPD           | 0.44           |
| MI             | 0.41           |
| ANALGESICS     | 0.37           |
| TURP           | 0.36           |
| GONADOTROPIN   | 0.27           |
| LYMPH_NODES    | 0.21           |
| CA             | 0.20           |

### KAPLAN MEIER PLOTS
<!-- {% include gallery caption="" %} -->



```r
coxfit <- coxph(Surv(time = train_data$LKADT_P, event = train_data$DEATH) ~
                  LIVER + ANALGESICS + PLEURA + MI + GONADOTROPIN,
                data = train_data)
```

## Hazard ratios box plot
<img src="{{ site.url }}{{ site.baseurl }}/images/Toronto-Cityscape.jpg" alt="">

## Survival plots
<img src="{{ site.url }}{{ site.baseurl }}/images/Toronto-Cityscape.jpg" alt="">

#### link to app
#### link to repo

Project Description.
