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
- * [NTSB Aviation Accident Database](https://www.ntsb.gov/Pages/AviationQueryV2.aspx)
---
## Analysis  ---THIS IS NOT COMPLETE, BUT KEEPNG FOR EASIER FORMATTING

A total of 2800 posts were collected and the text inside post titles and bodies were analyzed.  1557 posts (56%) originated from r/fountains and 1243 (44%) from r/pens.  Four posts were dropped due to no text (emojis and photos only) or if they just contained stop words.

SpaCy was used as the natural language processor before modeling the data.
* [Text Classification using Python SpaCy](https://machinelearninggeek.com/text-classification-using-python-spacy/)
 * [SpaCy Tokens](https://spacy.io/api/token)
 * [Natural Language Processing With SpaCy in Python](https://realpython.com/natural-language-processing-spacy-python/#lemmatization)


The following table outlines which models were implemented and their results.  The goal was to achieve a high level of specificity, therefore the Bernoulli Naive Bayes & CountVectorizer model was chosen for text analysis. Ideally, the model should never classify a r/pens post as a r/fountainpens post as it is a more specialized group.

r/fountainpens posts is considered the positive target. The baseline model is 56%.
|Model Type|Train Data Accuracy|Test Data Accuracy|Prediction Accuracy|Sensitivity|Specificity|Precision|
|---|---|---|---|---|---|---|
|**Logistic Regression & CountVectorizer**|98%|89%|89%|92%|87%|90%|
|**Logistic Regression & TD IDF**|89%|87%|87%|91%|82%|86%|
**Bernoulli Naive Bayes & CountVectorizer**|90%|90%|90%|88%|91%|93%

#### Parameters used per model

*Logistic Regression &  CountVectorizer*<br>
    cvec__max_features 10000<br>
    cvec__min_df: 0.001<br>
    cvec__max_df: 0.7<br>
    cvec__ngram_range: (1,2)<br>
    logreg__C: 1.0<br>
    logreg__penalty: l2 (Ridge)<br>
    logreg__solver: liblinear'<br>

*Logistic Regression & TFI DF*<br>
    tvec__max_features: 300<br>
    tvec__min_df: 0.001<br>
    tvec__max_df: 0.7<br>
    tvec__ngram_range: (1, 2)<br>
    logreg__C: 1.0<br>
    logreg__penalty: l2 (Ridge)<br>
    logreg__solver: liblinear

*Bernoulli Naive Bayes & CountVectorizer*<br>
    bb__alpha: 0.75,<br>
    cvec__max_features: 2000<br>
    cvec__min_df: 3<br>
    cvec__max_df: 0.7<br>
    cvec__ngram_range: (1,3)

### Confusion Matrices
* [Logistic Regression & CountVectorizer](plot_images/logreg_cvec_confusion_matrix.png)
* [Logistic Regression & TF IDF](plot_images/logreg_tfidf_confusion_matrix.png)
* [Bernoulli Naive Bayes & CountVectorizer](plot_images/bernoulli_nb_cvec_confusion_matrix.png)


---

# Conclusion -- ALSO NOT COMPLETE

I was able to successfully create a model using SpaCy natural language processing, Bernoullin Naive Bayes, and CountVectorizer to categorize r/fountainpens and r/pens subreddit posts.  The model has a 90% prediction accuracy which is an improvement over the baseline model of 56%.  Furthermore, the model has a 91% specificity score which was a major goal of this project.  Only fountain pen related posts should be categorized as part of r/fountainpens.

I was able to gather insight into what community members were talking about after processing the text through Spacy and vectorizing the data.  Obvious words like 'fountain pen', 'pen ink', and 'gel pen' appeared, but I was able to see other trends.  There are many posts about showing off a new pen purchase or a specific brand of pen.  

Taking this one step further, I collected a list of brands sold by Goulet Pens and generated a chart showing which of them were talked about the most on the Reddit subforums.  I was also able to pull out the word 'Goulet' and compare its counts to other brands.

A model similar to this could be modified and used for market research and sentiment analysis.  If I were to spend more time on this project, I would like to learn more about SpaCy as I think it has more potential than this project shows.  I would also integrate post comments into the dataset.