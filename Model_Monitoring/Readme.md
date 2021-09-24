# Model Monitoring

Monitoring production machine learning models

### Data shift
As the its name suggests, a data shift occurs when there is a change in the data distribution. When building a Machine Learning model, one tries to unearth the (possibly non-linear) relations between the input and the target variable. Upon creating a model on this data, he then might feed new data of the same distribution and expect to get similar results. But in real-world cases this is rarely true. Changing consumer habbits, technology breakthroughs, political, socioeconomic and other unpredictable factors can dramatically change either a) the input dataset or b) the target variable or c) the underlying patterns and relations betweeen the input and output data. Each one of these situations has a distinct name in Data Science but they all lead to the same thing: model performance degradation.

_Data shift or data drift, concept shift, changing environments, data fractures are all similar terms that describe the same phenomenon: the different distribution of data between train and test sets_

#### 1. Covariate shift

Covariate shift is the change of distributions in one or more of the independent variables (input features). Covariate shift may happen due to a changing environment that affects the input variables but not the target variable.

Definition 1. Covariate shift is termed the situation where Ptrn(Y|X)=Ptst(Y|X) but Ptrn(X) ≠Ptst(X)

How to Detect?

Ddeploy a machine learning solution but, instead of trying to predict the target variable, we will build a classifier that will try to distinguish between the train and test sets. 

In order to check if the given test set is vastly different from the train set we are going to create a new dummy variable called ‘is_train’. This variable will contain all ones (1) in the train set and all zeroes (0) in the test set. We will then use every indepedent variable, in turn, and on its own, to try to predict the new target variable ‘is_train’. If an input variable is able to predict the new dummy variable, i.e. to separate between the train and test set, this means that this variable presents covariate shift between the train and test set and must be taken care of. 

- Create new variable with ones in train set and zeroes in test set.
- Merge the two sets and shuffle randomly.
- Split in new train-test at 80%-20%
- For each single input variable:
  - Fit a simple classifier (e.g. Random Forests)
  - Predict ‘is_train’ variable
  - Calculate AUC
  - If AUC exceeds some threshold (e.g. 80%), feature displays data shift
  
A similar solution is to fit the model using ALL the input variables and then to use a model interpretability technique (such as SHAP) to examine the impact of each variable in the prediction, therefore understanding which ones present covariate shift.

#### 2. Prior probability shift

Prior probability shift can be thought of as the exact opposite of covariate shift: it is the case that input feature distributions remain the same but the distribution of the target variable changes. A prior probability shift can occur in cases where despite the input variables remain the same, our target variable changes. 

Definition 2: Prior probability shift is termed the situation where Ptrn(X|Y)=Ptst(X|Y) but Ptrn(Y) ≠Ptst(Y)

Detecting prior probability shift is really straight-forward. The first way to detect it is simply to plot a histogram of variable Y frequencies between train and test set. This way we can have a quick visual insight on how different our dependent variable looks. On a second stage and to confirm our visual findings we can apply some statistical test of mean difference (t-test, ANOVA, etc). Let’s see how these apply below.

#### 3. Concept drift

Definition 3. A concept drift is termed the situation where Ptrn(Y|X) ≠ Ptst(Y|X).A concept drift happens where the relations between the input and output variables change. So we are not anymore only focusing on X variables or only the Y variable but on the relations between them. 

Definition 3. A concept drift is termed the situation where Ptrn(Y|X) ≠ Ptst(Y|X).



