
You can train a Word2Vec model using Python’s Gensim library, which provides a fast and straightforward implementation. [1](https://medium.com/@manansuri/a-dummys-guide-to-word2vec-456444f3c673)

## Step 1: Prepare Your Text Data

* Clean and tokenize your text corpus into a list of lists (where each inner list represents a tokenized sentence/document of lowercased words).
* Remove punctuation and filter out extremely rare words if needed. [1](https://jaketae.github.io/study/word2vec/), [2](https://towardsdatascience.com/how-to-train-a-word2vec-model-from-scratch-with-gensim-c457d587e031/)

## Step 2: Choose Your Training Architecture

Word2Vec uses a shallow two-layer neural network and offers two main model types: [1](https://mbrenndoerfer.com/writing/word2vec-neural-word-embeddings),[2](https://www.quora.com/How-do-you-train-a-Word2Vec-model), [3](https://medium.com/@manansuri/a-dummys-guide-to-word2vec-456444f3c673)
* CBOW (Continuous Bag-of-Words): Predicts a target center word based on its surrounding context words (faster to train, good for frequent words).
* Skip-Gram: Predicts the surrounding context words given a target center word (better for rare words and smaller datasets). [1](https://medium.com/@manansuri/a-dummys-guide-to-word2vec-456444f3c673), [2](https://mbrenndoerfer.com/writing/word2vec-neural-word-embeddings)

## Step 3: Train the Model with Gensim

You can train a model from scratch using the Gensim Word2Vec Guide and Towards Data Science Tutorial: [1](https://towardsdatascience.com/how-to-train-a-word2vec-model-from-scratch-with-gensim-c457d587e031/), [2](https://medium.com/@manansuri/a-dummys-guide-to-word2vec-456444f3c673)
```python
from gensim.models import Word2Vec

# Example tokenized sentences
sentences = [
    ["cat", "sat", "on", "the", "mat"],
    ["dog", "ate", "my", "homework"],
]

# Train Word2Vec
model = Word2Vec(
    sentences=sentences,
    vector_size=100,  # Dimensionality of word vectors
    window=5,  # Maximum distance between current and predicted word
    min_count=1,  # Ignore words with total frequency lower than this
    workers=4,  # Number of CPU threads
    sg=0,  # 0 for CBOW, 1 for Skip-Gram
)

# Access a word vector
vector = model.wv["cat"]

# Find similar words
similar = model.wv.most_similar("cat")
```
Använd koden med försiktighet.

## Step 4: Extract and Save Your Embeddings

* Once training finishes, the weights of the hidden layer become your dense semantic word vectors.
* You can discard the output layer and save your model via model.save("word2vec.model") for future use. [1](https://mbrenndoerfer.com/writing/word2vec-neural-word-embeddings)

Would you like to see how to load a pre-trained Word2Vec model instead, or how to train one using PyTorch/TensorFlow from scratch?
