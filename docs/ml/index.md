# Machine Learning

[![Watch on YouTube](https://img.youtube.com/vi/DQWI1kvmwRg/3.jpg)](https://www.youtube.com/watch?v=DQWI1kvmwRg)

An inductive learner uses examples to find the best hypothesis h* from some class H. H is the set of possible answers to the problem. Think of hypothesis h as candidate answers.

## Computation Learning Theory

log base 2, is like keep dividing by 2

### PAC Learning : Probably Approximately Correct : Error of Hypothesis h 

Training Error: Fraction of training examples misclassified by h.  
Target concept should have training error of zero.  
\\(error_d (h_i) > \epsilon\\) means h is wrong, aka  \\( h_i \neq c_i \\)

True Error: Fraction of examples that *would be* misclassified on sample drawn from D in, essentially, the infinite limit. The probability that a sample drawn from D would be misclassified by some hypothesis h. 

\\(  error_d(h) = P(c(x) \neq h(x))\\) in x~D distribution 

So we are not penalized for examples that we never see. And examples that we see rarely, we only get a time bit of contribution to the overall error.

C is PAC-Learnable by *L* using *H*, \\(iff\\) learning *L* will, with high probability ( at least \\(  1-\delta\\)), output a hypothesis h \\(\in\\) *H* that it is very accurate \\( (error_D(h) \leq \epsilon)\\); and in time and samples it is bounded by polynomial in \\(  1/\epsilon, 1/\delta,\\)and n. 

If we want perfect error and perfect certainty than the denominators go to infinity as we look at all the data.

Meaning: something is PAC-Learnable if we can learn to get a low error, with high confidence, in time that is sort of polynomial in all the parameters.

- True Hypothesis: \\(  c \in  H \\) : hypothesis aka function
- Concept: *c* aka label
- Concept Class: The class from which the concept that we are trying to learn comes from.   
- Hypothesis Space: H : The set of mappings that the learning is going to consider.  
- Size of the hypothesis space: |H|: n  
- Distribution over inputs: D 
- **Version Space**: VS(S) = {h s.t. h \\(\in H consistent wrt S\\) : hypothesis consistent with examples : **the function we learned labels the data correctly**.   
- Error Goal: \\(   0 \leq \epsilon \leq 1/2\\) : We'd like the error in the hypothesis that we produce to be no larger than epsilon.  *Approximately*  
- Certainty Goal: \\(   0 \leq \delta 1/2\\) : We might be unlucky and not meet our error goal, \\(   \delta\\)allows us to set a certainty goal, which means with \\(   P(1 - \delta)\\), the algorithm has to work. To work, here, means to produce a True Error \\(   \leq to \epsilon\\). *Probably*

Haussler's Theorem (Realizable Case)

Let H be a finite hypothesis class, and let h \\(\in\\) H be a hypothesis consistent with a training set of *m* examples drawn i.i.d. from an unknown distribution *D*. Suppose the target function *f* is h \\(\in\\) H (i.e., the realizable case).

Then for any \\(\epsilon > 0\\) and \\(\delta \in (0, 1)\\), if

\\(
m \geq \frac{1}{\epsilon} \left( \ln |H| + \ln \frac{1}{\delta} \right),
\\)

then with probability at least \\(1 - \delta\\), every hypothesis \\(h \in H\\)that is consistent with the training data satisfies

\\(
\Pr_{x \sim D} [h(x) \neq f(x)] \leq \epsilon.
\\)


And for infinite hypothesis class:

\\(
m \geq \frac{1}{\epsilon} \left( (\dot VC |H|) /dot /log_2 + 4\log \frac{2}{\delta} \right),
\\)

*So...*     
**H is PAC-Learnable \\(iff\\) VC dimension is finite.**


## VC Dimensions

VC Dimensions help us determine how much data we need to learn effectively even if the hypothesis class is infinite. It expresses how expressive or powerful a model class is in terms of what patterns it can learn: this is a measure of complexity or capacity. 

Most of the algorithms we use have an infinite hypothesis space:  
- linear separators: there are infinite number of lines to choose from since m x and b are reals  
- NN: each weights has an infinite amount of numbers it can be since weights are reals  
- decision trees with continuous inputs: infinite number of questions we can ask at decision nodes  
- ...

How all of our ML algorithms work is like this: keep track of *all* hypotheses and keep this **version space**. Once we see enough examples randomly drawn we know we've **epsilon-exhausted** the version space. So any hypothesis that is left is going to be ok.

But we can't keep track of infinite hypotheses.

So we will find a way to keep track of only the hypothesis that can affect the outcome differently. 

Example:
X: {1,2,3,4,5,6,7,8,9,10}  
H: {h(x) = x \\(  \leq \theta \\)}

|H| = \\(  \infinite \\)

But only \\(  \theta \\) above 10 is important

VC Dimension it the maximum number of points that can be shattered by the hypotheses in that space.

Shatter: For every possible labeling (0/1 classification) of the points, there exists a hypothesis (a classifier from your class) that perfectly separates the points according to that labeling.

VC dimension depends on the hypothesis class, not strictly the number of input features. Example: Both a circle and a sphere have 2 VC dimensions

High VC dimension mean the model can shatter more configurations, increasing the risk of over fit, likewise lower VC dimensions may to under fitting and poor accuracy if they are too low.

