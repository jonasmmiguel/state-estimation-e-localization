# Content

The method of least squares
- history
- ordinary LS
- weighted LS
- recursive LS
- relation to maximum likelihood  

# 1. Ordinary & Weighted Least Squares Estimator

OLS / WLS: Parameter Estimators, i.e. methods for estimating (static) parameters

["(static) parameters" = variables of interest that remain (mostly) constant from the first to the last measurement] 

## History

Gauss: "The *most probable* value of an unknown parameter is that which *minimizes the sum of squared errors* between what we observe and what we expect".

## The Ordinary Least Squares

A.k.a. 

- Regular Least Squares method
- Ordinary Squared Error Criterion

- the **squared error loss function** $\mathscr{L}_{LS}$

Example: determining the [most plausible] value of a resistance for a given resistor at hand, with known nominal resistance $x$ and tolerance (e.g. 5%)

![image-20201223182534702](/home/jonasmmiguel/.config/Typora/typora-user-images/image-20201223182534702.png)

We assume that our **noisy measurement (vector)** $y = Hx + v$, where the noise $v \sim \mathcal{N}(\boldsymbol{0}, \boldsymbol{R})$, with $\boldsymbol{R} = \begin{bmatrix}\  \sigma^2 \cdots \  0 \\ \ddots \\ 0 \  \cdots \ \sigma^2 \end{bmatrix}$ 

- i.e. the measurement vector $\boldsymbol{y} = y_{1...m}$ is some linear combination of the parameters ground truth $\boldsymbol{x} = x_{1...n}$, with additive, i.i.d. zero-mean Gaussian measurement noise.

- which implies

  1. linear measurement model

  2. **i.i.d.** [^1], zero-mean, additive, Gaussian noise 

     - i.e. measurements noises are random variables mutually independent, being normally distributed, and having all the same mean (zero) and the same variance ($\sigma ^2$)
     - mutually independent $\implies$ zero (i.e. no) covariance btw. any pair of the $m$ measurements instances  

     [^1]: i.i.d. (independent and identically distributed) means all samples (i.e. measured instances) stem from the same generative process and that this generative process has no memory of past generated samples. This assumption underlies the classical form of the Central Limit Theorem, which states that the probability distribution of the sum (or average) of i.i.d. variables with finite variance approaches a normal distribution.

     

$\hat{x}_{LS} = argmin_x\sum_{i=1}^m e_i^2 = argmin_x\mathscr{L}_{LS}(x) $

- $e_i^2 = (y_i - x)^2$
  - $y_i$: $i$-th measurement 
  - $x$: expected value (e.g. nominal value) 

Let's express this loss function in a vectorial form, for the general case of $n$ parameters ($\boldsymbol{x}_{n\times1}$)

$\mathscr{L}_{LS}(x) = \boldsymbol e^\intercal \boldsymbol e$

$\boldsymbol{e} = \begin{bmatrix} e_1 \\ e_2 \\ e_3 \\ e_4\end{bmatrix} = \begin{bmatrix} y_1 \\ y_2 \\ y_3 \\ y_4\end{bmatrix} - \begin{bmatrix} 1 \\ 1 \\ 1 \\ 1 \end{bmatrix} x$

$\phantom{\boldsymbol{e} = \begin{bmatrix} e_1 \\ e_2 \\ e_3 \\ e_4\end{bmatrix}} = \boldsymbol{y} - \boldsymbol{H}\boldsymbol{x}$

- $\boldsymbol{H}_{m\times n} \triangleq Jacobian\ Matrix $ (relates the parameters to the measurements)
  - $m \triangleq \#\  measurements$
  - $n \triangleq \# \  estimated\ pars$
- $\boldsymbol{x_{n \times 1}} \triangleq \ parameters \ vector\  (general\ case)$

