# Bayesian Learning Overview

In machine learning we are trying to learn the "best" hypothesis that we can, given some data and some domain knowledge. We do this by searching the hypothesis space. In Bayesian learning we think of best as the most probable or the most likely to be correct. 

\\(P(h \in H | Data)\\)

\\(\arg\max_{h \in H} P(h \mid D)\\)

## Bayes Rule

\\(
P(h \mid D) = \frac{P(D \mid h) \cdot P(h)}{P(D)}
\\)

**Posterior** \\(P(h \mid D)\\): Probability of hypothesis h given data 
𝐷
D

**Likelihood** \\(P(D \mid h)\\): Probability we see some data given h is true. Usually easy to find.

**Prior** \\(P(h)\\): Our belief about the hypothesis before seeing the data.

**Evidence** \\(P(D)\\): Prior on the data. It represents the overall probability of observing the data under all possible hypotheses. It is used to normalize the posterior probabilities, ensuring they sum to 1. Often, it doesn't matter when comparing hypotheses for classification because it is constant with respect to h, and thus cancels out in \\(\arg\max_h P(h \mid D)\\).

Bayes’ Rule allows us to invert a conditional probability, switching from \\(P(h \mid D)\\) to \\(P(D \mid h)\\), so that we can reason about a hypothesis even when \\(P(h \mid D)\\) is hard to compute directly. This works because of the definition of conditional probability and the symmetry of joint probability:


\\( P(D,h) = (P(D \mid h) P(D) = P(h \mid D) P(D) \\)

This lets us rewrite problems in terms of what is easier to estimate or known, by simply moving the denominator to isolate the posterior.

## Priors Matter: Example

Consider a person diagnosed with an extremely rare disease that occurs in 8 of 1000 people. The test used to diagnose them returns correct positives 97% of the time, and correct negatives 98% of the time. Even though they were diagnosed with the disease, do they actually have it?

**Given:**

- Prior probability of disease:  
  \\( P(\text{Disease}) = \frac{8}{1000} = 0.008 \\)

- Probability of positive test given disease (True Positive rate):  
  \\( P(\text{Positive} \mid \text{Disease}) = 0.97 \\)

- Probability of negative test given no disease (True Negative rate):  
  \\( P(\text{Negative} \mid \text{No Disease}) = 0.98 \\)

- Therefore, False Positive rate:  
  \\( P(\text{Positive} \mid \text{No Disease}) = 0.02 \\)


We want to compute the **posterior probability** that a person has the disease given a positive test:

\\(
P(\text{Disease} \mid \text{Positive}) = \frac{P(\text{Positive} \mid \text{Disease}) \cdot P(\text{Disease})}{P(\text{Positive})}
\\)


\\(
P(\text{Disease} \mid \text{Positive}) = 0.97 \cdot 0.008 \approx 0.00776
\\)

vs

\\(
P(\text{not Disease} \mid \text{Positive}) = \frac{P(\text{Positive} \mid \text{not Disease}) \cdot P(\text{not Disease})}{P(\text{Positive})}
\\)

\\(
P(\text{not Disease} \mid \text{Positive}) = 1 - 0.98 \cdot 1 - 0.008 \approx .01984
\\)

Since \\( 0.01984 > 0.00776 \\), using argmax we choose the second hypothesis: Even with a positive test result, it is **more likely that the person does not have the disease** because the disease is so rare. 



