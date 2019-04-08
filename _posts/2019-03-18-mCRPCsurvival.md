---
title: "Overall Survival Prediction Model for mCRPC Patients"
date: 2019-03-18
tags: [machine learning, data science, prostate cancer, R]
header:
    #image: "images/mcrpc/image.jpg"
excerpt: "Machine Learning, Data Science, Prostate Cancer"
---

## Problem definition

Prostate cancer
## Solution overview

## LDA background

## Code snippets
```r
pcdata$DEATH <- as.factor(pcdata$DEATH)
train_control = trainControl(method = 'cv', number = 10)
fitC <- train(DEATH~., data = pcdata, trControl = train_control, method = 'lda')
```

## Cox PH background

## Code snippets
```r
survival_obj = Surv(time = pcdata$LKADT_P, event = pcdata$DEATH)
fit.coxph <- coxph(survival_obj ~ ALP + AST + CA + HB + LDH + PSA + TBILI + PROSTATE + LYMPH_NODES + LIVER +
                     KIDNEYS + PLEURA + CORTICOSTEROID + ANALGESICS + GLUCOCORTICOID + ANTI_ANDROGENS + GONADOTROPIN +
                     LYMPHADENECTOMY + SPINAL_CORD_SURGERY + HEAD_AND_NECK + TURP + GIBLEED + MI + COPD + DVT + MHSOCIAL +
                     MHSURG + MHNEOPLA + MHMUSCLE + MHNERV + Age1 + Age2 + Age3 + Asian + Black + White + OtherRace,
                   data = pcdata)
ggforest(fit.coxph, data = pcdata)
```

```r
coxfit <- coxph(Surv(time = train_data$LKADT_P, event = train_data$DEATH) ~
                  LIVER + ANALGESICS + PLEURA + MI + GONADOTROPIN,
                data = train_data)
```

<img src="{{ site.url }}{{ site.baseurl }}/images/Toronto-Cityscape.jpg" alt="">

## Hazard ratios box plot
<img src="{{ site.url }}{{ site.baseurl }}/images/Toronto-Cityscape.jpg" alt="">

## Survival plots
<img src="{{ site.url }}{{ site.baseurl }}/images/Toronto-Cityscape.jpg" alt="">

#### link to app
#### link to repo

Project Description.
