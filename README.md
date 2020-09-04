# NLP and the Talmud

The overall goal of this project has two parts. The first is to create a named entity recognizer that is able to analyze the names of Tannaic and Amoraic characters in the Talmud and tag relationships between authors. The second is to use that data, as well as data taken from Sefaria's database, in order to create a visual flow chart and graph of the back-and-forth in the Talmud. This is done by creating a machine learning model that will tag sentences in the Talmud as questions, answers, and statements.
For both parts of the project, NLP tools to analyze the Talmud, specifically through a NER and sentance tagging ML Model.

## Getting Started

These instructions will assume a basic understanding of how to clone a repository to a local machine and how to download relevant packages  

First, clone the repository to your local machine. Both files Berakhot.txt.zip and Berakhot.tsv.zip in Sentence Tagging ML Model must be unzipped before any code can be executed.

The packages that are needed for this project are: 
- Python
- nltk
- xlsxwriter
- openpyxl
- pickle
- random
- re
- pymongo
- csv

Some of the Python files assume that you have Sefaria's MongoDB set up locally on your system, so that they can be queried via pymongo. These include the alignment of punctuated text (to create the initial *.tsv files) as well as the initial queries of the Named Entities from Jastrow and Brown-Driver-Briggs. You can install the MongoDB program from here: https://www.mongodb.com/download-center/community
You can install Sefaria's database following the instructions here: https://github.com/Sefaria/Sefaria-Export

In the first stage, we assembled an aligned trilingual corpus from the Sefaria texts collection, fetching the William Davidson Aramaic edition (that is, the regular text of the talmud Talmud), and William Davidson English, and Hebrew (that is, Rav Steinsaltz's running commentary) for each masechet. These were already paragraph- or sentence-aligned in the database, but using alignment algorithms, the Aramaic and Hebrew commentary were further word-aligned, and punctuation was projected from the Hebrew commentary onto the Aramaic. These aligned sentences form the basis for the further work, in Discourse Classification, or in building gazetteers and applying Named Entity recognition.


## Description of Features 

This section describes the two branches of this project, their features and methods, and their descriptions.

### Named Entity Recognizer

This part of the project aims to create a Named Entity Recognizer that is able to analyze the Talmud. 
Although NERs have been created that process Hebrew, there are currently limited access to NER that process Aramaic. 
In order to create a NER  Sefaria’s database of the Jastrow Dictionary and the Brown Drivers Briggs was used to create a gazetteer of Location and People names included in both the Talmud and Bible. 
This project is part of a larger project, facilitated within the Stern Natural Language Processing Lab, that aims to use NLP to analyze the Talmud. 

We created several gazetteers that could then be used to train a NER. 
Examples of gazetteers included: 
	hebrewnames.txt : that compiled a list of all hebrew male and female names
	englishnamesfull.csv : a gazetteer of english names
	locationgazeteer.txt : a gazetteer of hebrew location names

Hebrew and Aramaic are languages that have high morphological ambiguity meaning that words have multiple meanings, therefore these gazetteers are not able to directly tag Talmudic texts with high accuracy.

These gazetteers were written using regex expressions to gather data from the Jastrow Dictionary and the Brown Driver Briggs Dictionary. The regex expressions used tags included in the source.content of each dictionary entry, such as pr.n.m and pr.n.f or pr.n.loc. For tags that only specified pr.n more regex expressions were used such as “angel”.

The gazetteers would then be used to tag Talmudic texts in an IOB tagging. These talmudic tagged texts would then be used to train a NER. Currently we have created a training model for a NER model that is only trained using just the words themselves from the gazetteers, using NaiveBayesClassifier. In order to make the model more accurate we aim to train the model using entire sentences that will give the training model context, furthermore we plan to use a CFR model. 

### Sentence Tagging ML Model

This part of the project attempts to make a machine learning model that can independently tag sentences is any tractate in Talmud as either a 
question, answer, or statement, with the end goal of creating flow charts of the back-and-forth of the Talmud. Although sentence tagging data already exists, it is not for public use and is privately owned.
There are three files that need to be executed, in order, to run the program. 
 - ScrapeAlgo2.py : Scrapes the text of the Tractate from the coordinated tsv file into a string. Then, splits it into sentences and writies the sentences into a excel file for tagging. 
 - BagOfWords.py : Creates a bag of words, bigrams, and trigrams that is used in the feature set for the NLP model. 
 - MLTaggingAlgo.py : Main program that creates a machine learning model for tagging the sentences of the talmud in the excel file.  Contains the feature set for the model and the classifier.
 
 In MLTaggingAlgo.py the eventual goal will be to use the Tsv file as the source for the Talmudic text. Current issues involving edge cases have prevented its use. Instead ScrapeAlgo.py uses a different method to create the excel file. 
 File also contains the data from the machine model and the relevant features found.
 
## Running the tests

Code within named entity.py tests both how accurate the NER is and records which words were incorectly labelled.
Code within named MLTaggingAlgo.py can be used to run classifier module tests.

### Break down into end to end tests

#### Named Entity Recognizer
- The accuracy found using the nltk.classifier module is 0.9245.
- The NER only tagged words incorrectly 7% of the time.

#### Sentence Tagging ML Model
- The accuracy found using the nltk.classify.MaxentClassifier module is 46.2% on the training set
- The accuracy found using the nltk.classify.MaxentClassifier module is 38% on the testing set
- Relevant features and incorrect guesses are printed along with the percent accuracy. 


## Built With

* [Sefaria Database](https://github.com/Sefaria/Sefaria-Project) - Database for all machine learning data. We used the Sefaria lexicon_entry collection as the basis for building our gazetteers


## Versioning

This is a work in progress project, therefore no final version was ever fully developed 

## Authors

* **Adina Bruce** - [adinabruce](https://github.com/adinabruce)
* **Noah Kalandar** - [noahkalandar](https://github.com/nokal123)


## Acknowledgments
We owe much to the mentorship of Professor Waxman, who ran the Stern Natural Language Processing Lab, through which we were able to create our project. Some of his code was used within our project. He provided constant support and was always available to answer any of our questions. 

Features used in training the NER were based off of features described in (Ben Mordecai and Elhadad, 2005)


