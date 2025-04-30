# **Reinforcement Learning**

## **Markov Decision Process (MDP)**

An **MDP** is a formal framework used in reinforcement learning to model decision-making in environments with **stochastic outcomes**. It provides a structured way to reason about states, actions, rewards, and transitions over time. An MDP assumes the **Markov property**, meaning the future depends only on the current state and action, not the full history.

An MDP is defined by the tuple:

\\(
(\mathcal{S}, \mathcal{A}, \mathcal{P}, \mathcal{R}, \gamma)
\\)

- \\( \mathcal{S} \\): set of possible **states**  
- \\( \mathcal{A} \\): set of possible **actions**  
- \\( \mathcal{P}(s' \mid s, a) \\): **transition probability**   function
- \\( \mathcal{R}(s, a) \\): **reward function**  
- \\( \gamma \in [0,1] \\): **discount factor**, which prioritizes immediate rewards over distant ones  

## **Policy (\\( \pi \\))**

A **policy** defines the behavior of an agent in a MDP. It maps **states to actions**, specifying what action the agent should take in each state.

- A **deterministic policy** is a function:  
  \\( \pi(s) = a \\)  

- A **stochastic policy** gives a probability distribution over actions:  
  \\( \pi(a \mid s) = P(\text{take action } a \text{ in state } s) \\)

The goal of reinforcement learning is to **find a policy that maximizes expected cumulative reward** over time.


## **Bellman Equation**

The **Bellman Equation** expresses the value of a state as the **expected return** starting from that state and following a given policy. It breaks down the value into **immediate reward plus the discounted value of the next state**, enabling recursive computation of value functions.

For a **policy \\( \pi \\)**, the **Bellman Expectation Equation** is:

\\(
V^\pi(s) = \mathbb{E}_\pi \left[ R(s, a) + \gamma V^\pi(s') \right]
\\)

The Bellman Equation doesn't tell you which action to take — it tells you the expected value (i.e., long-term return) of being in a state (or taking an action in a state) under a specific policy.


For the **optimal policy**, the **Bellman Optimality Equation** is:

\\( V^\ast(s) = \max_a \mathbb{E} \left[ R(s, a) + \gamma V^\ast(s') \right] \\)


That’s where we actually optimize over actions to find the best action.

## **Value Iteration (VI)**

**Value Iteration** is a dynamic programming algorithm that finds the **optimal value function** by repeatedly applying the **Bellman Optimality Equation**. At each step, it updates the value of every state based on the **best possible action**, gradually converging to the optimal value function \\( V^* \\).

Update rule:

\\(
V_{k+1}(s) = \max_a \mathbb{E} \left[ R(s, a) + \gamma V_k(s') \right]
\\)

Once \\( V^\ast \\) has converged, the **optimal policy** can be extracted by choosing the action that maximizes the expected return:

\\( \pi^\ast(s) = \arg\max_a \mathbb{E} \left[ R(s, a) + \gamma V^*(s') \right] \\)

## **Policy Iteration (PI)**

**Policy Iteration** is a dynamic programming method that alternates between **evaluating a policy** and **improving it** until convergence to the **optimal policy** \\( \pi^* \\).

It has two main steps:

1. **Policy Evaluation**:  
   Compute the value function \\( V^\pi(s) \\) for the current policy using the Bellman Expectation Equation.

2. **Policy Improvement**:  
   Update the policy by choosing the action that maximizes expected return using \\( V^\pi(s) \\):

   \\(
   \pi'(s) = \arg\max_a \mathbb{E} \left[ R(s, a) + \gamma V^\pi(s') \right]
   \\)

Repeat these steps until the policy stops changing — at that point, \\( \pi^* \\) is the **optimal policy**.


More expensive than VI, but less iterations.

## **Q-Learning**

Value Iteration (VI) and Policy Iteration (PI)are used to solve MDPs, but they are not considered true reinforcement learning algorithms, because they assume full knowledge of the environment:

They require access to the **transition probabilities** \\( P(s' \mid s, a) \\) and the **reward function** \\( R(s, a) \\).

In contrast, **reinforcement learning** (like Q-learning or SARSA) is designed for situations where the agent must **learn by interacting with the environment**, without knowing the transition or reward model in advance.

**Q-learning** is an **off-policy reinforcement learning algorithm** that learns the **optimal action-value function** \\( Q^*(s, a) \\) without requiring a model of the environment. It updates estimates of Q-values based on observed rewards and the maximum estimated future return.

The update rule is:

\\(
Q(s, a) \leftarrow Q(s, a) + \alpha \left[ r + \gamma \max_{a'} Q(s', a') - Q(s, a) \right]
\\)

- \\( \alpha \\): learning rate  
- \\( \gamma \\): discount factor  
- \\( r \\): reward from taking action \\( a \\) in state \\( s \\)  
- \\( s' \\): resulting next state

Over time, Q-learning converges to the optimal Q-function, and the **optimal policy** is:

\\(
\pi^\ast(s) = \arg\max_a Q^\ast(s, a)
\\)


**Q-Values** represent expected cumulative reward for taking a specific action in a given state.

Q-values represent the **expected cumulative reward** for taking a specific **action** in a given **state**, and then following the policy thereafter. So:

\\(
Q(s, a) = \mathbb{E} \left[ \sum_{t=0}^{\infty} \gamma^t R_t \mid s_0 = s, a_0 = a \right]
\\)

Where:
- \\( s \\) is the current state
- \\( a \\) is the action taken
- \\( \gamma \\) is the discount factor
- \\( R_t \\) are the rewards

So it’s about both **state and action** (not just the state).

**epsilon decay ration**
Even if we think a certain action has the highest Q-Value(based on earlier exploration) in a state, we still randomly try other actions with probability epsilon. This helps us discover better actions we might have missed early on.

**alpha: learning rate**
Controls how much we update the Q-value based on new information. Higher values is bigger updates to Q-values.

Epsilon controls which actions we try (explore).

Alpha controls how strongly each experience influences learning. 

\\(
Q(s, a) \leftarrow Q(s, a) + \alpha \cdot \big[ \text{target} - Q(s, a) \big]
\\)


---
| Algorithm     | Key Idea                                   | Pros                          | Cons                          |
|---------------|--------------------------------------------|-------------------------------|-------------------------------|
| **Value Iteration (VI)** | Iteratively update value function using Bellman optimality | Simple to implement; guaranteed convergence | Requires full model; can be slow to converge |
| **Policy Iteration (PI)** | Alternates between policy evaluation and improvement | Often faster convergence than VI | Requires full model; policy evaluation can be expensive |
| **Q-Learning**         | Learn action-value function via experience; off-policy | Doesn’t need model; learns while acting | Slower convergence; sensitive to exploration strategy |


The main difference between Value Iteration (VI) and Policy Iteration (PI) is how they update the policy:

VI updates the value function first, using the Bellman optimality equation, and derives the policy afterward.

PI starts with a policy and alternates between evaluating the current policy and improving it based on the value estimates.
