# Disaster Response Pipeline Project

### Table of Contents

1. [Installation](#installation)
2. [Project Motivation](#motivation)
3. [Approach](#approach)
4. [Instructions](#instructions)
5. [Files in the repository](#files)

## Installation <a name="installation"></a>

There should be no necessary libraries to run the code here beyond the Anaconda distribution of Python.  The code should run with no issues using Python versions 3.*.

## Project Motivation<a name="motivation"></a>
This project is part of my udacity Nanodegree Data Science. It combines an ETL and a NLP pipeline to provide a model with which messages can be classified into 36 categories. All of those messages come from real-life disasters and were prelabeled at figure eight https://appen.com/. The final goal is to provide disaster responders a quick way of getting information about the needs of the people in certain areas struck by disaster.

## Approach<a name="approach"></a>

### 1. Checking the data
As a first task I check the task for duplicates, nan-values, wrong categorizations or empty categories. My findings were:
 
- the category "child_alone" has anly "0" values and was dropped
- the category "related" has only "1" values and was dropped
- the category original had 61% nan values and there were 138 rows with more than 90% nan values
- I dropped the rows
- the category nan's were filled with 0
- 45 rows had more than 15 categories assigned and were dropped
- the column "related" had several rows with "2" which were droped
- all in all the classifications are very imbalanced and fixing this in an over- or undersampling would probably improve the results

### 2. NLP 
I followed the approach of the course here and used a custom tokenizer that:

- normalized the text by only allowing letters and numbers
- tokenizes the text into words
- removes stopwords
- lemmatizes the remaining words

My NLP pipeline:

- vectorized via CountVectorize including the custom tokenizer
- transformed and nomalized with a tf-idf transformer

### 3. Classification
Since my classes are a 2D matrix I used MultiOutputClassifier to fit them all together. 
So far I got the best overall performance for a DecisionTreeClassifier. Due to the imbalanced nature of my dataset
the classes with few counts are generally fit poorly and I got the best results with this classifier. I also tried:  

- KNeighborsClassifier
- RandomForestClassifier
- AdaBoostClassifier
- MultinomialNB

All of those had very poor performance for the low count classes.

## Instructions <a name="instructions"></a>
1. Run the following commands in the project's root directory to set up your database and model.

- To run ETL pipeline that cleans data and stores in database
    `python data/process_data.py data/disaster_messages.csv data/disaster_categories.csv data/DisasterResponse.db`
- To run ML pipeline that trains classifier and saves
    `python models/train_classifier.py data/DisasterResponse.db models/classifier.pkl`

2. Run the following command in the app's directory to run your web app.
    `python run.py`

3. Go to http://0.0.0.0:3001/ or http://localhost:3001

## Files in the repository <a name="files"></a>

    app
    | - template
    | |- master.html # main page of web app
    | |- go.html # classification result page of web app
    |- run.py # Flask file that runs app
    data
    |- disaster_categories.csv # data to process
    |- disaster_messages.csv # data to process
    |- process_data.py
    |- DisasterResponse.db # database to save clean data to
    models
    |- train_classifier.py
    |- classifier.pkl # saved model
    README.md
    results.xlsx #scoring values for each category

