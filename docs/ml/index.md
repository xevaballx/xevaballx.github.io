# **Machine Learning**

[![Watch on YouTube](https://img.youtube.com/vi/DQWI1kvmwRg/3.jpg)](https://www.youtube.com/watch?v=DQWI1kvmwRg)

An inductive learner uses examples to find the best hypothesis h* from some class H. H is the set of possible answers to the problem. Think of hypothesis h as candidate answers.

## **Computation Learning Theory**

log base 2, is like keep dividing by 2

### **PAC Learning : Probably Approximately Correct : Error of Hypothesis h** 

Training Error: Fraction of training examples misclassified by h.  
Target concept should have training error of zero.  
\\(error_d (h_i) > \epsilon\\) means h is wrong, aka  \\( h_i \neq c_i \\)

True Error: Fraction of examples that *would be* misclassified on sample drawn from D in, essentially, the infinite limit. The probability that a sample drawn from D would be misclassified by some hypothesis h. 

\\(  error_d(h) = P(c(x) \neq h(x))\\) in x~D distribution 

So we are not penalized for examples that we never see. And examples that we see rarely, we only get a time bit of contribution to the overall error.

C is PAC-Learnable by *L* using *H*, \\(iff\\) learning *L* will, with high probability ( at least \\(  1-\delta\\)), output a hypothesis h \\(\in\\) *H* that it is very accurate \\( (error_D(h) \leq \epsilon)\\); and in time and samples it is bounded by polynomial in \\(  1/\epsilon, 1/\delta,\\)and n. 

If we want perfect error and perfect certainty than the denominators go to infinity as we look at all the data.

**Something is PAC-learnable if we can learn a hypothesis with low error and high confidence in polynomial time. Sample complexity tells us how many training examples we need to guarantee this level of performance.** 

- True Hypothesis: \\(  c \in  H \\) : hypothesis aka function
- Concept: *c* aka label
- Concept Class: The class from which the concept that we are trying to learn comes from.   
- Hypothesis Space: H : The set of mappings that the learning is going to consider.  
- Size of the hypothesis space: |H|: n  
- Distribution over inputs: D 
- **Version Space**: VS(S) = { *h* s.t. h \\(\in H \text{consistent wrt} S\\) : hypothesis consistent with examples : **the function we learned labels the data correctly**.   
- Error Goal: \\(   0 \leq \epsilon \leq 1/2\\) : We'd like the error in the hypothesis that we produce to be no larger than epsilon.  *Approximately* in PAC  
- Certainty Goal: \\(  0 \leq \delta \leq 1/2\\) : We might be unlucky and not meet our error goal, \\(  \delta\\) allows us to set a certainty goal, which means with \\(   P(1 - \delta)\\), the algorithm has to work. To work, here, means to produce a True Error \\( \leq  \epsilon\\). *Probably* in PAC. \\(   \delta\\) is our confidence paramater in PAC

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
m \geq \frac{1}{\epsilon} \left( (8 \cdot VC |H|) \cdot \log_2 + 4\log_2 \frac{2}{\delta} \right),
\\)

As VS|H| gets bigger we need more data.

*So...*     
**H is PAC-Learnable \\(iff\\) VC dimension is finite.**


### **VC Dimensions**

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

|H| = \\(  \infty \\)

But only \\(  \theta \\) above 10 is important

VC Dimension it the maximum number of points that can be shattered by the hypotheses in that space.

Shatter: For every possible labeling (0/1 classification) of the points, there exists a hypothesis (a classifier from your class) that perfectly separates the points according to that labeling.

VC dimension depends on the hypothesis class, not strictly the number of input features. Example: Both a circle and a sphere have 2 VC dimensions

High VC dimension mean the model can shatter more configurations, increasing the risk of over fit, likewise lower VC dimensions may to under fitting and poor accuracy if they are too low.

## **Information Theory**

In ML we want to know:

1. How each input relates to *y*
2. Which input splits our output best, giving us the most *information* about y

In general every input vector and output vector can be considered a *probability density function*.

**Information Theory** is a mathematical framework that allows us to compare these density functions.

Are input vectors are similar:  **mutual information**

Does this feature have any information: **entropy**

If output is predictable we don't need to communicate. Uncertain, meaningful information is harder to communicate and require more resources.

more predictable \\(\approx\\) less uncertainty \\(\approx\\) less information \\(\approx\\) less entropy

Example: Which message has more information?  
Given:  
- Language L={A,B,C,D}  
- Each word is represented by 2 bits, and occur with a specified probability.

First lets assume all words occur equally:

A: 00   25%  
B: 01   25%  
C: 10   25%  
D: 11   25%  

We have a sequence spelling BAD: 01 00 11

2 bit (1 or 0) symbol representation means we have to ask two yes or no questions per symbol.

Think of it as a tree. When we have a new bit coming in it can be 0 or 1 equally. So we ask two question, each "Is it 1?"

Is it one? Answer:   0 1  
Is it one? Answer:0 1   0 1  
Conclusion:       A B   C D  

Second message:

A: 50%  
B: 12.5%  
C: 12.5%  
D: 25%  

Since A occurs most frequently we can actually use less than two bits to represent each symbol. How should we represent it?

For the tree our first question is "Is it A?"

Is it A?:           0 1   
Semi-conclusion:    A  
Is it D?:             0 1  
Semi-conclusion:      D  
Is it 1?                0 1   
                        B C  

So  
A: 0  
D: 10  
B: 110  
C: 111   

Since A occurs most frequently and we only have to ask one question to determine if the symbol is A, we can ask less questions overall. We use the expected value to find out how much exactly.

\\(\sum P(\text{symbol}) \times \text{size of symbol}\\): (**Entropy**)
= 1P(A) + 2P(D) + 3P(B) + 3P(C)  
= 0.5 + 0.5 + 0.375 + 0.375  
= 1.75 bits per symbol  

= 1.75 bits < 2 bits 

The second language has more information and less randomness.

Note: This technique is called *variable length encoding* and explains why in morse code some symbols are smaller than others.

**Entropy**

Entropy captures the in amount of information contained in the variable by quantifying the uncertainty or unpredictability of the variable. 

higher entropy \\(\approx\\) more uncertainty \\(\approx\\) more information 

As shown above:  
\\(\sum P(\text{symbol}) \times \text{size of symbol}\\)

But we need to denote the size of each symbol more properly:

\\(\sum P(\text{symbol}) \times \frac{1}{P(\text{size of symbol})}\\)


\\(H(L) = - \sum P(s) \cdot \log_2 P(s)\\)

### **Information Between Variables**

Having information about multiple variables can change their probability.

If I hear thunder, it might change my prediction about rain and vice versa.

**Joint Entropy**
The amount of uncertainty contained in two variables together:

\\(H(x,y) = - \sum P(x,y) \cdot \log P(x,y)\\)

**Joint Entropy**
\\(H(y|x) = - \sum P(x,y) \cdot \log P(y|x)\\)

**Mutual information** measures the amount of shared information between two variables, by quantifying the reduction in uncertainty of one variable given knowledge of the other — i.e., statistical dependence.

I(X;Y) = H(Y) - H(Y|X)

It is symmetric: 

I(Y;X) = I(X;Y) = H(Y)−H(Y∣X) = H(X)−H(X∣Y)