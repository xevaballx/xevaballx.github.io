Policy: mapping of all possible states to actions

**Q-Values** represent expected cumulative reward for taking a specific action in a given state.

Q-values represent the **expected cumulative reward** for taking a specific **action** in a given **state**, and then following the policy thereafter. So:

\[
Q(s, a) = \mathbb{E} \left[ \sum_{t=0}^{\infty} \gamma^t R_t \mid s_0 = s, a_0 = a \right]
\]

Where:
- \( s \) is the current state
- \( a \) is the action taken
- \( \gamma \) is the discount factor
- \( R_t \) are the rewards

So it’s about both **state and action** (not just the state).

**epsilon decay ration**
Even if we think a certain action has the highest Q-Value(based on earlier exploration) in a state, we still randomly try other actions with probability epsilon. This helps us discover better actions we might have missed early on.

**alpha: learning rate**
Controls how much we update the Q-value based on new information. Higher values is bigger updates to Q-values.

Epsilon controls which actions we try (explore).

Alpha controls how strongly each experience influences learning. 

\[
Q(s, a) \leftarrow Q(s, a) + \alpha \cdot \big[ \text{target} - Q(s, a) \big]
\]

