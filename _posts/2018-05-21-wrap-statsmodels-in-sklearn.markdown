---
layout: post
subtitle: "How to use sklearn cross-validation on a statsmodel formula-style model"
picture: ""
pwidth: ""
pheight: ""
long: "T"
showpic: "F"
categories: data

---


So, you want to use scikitlearn's cross-validation framework, but want to use a
statsmodels object with the formula syntax? No problem.

David Dale has an excellent [SO post](https://stackoverflow.com/questions/41045752/using-statsmodel-estimations-with-scikit-learn-cross-validation-is-it-possible/), but his answer is for the standard statsmodels.api. 
Here's a slight adaptation for statsmodels.formula.api:


```
from sklearn.base import BaseEstimator, RegressorMixin

class SMFormulaWrapper(BaseEstimator, RegressorMixin):
    """ A sklearn-style wrapper for formula-based statsmodels regressors """
    def __init__(self, model_class, formula):
        self.model_class = model_class
        self.formula = formula
    def fit(self, X, y=None):
        self.model_ = self.model_class(self.formula, data=X)
        self.results_ = self.model_.fit()
    def predict(self, X):
        return self.results_.predict(X)

```

Then you can do something like:

```
import statsmodels.formula.api as smf
formula = 'y  ~ numeric_var + categorical_var1'
cv_ols = cross_val_score(SMFormulaWrapper(smf.ols, formula), Xy, None, cv=3)
```

You don't need to pass in a y, since that information is already included in `formula`