$\therefore \mathscr{L}_{LS}(x) = \boldsymbol e^\intercal \boldsymbol e = \boldsymbol{y}^\intercal\boldsymbol{y} - \boldsymbol{y}^\intercal\boldsymbol{H}\boldsymbol{x}-\boldsymbol{x}^\intercal\boldsymbol{H}^\intercal\boldsymbol{y} + \boldsymbol{x}^\intercal\boldsymbol{H}^\intercal\boldsymbol{H}\boldsymbol{x}$  

$\therefore \hat{x}_{LS} = argmin_\boldsymbol{x}\mathscr{L}_{LS}(\boldsymbol{x}) = (\boldsymbol{H}^\intercal\ \boldsymbol{H} )^{-1} \boldsymbol{H}^\intercal\boldsymbol{y}$

which has a unique solution if and only if H is not singular, i.e. $m \ge n$, i.e. *we have equal or more measurements than unknown parameters*

<img src="/home/jonasmmiguel/Downloads/notes_01-Copy.jpg" alt="notes_01-Copy" style="zoom:20%;" />

In the resistor example above, we find out that the $\hat{x}_{LS} = \bar{y}$. That is, for measurements of the form $y = x + v, with\  noise\ v \sim \mathcal{N}$, the least squares solution is simply the average of the measurements. 

| assumption                                                   | limitation                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| measurement model is linear $y = x + v, with\  noise\ v \sim \mathcal{N}$ (also implies measurements are linearly i.i.d.) | cannot express systems with significant non-linearities      |
| measurements have all the same weight (importance) when calculating the error (loss function) | limited suitability for handling measurements from different sensors, each having significantly different accuracies (noise levels) |
| $m\ge n$, i.e. the number of measurements available $m$ is greater than the number of parameters $n$ that we are trying to estimate | –                                                            |

Note: we don't have to know the noise variance $\sigma^2$

**Next Step**: what if we have more confidence in some measurements than others? How to get information from less reliable measurements without compromising the overall estimate? 

![image-20201223231739351](/home/jonasmmiguel/.config/Typora/typora-user-images/image-20201223231739351.png)



## Weighted Least Squares

We modify our assumptions in OLS, so that the variance of the measurement noise may be different for every measurement (i.e. for every $y$): 

$\boldsymbol{R} = \begin{bmatrix}\  \sigma_1^2 \cdots \  0 \\ \ddots \\ 0 \  \cdots \ \sigma_m^2 \end{bmatrix}$ 

- notice: $\boldsymbol{R}_{m\times m}$

The loss function then becomes:

$\mathscr{L}_{WLS}(\boldsymbol{x}) = \boldsymbol{e}^\intercal R^{-1}\boldsymbol{e}$

$\phantom{\mathscr{L}_{WLS}(\boldsymbol{x})} = {e^2_1}/{\sigma^2_1} + {e^2_2}/{\sigma^2_2} + \cdots + {e^2_m}/{\sigma^2_m}$

- notice: the higher the expected noise, the lower the weight we place on the measurement 

The minimization of the loss function then leads to:

$\hat{\boldsymbol{x}}_{WLS} = argmin_\boldsymbol{x}\mathscr{L}_{WLS}(\boldsymbol{x}) = (\boldsymbol{H}^\intercal\  \boldsymbol{R}^{-1}\boldsymbol{H} )^{-1} \boldsymbol{H}^\intercal \boldsymbol{R}^{-1} \boldsymbol{y}$



![image-20201224000446992](/home/jonasmmiguel/.config/Typora/typora-user-images/image-20201224000446992.png)

![image-20201224000841042](/home/jonasmmiguel/.config/Typora/typora-user-images/image-20201224000841042.png)

| assumption                                                   | limitation                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| measurement model is linear $y = x + v, with\  noise\ v \sim \mathcal{N}$ (also implies linearly i.i.d. measurements) | cannot express systems with significant non-linearities      |
| $m\ge n$                                                     | we must have more measurements than variables of interest (state variables) |
| $\sigma_i\ge 0$                                              | ($\sigma_i$ must be known beforehand)                        |

**Summary**

![image-20201224000921861](/home/jonasmmiguel/.config/Typora/typora-user-images/image-20201224000921861.png)

