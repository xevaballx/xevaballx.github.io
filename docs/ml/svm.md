
# Support Vector Machines (SVMs)

Support Vector Machines are **model-based supervised learning algorithms** used for classification and regression. They are linear classifier with a kernel trick. Unlike instance-based methods, SVMs learn an explicit decision boundary by optimizing a global objective during training. They aim to find the **hyperplane** that maximally separates classes with the **largest margin**. This leaves the biggest margin of error if some hidden dots got discovered. 

![SVM margin illustration](../assets/images/SVM_margin.png)


Although SVMs build a global model, only a subset of the training examples, **support vectors** are used to define the decision boundary. The number of support vectors increases naturally when the data is more complex or less separable, reflecting greater model expressiveness.

Support vectors are key data points, not features or dimensions.
The number of support vectors reflects how “hard” the boundary is to define, but it does not correspond to added dimensions in feature space. Kernel functions, not support vectors, are what increase dimensionality.

SVMs work well for **both linearly and non-linearly separable data** by using the **kernel trick**, which implicitly maps data into higher-dimensional spaces where linear separation is possible.

SVMs can overfit if the **kernel** or **regularization parameter** is poorly chosen, but they are generally more robust to noise than instance-based methods, since irrelevant data points that do not affect the margin are ignored.

Training time can be high due to solving a convex optimization problem, but inference is efficient and depends only on the number of support vectors.

Decision trees can handle linearly separable data, but they are not optimal because they use axis-aligned splits and may require many steps to approximate a linear boundary. Algorithms like logistic regression or linear SVM are typically more efficient in these situations.

## SVM: Basic Idea

Given:  

- Training data: \\( D = \{(x_1, y_1), \dots, (x_n, y_n)\} \\), where \\( y_i \in \{-1, +1\} \\)  

- Input feature space: \\( R^d \\)  

- Optional kernel function: \\( K(x_i, x_j) \\)  


Steps:  

1. Maximize the margin between classes by solving:
   \\( \min_{w, b} \frac{1}{2} \|w\|^2 \quad \text{subject to } y_i(w^\top x_i + b) \geq 1 \\)

2. For non-linearly separable data, add slack variables and regularization:  
   \\( \min_{w, b, \xi} \frac{1}{2} \|w\|^2 + C \sum \xi_i \\)

3. Use the kernel trick to replace dot products:  
   \\( K(x_i, x_j) = \phi(x_i)^\top \phi(x_j) \\)

4. Prediction function:  
   \\( f(x) = \text{sign} \left( \sum_{i \in \text{SV}} \alpha_i y_i K(x_i, x) + b \right) \\)

## Preference Bias

1. **Maximum Margin**: Prefers decision boundaries that maximize distance from the closest points.  
2. **Support Vector Focus**: Only the most critical training points (support vectors) define the boundary.  
3. **Kernel Assumption**: Assumes data can be linearly separable in some higher-dimensional space.  
 
SVMs resemble instance-based methods (like KNN) in that they rely only on critical training examples at prediction time. But unlike KNN, SVMs perform model-based optimization to identify and retain only the important instances (support vectors), discarding the rest.