---
title: "Clustering Similar Headlines"
excerpt: "Cosine Similarity of Headlines <br/><img src='/images/500x300.png'>"
collection: portfolio
---

# 🧠 Clustering Similar Headlines Using Word Embeddings

## 📌 What I Did

The script clusters together similar news headlines based on their semantic meaning using **Word2Vec embeddings** and **cosine similarity**.

## 🔍 How I Did It

### 1. **Load the Dataset**
- Used `pandas` to read a CSV file `headlines.csv` with a `Title` column containing the news headlines.

### 2. **Tokenize the Text**
- Used `RegexpTokenizer` from `nltk` to split headlines into word tokens, removing punctuation.

```python
tknzr = RegexpTokenizer(r'\w+')

### 3. **Remove Stop Words**
- Used NLTK’s predefined list of English stop words.
- Removed these stop words from each tokenized headline to reduce noise and improve model quality.

```python
stp_wrds = set(stopwords.words('english'))
ts = [w for w in tokkens[i] if not w in stp_wrds]

### 4. **Train a Word2Vec Model**
- Trained a Word2Vec model using Gensim on the tokenized, stop word–free headlines.
- Used `min_count=1` to include all words, ensuring even infrequent terms are embedded.

```python
wrdvec = Word2Vec(tkns_wo_stp, min_count=1)

### 5. **Create Sentence Vectors**
- For each headline, created a sentence vector by summing the Word2Vec embeddings of each word in the headline.
- Initialized a zero vector of length 100 (default Word2Vec vector size), and accumulated word vectors.

```python
def create_sen_vectors(t, v):
    s = []
    for i in range(len(t)):
        sv = [0] * 100
        for w in t[i]:
            sv = sv + v[w]  # Add each word's vector to sentence vector
        s.append(sv)
    return s

### 6. **Compute Cosine Similarity**
- Used `cosine_similarity` from `sklearn.metrics.pairwise` to measure similarity between sentence vectors.
- Removed self-similarity from the matrix by subtracting -1 and dividing by 2 to normalize.

```python
def cosine_score(t, s):
    l = len(t)
    z = np.asarray([-1] * (l * l)).reshape(l, l)  # Matrix of -1s
    s = np.asarray(s)
    score = (cosine_similarity(s) - z) / 2  # Normalize scores
    return pd.DataFrame(score)

### 7. **Cluster Similar Headlines**
- Iterated over each headline to form clusters based on semantic similarity:
  - If it's the first headline, it starts its own cluster.
  - For every other headline, it is compared with all earlier headlines.
    - If its cosine similarity score with any previous headline is **≥ 0.9**, it's added to that cluster.
    - If no similar headline is found, a new cluster is created.

```python
def check(i):
    if i == 0:
        l[i].append(titles[i])  # First headline starts its own cluster
    else:
        added = False
        for j in range(i):
            if scores[i][j] >= 0.9:  # High similarity found
                l[j].append(titles[i])
                added = True
                break
        if not added:
            l[i].append(titles[i])  # No match found, new cluster





# 🧠 Clustering Similar Headlines Using Word Embeddings

## 📌 What I Did
The script clusters together similar news headlines based on their semantic meaning using **Word2Vec embeddings** and **cosine similarity**.

## 🔍 How I Did It

### 1. **Load the Dataset**
- Used `pandas` to read a CSV file `headlines.csv` with a `Title` column containing the news headlines.

```python
import pandas as pd
import numpy as np
from nltk.tokenize import RegexpTokenizer
from nltk.corpus import stopwords
from gensim.models import Word2Vec
from sklearn.metrics.pairwise import cosine_similarity

# Load the headlines from CSV file
data = pd.read_csv('headlines.csv')
titles = data['Title'].tolist()
```

### 2. **Tokenize the Text**
- Used `RegexpTokenizer` from `nltk` to split headlines into word tokens, removing punctuation.

```python
# Initialize tokenizer
tknzr = RegexpTokenizer(r'\w+')

# Tokenize all headlines
tokens = []
for title in titles:
    tokens.append(tknzr.tokenize(title.lower()))
```

### 3. **Remove Stop Words**
- Used NLTK's predefined list of English stop words.
- Removed these stop words from each tokenized headline to reduce noise and improve model quality.

```python
# Get English stop words
stop_words = set(stopwords.words('english'))

# Remove stop words from tokens
tokens_no_stop = []
for i in range(len(tokens)):
    tokens_no_stop.append([w for w in tokens[i] if w not in stop_words])
```

### 4. **Train a Word2Vec Model**
- Trained a Word2Vec model using Gensim on the tokenized, stop word–free headlines.
- Used `min_count=1` to include all words, ensuring even infrequent terms are embedded.

```python
# Train Word2Vec model
word_vectors = Word2Vec(tokens_no_stop, min_count=1, vector_size=100)
```

### 5. **Create Sentence Vectors**
- For each headline, created a sentence vector by summing the Word2Vec embeddings of each word in the headline.
- Initialized a zero vector of length 100 (default Word2Vec vector size), and accumulated word vectors.

```python
def
