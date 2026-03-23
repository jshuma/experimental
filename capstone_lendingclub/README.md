# Capstone Project
## LendingClub Final Report
- [EDA notebook](https://github.com/jshuma/experimental/blob/main/capstone_lendingclub/lendingclub_eda.ipynb): An examination of input data.
- [Analysis notebook](https://github.com/jshuma/experimental/blob/main/capstone_lendingclub/lendingclub_analysis.ipynb): The central analysis, with results.

### Problem
[LendingClub](http://lendingclub.com) is a financial services firm that previously offered peer-to-peer loans to U.S. borrowers: Individual borrowers would submit loan applications (along with both structured data and unstructured narratives); then individual investors would review these applications to make decisions whether to fund those loans, personally assuming a portion of the loans' default risk in exchange for interest.

Investors will be interested in predicting an answer the question: **from available data about a given loan application, is this loan a good investment?** To do this, they have traditionally relied on measures of a borrower's creditworthiness, in the absence of the larger economic picture.

This project aims to evaluate the extent to which better predictions can be made by evaluating *macroeconomic conditions*  prevailing specifically *at the time of loan origination*.
### Predictions
This project aimed to prove that **macroeconomic conditions, taken in conjunction with borrower data, would provide better prediction of default risk than borrower data alone.**

The hope is that doing this would lead not just to better financial performance for lenders, but also for their being able to use those savings to eventually lower interest rates for all borrowers. In essence, the lending market would become more efficient.
### Data Acquisition
In addition to data from LendingClub, we rely on the following two signals for macroeconomic context:
- **S&P 500**: Prepared by Standard & Poor's, this is a weighted average of the value of the 500 largest publicly-traded companies in the United States. It is often seen as an excellent proxy for the performance of the overall United States stock market.
- **Effective Federal Funds Rate**: This refers to the rate chosen by the United States Federal Reserve to be the rate at which money will be lent from the federal government on a short-term basis. It is widely seen as an indicator of the overall United States economy, with lower numbers corresponding to a poor economy (and a corresponding effort from the Fed to stimulate the conomy), and higher numbers corresponding to a stronger economy (with the Fed attempting to slow things down in order to prevent inflation).

The following datasets were obtained from Kaggle:
- **Primary data source**: LendingClub has offered a rich dataset on loans that were accepted, including risk factors and actual current payment statuses, totaling 151 columns; plus less-rich data on loans that were rejected. ([source](https://www.kaggle.com/datasets/wordsforthewise/lending-club)) This massive dataset, spanning 2007 through 2018, includes data on 2,260,701 accepted loans and 27,648,741 rejected ones. It covers important periods such as the 2008 "Great Recession".
- Effective Federal Funds Rate data, directly from the St. Louis Fed ([source](https://fred.stlouisfed.org/series/FEDFUNDS)), covering all time periods in the LendingClub data and more.
- S&P 500 data from 1927 to 2020 ([source](https://www.kaggle.com/datasets/henryhan117/sp-500-historical-data))
### Data Processing/Preparation
Data was cleaned according to standard preparation steps, detailed in the project's [EDA notebook](https://github.com/jshuma/experimental/blob/main/capstone_lendingclub/lendingclub_eda.ipynb). To briefly summarize, this included:
- Dropping all columns containing >20% NaN data. From there, only the most relevant few columns were selected. Having done this, all rows containing an NaN in _any_ column were dropped, resulting in a negligible total data loss.
- Considering only S&P 500 data above a moving average, in order to look specifically at recent performance rather long-term trends, in order to picture a rising or falling market.
- Considering both the absolute Fed funds rate, and its derivative (to indicate whether it's rising or falling).
- Dates of all types were converted to an index of the month in question; this data was then used to join these datasets.
### Modeling
No attempt was made to assess borrower grade; that work has been done countless times before, and is not the central interest of this project. Instead, LendingClub's own assessment of borrower grade, made from many data points, was used.

All models used in the project were a two-layer neural network. The first layer was a 31-node ReLu layer (with node count selected from the number of borrower grades available), while the second layer was an 8-node softmax layer (selected for the number of loan outcomes available). Other architectures were evaluated on an _ad hoc_ basis, without a significant change in model performance.

From here, several models were trained:
- A model predicting loan performance simply from borrower grade.
- A model predicting loan performance from both borrower grade and loan amount.
- A model that revised this to split not randomly, but in time (see below).
- A model that revised this to explicitly factor in time as an independent variable.
- A model that predicted loan performance from borrower grade, loan amount, and _how far the S&P 500 closed above its recent moving average_.
- A model that predicted loan performance from borrower grade, loan amount, and _the Effective Federal Funds rate (plus its derivative)_.
- A model that predicted loan performance from borrower grade, loan amount, and _both S&P 500 and Fed funds rate data_.
The thinking guiding the evolution of this model is detailed in the [analysis notebook](https://github.com/jshuma/experimental/blob/main/capstone_lendingclub/lendingclub_analysis.ipynb).
### Model Evaluation
Models were visualized by their training loss, evaluated based on validation loss, and finally compared based on baseline-vs.-validation accuracy and loss.
### Results
Borrower grade was able to predict actual loan performance (validation loss ≈ 1.41 for typical runs), but borrower grade plus loan amount predicted performance even better (validation loss ≈ 0.138). The latter model was selected as a baseline.

Factoring in S&P 500 data alone, in addition to borrower grade and loan amount, resulted in better predictions early on (validation loss ≈ 1.05), but overfitting eventually resulted in the model performing worse than baseline (validation loss ≈ 1.45).

Factoring in Fed funds rate data alone, in addition to borrower grade and loan amount, resulted in even better predictions than the above S&P 500-based analysis (validation loss ≈ 0.6 early on, but eventually being overtaken by overfitting).

But factoring in _both_ the S&P 500 _and_ Federal funds rate data, in addition to data available in the baseline, resulted in the best prediction evaluated (validation loss ≈ 0.55); this suggests that the S&P 500 and the Effective Federal Funds Rate each provide useful input, and they are not mutually redundant.
### Important findings
Models that split randomly into a test set and a training set performed better than those splitting before and after a cutoff date (where the cutoff date was selected such that 25% of data was reserved as a test set). As expected, the cutoff data performed worse; this suggests data leakage that gave an unfair advantage to learning data simply based on coincidental similarity of input data. Therefore, subsequent analyses were performed based on _cutoff date_ rather than randomly. As it turns out, this is a much more realistic comparison to real models, which would train on past data and use it to make predictions about unseen future data.

Macroeconomic data (including only that data known at loan origination time) provide a useful additional consideration beyond borrower performance, which can predict future loan performance. This suggests that lenders should assess loans differently based on overall conditions when an application is received.
### Next steps
This analysis suggests that additional macroeconomic data may perform useful. It would be useful to examine several more sources and select the most important ones.

Additional work on discovering useful model architectures can likely provide better performance. The particular model was chosen based on intuition rather than an exhaustive search, since proving certain hypotheses was the intent of the analysis, rather than training the best possible model. A realistic deployment of a model like this would necessitate more work on choosing a more performant architecture.