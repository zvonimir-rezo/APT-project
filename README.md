# APT-project

Repository for the text analysis and retrieval project.

Irony detection in tweets with a focus on feature engineering, feature importance and topic modeling.

## Project notes

### Data
- data taken from [SemEval2018 Task 3: Irony detection in English tweets](https://competitions.codalab.org/competitions/17468#learn_the_details-overview)

- distribution of ironic and non-ironic tweets in the dataset:

|           | Ironic  | Non-ironic |
| --------- | ------- | ---------- |
| Train set | 1911    | 1923       |
| Test set  | 311     | 473        |

#### Preprocessing
- tokenization, hashtag segmentation, twitter specific symbol removal, stopword removal
- emoji, emoticon, upright slash, underscore, hyperlink, ellipses and short word (3 characters or less) removal
- lemmatization

#### Feature extraction
- Basic structure features
    - word count, char count, all uppercase count, all lowercase count, capitalised count digit count
- Twitter related features
    - tag count, hashtag count, link count, smiley count
- Punctuation features
    - exclamation mark count, question mark count, ellipsis count
- NER features
   - ORG tag count, NORP tag count, GPE tag count, PERSON tag count
- Sentiment analysis features
    - negative words and their count, positive words and their count, mixed sentiment present
- Topic feature

### Topic model
- baselines
    - k-means with BoW vectorization, k-means with TF-IDF vectorization
- BERTopic model 
    - represents the documents as embeddings
    - dimensionality reduction with these embeddings
    - clustering the reduced vectors
    - tokenize each topic into a BoW representation
    - calculate important words for each topic with class-based TF-IDF

- easily interpretable topics
![Topics](https://github.com/ir2718/text-analysis-and-retrieval-project/blob/master/topics.png)

### Results
- test set results using a logistic regression classifier:

| Model                               | Accuracy   | Precision   | Recall      | F1 score   |
| ----------------------------------- | ---------- | ----------  | ----------  | ---------- |
| Baseline LR                         | 0.568      | 0.579       | 0.582       | 0.566      |
| LR with topics(L1, L2, Elastic net) | **0.633**  | **0.627**   | **0.633**   | **0.626**  |
| LR with topics(L1)                  | 0.631      | 0.626       | 0.631       | 0.625      |
| LR without topics                   | 0.621      | 0.616       | 0.621       | 0.615      |

- training classifiers only on the subset of tweets that have the same topic 
- test set results for 4 most frequent topics:


| Most frequent words                 | Accuracy   | Precision   | Recall      | F1 score   |
| ----------------------------------- | ---------- | ----------  | ----------  | ---------- |
| work, love, day                     | 0.642      | 0.629       | 0.632       | 0.630      |
| police, black, obama                | 569        | 0.567       | 0.568       | 0.567      |
| fan, game, play                     | 0.425      | 0.380       | 0.376       | 0.378      |
| lol, funny, know                    | 0.750      | 0.725       | 0.796       | 0.724      |

- very hard to design an experiment to test the importance of a feature because of the information the other features provide

- weight for each feature depending on the topic:
![Topics](https://github.com/ir2718/text-analysis-and-retrieval-project/blob/master/plot.png)
