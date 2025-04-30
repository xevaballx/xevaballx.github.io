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

The acceptance probability is the "else" case is directly related to the difference in fitness between the current solution and the proposed neighbor. If the neighbor is much worse, the exponent becomes very negative, so there’s a lower chance of accepting it. Instead, we stay at the current solution and take our chances trying a different neighbor in the next iteration.

## **Genetic Algorithms (GA)**

Inspired by biological evolution. Maintains a population of solutions that evolve over time via selection, crossover, and mutation. Good at global exploration and well-suited for discrete and combinatorial problems, but can be computationally expensive.

Steps:
1. Randomly initialize a population P of candidate solutions (chromosomes) of size k  
2. Evaluate the fitness of each individual in the population, all \\(x \in P\\)  
3. Select parents from the population based on high fitness
4. **Crossover**: Combine parts of two parents to create offspring  
5. **Mutate** some of the offspring randomly to maintain diversity (exploration)  
6. Evaluate fitness of the offspring  
7. Replace individuals in the population with the new offspring  
8. Go back to step 3 util stopping condition met  

Genetic Algorithms use probability at almost every step — selection, crossover, mutation — to explore the search space stochastically rather than deterministically.

## **MIMIC (Mutual Information Maximizing Input Clustering)**

A probabilistic optimization method that samples solutions based on a learned probability model. At each iteration, it builds a model from the best samples and uses it to generate better ones. Powerful for problems with interdependent variables, but computationally heavy.

---

| Algorithm | Best For | Not Good At |
|-----------|----------|-------------|
| **Randomized Hill Climbing (RHC)** | Fast and simple local search; works well for smooth landscapes with a single peak | Easily gets stuck in local optima; poor global exploration |
| **Simulated Annealing (SA)** | Escaping local optima; balancing exploration and exploitation via temperature control | Requires careful tuning of temperature schedule; still not great for very rugged landscapes |
| **Genetic Algorithm (GA)** | Exploring large, complex, and discrete search spaces; combinatorial optimization | Can be slow to converge; sensitive to hyperparameters like mutation and crossover rates |
| **MIMIC** | Modeling dependencies between variables; structured problems with correlated inputs | Computationally expensive; not suitable for very large or high-dimensional search spaces |
