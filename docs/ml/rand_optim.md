# **Random Optimization**

Random optimization methods search for optimal solutions by incorporating randomness into the search process, rather than relying on gradients or deterministic rules. These techniques are especially useful for non-differentiable, noisy, or high-dimensional objective functions and data with many local optima, where traditional optimization methods struggle. They aim to balance exploration of the search space and exploitation of good candidates, often using heuristics or probability driven decisions.

## **Randomized Hill Climbing (RHC)**

A simple local search algorithm that starts with a random solution and iteratively makes small random changes that move it towards the optima. If the change improves the objective, it’s accepted; otherwise, it's rejected. Fast and intuitive, but prone to getting stuck in local optima. It can randomly restart to avoid getting stuck in local optima.

Steps:  
1. Initialize a random guess \\(x \in X \\)  
2. Generate a neighbor \\( n \in \mathcal{N}(x) \\) (where \\( \mathcal{N}(x) \\) is the neighborhood of \\( x \\))  
3. If \\( f(n) > f(x) \\), then set \\( x \leftarrow n \\) and go back to step 2  
4. Else, consider \\( x \\) a local optimum  
5. Optionally: Restart from a new random guess and repeat  


## **Simulated Annealing (SA)**

Like hill climbing, but occasionally accepts worse solutions to escape local optima. The probability of accepting worse solutions decreases over time according to a temperature schedule, inspired by the annealing process in metallurgy. Balances exploration and exploitation.

Steps:  
1. Initialize a random guess \\( x \in X \\) and a high temperature \\( T > 0 \\)

2. Generate a neighbor \\( n \in \mathcal{N}(x) \\)

3. If \\( f(n) > f(x) \\), then set \\( x \leftarrow n \\)

4. Else, accept \\( n \\) with probability:

\\( P = \exp\left( \frac{f(n) - f(x)}{T} \right) \\)

5. Cool down the temperature: \\( T \leftarrow \alpha T \\), where \\( \alpha \in (0, 1) \\)

Repeat steps 2–5 until stopping criteria is met (e.g., \\( T \\) is very small or max iterations reached)

## **Genetic Algorithms (GA)**

Inspired by biological evolution. Maintains a population of solutions that evolve over time via selection, crossover, and mutation. Good at global exploration and well-suited for discrete and combinatorial problems, but can be computationally expensive.

## **MIMIC (Mutual Information Maximizing Input Clustering)**

A probabilistic optimization method that samples solutions based on a learned probability model. At each iteration, it builds a model from the best samples and uses it to generate better ones. Powerful for problems with interdependent variables, but computationally heavy.