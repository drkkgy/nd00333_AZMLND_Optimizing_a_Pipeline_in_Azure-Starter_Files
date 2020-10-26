# Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree.
In this project, we build and optimize an Azure ML pipeline using the Python SDK and a provided Scikit-learn model.
This model is then compared to an Azure AutoML run.

## Summary
**In 1-2 sentences, explain the problem statement: e.g "This dataset contains data about... we seek to predict..."**
Here we are leveraging machine learning to perform classification and predict whether a Term Deposit will be opened or not by the potential customer.
Dataset has various personal info about the employee and his past experience with Term Deposit. We seek to predict if he will open a Term Deposit or Not

**In 1-2 sentences, explain the solution: e.g. "The best performing model was a ..."**
AutoML Model outperformed the model trained with the help of Hyperdrive. The reason could be the parameter sample space 
given to hyperdrive and also AutoML was able to try out several different models while in our Hyperdrive experiment we were using 
only Logistic Regression model 
## Scikit-learn Pipeline
**Explain the pipeline architecture, including data, hyperparameter tuning, and classification algorithm.**
1. The Architecture consists of a Training Script (train.py) that has three main section
-- Loading Data from URL using TabularDatasetFactory
-- Cleaning the data using the custom Clean Method Provided
-- Splitting and training the Model using Logistic Regression model from SK Learn Library.
2. We have an SK Learn Estimator where we load the train.py file as well as compute to run an experiment on.
3. We Create a Hyperdrive Config File with RandomParemeterSampling for a parameter space consisting of (max_iter,--C ) variables that are to be optimised and Bandit Policy is used as an Early stoping policy
4. "max_iter" is the maximum no of iteration a model will run over for the solver to converge. It's critical as if we get too less value it will not allow the solver to converge 
   and if too high it may lead to overfitting and waste of computing resource in training a model that does not improve anymore.
5. "C" represents the inverse of Regularisation Strength. It is used to boost generalisation of the model or rather prevent it from overfitting.
   Smaller values of "C" specifies stronger regularisation. 
6. Classification algorithm used for this model training is Logistic Regression Classifier. The Highest accuracy achieved by this model was 0.91462 after Hyperdrive optimisation.
   The above results were achieved with C: Value of 0.1 and max_iter Value of (300,200,50) so it's quite clear that even 50 iterations were enough for the model to converge and increasing the iteration didn't improve it. But on the other hand, Hyperparameter "C" had a direct impact on training on reducing its value by a factor of 10 made the model perform worse.
**What are the benefits of the parameter sampler you chose?**
We have taken a Random sampling approach of randomly trying out different combinations to reach the best result. We could have taken Bayesian Sampling which can converge faster.
But with the help of early stoping property Bandit Policy in this case server the similar purpose 
**What are the benefits of the early stopping policy you chose?**
The main benefits of Bandit policy are that it helps us get an idea when we are not improving the model or performing worse hence saves resources which otherwise may get wasted in trying to improve the model when it's not !!
## AutoML
**In 1-2 sentences describe the model and hyperparameters generated by AutoML.**
The Best Performing model generated by AutoML is LightGBM Classifier with Accuracy Score of "0.91511" 
Some hyperparameters generated by AutoML are
-> Learning rate was taken to be 0.1 for this model
-> max_depth = -1
-> min_child_samples=20
-> min_child_weight=0.001
..etc
The next best model generated by AutoML was XGBoostClassifier with Accuracy of "0.91496
## Pipeline comparison
**Compare the two models and their performance. What are the differences in accuracy? In architecture? If there was a difference, why do you think there was one?**
1.   Accuracy LightGBM(Automl) = 0.91511 , logistic Regressor (Hyperdrive) = 0.91462. AutoML generates a more accurate model as compared to hyperdrive
2.   While Logistic Regression is a linear Classifier trained by Hyperdrive the LightGBM is a Gradient Boosting based tree model. 

In a general sense, a Gradient boosting tree model can outperform a Linear Regression model in most cases but here the difference is just of "0.001" 
as a result of limited runtime given to AutoML(30 Minutes) While the sampling can be improved to get a better result from hyperdrive experiment 
	 	
## Future work
**What are some areas of improvement for future experiments? Why might these improvements help the model?**
Some Areas for Future work are:-
1. Running the AutoML with lesser restriction to get the best possible model
2. In the Case of Hyperdrive instead of suggesting discrete values distributions and ranges can be passed as parameters like Normal distribution etc to
get better results. 
## Proof of cluster clean up

**Image of cluster marked for deletion**
--Attached the image of the code for deleting cluster in the git repository "Cluster Marked for Deletion.PNG"
--Attached the image of manually deleting compute cluster in the git repository "Deleting cluster manually.jpg"
--Attached the image of machine deleting compute in the git repository "Deleting compute manually.jpg"

