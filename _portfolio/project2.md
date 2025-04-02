---
title: "Clustering Similar Headlines"
excerpt: "Cosine Similarity of Headlines <br/><img src='/images/headline_clustering.png'>"
collection: portfolio
---

Link to the [GitHub repository](https://github.com/HenamSingla/Similartext)

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
def create_sentence_vectors(tokenized_sentences, word2vec_model):
    sentence_vectors = []
    for sentence in tokenized_sentences:
        # Initialize empty vector with correct dimensions
        sentence_vector = np.zeros(word2vec_model.vector_size)
        word_count = 0
        
        # Add vectors for each word in the sentence
        for word in sentence:
            if word in word2vec_model.wv:
                sentence_vector += word2vec_model.wv[word]
                word_count += 1
        
        # Average the vectors if there are words in the sentence
        if word_count > 0:
            sentence_vector /= word_count
            
        sentence_vectors.append(sentence_vector)
    return sentence_vectors

# Create sentence vectors
sentence_vectors = create_sentence_vectors(tokens_no_stop, word_vectors)
```

### 6. **Compute Cosine Similarity**
- Used `cosine_similarity` from `sklearn.metrics.pairwise` to measure similarity between sentence vectors.
- Created a similarity matrix for all headlines.

```python
def compute_similarity_matrix(sentence_vectors):
    # Convert list to numpy array for cosine_similarity function
    vectors_array = np.array(sentence_vectors)
    
    # Compute cosine similarity matrix
    similarity_matrix = cosine_similarity(vectors_array)
    
    return similarity_matrix

# Compute similarity scores
similarity_matrix = compute_similarity_matrix(sentence_vectors)
```

### 7. **Cluster Similar Headlines**
- Iterated over each headline to form clusters based on semantic similarity:
  - If it's the first headline, it starts its own cluster.
  - For every other headline, it is compared with all earlier headlines.
    - If its cosine similarity score with any previous headline is **≥ 0.9**, it's added to that cluster.
    - If no similar headline is found, a new cluster is created.

```python
def cluster_headlines(titles, similarity_matrix, threshold=0.9):
    clusters = [[] for _ in range(len(titles))]
    cluster_indices = []  # Track which cluster each headline belongs to
    
    for i in range(len(titles)):
        if i == 0:
            # First headline starts its own cluster
            clusters[0].append(titles[i])
            cluster_indices.append(0)
        else:
            added = False
            # Check similarity with headlines in existing clusters
            for j in range(i):
                if similarity_matrix[i][j] >= threshold:
                    # Add to existing cluster
                    cluster_idx = cluster_indices[j]
                    clusters[cluster_idx].append(titles[i])
                    cluster_indices.append(cluster_idx)
                    added = True
                    break
            
            if not added:
                # Create new cluster
                clusters[i].append(titles[i])
                cluster_indices.append(i)
    
    # Remove empty clusters
    return [cluster for cluster in clusters if cluster]

# Cluster headlines
headline_clusters = cluster_headlines(titles, similarity_matrix)

# Print the results
print(f"Found {len(headline_clusters)} clusters")
for i, cluster in enumerate(headline_clusters):
    print(f"\nCluster {i+1} - {len(cluster)} headlines:")
    for headline in cluster:
        print(f"  • {headline}")
```

## 📊 Complete Example

```python
import pandas as pd
import numpy as np
from nltk.tokenize import RegexpTokenizer
from nltk.corpus import stopwords
from gensim.models import Word2Vec
from sklearn.metrics.pairwise import cosine_similarity

# 1. Load the dataset
data = pd.read_csv('headlines.csv')
titles = data['Title'].tolist()

# 2. Tokenize the text
tknzr = RegexpTokenizer(r'\w+')
tokens = []
for title in titles:
    tokens.append(tknzr.tokenize(title.lower()))

# 3. Remove stop words
stop_words = set(stopwords.words('english'))
tokens_no_stop = []
for i in range(len(tokens)):
    tokens_no_stop.append([w for w in tokens[i] if w not in stop_words])

# 4. Train a Word2Vec model
word_vectors = Word2Vec(tokens_no_stop, min_count=1, vector_size=100)

# 5. Create sentence vectors
def create_sentence_vectors(tokenized_sentences, word2vec_model):
    sentence_vectors = []
    for sentence in tokenized_sentences:
        sentence_vector = np.zeros(word2vec_model.vector_size)
        word_count = 0
        
        for word in sentence:
            if word in word2vec_model.wv:
                sentence_vector += word2vec_model.wv[word]
                word_count += 1
        
        if word_count > 0:
            sentence_vector /= word_count
            
        sentence_vectors.append(sentence_vector)
    return sentence_vectors

sentence_vectors = create_sentence_vectors(tokens_no_stop, word_vectors)

# 6. Compute cosine similarity
similarity_matrix = cosine_similarity(np.array(sentence_vectors))

# 7. Cluster similar headlines
def cluster_headlines(titles, similarity_matrix, threshold=0.9):
    clusters = [[] for _ in range(len(titles))]
    cluster_indices = []
    
    for i in range(len(titles)):
        if i == 0:
            clusters[0].append(titles[i])
            cluster_indices.append(0)
        else:
            added = False
            for j in range(i):
                if similarity_matrix[i][j] >= threshold:
                    cluster_idx = cluster_indices[j]
                    clusters[cluster_idx].append(titles[i])
                    cluster_indices.append(cluster_idx)
                    added = True
                    break
            
            if not added:
                clusters[i].append(titles[i])
                cluster_indices.append(i)
    
    return [cluster for cluster in clusters if cluster]

headline_clusters = cluster_headlines(titles, similarity_matrix)

# Print results
print(f"Found {len(headline_clusters)} clusters")
for i, cluster in enumerate(headline_clusters):
    print(f"\nCluster {i+1} - {len(cluster)} headlines:")
    for headline in cluster:
        print(f"  • {headline}")
```
