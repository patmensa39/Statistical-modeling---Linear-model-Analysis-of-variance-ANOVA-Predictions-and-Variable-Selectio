# Statistical-modeling---Linear-model-Analysis-of-variance-ANOVA-Predictions-and-Variable-Selection

---
title: "PROJECT MODE"
output:
  pdf_document: default
  html_document: default
  word_document: default
date: "November 2020"
theme: cerulean
---

<!-- For more info on RMarkdown see http://rmarkdown.rstudio.com/ -->


### Statistical modeling---Linear model, Analysis of variance (ANOVA), Predictions and Variable Selection

##install.packages("olsrr")

```{r}
### Regression model 
### Making a matrix scatter plot using the Mtcars dataset
mtcars.philant<-mtcars[, 1:7]
mtcars.philant
pairs(mtcars.philant)
```

```{r}
### Fitting a linear regression model for the above dataset
philant.model1<- lm(mpg~., data = mtcars.philant)
philant.model1

### This is the same as 
philant.model2<- lm(mpg~cyl+disp+hp+drat+qsec, data = mtcars.philant)
philant.model2
```

```{r}
### Using model fit 
summary(philant.model2)

summary(philant.model1)
```

```{r}
### Analysis of variance ANOVA
anova(philant.model1)
anova(philant.model2)
```

```{r}
### The coefficient estimate giving the effect on the response of each covariate taking in account the presents of other covariates.
model1<-lm(mpg~cyl, data = mtcars.philant)
summary(model1)

model2<-lm(disp~wt, data = mtcars.philant)
summary(model2)

### ANOVA table for for each model. Remember the other covariates does not account for the residuals in the data.
anova(model1)
anova(model2)
```

```{r}
### Modeling check using residuals to check linearity, constant variance, independence assumption. this can be done using the residaul plot
resid(model1)
resid(model2)
```


```{r}
### Fitted models
fitted(model1)
fitted(model2)
plot(model1)
plot(model2)
```

```{r}
### Predictions. 
predict(model1, newdata = data.frame(cyl=c(5,7)))
predict(model1, newdata = data.frame(cyl=c(5,7)),se.fit = FALSE)
predict(model2, newdata = data.frame(wt=c(3,9)), se.fit = TRUE)
predict(model2, newdata = data.frame(wt=c(3,9)), se.fit = TRUE)
predict(model2, newdata = data.frame(wt=c(3,9)),interval = "confidence")
predict(model1, newdata = data.frame(cyl=c(5,7)),se.fit = FALSE, interval = "prediction")
```


```{r}
### Variable selection
### All possible regression. This test all the possible subsets of day set of potential independent variables. 
library(olsrr)
ols_step_all_possible(philant.model1, details=TRUE)
philant<-ols_step_all_possible(philant.model1)
plot(philant)
```

```{r}
### BEST SUBSET REGRESSION. This selects the best subset of predictors that's do the best at meeting some well-defined objects criterion, Such as having the best our largest all square value or the smallest MSE, Mallow CP all  AIC 
ols_step_best_subset(philant.model1, details=TRUE)
philant1<-ols_step_best_subset(philant.model1)
plot(philant1)
```


```{r}
### Stepwise forward regression. This put in one variable at a time and check which one is the best, and later keep on adding the variables and check which one is the best till they finish.  
ols_step_forward_p(philant.model1, details = TRUE)
philant3<-ols_step_forward_p(philant.model1)
plot(philant3)
```

```{r}
### Backwards elimination. this starts with all the variable, check the worst one and remove it till we get a good model 
ols_step_backward_p(philant.model1, details = TRUE)
philant4<-ols_step_backward_p(philant.model1)
plot(philant4)
```

```{r}
### Step-wise regression. In this regression there are no variables left to enter or remove any more. The model includes all the predictor variables.
ols_step_both_p(philant.model1, details = TRUE)
philant5<-ols_step_both_p(philant.model1)
plot(philant5)
```

```{r}
### Stepwise AIC forward regression
ols_step_forward_aic(philant.model1, details = TRUE)
philant6<-ols_step_forward_aic(philant.model1)
plot(philant6)

```

```{r}
### Stepwise AIC backwards regression
ols_step_backward_aic(philant.model1, details = TRUE)
philant7<-ols_step_backward_aic(philant.model1)
plot(philant7)

```


```{r}
### Stepwise AIC regression
ols_step_both_aic(philant.model1, details = TRUE)
philant8<-ols_step_both_aic(philant.model1)
plot(philant8)
```
