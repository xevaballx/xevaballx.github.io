# **Distributional Semantics**


- Word embeddings
- Word2Vec and Skip-gram
- CBOW
- GloVe
- Cosine similarity
- Analogy tasks
- Contextual embeddings (BERT, ELMo, etc.)

## **Continuous Bag of Words (CBOW)**

Predict a single word based on surrounding words.

$'crown with the ___ on the throne'$

**Loss**: Cross entropy between the predicted distribution and the true center word (i.e., the word that was masked out).

## **Skip-gram**

Predict surrounding words based on single word.

$'___ ___ ___ queen ___ ___ ___'$

**Loss**: Sum of cross entropy losses for each context word given the center word.

## **Word2Vec**

```python
class CBOW(nn.Module):
  def __init__(self, vocab_size, embed_size):
    super(CBOW, self).__init__()
    self.embedding_layer = nn.Embedding(vocab_size,embed_size)
    self.linear_layer = nn.Linear(embed_size,vocab_size)

  def forward(self, x):
    """ 
    Args: x: component of data: list of indices (window*2)
    """
    probs = None
    output = self.embedding_layer(x)
    output = F.sigmoid(output)
    output = output.mean(dim=1)
    output = self.linear_layer(output)
    probs = F.log_softmax(output, dim=1)
    return probs
```

```python
class SkipGram(nn.Module):
  def __init__(self, vocab_size, embed_size):
    super(SkipGram, self).__init__()
    self.embedding_layer = nn.Embedding(vocab_size,embed_size)
    self.linear_layer = nn.Linear(embed_size,vocab_size)

  def forward(self, x):
    """ 
    Args: x: component of data: a single token index
    Returns: probs: log softmax distro over the vocabulary
    """
    probs = None
    ### BEGIN SOLUTION
    output = self.embedding_layer(x)
    output = F.sigmoid(output)
    output = self.linear_layer(output)
    probs = F.log_softmax(output, dim=1)
    return probs
```