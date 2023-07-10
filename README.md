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



 

2- impute-
handling missing numerical data
types-
	univariate imputation- if statistical tech applied to that single column for which deaing with missing values.
	multivariate imutation- when considering multiple or all the columnns of dataset for filling mkssing value of single column
		types KNN imputer and iterative imputer

univariate for numerical
	mean/median
	arbitary
	end of distribution
	random

benifit- simple to use
	choose mean if data dist. is near to normal and median  if skewed
	in production useful as can replace missing value with mean/median
disadv- it change distibution shape, as puting mean/median for all
	use when <5%, it is not generally used
	extra outliers comes, which previously not there
	covariance/correlation changes (relationship with other var)
all above disadv is very dangerous

when to use-
	data missing completely at random
	<5%

two ways of doing-
	pandas
	scikit learn( prefer this as can be put in the production easily)


handling missing catagorical data-
	most frequent se replace
	create new category missing

if MCAR and <5% then apply mode(frequency)
change data dist.

if not MCAR and >10% then make missing category


Random imputer-
replace nan with other numbers in dataset choosen randomly
applied to both cat. and num. data
not present in sklearn, it is in pandas
it dont change the data distribution and variance because when we take no. randomly so-chance of getting the n. that occur more is higher so like think in terms of normal dist.-
middle part remain like that.
work best with linear or log regression as data dist remain same is good, but-
with dicision tree not work good as we put randomness
covariance got changed
memory heavy for deployment as need to keep training data

 

missing Indicator-
prepare a column that contain true when corresponding value in other col. is nan and false when not nan like creating age_na for age column
it is very good in performance sometimes, clear working not understandable but model learn to-
differentiates between missing and non missing rows
not always good but can try it if want to improve
it was once used by student in KDD competition and the won



Automatic select the best imputer-
use gridsearchCV, present in sklearn


for multivariate- 
KNN imputer-
if a value is missing in a row than fill value with that value whose row is most similar to
it. similarity is found using euclidean distance
use KNN for that and k value if 3 then take mean of those, if 1 then directly that neighbour value.
but here values are missing so use nan-eucledian distance
nan-euclidean calc. as 
only put those in formulae whose x and y are given in sqrt(w*(x1-y1)^2+(x2-y2)^2) and
w is total no. of col. divided by no. of col whose both value given

more accurate
more no. of calculation
when deployed so full train set should be on server so speed less and more memory
better result then mean/median

iterative imputer MICE-
multivariate imputation by chained equation


MCAR means data is not collected from source data missing
MAR(missing at random) means cell is optional so by will not put value there, here we can fill value by other cols semilarity like age
MNAR means jaanmujhkar data hataya gaya hai, so no relation between other cols

assumption for this-
use MICE when MAR

same disadv as KNN imputer


replace nan with mean of that col.
then start from left col and put back its nan value there of that col. and treat that col.
as output and remaing col as input and ofnthe col. value that is nan pick them as test set and other as train and now apply any ML model to predict that
Now same process repeated go to 2nd col put its nan there and treat it as output col and apply ML model to predict the nan value
and so on..
Now this is stage 1
 and now we cal. difference of stage0(mean replacement) and stage1 then we get all 0 except the nan value cells, so repeate process like stage 1 we do rplace with nan then predict like that and get stage2 and do this some iterations to make the difference small
 
