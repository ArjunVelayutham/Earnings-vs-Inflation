# Earnings-vs-Inflation
This compares the change in the earnings of Johnson &amp; Johnson to the inflation rate of USD.
---
title: "EarningIncrease vs Inflation"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## R Markdown



```{r JohnsonJohnsonnumbers}
summary(JohnsonJohnson)
```

## Johnson&Johnson Quarterly Earnings per Share

At the moment the above numbers are, well, numbers. A plot that can more easily convey the information has been put below.
```{r JohnsonJohnson, echo=FALSE, fig.width = 10, fig.height = 7}
plot(JohnsonJohnson)
```
As you can see, the earnings per share of Johnson & Johnson have gone up dramatically over time. However, inflation should also be taken into consideration. If you didn't know, inflation is when, over time, money becomes worth less than it was before. When the inflation percent is high, that mean money has become less valuable, and when its low, a currency has become more valuable.

```{r US Dollar Inflation, echo=FALSE, fig.width = 10, fig.height = 7, message=FALSE}
library(rvest)
library(dplyr)
library(lubridate)
library(xts)
inflation_url <- "http://inflationdata.com/inflation/Inflation_Rate/HistoricalInflation.aspx"

# read in the HTML table with the raw data
input.df <-
    read_html(inflation_url) %>%
    html_node("table#GridView1") %>%
    html_table(header = TRUE, trim = TRUE, fill = TRUE, dec = ".") 

# convert character percentages to numeric
inflation.df <- as.data.frame(lapply(input.df, function(x) as.numeric(sub(" %","",x))/100))
inflation.df$Year <- as.integer(inflation.df$Year*100)
mean.inflation <- mean(inflation.df$Ave.*100,na.rm=TRUE)

plot(inflation.df$Year, inflation.df$Ave.*100, pch=20, type="b",
     xlab="Year",ylab="Annual Avg. Inflation Rate")
title("Historical US Inflation Rates Since 1914")

abline(h=0, lty=2, col="black")
abline(h=mean.inflation, lty=2, col="red")
text(1950, mean.inflation,labels=paste0("Overall Average: ", round(mean.inflation, 2), "%"), pos=3, col="red")
```
Inflation is compounded annually. If the US Dollar inflated 3% from 1960-1961, then $1 in 1960 is worth $1.03 in 1961. As the graph above shows, the annual inflation rate is about 3%. This means that the amount needed to buy things will, on average, increase by more each year. This also means that company earnings need to match that increase to remain as profitable as they were before.
