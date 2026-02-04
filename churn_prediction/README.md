# Methodology

The churn dataset was explored and cleaned using industry-standard techniques. Data gaps and quality problems were addressed. Numerical columns were normalized using `StandardScaler`, and categorical columns were one-hot encoded using `OneHotEncoder`. Linear regression, K nearest neighbors, decision tree, and SVC classifiers were evaluated; where feasible, the analysis was repeated while biasing for recall over precision.

See notebook [here](https://github.com/jshuma/experimental/blob/main/churn_prediction/prompt_III.ipynb).

# Findings

A baseline of simply selecting all samples as "no" was used, providing an accuracy of 89%. All classifiers tested improved very slightly on this, but were not significantly different in performance between each other; fine-tuning hyperparameters did not make a significant improvement.

Since false negatives cost banks significantly more than false positives, we optimize for recall at the expense of precision. In doing so, major improvements in recall were achieved, but produced a regression in precision; however, the improvement was much more significant than the regression, so this is a significant net win.

A more complete discussion of findings is presented in the notebook [here](https://github.com/jshuma/experimental/blob/main/churn_prediction/prompt_III.ipynb).

# Recommendations

A more rigorous definition of the tradeoffs is needed, in particular how many unnecessary customer reachouts are tolerable in order to retain one additional customer. Armed with this definition, it will be possible to refine class weights in order to achieve an optimal classifier. 

Specific recommendations are available under "Next Steps" in the  [notebook](https://github.com/jshuma/experimental/blob/main/churn_prediction/prompt_III.ipynb).

