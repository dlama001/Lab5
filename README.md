# Lab5--
title: "LAB 5"
author: "dawa lama"
date: "10/29/2020"
output:
  pdf_document: default
  html_document: default
  word_document: default
editor_options: 
  chunk_output_type: inline
---


```{r}
load("C:/Users/singh/OneDrive/Desktop/project new/lab 5/acs2017_ny_data.RData")
suppressMessages(attach(acs2017_ny))
use_varb <- (AGE >= 25) & (AGE <= 65) & (LABFORCE == 2) & (WKSWORK2 > 4) & (UHRSWORK >= 35) & (female == 1) 
dat_use <- subset(acs2017_ny,use_varb)
detach()
```



For regression, I have select and use 'INCWAGE' observations as our measure of wage, INCWAGE is set as a dependent variable in this regression analysis. I have use female in subset. In MOdel 1, I am trying to find out the effect of independent variable such as age, educational level and ethnicity(AfAm and Hispanic) .



```{r}
suppressMessages(attach(dat_use))
model1 <- lm(INCWAGE ~ AGE + educ_hs + educ_college + educ_advdeg + AfAm + Hispanic)
summary(model1)
```




In model1, Concentrate on a smaller subset than previous. For instance, I look at wages for Hispanic women with at least a college degree,on different age that is below
Model 1 illustrates the linear regression of income wages based on education level, age, and ethnicity.
```{r}
suppressMessages(require(stargazer))
stargazer(model1, type = "text")

```

```{r}
detach()

NNobs <- length(INCWAGE)
set.seed(12345)
graph_obs <- (runif(NNobs) < 0.1)
dat_graph <-subset(dat_use,graph_obs)  

plot(INCWAGE ~ AGE, pch = 16, col = rgb(0.5, 0.5, 0.5, alpha = 0.2), ylim = c(0,150000), data = dat_graph)

to_be_predicted1 <- data.frame(AGE = 25:65, female = 1, Hispanic = 0, AfAm =0, educ_hs = 0, educ_college = 1, educ_advdeg = 0)
to_be_predicted1$yhat <- predict(model1, newdata = to_be_predicted1)

lines(yhat ~ AGE, data = to_be_predicted1)
```

IN above's result of model 1, we can see than when age is increasing peoples income also go up, may be it happened because of people's experience works and grade salary increasing worked.
The coefficient estimates for Model 1 linear regression demonstrates multiple income wages depending on education level (high school degree, college degree, Advance degree) & ethnicity ( African American & Hispanics). The sample for this regression includes females with ages ranging from 25 to 65 years old and are currently part of the labor force and are fully employed with (35 hour work week). The graph aboves hows a positive correlation for income wages as the age increases, and regression line refers to the predicted values taking into consideration white females with a college degree.
Model 2 illustrates the linear regression of income wages based on education level, age, age squared, and ethnicity.

```{r}
suppressMessages(attach(dat_use))
model2 <- lm(INCWAGE ~ AGE + I(AGE^2) + educ_hs + educ_college + educ_advdeg + AfAm + Hispanic)
summary(model2)
```


```{r}
suppressMessages(require(stargazer))
stargazer(model2, type = "text")
```


```{r}
NNobs <- length(INCWAGE)
set.seed(12345)
graph_obs <- (runif(NNobs) < 0.1)
dat_graph <-subset(dat_use,graph_obs)  

plot(INCWAGE ~ AGE, pch = 16, col = rgb(0.5, 0.5, 0.5, alpha = 0.2), ylim = c(0,150000), data = dat_graph)

to_be_predicted2 <- data.frame(AGE = 25:65, female = 1, Hispanic = 0, AfAm = 0,educ_hs = 0, educ_college = 1, educ_advdeg = 0)
to_be_predicted2$yhat <- predict(model2, newdata = to_be_predicted2)

lines(yhat ~ AGE, data = to_be_predicted2)
```
In model 2's result we can say that advance degree and college degree play vital role to determine the salary or income level..The coefficient estimates for Model 2 linear regression continues to demonstrate multiple income wages depending on education level (high school degree, college degree, Advance degree), ethnicity (African American & Hispanics) but also taking into consideration age squared. The sample for this regression includes females with ages ranging from 25 to 65 years old and are currently part of the labor force and are fully employed with (35 hour work week). The graph above shows a positive correlation for income wages as the age increases, and regression line refers to the predicted values taking into consideration white females with a college degree. However the regression slope of the graph is positive as age increases, then decreases, indicating diminishing returns for wages as workers reach retirement age (62 years old).
Model 3 illustrates the linear regression of income wages based on education level, log of age, log of age squared, and ethnicity.
```{r}
suppressMessages(attach(dat_use))
model3 <- lm(INCWAGE ~ log(AGE) + I(log(AGE^2)) + educ_hs + educ_college + educ_advdeg + AfAm + Hispanic)
summary(model3)
```

