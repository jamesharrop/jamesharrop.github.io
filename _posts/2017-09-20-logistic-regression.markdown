---
title:  "Logistic regression: a simple explanation and worked example"
categories: python machine_learning
---
Logistic regression is a technique for creating a model of the relationship between one or more input ("independent") variables, and an output ("dependent") variable which is categorical. Categorical means that the dependent variable can not take just any value, it can only take certain values. 

Let's look at an example to clarify this. Say we are trying to decide whether to buy a car or not. For the purposes of this example, all we know about the car is its age. The age of the car is the "independent variable" here. Our outcome is whether or not the car lasts for 1 year without breaking down. Therefore this is a categorical outcome variable - either the car breaks down or it does not. For the purposes of our example, there is no such thing as a partial car break-down. 

To collect some data, we ask some of our friends, how old was your car when you bought it, and did it break down in the first year that you owned it? Here's the results:

Age (in years)| Breakdown?
:------------:|:-----------:
15 | Yes
10 | Yes
8 | No
7 | Yes
4 | No
3 | No
2 | Yes
1 | No

To analyse this, we need a mathmatical function which takes as input the age of the car, and outputs a binary (0 or 1) variable. The function which we use is called the sigmoid function:

y = 1 / (1 + exp(-x))

We can see that this equation does what we want: as x tends to very negative numbers, y quickly approaches zero. As x tends to very positive numbers, y quickly approaches one. 

We will say that if x>0.5 then the output is 1.
If x<0.5 then the output is 0.

How can we use this with our car age data? 

First let's say that: y = 1 for a breakdown and y = 0 for no breakdown.

We then say that x and the car age are related by some parameters which we don't yet know:

$$x=B + C . (carAge)$$

An example - let's take the 15 year old car from the chart above. This car broke down. So the equation we have is:

$$y = \frac{1}{1+\exp(-(B+15C))}$$

We want values of B and C such that y > 0.5. We could do the same with the 8 year old car.

$$y = \frac{1}{1+\exp(-(B+8C))}$$

For this car, we want values of B and C such that $$y < 0.5$$.

We could carry on in the same way for the other cars.

The challenge now is, what are the values of B and C to give us the correct predictions (correct values of y) for all the cars? It turns out that we can define a "cost function" involving these parameters. In effect we then adjust the parameters to find the minimum value of this cost function. Those parameter values are the ones we need for our model. We can plug in another car age and use those parameter values in the equations and output a value for whether our model predicts that car will break down or not.

Next, let's use Python and scikit-learn to find those parameter values.

## Logistic regression with scikit-learn

Scikit-learn is a powerful machine learning toolkit in Python.  This is a simple example of how use scikit-learn to carry out logistic regression.

We start by importing the modules which we will need. We will be using NumPy and scikit-learn.

{% highlight python3 %}
import numpy as np
from sklearn.linear_model import LogisticRegression
from sklearn.preprocessing import StandardScaler
{% endhighlight %}

Now let's input the data. We only have a small amount of data so we can type the data in as a NumPy array. X_train is the name for the array containing the car ages in the table above. y_train is the name for the array containing the breakdown information (1 = breakdown, 0 = no breakdown).

```python3
X_train = np.array([15.0, 10.0, 8.0, 7.0, 4.0, 3.0, 2.0, 1.0])
y_train = np.array([1, 1, 0, 1, 0, 0, 1, 0])
```

We need to reshape the X_train array so that it is in the correct format for scikit. In the reshape command, minus 1 means that dimension is inferred - and so this reshapes the data into one column:

```python3
X_train = X_train.reshape(-1, 1)
```

Now we scale our training data:
```python3
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
```

And run the logistic regression:
```python3
clf = LogisticRegression()
clf.fit(X_train, y_train)
```

Scikit has now fitted parameters to a logistic regression model of our data. Let's check out the results using some test data. Say we have five cars with ages 20, 14, 6, 2, 1 years. According to our model, will those cars break down?

We create an array to hold that test data, reshape it into a column, and scale it (using the scaler parameters which were fitted to the training data):

```python3
X_test = np.array([20.0,14.0,6.0,2.0,1.0])
X_test = X_test.reshape(-1, 1)
X_test = scaler.transform(X_test)
```

Now to run the logistic regression model on that test data:

```python3
y_test = clf.predict(X_test)
```

Our output is:

array([1, 1, 0, 0, 0])

This matches what we might expect from the training data, ie that the older cars are more likely to break down.

Let's look behind the scenes at what's happening here.

```python3
print(clf.coef_)
print(clf.intercept_)
```

And we find that:
clf.coef_ = [[ 0.72432097]] and clf.intercept_ = [ 0.01193235]

Let's put these values (rounded a little for simplicity) into the formula from the previous post:

y = 1 / ( 1 + exp(-(0.0119 + 0.724 * age)))

We can try a few values for age (use the scaled values) in this formula and confirm the results for y_test.

Now, let's investigate the "cost function" and how the coefficient and intercept values are calculated.

## Logistic regression: the cost function

A logistic regression model is fitted by minimising the value of a "cost function". We adjust the value of the parameters and calculate the cost function - when the cost function is as low as possible then those parameters are the best fit to the data. This minimisation can be accomplished using a variety of algorithms, the simplest of which is gradient descent, but in practice more sophisticated algorithms are used.

First we define a function h which is takes the two parameters (here we use letters B and C for those) and also an independent variable x which in our example is the age of the car (after scaling):
 
<pre class="lang:default decode:true " >def h(B, C, x):
    return 1 / (1 + exp(-(B + C * x)))</pre> 

Then we can calculate the following formula for each value of x, B, C, and y (y is the dependent variable, here whether the car breaks down or not):

<pre class="lang:default decode:true " >y * log(h(B, C, x)) + (1-y)*log(1-h(B, C, x))</pre> 

We sum over all the values for x and y and multiply by -(1/m) where m is the number of data points.

Here is the full function in maths syntax:

$$J=-\frac{1}{m}\sum_{i=1}^{m}[y.\log{h(x)}+(1-y).\log{(1-h(x))}]$$

And in code:

<pre class="lang:python decode:true " >def J(B, C):

    def h(B, C, x):
        return 1 / (1 + exp(-(B + C * x)))

    m = X_train.size
    s = 0
    
    for r in range(0, m):
        x = X_train[r][0]
        y = y_train[r]
        s += y * log(h(B, C, x)) + (1-y)*log(1-h(B, C, x))
    s = -s/m
    return s</pre> 

We can use matplotlib to generate a 3D plot of this function:

![3D plot of cost function]({{ "/assets/cost_fn_1.png" | absolute_url }})

We can rotate the plot to better see the minimum value of the cost function as related to the coefficient (which we have been calling C):

![3D plot of cost function to show coefficient minimum]({{ "/assets/cost_fn_coef.png" | absolute_url }})

And the minimum as related to the intercept (B):

![3D plot of cost function to show intercept minimum]({{ "/assets/cost_fn_intercept.png" | absolute_url }})

This appears to match up with the values which were calculated by scikit-learn, of approximately 0.7 for the coefficient and 0.01 for the intercept.