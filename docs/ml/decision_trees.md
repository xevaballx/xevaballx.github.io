# Decision Trees Overview
Decision Trees are a form of classification learning.
Decision Trees are representations of our **features** and we need to determine representation before deciding on algo. First we identify our problem (root question). Then we identify attributes/features and ask questions about those. These are the **decision nodes**. Examples: Is it raining? What type of restaurant is it? 

**Edges** represent the **value** of each decision node. Examples: Yes and No. Pizza, Thai, Mexican. 

The **leaf** is the answer to the problem (root question), output. Example: Should we each in this restaurant? Should I play tennis? Leaf is yes or no.

We need to build a decision tree as a result of processing training data. The order of our decision nodes should correlate to the node's ability to reduce space (number of nodes?). All nodes/features may not be necessary to make our root decision. If we only want to play tennis if its not raining, that might be near the top of our tree as it can rule out other branches and other features.

Naive Decision Tree algo (to build a tree)
1. Pick the "best" feature — ideally one that splits the data in half or reduces entropy the most
2. Ask a question based on this feature
3. Follow the answer path
4. If not a leaf node, go back to step 1 with the remaining features and data subset.

Problem here is that we have to follow *all* possible paths and think of all possible best next features until we can completely answer any question. So we aren't *learning* the tree, we are using a brute-force approach to build it.

Given *n* boolean features, there are \\(2^n\\) possible ways to arrange the features and \\(2^{2^n}\\) ways to assign binary labels to those combinations. The hypothesis space (\\(2^{2^n}\\)) of the decision tree is very expressive because there are many possible functions a decision tree could represent: it has the *capacity* to represent a massive number of different functions from inputs to outputs.

# ID3 Algo
Loop:  
1. *A* = 'best' feature based on Best Information Gain.  
2. Assign *A* as decision feature at this node (ask a question)  
3. For each \\(v \in A\\), create a branch from current node  
4. Group the training examples where feature *A = v* into the corresponding branch  
5. If all examples in a branch are of the same class (pure leaf), stop  
6.  Else, repeat the process recursively on that branch with remaining features  

Example: A is Weather, values are {Sunny, Rainy, Overcast}. A becomes, "What is the Weather?" and \\(v \in A\\) is one of the possible answers.

**Information Gain** measures how much knowing the value of a feature helps us predict the label of a data point.  
- Before the split on a feature, our data might be a mix of labels (50% Yes, 50% No ~ high uncertainty)  
- After the split, if a feature separates the data such that one side is mostly Yes and the other mostly No, we’ve reduced our uncertainty about the label by asking about that feature.  


That's the whole goal of decision trees: to use features to reduce label uncertainty step by step.

\\(
\text{Gain}(S, A) = \text{Entropy}(S) - \sum_{v \in \text{Values}(A)} \frac{|S_v|}{|S|} \cdot \text{Entropy}(S_v)
\\)

Where:  
- *S* is current set of training examples  
- *A* is the feature we are evaluating (Weather)
- *Values(A)* are the possible values of feature *A* ({Sunny, Rainy, Overcast})  
- \\(S_v\\) is subset of *S* where feature *A = v*  
- *Entropy(S)* is the entropy of the full set

Everything after the minus sign in the Information Gain formula represents the expected entropy after the split: the average uncertainty over each subset, weighted by the proportion of examples in that subset.

**Entropy** is the measure of randomness.

Example: A coin has a 50% chance of producing either outcome, so it has 1 bit of entropy. Vs a coin with two heads, which as no entropy or zero bits.

\\( Entropy(S) = -\sum_{v \in V} p(v) \cdot \log_2 p(v) \\)

\\(
\text{BestFeature} = \underset{A}{\arg\max} \ \text{Gain}(S, A) 
\quad \text{(i.e., feature that reduces entropy the most)}
\\)

# Inductive Bias

There are two kinds of bias we need to consider when designing any classifier:

**Restriction Bias**: H  
This bias arises automatically from our choice of hypothesis set, H. It limits the functions we are capable of learning. When using decision trees, *H* includes only functions that can be represented by decision trees: this is our restriction bias.

**Preference Bias** \\( h \in H \\)  
Given \\( h \in H \\), this is the bias toward which hypotheses within *H* we prefer. It influences how we search within *H*, and determines which solutions we favor when multiple hypotheses fit the data.

So which decision would ID3 prefer:  
- Good **splits** near the top: ID3 greedily chooses features with the highest information gain from the top down. 
- **Correct** over incorrect: ID3 expands nodes until all training examples are correctly classified.
- **Shallow trees (implicitly)**: Because it prioritizes informative splits early, ID3 tends to prefer trees with shallower depth, even though this is not explicitly enforced.

# Continuous Features  

ID3 attempts to split on *every possible value* for (\\v \in A\\). This is infeasible for many features especially continuous ones. The solution is to discretize or bin values. Example: If A = age, we could create a value like v = 20 < Age < 30.

# Stopping Point

ID3 stops expanding the tree when all training examples are perfectly classified: when every leaf is pure. This may lead to very deep trees and overfitting, especially if the data contains noise or exceptions.

Example: Suppose Alice plays tennis when it’s raining and windy, but Tony does not. If these cases contradict, the naive ID3 algorithm would keep splitting forever in an attempt to fit both.

Solution: **Validation-based early stopping**  
Hold out a validation set. Each time the tree expands, evaluate performance on the validation data. If adding a split doesn’t reduce validation error meaningfully, stop expanding. This prevents overfitting and infinite splitting on noisy examples.

# Regression

While classification predicts discrete labels (*yes* or *no*), **regression** predicts continuous numerical values (temperature, price, or age). While classification trees aim to minimize impurity (like Gini or entropy), regression trees aim to minimize a continuous loss function — typically Mean Squared Error (MSE) — to better predict numeric outcomes. While classification trees typically use information gain to decide splits, regression trees minimize a continuous loss function such as Mean Squared Error (MSE).

\\(
\text{MSE} = \frac{1}{n} \sum_{i=1}^{n} (\hat{y}_i - y_i)^2
\\)

MSE predicts the **average** value of the target variable in each leaf and chooses splits that minimize the variance of the output.

# Lazy vs Eager

ID3 is an **eager** algorithm that builds a full decision tree during training, *before* it sees test data. While a **lazy** algorithm delays computation until a predication is needed.

A lazy version of ID3 would store the training data and construct just enough of the decision tree on-demand at prediction time to classify the input. This avoids constructing the entire tree ahead of time.