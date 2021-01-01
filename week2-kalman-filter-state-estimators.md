# Content

- history
- linear Kalman Filter LKF
- LKF as the BLUE
- extending LKF to nonlinear systems via linearization
- limitations of linearization
- unscented Kalman filter

# Motivation

While recursive least squares updates the estimate of a static parameter,  the Kalman filter is able to *update* and *estimate* of an evolving state 

# Notation

![image-20201224212752067](/home/jonasmmiguel/.config/Typora/typora-user-images/image-20201224212752067.png)

# (Linear) Kalman Filter

The linear KF is a recursive LS estimator that also includes **a motion model**, which tells us how the state evolves over time

We can think of the KF as a technique to **fuse information** from different sensors to produce a final estimate of some unknown state

## Estimating the state in 2 stages: predict & correct

1.  **Predict** the state k via our process/**motion model**

2.a. Compute the optimal gain

2.b. **Correct** our estimate via the **measurement model**

![image-20201224214054082](/home/jonasmmiguel/.config/Typora/typora-user-images/image-20201224214054082.png)



![image-20201224214142059](/home/jonasmmiguel/.config/Typora/typora-user-images/image-20201224214142059.png)

(control) input:  an external signal that affects the evolution of our system state

- e.g. a wheel torque applied to speed up and change lanes 

![image-20201224214205592](/home/jonasmmiguel/.config/Typora/typora-user-images/image-20201224214205592.png)

![image-20201224214249357](/home/jonasmmiguel/.config/Typora/typora-user-images/image-20201224214249357.png)

## A Short Example

<img src="/home/jonasmmiguel/.config/Typora/typora-user-images/image-20201224214710491.png" alt="image-20201224214710491" style="zoom:50%;" />

<img src="/home/jonasmmiguel/.config/Typora/typora-user-images/image-20201224214723608.png" alt="image-20201224214723608" style="zoom:50%;" />

<img src="/home/jonasmmiguel/.config/Typora/typora-user-images/image-20201224214736316.png" alt="image-20201224214736316" style="zoom:50%;" />

<img src="/home/jonasmmiguel/.config/Typora/typora-user-images/image-20201224214751001.png" alt="image-20201224214751001" style="zoom:50%;" />

<img src="/home/jonasmmiguel/.config/Typora/typora-user-images/image-20201224214809031.png" alt="image-20201224214809031" style="zoom:50%;" />

<img src="/home/jonasmmiguel/.config/Typora/typora-user-images/image-20201224214820290.png" alt="image-20201224214820290" style="zoom:50%;" />



# Kalman Filter: the Best Linear Unbiased Estimator (BLUE)

Meaning: 

- the KF is the best among all linear filters providing an unbiased, consistent estimation

Meaning:

​	$\checkmark$ given white uncorrelated zero mean noise

​	$\therefore$ the KF estimator produces (1) **unbiased** estimates with (2) the minimum possible variance (**consistent**)



​	Unbiased because:

- $\mathbb{E}[\mathbf{\hat{e}}_{k|k}] = 0$
- i.e. $\mathbb{E}[\mathbf{{x}}_{k}-\mathbf{\hat{x}}_{k|k}] = 0$
- i.e. the expected posterior error converges to zero, over sufficient $k$ "predict-measurement-correction" steps

​     Consistent because:

- $\mathbb{E}[\mathbf{\hat{e}}_{k|k}^2] = \hat{\mathbf{P}}_k$
- i.e. (for a scalar state) the filter reports a variance $\hat{\mathbf{P}}_k$ matching* the empirical variance of our estimate $\mathbb{E}[\mathbf{\hat{e}}_{k|k}^2]$
  - i.e. converging over sufficient $k$ measurements
- i.e. the filter is neither overconfident (optimistically reports smaller covariances than the empirical variance our our estimate) nor underconfident





# Extended Kalman Filter (EKF)

- this is sort of a bootstrap method
- we linearize the nonlinear system around the Kalman filter estimate, and the Kalman filter estimate is based on the linearized system

# Recommended Resources

|                                                   |                                                              |
| ------------------------------------------------- | ------------------------------------------------------------ |
| illustrative, engaging intro to LKF (blog post)   | https://www.bzarg.com/p/how-a-kalman-filter-works-in-pictures/ |
| Extensive & detailed review of KF in chp 3 sec 3  | [Timothy D. Barfoot, State Estimation for Robotics (2017)](http://asrl.utias.utoronto.ca/~tdb/bib/barfoot_ser17.pdf): chp 3 sec 3 |
| KF: intuitive derivation, properties, limitations | [Timothy D. Barfoot, State Estimation for Robotics (2017)](http://asrl.utias.utoronto.ca/~tdb/bib/barfoot_ser17.pdf): chp5 |
| Detailed review of KF in chp 5 sec 1              | [Dan Simon, Optimal State Estimation (2006)](https://onlinelibrary.wiley.com/doi/book/10.1002/0470045345), chp 5 sec 1 |
| Compilation of great resources on KF              | https://www.cs.unc.edu/~welch/kalman/                        |
| LKS seminal paper                                 | https://www.cs.unc.edu/~welch/kalman/kalmanPaper.html        |

