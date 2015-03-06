```python
import pandas as pd
import numpy as np

import sklearn.cross_validation as cv # KFolds
import sklearn.linear_model as lm     # LinearRegression()
import sklearn.metrics as mt          # R^2

##################
# HELPER FUNCTIONS
##################

# For a test set X and the known ys, compute R^2 score
def assess(model, X, y):
    y_pred = model.predict(X)         # for each point x, predict a y
    return mt.r2_score(y, y_pred)     # compute R^2 for these predictions


##################
# ENTRY POINT
##################

# Load the data
path = '.'   # ensure this path is your Excel file's directory
xl = pd.read_excel(path + '/US_Population.xlsx')

model = lm.LinearRegression()          # we want a linear regression model

n_rows = xl.shape[0]                     # number of rows of data - same as len(xl)
folds = cv.KFold(n_rows, n_folds = 5, shuffle=True)    # split data into five training sets

# Each fold contains a training set and a testing set.
# Each training/testing set contains INDICES into the data array
#   .. meaning, they are numbers from 0..n-1, where there are n points in the data array
for fold in folds:
    training = fold[0]                 # contains (rows - rows/n_folds) datapoints
    testing  = fold[1]                 # contains (rows/n_folds) datapoints

    # Create X and y training matrices for creating the model
    X_train  = xl.ix[training, 'Year']               # 1xn matrix of years (horizontal)
    X_train  = X_train.reshape(X_train.shape[0], 1)  # 1xn -> nx1 matrix of years (vertical)
    y_train  = xl.ix[training, 'Population']
    
    # Createte X and y testing matrices for testing the model
    X_test   = xl.ix[testing, 'Year']              # 1xn matrix of years (horizontal)
    X_test   = X_test.reshape(X_test.shape[0], 1)  # 1xn -> nx1 matrix of years (vertical)
    y_test   = xl.ix[testing, 'Population']
    
    # Calculate the linear regression coefficients, using the training data
    params = model.fit(X_train, y_train)
    print 'population = ' + str(params.coef_[0]) + '*year + ' + str(params.intercept_) + '\n'
    
    # Using the testing data, compute the R^2,
    #   .... against the model made with the training data
    print 'In sample R^2: ', assess(model, X_train, y_train)
    print 'Out of sample R^2: ', assess(model, X_test, y_test)
    print '------'
```
