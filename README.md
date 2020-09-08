# Project 6: Predicting Adoption Speeds for Shelter Animals
#### Project completed by Matt Williams
#### September 8th, 2020
 
### Problem Statement:

Each year, millions of pets worldwide end up in animal shelters.  In the US alone, an estimated 6.5 million pets are surrendered to shelters each year, and nearly 25% of them are not adopted.Our goal will be to identify factors that contribute to quicker adoption speeds, and attempt to build a model to predict how quickly shelter animals are adopted.
 
---

### Contents:
- [Data](#Data)
- [EDA](#EDA)
- [Modeling](#Modeling)
- [Presentation](#Presentation)
- [Conclusions](#Conclusions)

---
### Data:

The data comes from a Kaggle competition: https://www.kaggle.com/c/petfinder-adoption-prediction/overview 

The data consists of both a training set and testing set, which would be used to perform predictions upon for the competition. For the purpose of this project, I will be using only the training set for modeling and performing a train-test split on this data. 

The data dictionary & descriptions can be found here: https://www.kaggle.com/c/petfinder-adoption-prediction/data 

The target variable - 'AdoptionSpeed' - is constructed as follows:

- 0: Pet was adopted on the same day as it was listed.
- 1: Pet was adopted between 1 and 7 days (1st week) after being listed.
- 2: Pet was adopted between 8 and 30 days (1st month) after being listed.
- 3: Pet was adopted between 31 and 90 days (2nd & 3rd month) after being listed.
- 4: No adoption after 100 days of being listed. (There are no pets in this dataset that waited between 90 and 100 days).

There were three supplementary files of data used. These files will be omitted from the final repo due to size restrictions of GitHub:

- Images: each animal listing had at least one image associated with the pet, some with many more.  This file contains those images, with each file having the naming convention '{'PetID'}-{number of photo for the pet}.jpg'
- Metadata: The metadata was the result of processing the images using Google Vision API. Once the images were read, a JSON file was generated for each image. Each file contains information on features that Google Vision detects from the image - characteristics of the photo, color annotations, and crop marking, each ranked in order of importance for the image. 
- Sentiment: this file contains sentence-by-sentence sentiment analysis for each row in the 'Description' column of the data set. However, there were a number of entries that were missing a corresponding sentiment file. As such, I conducted my own sentiment analysis using TextBlob and used these results in my modeling instead. 

A number of features were engineered:

- Description word count: length of each value in the 'Description' column
- Description Sentiment (Polarity) and Subjectivity: generated using TextBlob. Sentiment scores range from -1 (most negative) to 1 (most positive). Subjectivity scores range from 0 (most objective) to 1 (most subjective). 
- Description Point of View: After reading through some of the descriptions, I noticed that some of the were written in first person (either from the persepctive of the rescuer or from the perspective of the animal), second person and third person. I used the Python library pointofview to generate these results. It was unable to determine the point of view of nearly 1/4 of the data and these descriptions were mainly sentence fragments that did not contain any pronouns.
 
### EDA:

Our target variable contained fairly balanced classes, with the exception of the 0 category, which made up about 3% of the data. 

I performed analysis on each of the categorical variables with respect to our target. Overall, while the exploration was interesting, it did not generate a lot of insight into how to impact the target variable. Observations include:
- There was some variation in the health categories with respect to whether the animal was a dog or a cat
- The younger the animal, the more likely it is the be adopted (at any speed). Sadly, older animals - which they may be better trained, in good health and have all of their vaccinations up to date - tend to go unadopted. 
- Contrary to what I though, the breed and color of the animal's coat did not have much impact on the adoption speed. People adopting pets from shelters may be less specific about breed and color than customers of breeders.
- None of the numerical variables correlated strongly with our target.  


---

### Modeling:

#### Baseline Model:
Our dominant class is category 4 with nearly .28 of the data. This will be our baseline accuracy to measure model performance against. 

#### Logistic Regression:
After attempting to use all of the variables, the resulting models were extremely overfit. After scaling back the number of features, the logistic regression model performed best when using only the numeric variables. The models performed poorly - only a training accuracy of .3742 and a test accuracy of .3153 - but they do outperfom our baseline model. 
 
#### Sequential Neural Network: 
I used a variety of models with different parameters and regularization techniques (l2, dropout, and early stopping). The top performing model again used only the numerical variables - when the text data and dummy variables were added, this just led to extreme overfitting. It still did not perform very well, with a testing accuracy of .3772 and a testing accuracy of .3671. 


---

### Presentation:

The pdf of the presentation may be found in this repository.  

---

### Conclusions:

The adoption speeds prove to be a difficult value to predict. There was little correlation amongst the numerical values with the target, not was there really a relationship between most of the cateforical variables and the target. 

The features that seemed to provide the most predictive power lie in the metadata of the images. There is a lot of room to add features from this data and would be the first place I'd look to try to improve future models. 
 
Another step could be to account for the unbalanced class (adopted the same day). While most of the other categories were balanced, modeling may improve if I use some tool to artifically boost that category. 

I will also explore using other evaluation metrics as accuracy may not be the best fit. 
 
 
 

