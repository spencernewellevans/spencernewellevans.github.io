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

## Linear Discriminant Analysis

Linear discriminant analysis (LDA) is...

The caret 

```r
pcdata$DEATH <- as.factor(pcdata$DEATH)
train_control = trainControl(method = 'cv', number = 10)
fitC <- train(DEATH~., data = pcdata, trControl = train_control, method = 'lda')
```

## Cox Proportional Hazards

Cox proportional hazards is a method of logistic regression that is commonly used in survival analysis. In order to produce an accurate estimate of the time to death measurement the most significant variables needed to be selected.

The Cox proportional hazards model generates an estimate of the effect that each variable has on the probability of survival. The following R code uses the coxph function from the "survival" library to determined  the risk associated with each variable in our dataset:

```r
survival_obj = Surv(time = pcdata$LKADT_P, event = pcdata$DEATH)
fit.coxph <- coxph(survival_obj ~ ALP + AST + CA + HB + LDH + PSA + TBILI + PROSTATE + LYMPH_NODES + LIVER +
                     KIDNEYS + PLEURA + CORTICOSTEROID + ANALGESICS + GLUCOCORTICOID + ANTI_ANDROGENS + GONADOTROPIN +
                     LYMPHADENECTOMY + SPINAL_CORD_SURGERY + HEAD_AND_NECK + TURP + GIBLEED + MI + COPD + DVT + MHSOCIAL +
                     MHSURG + MHNEOPLA + MHMUSCLE + MHNERV + Age1 + Age2 + Age3 + Asian + Black + White + OtherRace,
                   data = pcdata)
```
The variables with the largest hazard ratios are included in the table below.

<table>
  <caption>Top hazard ratios</caption>
  <tr>
    <th>Variable</th>
    <th>Hazard ratio</th>
  </tr>
  <tr>
    <td>LIVER</td>
    <td>0.59</td>
  </tr>
  <tr>
    <td>GIBLEED</td>
    <td>0.50</td>
  </tr>
  <tr>
    <td>PLEURA</td>
    <td>0.49</td>
  </tr>
  <tr>
    <td>COPD</td>
    <td>0.44</td>
  </tr>
  <tr>
    <td>MI</td>
    <td>0.41</td>
  </tr>
  <tr>
    <td>ANALGESICS</td>
    <td>0.37</td>
  </tr>
  <tr>
    <td>TURP</td>
    <td>0.36</td>
  </tr>
  <tr>
    <td>GONADOTROPIN</td>
    <td>0.27</td>
  </tr>
  <tr>
    <td>LYMPH_NODES</td>
    <td>0.21</td>
  </tr>
</table>


<table>
  <caption>Kaplan-Meier plots for vavriables with significant p-values</caption>
  <tr>
    <td><img src="/images/figures/liver.png" alt=""></td>
    <td><img src="/images/figures/ANALGESICS.png" alt=""></td>
    <td><img src="/images/figures/PLEURA.png" alt=""></td>
  </tr>
  <tr>
    <td><img src="/images/figures/TURP.png" alt=""></td>
    <td><img src="/images/figures/MI.png" alt=""></td>
    <td><img src="/images/figures/GONA.png" alt=""></td>
  </tr>
</table>




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
