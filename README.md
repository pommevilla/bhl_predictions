# BHL Prediction

This is a project to predict the emergence of antimicrobial genes in Black Hawk Lake.
Below is a broad overview of the analysis carried out.

## Clean and explore the data

* Converting <LOD/<LOQ to 0
* Replacing NAs with appropriate values
* Figuring out what to do with missing values
* Might consider some feature transformations/feature engineering
  * Example of feature engineering - TP:TN
* Also need to figure out the class cutoffs 
  * Tentatively going with <=1 = Low, 2 = Medium, 3+ High Risk
  * Could revisit if something doesn't work out
* Figure out class distributions
  * We can even out classes using oversampling (SMOTE algorithm)
* Do unsupervised learning
  * PCA/NMDS/clustering/ordination stuff
  
## Do feature selection

* We'll start by creating a testing and training split
  * 80% training, 20% testing
* Train a random forest on the training split with no hyperparameter tuning
* After model training, the random forest will assign a importance to each factor, and we can select which features we will use in the final model based on these scores
  * Reduce the dataset to only use those important features
  
## Model training

* With our reduced data set, we'll train models
  * XGBoost
    * number of trees, minimum sample number to split, learning rate
  * Linear regression
  * Maybe others
* Hyperparameter tuning
  * Cross-fold validation
    * 80% training set > 5 cross-folds (5 sets of equal number of samples from the 80%)
    * For each set of hyperparamaters...
      * Train that model on 4 of the folds and test against the 5th one
  * Tune the models to get the best hyperparameters according to cross-validation
* Create the model the with those hyperparameters
* Do one last fit on the training set 

## Get predictions

* Use fit from above to predict on the testing set
* We'll gather metrics
  * Accuracy, specificity/sensitivity, ROC-AUC (multi-class version)
* Look at predictions and see where it did well, where it did poorly
* Iterate...
  
