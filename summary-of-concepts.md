# Filtering vs Smoothing vs Prediction

Problems involving estimating the true value of an observed variable that evolves in time:

- **filtering**: estimate the <span style="color:orange">*current*</span> value given <span style="color:blue">*past*</span>  and <span style="color:orange">*current*</span>  observations
- **smoothing**: estimate <span style="color:blue">*past*</span> values given past and <span style="color:orange">*current*</span>  observations
- **prediction**:  when estimating a probable <span style="color:green">*future*</span>  value given past and <span style="color:orange">*current*</span>  observations

[¹]: https://en.wikipedia.org/wiki/Recursive_Bayesian_estimation#Sequential_Bayesian_filtering

# { Recursive Bayes Estimation, Bayes Filter }

- a general probabilistic aproach for estimating an unknown PDF recursively over time using incoming measurements and a mathematical process model

- assumes $\checkmark$ the measurements $\mathbf{z}_k$ are observations of a Hidden Markov Model

  - Hidden Markov Model: the true state $\mathbf{x}$ is an {unobseved, hidden} Markov process

    ![Hidden Markov model](https://upload.wikimedia.org/wikipedia/commons/thumb/8/81/HMM_Kalman_Filter_Derivation.svg/466px-HMM_Kalman_Filter_Derivation.svg.png)

# Sequential Bayesian Filtering

An extension of the Bayes filter for when the true state $\mathbf{x}$ evolves (changes) over time

- e.g. Kalman Filter, {Particle Filter, Sequential Monte Carlo}, Grid-based Estimators

![simon2006optimal](/home/jonasmmiguel/.config/Typora/typora-user-images/image-20210101152620807.png)

# (Linear) Kalman Filter

- a recursive linear squared error estimator
- a Bayesian filter for (often multivariate) $\checkmark$ normal distributions with $\checkmark$ linear transitions

# Time Series

a *discrete* set of time-indexed observations

# Stochastic Process

- a family of random variables
- aka **random process** = random function
- if multiple random variables: **random field**

- continuous by nature
- can often be represented by time series

# State

[value assumed by a given set of random variables]

# State Space

the Euclidean space where each dimension (axis) is a state variable

# (State) Transition

change of system's state 

# Markov property (of a stochastic process) & Markov assumption

- the **Markov property** = memorylessness $\triangleq$ [future states depend only on current state]

- the **Markov assumption** $\triangleq$ the system at hand is modeled as memoryless

  

- a {Markov, Markovian} process $\triangleq$ a stochastic process following this property 
  
  - e.g. a Markov Chain

# Markov Chain

- a stochastic model describing *a sequence of possible events*

  in which the probability of each event depends only on the state attained in the previous event

- a Markov Chain defines a probabilistic transition model $T(x\rightarrow x')$ over states $x$

- useful for generating samples from a desired target distribution which might be intractable to sample from directly