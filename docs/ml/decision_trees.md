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
1. $A$ = 'best' feature based on Best Information Gain.  
2. Assign $A$ as decision feature at this node (ask a question)  
3. For each \\(v \in A\\), create a branch from current node  
4. Group the training examples where feature $A = v$ into the corresponding branch  
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

max Gain(G,A) = least randomness = 'best' feature

\\(
\text{BestFeature} = \underset{A}{\arg\max} \ \text{Gain}(S, A) 
\quad \text{(i.e., feature that reduces entropy the most)}
\\)

