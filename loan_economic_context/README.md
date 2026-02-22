# Loan Performance Compared with Economic Context
This analysis examines loan performance as a function of not just borrower
characteristics, but also economic context.

A complete analysis is [here](https://github.com/jshuma/experimental/blob/main/loan_economic_context/lendingclub.ipynb).
This dataset pulls directly from Kaggle and the St. Louis Fed, but prompts for
a Kaggle username and API key in order to do so. Be sure to set the
`files_base` variable to match your local environment when running.

## Summary

We have obtained and explored three datasets:
- A dataset containing loan performance data from LendingClub.
- The effective Federal funds rate over time.
- S&P 500 performance over time.

The first of these datasets contains indicators that may be useful for predicting actual loan performance, but a follow-up analysis aims to analyze whether economic context is more important than borrower characteristics in determining actual loan performance.

To do this, the following approaches are expected to be used:
- A logistic regression model with the LendingClub dataset, to assess how accurately we can predict loan status as a function of borrower grade (which is itself determined by LendingClub's best efforts to model borrower characteristics in a simple measure).
  - It remains to be seen whether the model will be more accurate in predicting precisely which grade borrowers fall into, or in more-general cateogires (in which current and paid off are considered "good" while the various types of being late or in default are considered "bad").
- An ARMA process to model the effective Federal funds rate over time. (ARMA is more appropriate than ARIMA because the data is expected to be stationary, with no seasonal or long-term variation over time.)
- An ARIMA process to model the S&P 500. (ARIMA is more appropriate than ARMA, because the data is not expected to be stationary; it shows both seasonality (often captured glibly in the advice to "Sell in May and walk away") and long-term trends.

The analysis aims to train multiple models and compare their performance:
- The logistic regression model mentioned above, predicting borrower performance as a function of _only_ the borrower grade.
- Predicting borrower performance as a function of _only_ economic context. (For these purposes, we may analyze either the effective Federal funds rate, or the S&P 500 dataset, or a combination of both.)
- Predicting borrower performance as a function of _both_ borrower grade _and_ economic context.
