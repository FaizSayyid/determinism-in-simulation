## Should simulations always incorporate stochasticity?
There are many forms of simulation in the compuational community. One way to classify them is to divide them into stochastic and deterministic: that is given the same input, should the model always produce the same output? In this short article we discuss this question and the situations in which each approach is appropriate. 

## Simulation
What is the point of a simulation? The goal of a simulation is to answer a question by constructing a model of the system under study. An ideal simulation should capture the *essential* mechanics relevant to the question at hand without containing extraneous computational or mathematical details. If we capture too few details then the simulation will provide misleading answers. On the other hand if we include too many we risk modelling components that have no impact upon the question being studied. The art of simulation is therefore to capture the essentials of the system, acknowledging that smaller effects will neglected but hoping that their omission will not too matter too much. In general, broader questions can be addressed with simpler, more abstract simulations, whereas specific or fine-grained questions typically require greater simulation detail.

A tempting question to ask is if we could construct a simulation in which could perfectly predict the future.

## Laplace's Demon

> We may regard the present state of the universe as the effect of its past and the cause of its future. An intellect which at a certain moment would know all forces that set nature in motion, and all positions of all items of which nature is composed, if this intellect were also vast enough to submit these data to analysis, it would embrace in a single formula the movements of the greatest bodies of the universe and those of the tiniest atom; for such an intellect nothing would be uncertain and the future just like the past could be present before its eyes.

— Pierre Simon Laplace, A Philosophical Essay on Probabilities (1814)

In 1814 Pierre Simon Laplace presented an argument for causal determinism.The passage above, later known as Laplace’s Demon, posits that if we knew all hidden variables with infinite precision and all relevant laws of nature we could, in principle, predict all future states of the universe. There are three main criticisms of this argument

1. Thermodynamic argument: many processes in nature are irreversable and hence previous states cannot be reconstructed from the current state. Laplace's demon would seem to require universal reversibility.
2. Quantum mechanical argument: under some interpretations (but not all) of quantum mechanics many quantites are fundamentally stochastic
3. The Chaos Theory Argument: small changes in initial conditions can result in vastly different outcomes. An obvious riposte to this argument is that an all knowing demon *would* know all initial conditions with infinite precision.

## How to make a realistic deterministic simulation
With the arguments against Laplace's demon laid out we can immediately identify some situations in which simulations must incorporate stochasitcity. 
1. When there is quantum mechanical uncertainty. e.g. simulating radioactive decay. This is a classic example in which we know, in expectation, how many decay events occur per unit time but we have no idea exactly which atoms will undergo decay
2. Irrevserable thermodynamic simulations

With those out of the way let us consider the less clear cut cases. Recall our criteria for a fully deterministic simulation we need:

1. To know all hidden variables and
2. To known them with infinite precision 

### Hidden variables
Let's say we are trying to simulate the height of a human. There's obviously a wide range of acceptable heights. If this simulation were deterministic and only returned say, 2 meters then we would regard this as a pretty poor model. 

An obvious improvement would be to specify the sex of the individual who's height we are trying to simulation. In probabality we call this conditioning, we change the model from $p(x)$ to $p(x|sex)$ e.g. $p(x) = N(u_men,\sigma_men)$. This reduces the variance in outcomes because we have incorporated a known causal factor.  We could go further we could add additional inputs to our simulation (or condition on) the heights of the parent:

 $p(x|sex,father_height, mother_height)$.

There will still be variance in this outcome; siblings of the same sex can still differ in height but adding this variable to our simulation further shrink the variance of outcomes. We could imagine continuing to condition upon relevant variables, each time the variance in outcomes would shrink. In the limit, if we could condition upon all relevant variables and we understood the mechanics of all those variables, then the distribution of the outcomes would collapse to a delta function (a spike with no variance), assuming no quantum mechancial or irreversable processes. The more variables we have, the more deterministic the simulation becomes.

In practice this isn't possible as we don't know all hidden variables nor do we know how they interact. In such cases one way to interpret the distribution over outcomes is to think of marginalisation over the hidden causes. For example if we assume that we can't know the conditions in the womb of then we can represent their influence as a sample from a distribution over womb conditions.



### The resulotion of hidden variables
As previously discussed in some systems a very small change in input can produce a very large change in output. If we can measure the variables with infinite precision (as a laplace's demon could) then there would be no variance in outcome. In reality computers have a limited amount of memory and as such cannot represent varaibles with infite precision even if they are known. Again with stochasticity we can attempt to marginalise over the uncertainty in precision. On the other hand if we presume that variables that we wish to model are integer valued or occupy a very small space then this stochasiticty isn't necessary.

### Summary
A deterministic simulation is plausible only when:

1. All relevant hidden variables are known  
2. and measured with sufficient precision, and
3. The system itself is not inherently stochastic (e.g. quantum or thermodynamic processes).

In every other case, stochasticity serves as a principled way to represent our ignorance about the system’s unobserved states or limited numerical resolution.

## Realism vs reproducability

In regulated domains such as pensions, healthcare, or insurance, it is often necessary to perfectly reproduce past results for audit purposes even when the underlying system is fundamentally stochastic. The challenge is therefore to design simulations that remain principled (accurately representing uncertainty) while being bitwise identical to the past.

There are several ways to trade off realism against reproducibility:

1. Ask broader questions rather than narrow ones.
Broad, coarse questions may be answered accurately with simpler simulations. Simpler simulations depend on fewer variables, making it feasible to condition on all of them obviating the need to marginalize over ignorance. This shifts the model closer to determinism without misrepresenting the underlying system.

2. Frame questions around discrete or low precision variables.
If the conditioning variables are integers or limited to coarse precision, the system’s state space is small. That makes it practical to enumerate or fix all relevant states, producing deterministic outputs. For example, modelling yearly transitions between discrete health states is far easier to audit than modelling continuous variables such as blood pressure.

3. Separate uncertainty from computation.
When stochasticity is truely necessary, treat the random seed as an explicit input to the simulation. 


