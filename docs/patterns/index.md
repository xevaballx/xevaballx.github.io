# **Patterns**

Reusable code and structures used across domains.

## **Docstring templates**

### **Function Template (Google Style)**

```python
def my_function(arg1, arg2):
    """Brief summary of function.

    Params:
        arg1, type: Description of first argument. (shape)
        arg2, type: Description of second argument. (shape)
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

--

## **Github Workflow**

Note: This doc is based on using the command line (terminal) in VSCode, not the VSCode extension for git.

### 0 Clone the repo
Go to GitHub in your browser. Go to our team repo and clone the project.
Click the green “code” button and clone the repo however you like. 
Copy the link.
On the command line type:
`git clone <paste link>`

### 0.1 Get your bearings
cd into the folder created by cloning, then type:
`ls` to see all the files

### 1 Ensure you are on the Main Branch
`git checkout main` 

### 2 Pull any new changes from Main Branch
Ensure you have the most up-to-date code
`git pull`

### 3 Create your branch
`git branch <YourFeatureHere>`
example: `git branch DogDoorActivator`

### 4 Checkout your branch
`git checkout <YourFeatureHere>`
example: `git checkout DogDoorActivator`

### 4.1 Make sure you are where you want to be
You can always type:
`git branch` to make sure you are on the branch you want to be on

### 5 Set the remote as upstream so you can eventually push changes to GitHub
`git push --set-upstream origin DogDoorActivator`

### 6 Write your code on this branch.

### 7 “Commit early push often.”
We want to push our commits to the GitHub remote feature branch often. When you are done for the day or want to take a break, commit and push.
From the root of the project folder do all three of the following:
1. `git add .` this tracks all of the files in this directory and its subdirectories 
2. `git commit -m “<YourCommitMessageHere>” `
3. `git push` this pushes any unsynced commits to remote branch on gitHub

### 7.1 Document/Comment and Test
Comment and test your code before you ask for a PR so the reviewer will have an easier time.

### 8 Pull Request (PR)
When you are completely done with your feature and you are ready to merge these changes to main, go to the GitHub repo and open a Pull Request (PR) from your branch to Main.

### 9 Ask one of your peers to review. 
If the peer has changes for you, do those changes on your branch. If it is perfect, they can merge it for you or approve it for you to merge. Click ‘squash and merge’ on the PR.

### 10 Delete your upstream branch on gitHub.
Or depending on your workflow, you can keep using your branch.

### 11 Repeat for your next feature! 
Starting at step 1.

## **For this io site**
After pushing repo, on command line `mkdocs gh-deploy`.

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

    The learning rate controls the size of step your optimizer takes when updating weights based on the gradient. It's arguably the most important hyperparameter to tune, and should be the the first you tune. A good learning rate balances speed and stability of training, IMO. Too small: slow convergence (or stuck in a local minimum).Too big: diverging loss or oscillating weights. Good: convergences fast to a good minimum. You can use a learning rate finder. Start with a very small learning rate, increase it exponentially, track the loss at each step, and plot loss vs. learning rate. The best learning rate is often just before the loss starts increasing rapidly.

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

    Weight decay is a regularization technique that helps prevent overfitting by discouraging large weights in the model. I do regularization tuning after learning rate, batch size, and model capacity. Decay too small: model may overfit because it memorizes training data. Too high: model underfits because it can’t capture patterns. Good: generalizes well by keeping weights. Start with a small weight decay value and gradually increase it until you see overfitting begin to diminish while training loss stays low. Once you find a good balance, it’s often worth restarting the tuning cycle at the learning rate as you might now be able to improve the loss further with a better learning rate in combination with the new regularization setting.

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