# Handling_missing_data

ML models in scikit cannot handle missing values so we  need to remove them before training
ways to handle them-
1- remove them( but not good as whole row deleted)
2-Impute (fill value in them)
	a-univatiate(one time one column focus)
	b-multivariate(many columns at once)

for univriate we have
	numerical data, for this we do mean/median, random no., end of distribution
	categorical data, for this we fill with mode or make it missing(it is a way)
for this we have simple imputer class in sklearn

for multivariate we use 
	KNN imputer(uses ML model to do this)
	iterative imputer MICE
we have class for this in sklearn


1- removing the data( it is called CCA complete case analysis)
CCA means analyzing only those observation for ehich all info is given in dataset
delete row for which any col value is is missing

we do CCA only when data is missing randomly, not like starting or ending values missing, or something else.
missing completlt at random(MCAR) is the assumption
distrivution of data befor removing must be same as after removing

easy to apply- use dropna in pd
disadv.- excluded info could be important
	model in production dont know how to handle missing data when encounters
so generally we dont prefer this

when to use CCA-
MCAR
less than 5% data misssing(if like 95%data missing in one column then remove that column)

in cca for numerical data we se distribution(hist or pdf) before after same or not
	for categorical data we see ration of each category should be same before after