Both solutions address tasks where measurements are entirely acquired beforehand [i.e. batch acquisition].

## Exercise Notes

### Python syntax

| elementwise multiplication | matrix multiplication |
| :------------------------: | :-------------------: |
|           A * B            |         A @ B         |

### How to define the Jacobian $\boldsymbol{H_{m \times n}}$

$\boldsymbol{y}$ is the measurements vector, and comprises the quantities actually measured

$\boldsymbol{x}$ is the state vector, comprising the quantities of interest

The Jacobian Matrix: $\boldsymbol{H}_{m \times n} = \begin{bmatrix} \partial_{x_1}{y}|_1\  \partial_{x_2}{y}|_1 \cdots \partial_{x_n}{y}|_1 \\  \partial_{x_1}{y}|_2\  \partial_{x_2}{y}|_2 \cdots \partial_{x_n}{y}|_2 \\ \vdots \\  \partial_{x_1}{y}|_m\  \partial_{x_2}{y}|_m \cdots \partial_{x_n}{y}|_m \end{bmatrix}$

- where $\partial_{x_n}{y}|_m$ is the partial derivative of the measured variable $y$ in the $m$-th measurement by the $n$-th VoI (i.e. state variable) $x$
- it contains the partial derivatives of the observations to the state variables (VoI's)
- it expresses how a given measurement is expected to vary as each underlying state variable changes
- a priori, we can only calculate the Jacobian if  we know an explicit relationship btw. $y_i$ and $x_i$ (e.g. a physical law)

#### Example

- we want to estimate the resistance $R$ of an resistor ($x_{1 \times 1} = R$, VoI), based on $m$ observations (measurements) of $(V_i, I_i),  i = 1..m$ (and using Ohm's Law $V = RI$)
- we could define $y_i = V_i$
    - in this case $\boldsymbol H_{m \times 1} = \begin{bmatrix} \partial_{R}{V}|_1  \\  \partial_{R}{V}|_2\  \\ \vdots \\  \partial_{R}{V}|_m\  \end{bmatrix} =  \begin{bmatrix} I_1 \\ I_2 \\ \vdots \\ I_4  \end{bmatrix}$  
- alternatively, we could define $y_i = V_i / I_i$
    - here, $\boldsymbol H_{m \times 1} = \begin{bmatrix} \partial_{R}{(V/I)}|_1  \\  \partial_{R}{(V/I)}|_2\  \\ \vdots \\  \partial_{R}{(V/I)}|_m\  \end{bmatrix} =  \begin{bmatrix} 1 \\ 1 \\ \vdots \\ 1  \end{bmatrix}$ 

**Next Step:** what if instead of computing an optimal estimate based on a batch of measurements, we wanted to do it based on a *stream* of measurements? 

- E.g. because we want estimations on basis of measurements updated real-time
- e.g. because we want the computational cost of our re-estimation to be independent of how many measurements are available

# 2. Recursive Least Squares Estimator

![image-20201224132552227](/home/jonasmmiguel/.config/Typora/typora-user-images/image-20201224132552227.png)

Key stepstones in reformulating the OLS into a *recursive* procedure

- consider the $k$-th measurement 
- $\mathbf{R}_k$ quantifies the measurement noise, being basically a covariance matrix for our measurements
- $\mathbf{P}_k = \mathbb{E}[\mathbf{e}^\intercal \mathbf{e}]$ quantifies the uncertainty in our estimate, being basically a covariance matrix for our estimates
- the variances in the covariance matrix diagonal tends to shrink as new measurements are available, thus $Tr(\boldsymbol{P})\rightarrow minimum$ as $k\rightarrow \infty$
- the **innovation** term $(\boldsymbol{y}_k-\boldsymbol{H}_{k-1}\boldsymbol{\hat{x}}_{k-1})$, which quantifies the how much information was brought in the last measurement 
  - aka. **correction term**
- the **estimator gain matrix** $\boldsymbol{K}_{k}$, which basically prescribes how much importance we give to the measurement $k$ in relation to previous ones for computing the estimate
  - the expression for $\mathbf{K}_k$ is obtained by minimizing $Tr(\mathbf{P}_k)$: the sum of the variances of the estimation errors at time $k$
  - this is in accordance with the idea that the 
- the **linear recursive update** rule: $\boldsymbol{\hat{x}}_{k} \leftarrow \boldsymbol{\hat{x}}_{k-1} + \boldsymbol{K}_{k}(\boldsymbol{y}_k-\boldsymbol{H}_{k-1}\boldsymbol{\hat{x}}_{k-1})$
  - note: if either the innovation or the gain is zero, then new measurement effectivelly does not change our estimate

Overall procedure

1. Initialize the estimator

   $\mathbf{\hat{x}}_0 = \mathbb{E}[\mathbf{{x}}]$

   $\mathbf{P}_0 = \mathbb{E}[(\mathbf{{x}}-\mathbf{\hat{x}}_0)(\mathbf{{x}}-\mathbf{\hat{x}}_0)^\intercal]$

2. Set up the measurement model, defining the Jacobian an the measurement covariant matrix:

   $\mathbf{y}_k = \mathbf{H}_k\mathbf{x} + \mathbf{v}_k$

3. Update the estimate of $\mathbf{\hat{x}}_k$ and the covariance $\mathbf{P}_k$ using:

   |      |                                |                                                              |
   | ---- | ------------------------------ | ------------------------------------------------------------ |
   | 1.   | Calculate the gain term        | $\mathbf{K}_k = \mathbf{P}_{k-1}\mathbf{H}_k^T\left(\mathbf{H}_k\mathbf{P}_{k-1}\mathbf{H}_k^T + \mathbf{R}_k\right)^{-1}$ |
   | 2.   | Update the parameter estimate  | $\hat{\mathbf{x}}_k = \hat{\mathbf{x}}_{k-1} + \mathbf{K}_k\left(\mathbf{y}_k - \mathbf{H}_k\hat{\mathbf{x}}_{k-1}\right)$ |
   | 3.   | Update the covariance estimate | $\mathbf{P}_k = \left(\mathbf{I} - \mathbf{K}_k\mathbf{H}_k\right)\mathbf{P}_{k-1}$ |

# 3. Why squared loss?

minimizing the squared error...

- amounts to solving a linear system of equations (convenient), for $\checkmark$ a linear measurement model
- is equivalent of maximizing the likelihood of estimators (i.e. equivalent to the Maximum Likelihood Estimate), for $\checkmark$ an i.i.d., additive, Gaussian measurement noise

<img src="/home/jonasmmiguel/.config/Typora/typora-user-images/image-20201224193835201.png" alt="image-20201224193835201" style="zoom: 67%;" />

# 4. Main Limitation of Least Squares method

**Sensitivity to outliers**

- measurement outliers can significantly effect our estimates

- why: MLE/LS puts significant importance to outliers

  - how: the estimate distribution will be skewed so that the outlying measurement is more likely

    [also: the computation of the gain $\mathbf{K}$ is not robust to outliers]

Note: modeling measurement noise as Gaussian is ok, for example, when there are various independent sources of noise (see Central Limit Theorem) 

# (Filtered) Recommended Resources

- [Dan Simon, Optimal State Estimation (2006)](https://onlinelibrary.wiley.com/doi/book/10.1002/0470045345) Chapter 3

-  [interactive explanation](http://mfviz.com/central-limit/) of the Central Limit Theorem by Michael Freeman

  - as the number of samples  taken[^2]increases, the distribution of sample means [^3] becomes Gaussian.

    [^2]: not instances / observations!
    [^3]: aka sampling distribution

  - why it matters: allows us (1) to attain reliable estimates of population values; (2) to quantify the confidence in our estimates 

    

# Other interesting resources I found

|                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| graduate-level statistics                                    | https://online.stat.psu.edu/stat414/lesson/introduction-stat-414 |
| how to systematically draw e.g. 1000 random numbers that follow a particular probability distribution? | https://online.stat.psu.edu/stat414/lesson/22/22.4           |

 