# Features

Curse of dimensionality: the amount of data we need grows exponentially in the number of features we have.

## Feature Selection

Create a subset of features needed for our learner. Can be used in supervised or unsupervised learning.

Algorithms Families: Filtering and Wrapping  

**Filtering**  
Search process directly reduces the dataset to a smaller feature set. Faster, but ignore bias.

Decision Trees are essentially a filtering algorithm since we split on information gain.


**Wrapping**  
The search process interacts directly with the learning algorithm to iteratively adjust the feature set based on performance. It considers model bias and performance, making it more accurate but often slower than filter methods.

Random optimization algorithms (like GA, SA) work well in this context by treating the learner’s output (e.g., accuracy) as a fitness function, guiding the search toward better feature subsets.

## Feature Transformation

These techniques are unsupervised 

**Principal Component Analysis (PCA)**
PCA is a linear transformation technique that projects data into a new space defined by the directions (principal components) of maximum variance. It reduces dimensionality while preserving as much of the total variance as possible. Components are **uncorrelated** and **ranked by the amount of variance they explain**. PCA enforces orthogonality to capture variance. 

PCA can prevent classification if the original feature is best, but the variance is small. PCA will throw away the original feature and we are left with ran data point that doesn't predict.

**Independent Component Analysis (ICA)**
ICA transforms the data into **components that are statistically independent**, not just uncorrelated. It’s often used when the goal is to separate mixed signals (e.g., audio source separation). Unlike PCA, ICA doesn't prioritize variance — it assumes underlying signals are non-Gaussian and independent.

ICA maximization of statistical independence is a stronger condition than uncorrelated-ness. 

**Randomized Component Analysis (RCA)**
RCA is a transformation method that uses random projections to reduce dimensionality. It preserves pairwise distances approximately using random linear mappings, and is especially useful for very high-dimensional data where exact methods like PCA are computationally expensive. RCA trades accuracy for speed.

---
| Method | Used For | Pros | Cons |
|--------|----------|------|------|
| **PCA** (Principal Component Analysis) | Dimensionality reduction, noise filtering, visualization | Captures max variance; uncorrelated components; fast & widely used | May discard predictive features with low variance; ignores labels |
| **ICA** (Independent Component Analysis) | Blind source separation (e.g., audio signals), uncovering hidden independent factors | Finds statistically independent components; useful for signal separation | Sensitive to noise; assumes non-Gaussianity; slower than PCA |
| **RCA** (Randomized Component Analysis / Projection) | Fast approximation for high-dimensional data; efficient dimensionality reduction | Scales well to large data; preserves pairwise distances approximately | Random — may lose interpretability or useful structure; less accurate |
