# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 4: Predicting Budget Airlines Based on NTSB Aviation Accident Database
### Background and Introduction
___

The aviation industry is a critical sector in the economy as it facilitates travel, business, and military. Despite signigicant advancements in technology and safety, accidents still occur which result in injuries and fatalities.

"The latest safety report from IATA, the trade association of the world's airlines, states that in 2022 there were a total of 39 commercial aviation accidents in the world, with 158 on-board fatalities – equalling one accident every 0.83 million flights." - By Jacopo Prisco, CNN | Feb 10, 2024

The National Transportation Safety Board curates a [database](https://www.ntsb.gov/Pages/AviationQueryV2.aspx) that includes civil aviation accidents in the United States and international waters.  According to the [International Air Transport Association (IATA)](https://www.statista.com/chart/31529/most-airplane-accidents-happen-during-landing/), a  bit more than half of all civil aviation accidents happen during landing, followed by takeoff, approach, and initial climb.  Fatal accidents are [cited](https://www.tc.faa.gov/its/worldpac/techrpt/am02-23.pdf) to be caused mostly from pilot and maintenance errors.  Preventing accidents requires in investment in staff training and maitanence upkeep.  We are interested in predicting airlines based on safety data records and if perceived budget airlines have similar safety records as non-budget airlines


## Problem Statement
___
The overall safety commercial aviation has improved over the years but there remains a pressing need to reduce the incidences of accidents and fatalities and mitigate their impact.  Preventing accidents requires in investment in staff training and maintanance upkeep.  We are interested in predicting airlines based on safety data records and if budget airlines have similar safety records as non-budget airlines.  We will do this by collecting data from the NTSB aviation accident database concerning american commercial airlines.  Since most accidents happen at or near airports, we will focus on occurances at different airports. The following combination of features will be considered:

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

Budget airlines typically forego services offered on other airlines in exchange for cheaper ticket prices.  They typically operate with only one type of plane which makes training and maintenance more streamlined.  We have catorgoized the airlines in the dataset as follows:

**BUDGET**
* Alaska
* Allegiant
* Frontier
* Jetblue
* Spirit
* Southwest
* Sun Country

  **NON BUGET**
* American
* Continental
* Delta
* Hawaiian
* United

We will explore classification models such as logistic regression, decision trees, random forests, and KNN.  Bagging and boosting will also be considered.  Our goal is to create a model that can predict budget airlines using NTSB accident data better than the baseline.  Our main target will be accuracy with no preferences to false negatives or positives.   Can we conclude which airlines are likely to have an injury or fatality? Do particular airlines need to focus on more safety measures than others?

## References
___
* [Statistica - Most Airplane Accidents Happen During Landing](https://www.statista.com/chart/31529/most-airplane-accidents-happen-during-landing/)
* [FAA - General Aviation Maintenance-Related Accidents: A Review of Ten Years of NTSB Data](https://www.tc.faa.gov/its/worldpac/techrpt/am02-23.pdf)
* [Worried About How Safe It Is To Fly?  Here's What the Experts Have To Say - CNN](https://www.cnn.com/travel/worried-about-flying-heres-what-the-experts-have-to-say/index.html)
* [Why This Plane Had A Dangerous Reputation: The DC-10](https://medium.com/@salman12788/why-this-plane-had-a-dangerous-reputation-the-dc-10-954f4da7aafe)
* [Uniterd Airlines Flight 232](https://en.wikipedia.org/wiki/United_Airlines_Flight_232)
* [A Comprehensive Guide to Aircraft Inspections: Types and Checklists](https://www.acumen.aero/aircraft-inspections#topic-20)
* [Accident or Incident?  Explaining Aircraft Damage Assessment](https://safetycompass.wordpress.com/2021/09/30/accident-or-incident-explaining-aircraft-damage-assessment/)

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
## Analysis

A total of 559 observations were cleaned and analysed from a raw dataset of 2563 observations.  The original dataset was filtered from the NTSB database based on commercial airlines only.  We ran into many issues which stem from a lack of consistency in data collection.  There were many permutations of the same value.  For example, Dallas/Forth Worth Airport was written or abbreviated differently multiple times.  Another example was a value of 'over the' in the city column and 'ocean' in the country column.  Our research indicated that most crashes happen on the ground, so we filtered out all columns with airport ID.  If two planes were involved in an accident, their make and model were both listed in the same cells.  There was no consistency betwen which one was always listed first and second, so we assumed the first values.

Even narrowing it down by available airpot ID, we had to clean many of the airline names and airplane make/models.  Like location, there seemed to be no clear standardization of data entry.  For airport ID and airplane model, there were a handful of values that appeared only once.  Due to the various ways in which the same value was represented, we tried to immplement replacement functions that were as general as possible.  Then we consolidated airplane make and model.  For example, there are over 20 variants of the Boeing 737, so we consolidated them into one '737' value.   This, along with a small dataset, was understood to likely not create a useful model.  However, due to time constraints and spending a lot of time on data cleaning, we decided to move forward with what we had.  This project is a very good example of how domain knowledge could have helped us.

SpaCy was used as the natural language processor before modeling the data.
* [Text Classification using Python SpaCy](https://machinelearninggeek.com/text-classification-using-python-spacy/)
 * [SpaCy Tokens](https://spacy.io/api/token)
 * [Natural Language Processing With SpaCy in Python](https://realpython.com/natural-language-processing-spacy-python/#lemmatization)

Other Python Libraries Used
* Pandas
* sci-kit learn
* NumPy
* Matplotlib


Kristina and Holly worked independently on varios models. The following table outlines the best models and their results.  The goal was to achieve high accuracy levels irregardless of sensitivity and specificity.  Predicting budget airlines based on accident data, including fatalities, should be correct as often as possible given the serious nature of the topic
Budget airlines are the positive target. The baseline model is 78%.
Contributor|Model Type|Train Data Accuracy|Test Data Accuracy|Prediction Accuracy|Sensitivity|Specificity|Precision|
|---|---|---|---|---|---|---|---|
**Holly**|**Random Forest**|78.08%|78.79%|78.57%|4%|100%|50%|
**Kristina**|**Logistic Regression & CountVectorizer**|99.52%|81.43%|77.86%|48.39%|90.83%|60%|


#### Parameters used per model

**Holly** - Random Forest*<br>
    rf_max_depth: 2<br>
    n_estimators: 200<br>

*Features Used*

Minor Injury Count<br>
Serious Injury Count<br>
Fatal Injury Count<br>
    


**Kristina** - Logistic Regression & CountVectorizer*<br>
    cvec_max_df: 0.9<br>
    cvec_max_features: 200<br>
    cvec_min_d': 3<br>
    cvec_ngram_range: (1, 3)<br>
    lr_C: 10<br>
    lr_penalty: l2<br>
    lr_solver: liblinear<br>

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
* [Kristina's Logistic Regression & CountVectorizer Confusion Matrix](images/logreg_cvec_confusion_matrix_display_kh.png)
* [Holly's Random Forest Confusion Matrix](images/knn_conf_matrix_display_hh.png)


---

# Conclusion

We were unable to develop a model that performed better than our baseline, which was 78%.  This may be due to the small dataset and multiple values appearing only once in a feature.  This dataset could have also benefitted from someone with more extensive domain knowledge and their ability to determine which features to use and how to consolidate them.

The language processing analysis was able to provide some insight into factors that may cause airline accidents more than others.  In the trigram analysis phrases that referred to staff, like captain or flight crew, appered more than airplane parts.  Captain, maintenance personnel, air traffic controller,  and flight crew always appeared at the top of the lists.  Different variations of landing gear also appeared frequently.  This data matches with research that indicates that typicallly people cause airline accidents.

There also appears to be a trend towards more accidents per year beginning in the mid-2000s.  The number of incidents seem to fluctuate with no clear pattern.  Even if there are accidents, most of them do not lead to injuries or fatalities.

We think this could be improved by spending more time getting familiar with the dataset and more thorough data cleaning.  Consider standardidizing how the NTSB enters their data.  If an accident or incident happens over the ocean, perhaps create a separate entry for this.  Standardize airport name entry and decide if ICAO or IATA airport codes should be used, but not both.  More data would have obviously been more helpful, but we would need more time to clean.