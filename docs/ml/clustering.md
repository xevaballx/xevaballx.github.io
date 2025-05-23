# Clustering

Clustering is a way to describe unlabeled by putting objects into groups or clusters. Clusters can help uncover hidden structure, and may align with true labels in structured datasets.

**Features scaling** ensures all features have equal importance and  is essential for all three algorithms below since similarity is measured by "distance." Euclidean distance for K-Means and SLC; and for EM distance is measured using Gaussian densities, where unscaled features skew covariance estimation and likelihood. 

All three methods are highly sensitive to **outliers**, especially SLC.


## **Single-Link Clustering (SLC)**

SLC is a type of hierarchical agglomerative clustering, similar to a spanning tree.

Steps:  
1. Each datapoint is assigned to its own cluster.  
2. At each set, merges the two clusters whose closest pair of points have the smallest distance.
3. Stop when k clusters have formed.

Forms elongated chain-like clusters. Can handle non-spherical shapes but is sensitive to noise and outliers.

**Complete-Link** clustering instead merges based on the *maximum distance* between points in two clusters, creating compact, spherical clusters.  
**Average-Link** clustering uses the **average distance** between all pairs of points across two clusters, providing a balance between chaining and compactness.

## **K-Means Clustering**

- K-Means is an **unsupervised learning** algorithm for **partitioning data into K clusters**.  
- Each cluster is defined by its **centroid** (mean of the points in the cluster).  
- The algorithm minimizes the **within-cluster sum of squared distances**:  

  \\( \sum_{i=1}^K \sum_{x \in C_i} \| x - \mu_i \|^2 \\)


**Steps**:  
1. Initialize K centroids (randomly or with heuristics like K-Means++).  
2. Assign each point to the nearest centroid.  
3. Update centroids as the mean of assigned points.  
4. Repeat steps 2–3 until convergence (no changes or small error).  

- Sensitive to initial centroids and may converge to **local minima**, random restarts help.
- Doesn’t model probability or handle overlapping clusters well.  
- Works best with **spherical**, equally sized clusters, which can be a problem as shown in the image below.

![Kmeans spherical illustration](../assets/images/kmeans-spheres.png){ width="25%" }


## **Expectation-Maximization (EM)**

- EM is a **probabilistic algorithm** for **maximum likelihood estimation** with **latent variables**.  
- Commonly used for **Gaussian Mixture Models (GMM)**.  
- **Assumes** each data point was generated by a hidden distribution (e.g., one of several Gaussians).  

**Steps**:  
1. **E-step**: Assign soft responsibilities (probabilities) to each point for belonging to each cluster, based on the current parameters (means, covariances, weights). Not a hard assignment like in K-Means. 
2. **M-step**: Update cluster parameters to **maximize the expected likelihood**, by averaging over all data points weighted by the responsibilities from the E-step.  
3. Iterates E and M steps until convergence.  

- Sensitive to initial centroids and may converge to **local minima**, initializing with K-Means centroids and random restarts can help.  
- Models **soft assignments** (fractional membership in clusters).  
- Can model **elliptical**, overlapping clusters.  
- More flexible than K-Means, but computationally heavier.   


| Algorithm               | Runtime (Typical)         | Best For                                          |
|------------------------|---------------------------|---------------------------------------------------|
| **Single-Link Clustering** | \\( \mathcal{O}(n^2 \log n) \\) or \\( \mathcal{O}(n^3) \\) | Finding non-spherical, chain-like clusters; dendrogram-based analysis |
| **K-Means**            | \\( \mathcal{O}(nkdT) \\) | Fast clustering of spherical, well-separated clusters in large datasets |
| **EM (e.g., for GMM)** | \\( \mathcal{O}(nkd^2T) \\) | Soft clustering; modeling overlapping, elliptical clusters with uncertainty |

K-Means is a simplified version of EM for GMMs, where all clusters have identical, fixed variance and points are hard-assigned to the nearest centroid.
