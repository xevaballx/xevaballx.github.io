# **Patterns**

Reusable code and structures used across domains.

## **Docstring templates**

### **Function Template (Google Style)**

```python
def my_function(arg1, arg2):
    """Brief summary of function.

    Args:
        arg1 (type): Description of first argument. (shape)
        arg2 (type): Description of second argument. (shape)

    Returns:
        type: Description of the return value.

    Raises:
        SomeError: When this happens.
    """
```
### **Class Template**


```python
class MyModel:
    """Short summary of class.

    Attributes:
        name (str): Description of the attribute.
        layers (list): Description of the model layers.
    """

    def __init__(self, name):
        """Initialize the model with a name."""
        self.name = name
```

## **General Hyperparameter Tuning Strategy**
Improve model performance by adjusting key training and model configuration parameters without overfitting or wasting compute.

Start with  baseline:  
- Run model with default or reasonable parameter  
- Record metrics: accuracy, loss, runtime  
- Use this to compare future improvements as we gradually increase complexity  
- Track experiments with Weights & Biases  

### **Tuning Order Priority**
1. Learning rate
    - Start with learning rate finder  
    - Often has largest effect on both convergence speed and final performance  

2. Batch Size  
    - Larger batches: more stable gradients but may generalize worse  
    - Smaller batches: more noise, sometimes better generalization
    - Changing batch size may require retuning learning rate  

3. Architecture/model capacity: decide if model too big/small  
    - Tune only after learning is stable    
    - Adjust number of layers, hidden units, width vs. depth    

4. Regularization  
    - Dropout, weight decay, early stopping, etc.
    - After we have a model that can overfit out data  

5.  Optimizer
    _ Different optimizers work better for different problems
    - Adam is good default 
    - Try SGD+momentum is better for CV and large-scale tasks

6. Advanced hyperparams specific to arch. (attention heads, embedding dims.)

### **Convergence**

The point where the model stops meaningfully improving 

**Signals of Convergence**  
- Validation loss stops decreasing or begins to increase  
- Evaluation metric (e.g., accuracy, F1) stops improving  
- Training loss decreases but validation loss worsens: overfitting  
- Model predictions stabilize (low variance across epochs)

**Best Practices**  
- Use early stopping (e.g., patience = 5 epochs without improvement)  
- Be cautious of slow convergence caused by learning rate that’s too low  

**Practical Convergence Criteria**  
- Task-specific thresholds that define “good enough”  
- Helps guide whether to continue training, stop early, or try a different approach  

**Examples**  
- "Reach 80% accuracy in under 5 epochs"  
- "Improve F1 score to 0.7 using fewer than 100K parameters"  
- "Train for no more than 30 minutes on a laptop GPU"  
- "Outperform logistic regression by 5%"


### **Evaluation Metrics**

**Accuracy**  
- Percentage of correct predictions  
- Best for: balanced datasets  
- Bad for: imbalanced classes  
(e.g., 95% accuracy when 95% of data is class A = misleading)

**Precision**  
- Of predicted positives, how many were correct?  
- Formula: `TP / (TP + FP)`  
- Best for: when false positives are costly (e.g., spam, security alerts)

**Recall**  
- Of actual positives, how many were correctly predicted?  
- Formula: `TP / (TP + FN)`  
- Best for: when false negatives are costly (e.g., cancer and fraud detection)

**F1 Score**  
- Harmonic mean of precision and recall  
- Formula: `2 * (P * R) / (P + R)`  
- Best for: imbalanced data where you care about both P and R

**AUC-ROC**  
- Probability the model ranks a positive higher than a negative  
- Best for: binary classification, threshold-based tasks

**Perplexity**  
- Measures how well a language model predicts a sample  
- Lower is better: lower perplexity means the model is more confident and assigns higher probability to the correct words   
- So, Perplexity of 1 means perfect prediction (model is certain). Perplexity of 10 means model is choosing among ~10 likely options per token.  
- Best for: language models trained for next-word or sequence prediction (e.g., RNNs, Transformers)

\\(
  \text{Perplexity} = 2^{-\frac{1}{N} \sum_{i=1}^{N} \log_2 p(w_i)}
\\)  

**MSE / MAE / RMSE**  
- Error metrics for continuous outputs  
- Best for: regression problems

## **Batching**
Basic batching

```python
def get_batch(data, index, batch_size=10):
    """ 
    Create batch of data of given batch_size, starting at given index.
    Check device, run on GPU if available. 

    Args: 
        data: CBOW data, list of (context_indices, target_index) tuples (2 * window, int)
    
    Returns:
        x: tensor of context words, one row per training example, each with it
            context word indices, (batch_size, window * 2)
        y: tensor of target words, one target word index per row (batch_size,)
    """
    batch = data[index:index + batch_size]
    x = [row[0] for row in batch]
    y = [row[1] for row in batch]

    x = torch.tensor(x, dtype=torch.long).to(DEVICE)
    y = torch.tensor(y, dtype=torch.long).to(DEVICE)

    return x, y
```