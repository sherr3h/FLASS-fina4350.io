---
layout: post
title: Kalman Filter on Simulated GPS Data
gh-repo: sherr3h/sherr3h.github.io
tags: [Kalman Filter, Statistics, Time Series]
comments: true
---


Kalman filter is a Bayesian method to update and improve parameter estimates from observations with uncertainty. It can be applied to time series formulated as a Linear Gaussian State Space Model. The kalman filter method was used by NASA in the Apollo Project to estimate trajectories of the manned spacecraft landing on the Moon and back (wow!). 

In this and the [next post](https://sherr3h.github.io/2023-02-12-Kalman-Filter-on-Electricity-Prices-and-GPS-Data-2/), I will apply Kalman filter to two time series datasets: (1) simulated GPS locations (in honour of the Apollo 11 Lunar Module), and (2) a power market electricity price dataset.

## Problem Formulation

Assumption: all noises are Gaussian, in which case the Kalman filter minimises the mean square error of estimated parameters. If the noises are not Gaussian, given only the mean and standard deviation, the Kalman filter is the best linear estimator.
 
The State Space Model, with observations  \\(Y_t \\) and hidden states \\(X_t\\):

<p>
\begin{align}
 Y_t &amp;= H_t X_t + \epsilon_t, \epsilon_t \sim N(0, R_t) \\
 X_{t+1} &amp;= A_t X_t + \eta_t, \eta_t \sim N(0, Q_t)
 \end{align}
 </p>

where \\(R_t\\) is the measurement error covariance matrix and \\(Q_t\\) is the process error covariance matrix.

Using conclusions from the marginal and conditional distributions of multivariate Gaussians, and let $$X_{t+1\lvert t}$$ be the predicted latent state with predicted mean \\(\mu_{t+1\lvert t}\\) and predicted covariance \\(\Sigma_{t+1\lvert t}\\), the Kalman filter algorithm:

- At time 0, initial guess is \\(X_0 \sim (\mu_{0\lvert 0}, \Sigma_{0\lvert 0}) \\)
- At time t, 
    - Prediction updates:
    <p>
    \begin{align}
    \mu_{t+1|t} &amp; = A_t \mu_{t|t} \\ 
    \Sigma_{t+1|t} &amp; = A_t \Sigma_{t|t}A_t ^T + Q_t 
    \end{align}
    </p>
    
    - Measurement updates:
    <p>
    \begin{align}
      K_{t+1} &amp;= \Sigma_{t+1|t}H_{t+1}^T \left(H_{t+1} \Sigma_{t+1|t}H_{t+1}^T + R_{t+1}  \right)^{-1}, \text{ the Kalman gain} \\
      \mu_{t+1|t+1} &amp;= \mu_{t+1|t} + K_{t+1} \left(Y_{t+1} - H_{t+1}\mu_{t+1|t} \right)  \\
                    &amp; =\left(I -  K_{t+1}H_{t+1}\right)\mu_{t+1|t} + K_{t+1} Y_{t+1}, \text{ the Innovation} \\
      \Sigma_{t+1|t+1} &amp; = \left(I -  K_{t+1}H_{t+1}\right) \Sigma_{t+1|t} 
       \end{align}
    </p>
   
## Simulated Race Car Location Data 

Suppose a race car is on a track with the following equations:

$$x=2\cos(t), y=\sin(3t), t\ge 0$$

 <img src="/img/simulate_motion.jpg" width="700" >

Suppose unfortunately the car jerks when accelerating (such that the acceleration is not constant), we consider the third order derivatives of the car's position as Gaussian variables $$a_x \sim N(0, \sigma_x^2), a_y \sim N(0, \sigma_y^2)$$.


By the Newton's law of motion,

<p>
\begin{align}
x(t+ \delta t) &amp;=x(t) + \delta t \cdot \dot{x}(t) + \frac{\delta t^2}{2} \ddot{x}(t)+ \frac{\delta t^3}{6} a_x \\

 \dot{x}(t+ \delta t) &amp;= \dot{x}(t) + \delta t \cdot \ddot{x}(t) +  \frac{\delta t^2}{2} a_x \\

\ddot{x}(t+ \delta t) &amp;=  \ddot{x}(t) + \delta t \cdot a_x
\end{align}
</p>

The dynamics are similar for dimension $$y$$.

Let the car carry a GPS device to observe its current physical position, with measurement error following $$\epsilon \sim N\left(0, \sigma_{GPS}^2 \right)$$ on both dimensions. The observations might look like this:

 <img src="/img/simulate_gps.png" width="400" >

Applying Newton dynamics to Kalman Filter, the state and observation vectors contain up to second order derivatives of the position (note $$x,y$$ are position on the 2D plane, and $$X, Y$$ are the hidden state and observation of the car's GPS position):

<p>
\begin{align}
X &amp;= \begin{bmatrix}x & \dot{x} & \ddot{x} & y & \dot{y} & \ddot{y}\end{bmatrix}^T \\
Y &amp;= \begin{bmatrix}x_{obs} & y_{obs}\end{bmatrix}^T 
\end{align}
 \[
A_t = \begin{bmatrix}
    1& \delta t& \frac{\delta t^2}{2}& 0& 0& 0\\
    0& 1& \delta t& 0& 0& 0\\
    0& 0& 1& 0& 0& 0\\
    0& 0& 0& 1& \delta t& \frac{\delta t^2}{2}\\
    0& 0& 0& 0& 1& \delta t\\
    0& 0& 0& 0& 0& 1\\
\end{bmatrix}
 \]
</p>


The process error matrix covariance $$Q$$ is 

<p>
\begin{align}
Q&amp;=Var(\begin{bmatrix}\frac{\delta t^3}{6}a_x & \frac{\delta t^2}{2} a_x & \delta t \cdot a_x & \frac{\delta t^3}{6}a_y & \frac{\delta t^2}{2} a_y & \delta t \cdot a_y\end{bmatrix}^T)\\
 
 &amp;= \sigma_x^2 uu^T + \sigma_y^2 vv^T 
\end{align}
</p>


where $$u = \begin{bmatrix}\frac{\delta t^3}{6}a_x & \frac{\delta t^2}{2} a_x &\delta t \cdot a_x &0&0&0\end{bmatrix}^T, v = \begin{bmatrix}0&0&0& \frac{\delta t^3}{6}a_y & \frac{\delta t^2}{2} a_y &\delta t \cdot a_y\end{bmatrix}^T$$


The GPS observes hidden states directly, so

<p>
 \[
H = \begin{bmatrix}
1&0&0&0&0&0\\
0&0&0&1&0&0\\
\end{bmatrix}
\]
</p>
    
and the measurement error covariance matrix $$R$$ is 

<p>
 \[
    R = \begin{bmatrix}
\sigma_{GPS}^2&0\\
0&\sigma_{GPS}^2\\
\end{bmatrix}
\]
</p>
    


At time 0, suppose the car starts at the rightmost race track location (2,0) with known velocity and acceleration, and the hidden state covariance is a relatively large value due to uncertainty. 


<p>
\begin{align}
X_0 &amp;= \begin{bmatrix}2 & 0& 2&  0&  3& 0\end{bmatrix}^T \\

\Sigma_0 &amp;= 0.5 I_{6\times 6}
\end{align}
</p>
    
Here're the results. The Kalman Filter algorithm updates the car's predicted position as each observation comes in, which effectiveliy reduces noises and smoothes the raw GPS observations.

 <img src="/img/simulate_kf.png" width="600" >

