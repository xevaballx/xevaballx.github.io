# Instance Based Learning

Instance-based learning stores training examples and makes predictions by comparing new inputs to these stored instances (data points) using similarity measures. It defers generalization until query time, earning the label "lazy learning." Unlike model-based learning, which discards the training data after learning a function (e.g., weights), instance-based methods do not learn an explicit model in advance, instead they generalize by referencing stored data at prediction time.

Training time is minimal or non-existent. KNN does not require linear separability. Its decision boundaries can be highly nonlinear, depending on the structure of the data and the value of k.

They are prone to overfitting, because they are sensitive to noise: they keep all of the noise. Irrelevant features can distort similarity calculations.

Inference can be slow, as it often requires comparing the query instance to man or all training examples.

## KNN: k-Nearest Neighbors Algorithm

Good for non-linearly separable problems with moderate dimensionality.

Given:  
- Training data: D = \\({(x_1, y_1), \dots, (x_n, y_n)} \\)  
- Query point: \\( x_q \\)  
- Number of neighbors: k  

Steps:
1. For each training point \\( x_i \in D \\), compute the distance to \\( x_q \\)
2. Sort the training points by distance to \\( x_q \\)
3. Select the k closest points to \\( x_q \\)
4. Let \\( N_k(x_q) \\) be the set of labels of the k nearest neighbors
5. If classification:
   - Return the **majority label** in \\( N_k(x_q) \\)
6. If regression:
   - Return the **average value** in \\( N_k(x_q) \\)

Preference Bias:  
1. Locality: Assumes near points are similar, so it is important to find the right distance function d() based on domain knowledge.
2. Smoothness: Predicts new values by averaging (or voting over) neighbors, assuming gradual changes in the target function.
3. Relevance: Assumes all features contribute equally unless weighting or feature selection is applied.

Distance Functions:  
Use Euclidean distance when your features are continuous and you care about straight-line distance in geometric space (e.g., image pixel values or spatial data).

Use Manhattan distance when your features are on grids, categorical/ordinal, or when you expect movement in axis-aligned paths (e.g., city block distances, text frequency counts).

