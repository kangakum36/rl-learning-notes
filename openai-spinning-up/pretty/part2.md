# Part 2 - Kinds of RL algorithms

## Intro

* Modularity of RL algorithms is not well-represented by a tree structure. Some advanced material (exploration, transfer learning, meta learning) is omitted. Goals are to highlight foundational design choices, expose trade offs within them, and place modern algorithms into context.

## Model-Free vs Model-Based RL

* Model-Based means agent has access to or learns a model of the environment, which is a function that predicts state transitions and rewards.
* The upside of models is that they allow the agent to plan by thinking ahead, seeing what would happen for a range of choices. These results can be distilled into a learned policy.
* Downside is that a ground truth model of the environment is usually not available, so the agent has to learn the model purely from experience in such cases. Biases in the model can and will be exploited by the agent, resulting in an agent which performs well wrt the model but terribly in real life.
* Model free methods are more popular.

## What to learn

* Algorithms learn different things. Usually, is either a policy, action-value function, value function, or environment model(s). I'm guessing that most Deep RL (e.g. PPO, GRPO) are going to learn a policy by adjusting params in an NN.
* Turns out deep RL also learns the value function which is used to figure out how to update.

## What to learn in Model-Free RL

* Two main approaches to learning are Policy Optimization and Q-Learning.
* Policy Optimization (where I will spend my time) represents the policy as $\pi_{\theta}(a | s)$ and optimizes the params $\theta$ either directly by gradient ascent on $J(\pi_{\theta})$ or indirectly, by maximizing local approx of $J(\pi_{\theta})$.
* The optimization is performed **on-policy**, so each update only uses data collected while acting according to the most recent version of the policy.
* Also usually involves learning an approximator $V_{\phi}(s)$ for the on-policy value function $V^{\pi}(s)$, which gets used in figuring out how to update the policy. I guess if you can plug the params of the policy and info about current state into such a function, you can figure out expected return and then gradient ascent to get direction?
* Basically the policy gradient theorem says that to calculate the gradient of the objective function, in order to perform gradient ascent on the neural network, we need to compute the log probability and the advantage. Theoretically, computing the advantage requires the On policy action value function and the on policy value function. But with some algebra and a bit of handwaving/sampling, we can do this with just an approximation of the on policy value function at each step. That is why we need $V_{\phi}(s)$

## Q-Learning

* Learn an approximator $Q_{\theta}(s, a)$ for the optimal action-value function. They use an objective function based on bellman equation. Not going to take a ton of notes here because I'm focusing on PPO/GRPO.
* Policy optimization is more stable because it directly optimizes for $J$ the performance objective. Q learning indirectly optimizes for agent performance, so there are more failures modes. However Q learning is more sample efficient.
* They can sometimes be equivalent and there are algorithms between the extremes.

## What to Learn in Model-Based RL

* Not going to take a ton of notes here because I want to focus on PPO/GRPO
* Background - Pure Planning. Most basic approach, never explicitly represents policy, and uses pure planning techniques like model-predictive control (MPC).
* Expert Iteration - Straightforward follow on to pure planning where we learn an explicit representation of the policy. The planning algorithm is the expert, and its output is used to update the policy to produce an action more like the planning algorithm's output.
* Data augmentation for model-free methods. Basically augment the model-free RL algorithm with fictitious experiences.
* Embed planning loops into policies. Embed the planning procedure into the policy as a subroutine. The complete plans are side information, so the policy can choose how and when to use the plan. If the model is bad in some states the policy can learn to ignore it.