```{r}
suppressMessages(require(stargazer))
stargazer(model3, type = "text")
```
 
```{r}
suppressMessages(attach(dat_use))
model4 <- lm(INCWAGE ~ AGE + I(AGE^2) + I(AGE^3) + I(AGE^4) +I(AGE^5) + I(AGE^6) + educ_hs + educ_college+ educ_advdeg + AfAm + Hispanic)

summary(model4)
```

```{r}
suppressMessages(require(stargazer))
stargazer(model4, type = "text")
```
```{r}
detach()

NNobs <- length(INCWAGE)
set.seed(12345)
graph_obs <- (runif(NNobs) < 0.1)
dat_graph <-subset(dat_use,graph_obs)  

plot(INCWAGE ~ (AGE), pch = 16, col = rgb(0.5, 0.5, 0.5, alpha = 0.2), ylim = c(0,150000), data = dat_graph)
to_be_predicted4 <- data.frame(AGE = 25:65, female = 1, white = 1, Hispanic =0, AfAm = 0,educ_hs = 0, educ_college = 1, educ_advdeg = 0)
to_be_predicted4$yhat <- predict(model4, newdata = to_be_predicted4)

lines(yhat ~ AGE, data = to_be_predicted4)
```


```{r}
suppressMessages(attach(dat_use))
model5 <- lm(log1p(INCWAGE) ~ AGE + I(AGE^2) + I(AGE^3) + I(AGE^4) + educ_hs + educ_college + educ_advdeg +white + AfAm + Hispanic)
summary(model5)
```

```{r}
suppressMessages(require(stargazer))
stargazer(model5, type = "text")
```

```{r}
NNobs <- length(log1p(INCWAGE))
set.seed(12345)
graph_obs <- (runif(NNobs) < 0.1)
dat_graph <-subset(dat_use,graph_obs)  

plot(log1p(INCWAGE) ~ (AGE), pch = 16, col = rgb(0.5, 0.5, 0.5, alpha = 0.2), ylim = c(10,11), data = dat_graph)

to_be_predicted5 <- data.frame(AGE = 25:65, female = 1, white = 1, Hispanic=0, AfAm = 0,educ_hs = 0, educ_college = 1, educ_advdeg = 0)
to_be_predicted5$yhat <- predict(model5, newdata = to_be_predicted5)
```

Model 5 expands on the conditions of model 2 but this time, the regression model is based on log of income wages which is the dependent variable. Since, Log expression is more sutable to simplyfy large numbers like income wages, the log values will be expressed within a smaller range compared to the original values. In our case, there would not be high values for the regression line given that the Log of income ranges between 10 to 11. Similar to the results from Model 2 regression line, the regression slope for Model 5 shows a positive slope as age increases, the income wages also increase but then decreases indicating a diminishing returns for wages as workers reach retirement age (62 years old).
The table below demonstrates the estimates for the coefficient variables for all the models that were run above (Models 1-5). The estimates coefficient values shows an increase of income wages for female workers (either white, hispanics or African American) depending on education level ranging from High school degree to college degree to Advance degree and also taking into consideration age. The reversing the variables is not ideal due to age and education level do not depend on different income wages.
```{r}
suppressMessages(require(stargazer))
stargazer(model1, model2, model3, model4, model5, type = "text")
```
