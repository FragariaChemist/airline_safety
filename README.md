# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 4: Predicting Budget Airlines Based on NTSB Aviation Accident Database
### Background
___

The National Transportation Safety Board curates a [database](https://www.ntsb.gov/Pages/AviationQueryV2.aspx) that includes civil aviation accidents in the United States and international waters.  According to the [International Air Transport Association (IATA)](https://www.statista.com/chart/31529/most-airplane-accidents-happen-during-landing/), a  bit more than half of all civil aviation accidents happen during landing, followed by takeoff, approach, and initial climb.  Fatal accidents are [cited](https://www.tc.faa.gov/its/worldpac/techrpt/am02-23.pdf) to be caused mostly from pilot and maintenance errors.  Preventing accidents requires in investment in staff training and maitanence upkeep.  We are interested in predicting airlines based on safety data records and if perceived budget airlines have similar safety records as non-budget airlines


## Problem Statement
___
Preventing accidents requires in investment in staff training and maintanance upkeep.  We are interested in predicting airlines based on safety data records and if perceived budget airlines have similar safety records as non-budget airlines.  We will do this by collecting data from the NTSB aviation accident database concerning american commercial airlines.  Since most accidents happen at or near airports, we will focus on occurances at different airports. The following combination of features will be considered:

* Event Type (Accident or Occurance)
* Highest Injury Level
* Fatal Injury Count
* Serious Injury Count
* Minor Injury Count
* Probable Cause
* Airport ID
* Airline
* Aircraft Make
* Aircraft Model
* Aircraft Damage
  
We will explore classification models such as logistic regression, decision trees, random forests, and KNN.  Bagging and boosting will also be considered.  Our goal is to create a model that can predict budget airlines using NTSB accident data better than the baseline.  Our main target will be accuracy with no preferences to false negatives or positives.

## References
___
* [Statistica - Most Airplane Accidents Happen During Landing](https://www.statista.com/chart/31529/most-airplane-accidents-happen-during-landing/)
* [FAA - General Aviation Maintenance-Related Accidents: A Review of Ten Years of NTSB Data](https://www.tc.faa.gov/its/worldpac/techrpt/am02-23.pdf)
## Dataset Dictionary
___

|Feature|Type|Source|Description|
|---|---|---|---|
|**aviation_data.csv**|*int/str*|[NTSB Accident Aviation Database](https://www.ntsb.gov/Pages/AviationQueryV2.aspx)|Collected and uncleanedc commercial aviation data.| 
|**aviation_data_no_commas.csv**|*int/str*|Generated|aviation_data csv in which all cells with a single comma have been removed via Excel.
|**clean_aviation_data.csv**|*various*|Generated|Cleaned aviation_data.csv that is ready for NLP processing and modeling.
|**text_aviation_data.csv**|*various*|Generated|clean_aviation_data.csv that has been processed with SpaCy and ready for modeling.
|
### Dataset Sources
* [NTSB Aviation Accident Database](https://www.ntsb.gov/Pages/AviationQueryV2.aspx)
---
## Analysis  ---THIS IS NOT COMPLETE, BUT KEEPNG FOR EASIER FORMATTING

A total of 559 observations were cleaned and analysed from a raw dataset of 2563 observations.  The original dataset was filtered from the NTSB database based on commercial airlines only.  We ran into many issues which stem from a lack of consistency in data collection.  There were many permutations of the same value.  For example, Dallas/Forth Worth Airport was written or abbreviated differently multiple times.  Another example was a value of 'over the' in the city column and 'ocean' in the country column.  Our research indicated that most crashes happen on the ground, so we filtered out all columns with airport ID.

Even narrowing it down by available airpot ID, we had to clean many of the airline names and airplane make/models.  Like location, there seemed to be no clear standardization of data entry.  For airport ID and airplane model, there were a handful of values that appeared only once.  This, along with a small dataset, was understood to likely not create a useful model.  However, due to time constraints and spending a lot of time on data cleaning, we decided to move forward with what we had.  This project is a very good example of how domain knowledge could have helped us.

SpaCy was used as the natural language processor before modeling the data.
* [Text Classification using Python SpaCy](https://machinelearninggeek.com/text-classification-using-python-spacy/)
 * [SpaCy Tokens](https://spacy.io/api/token)
 * [Natural Language Processing With SpaCy in Python](https://realpython.com/natural-language-processing-spacy-python/#lemmatization)


Kristina and Holly worked independently on varios models. The following table outlines the best models and their results.  The goal was to achieve high accuracy levels irregardless of sensitivity and specificity.  Predicting budget airlines based on accident data, including fatalities, should be correct as often as possible given the serious nature of the topic
Budget airlines are the positive target. The baseline model is 78%.
Contributor|Model Type|Train Data Accuracy|Test Data Accuracy|Prediction Accuracy|Sensitivity|Specificity|Precision|
|---|---|---|---|---|---|---|---|
**Holly**|**MODEL**|0%|0%|0%|0%|0%|90%|
**Kristina**|**Logistic Regression & CountVectorizer**|99.52%|81.43%|77.86%|48.39%|90.83%|60%|


#### Parameters used per model

**Holly** - Model*<br>
    Parameters

**Kristina** - Logistic Regression & CountVectorizer*<br>
    cvec__max_df: 0.9<br>
    cvec__max_features: 200<br>
    cvec__min_d': 3<br>
    cvec__ngram_range: (1, 3)<br>
    lr__C: 10<br>
    lr__penalty: l2<br>
    lr__solver: liblinear<br>

*Features Used*
    
Event Type<br>
Highest Injury Level<br>
Fatal Injury Count<br>
Serious Injury Count<br>
Minor Injury Count<br>
Probable Cause<br>
Airport ID<br>
Aircraft Damage<br>
Make<br>
Model<br>


### Confusion Matrices
* [Kristina's Logistic Regression & CountVectorizer](images/logreg_cvec_confusion_matrix_display_kh.png)



---

# Conclusion -- ALSO NOT COMPLETE

I was able to successfully create a model using SpaCy natural language processing, Bernoullin Naive Bayes, and CountVectorizer to categorize r/fountainpens and r/pens subreddit posts.  The model has a 90% prediction accuracy which is an improvement over the baseline model of 56%.  Furthermore, the model has a 91% specificity score which was a major goal of this project.  Only fountain pen related posts should be categorized as part of r/fountainpens.

I was able to gather insight into what community members were talking about after processing the text through Spacy and vectorizing the data.  Obvious words like 'fountain pen', 'pen ink', and 'gel pen' appeared, but I was able to see other trends.  There are many posts about showing off a new pen purchase or a specific brand of pen.  

Taking this one step further, I collected a list of brands sold by Goulet Pens and generated a chart showing which of them were talked about the most on the Reddit subforums.  I was also able to pull out the word 'Goulet' and compare its counts to other brands.

A model similar to this could be modified and used for market research and sentiment analysis.  If I were to spend more time on this project, I would like to learn more about SpaCy as I think it has more potential than this project shows.  I would also integrate post comments into the dataset.