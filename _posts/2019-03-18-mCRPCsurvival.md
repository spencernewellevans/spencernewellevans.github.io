---
title: "Overall Survival Prediction Model for mCRPC Patients"
date: 2019-03-18
tags: [machine learning, data science, prostate cancer, R]
header:
    #image: "images/mcrpc/image.jpg"
---

## Problem Definition

Prostate cancer is one of the significant clinical challenge that affects a large amount of men around the world. Machine learning can be used to aqcuire new insights on the relationships between certain clinical variables and death in prostate cancer patients.

The dataset used for this project is acquired from the Prostate Cancer Dream Project at https://www.synapse.org/. The data is taken from 4 clinical studies examining approximately 1600 patients with over 140 clinical variables.

## Linear Discriminant Analysis

Linear discriminant analysis (LDA) is used to create a binary classifier to predict the occurence of death on new data. Before developing the model the dataset is reduced to only contain variable will a low percentage of missing data and large relationship to the occurence of death.

The "Caret" package is a useful machine learning library in R. The following

```r
library(caret)
pcdata$DEATH <- as.factor(pcdata$DEATH)
train_control = trainControl(method = 'cv', number = 10)
fitC <- train(DEATH~., data = pcdata, trControl = train_control, method = 'lda')
```

## Cox Proportional Hazards

Cox proportional hazards is a method of logistic regression that is commonly used in survival analysis. In order to produce an accurate estimate of the time to death measurement the most significant variables needed to be selected.

The Cox proportional hazards model generates an estimate of the effect that each variable has on the probability of survival. The following R code uses the coxph function from the "survival" library to determined  the risk associated with each variable in the dataset. The "survminer" library is used to visualize these relationships.

```r
library(survival)
library(survminer)
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

These variables have the largest impact on the survival probability which means they will also be the best predictors of the time to death.

To further narrow down the variables that have the greatest impact on survival time we obverse how they directly impact the probability of survival time. A useful tool for visualizing this relationship is the Kaplan-Meier plot. Each plot shown below compares the survival probability as a function of time when the variable has a value 1 or 0.

The p-value measures the similarity between the two curves. A low p-value indicates that the survival probability is affected significantly by that variable.

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

<!-- {% include gallery caption="" %} -->

Using this information a new model is constructed using only the most significant variables. The variables used for this model are ANALGESICS, PLEURA, MI, GONADOTROPIN, and LIVER. The follow R code is used to create a new survival model based on only on these variables. Predictions can be made on new data by creating a new survival curve based on the user inputted data. The last day alive is predicted at the time where the survival probability reaches 75%.

```r
ttd_fit <- coxph(Surv(time = train_data$LKADT_P, event = train_data$DEATH) ~
          LIVER + ANALGESICS + PLEURA + MI + GONADOTROPIN, data = train_data)
      sfit <- survfit(ttd_fit, new_event_data)
      ttd_pred <- sfit$time[which.min(abs(sfit$surv - 0.75))]
```

Both models have been implemented in an R Shiny web app [here](https://spencernewellevans.shinyapps.io/capstone_OG07_2_0/).

The full source code can be found [here](https://github.com/spencernewellevans/mCRPC-overall-survival).
