# Spam Classificator Project

A Naibe-Bayes spam classificator. 

## Steps

### Preprocessing Dataset

This step is crucial to improve the performance of the Bayes spam classificator.

- **Lowercasing** the text;
- Removing unnecessary **punctuation and numbers**;
- **Tokenizing** the text;
- Removing **Stop Words** (such as "and", "the", "is") that often do not add meaningful context. Removing them reduces noise and focuses the model on the words most likely to help distinguish spam from ham messages;
- **Stemming**;

Topics learned:

- nltk library for **tokenization**, **stop word removal** and **stemming**

### Feature Extraction

**Feature Extraction** transforms SMS messages into numerical vectors suitable for ML algorithms, since models cannot directly process raw strings. They solely rely on words count or frequency to differentiate spam from ham.

**Bag-of-words** model: transforms each message into a vector of term counts. Using only **unigrams** (individual words) does not preserve the original word order; it treats each document as a collection of terms and their frequencies, independent of sequence. To introduce a limited sense of order, we also include **bigrams**, which are pairs of consecutive words. By incorporating bigrams, we capture some local ordering information (consider the "free prize" example instead of just "free");

*CountVectorizer* from the scikit-learn library efficiently implements the bag-of-words approach.

Key parameters for refining the feature set:

- min\_df=1: A term must appear in at least one document to be included. While this threshold is set to 1 here, higher values can be used in practice to exclude rare terms.
- max\_df=0.9: Terms that appear in more than 90% of the documents are excluded, removing overly common words that provide limited differentiation.
- ngram\_range=(1, 2): The feature matrix captures individual words and common word pairs by including unigrams and bigrams, potentially improving the model’s ability to detect spam patterns.

*CountVectorizer* operates in three main stages:

1. Tokenization (min\_df and max\_df);
2. Building the vocabulary (ngram\_range);
3. Vectorization of the documents.

Topics learned:
- *CountVectorizer* from sickit-learn. Converts collection of documents into a matrix of term coutns, where each row represents a message and each column to a term.

### Training and Evaluation

                  Raw text messages
                         | 
                         ▼
                GridSearchCV begins
                         │
      ┌──────────────────┼──────────────────┐
      │                  │                  │
 alpha=0.01         alpha=0.10        alpha=0.15 ...
      │                  │                  │
      ▼                  ▼                  ▼
  5-fold CV         5-fold CV         5-fold CV
      │                  │                  │
 Average F1         Average F1         Average F1
      │                  │                  │
      └──────────────────┼──────────────────┘
                         ▼
              Select highest average F1
                         ▼
     Retrain the entire pipeline on all data
                         ▼
                 best_estimator_

Topics learned:

- Pipeline to simplify the feature extraction and training in one step;
- Differences between **parameters** and **hyperparameters**;
- **GridSearchCV** is a tool in scikit-learn that automatically tries different hyperparameter combinations and tells you which one performs best using cross-validation. It was used to evaluate the model performance(f1) based on different hyperparameters (in this case, alpha from MultinomialNB). It basically choose the best model after training it with the values of hyperparameters specified.
