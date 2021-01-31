# Project description

This project extracts time period information relating to covid-19 virus by training a named entity recognition (NER) model in spaCy using sentences from academic literature. This project is part of a Kaggle competition submission, to develop text mining tools that can help the medical community develop answers to high priority questions. More information [here](https://www.kaggle.com/crispyc/coronawhy-task-ties-patient-descriptions).

# Data processing

Articles were obtained from [CORD-19 Open Research Dataset](https://www.kaggle.com/allen-institute-for-ai/CORD-19-research-challenge). The word embeddings of the articles in the dataset were used to created a search index using Facebook AI Similarity Search (FAISS). By normalizing the vectors and calculating the inner products between vectors, a K-nearest neighbor search based on cosine distance was performed to find similar articles. Articles were then further selected based on their frequencies from the similarity search with the premise that the more frequently an article surfaces in a search, the more likely the article belongs to the same topic and can contain relevant information. Sentences were then selected from the articles based on data fields that contain relevant information such as age, incubation period, and duration of viral shedding. A total of 461 sentences were retrieved and annotated as input to the model.

Links to data can be found in the [notebook](https://github.com/tjeng/NamedEntityRecognition/tree/master/Time_Period_NER.ipynb).

# Modeling

The model was trained on manually annotated data using [spaCy](https://spacy.io/usage/training#ner). 80% of the data was used for training and 20% for testing. Mini-batch (i.e. the number of training examples passed to the model at a time) with compounding batch size, where the initial batch size is set at 4 and increases to 32 when updating model parameters, was used. Adam solver was used as the optimization algorithm with a learning rate of 0.001, and a dropout rate of 0.2.

Complete notebook can be found [here](https://github.com/tjeng/NamedEntityRecognition/tree/master/Time_Period_NER.ipynb).

Model can be found [here](https://github.com/tjeng/NamedEntityRecognition/tree/master/model)

# Results

The model was evaluated based on confusion matrix calculation, details in this [link](https://towardsdatascience.com/entity-level-evaluation-for-ner-task-c21fb3a8edf), and the precision, recall, and F-1 score were calculated. We used a fuzzy string matching logic to compare actual and predicted text. For example, if the actual text is “approximately 5 days” and the predicted text is “5 days”, we count it as a true positive. With a small amount of training data the following results were achieved:

- Precision 0.77
- Recall 0.75
- F1 score 0.76

Notebook to evaluate results can be found [here](https://github.com/tjeng/NamedEntityRecognition/blob/master/notebooks/Evaluate_NER.ipynb)

The work is an open-source contribution that can extended to extract more COVID-19 related information and is part of a bigger task to use natural language processing techniques to help the medical community find answers to important questions. 
