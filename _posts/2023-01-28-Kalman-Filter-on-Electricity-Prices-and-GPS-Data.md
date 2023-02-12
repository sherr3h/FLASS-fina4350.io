---
layout: post
title: Kalman Filter on Electricity Prices and GPS Data.md
gh-repo: sherr3h/sherr3h.github.io
tags: [Kalman Filter, Statistics, Time Series]
comments: true
---


Kalman filter is a Bayesian method to update and improve parameter estimates from observations with uncertainty. It can be applied to time series formulated as a Linear Gaussian State Space Model. The kalman filter method was used by NASA in the Apollo Project to estimate trajectories of the manned spacecraft landing on the Moon and back (wow!). 

In this post, I will apply Kalman filter to two time series datasets: (1) simulated GPS locations, and (2) an electricity price dataset.

## Problem Formulation

Assumption: all noises are Gaussian, in which case the Kalman filter minimises the mean square error of estimated parameters. If the noises are not Gaussian, given only the mean and standard deviation, the Kalman filter is the
best linear estimator.
 
The State Space Model, with observations $Y_t$ and hidden states $X_t$:

$$Y_t = H_t X_t + \epsilon_t, \epsilon_t \sim N(0, R_t)$$
$$X_{t+1} = A_t X_t + \eta_t, \eta_t \sim N(0, Q_t)$$

where $R_t$ is the measurement error covariance matrix and $Q_t$ is the process error covariance matrix.

Using conclusions from the marginal and conditional distributions of multivariate Gaussians, and let $X_{t+1|t}$ be the predicted latent state with predicted mean $\mu_{t+1|t}$ and predicted covariance $\Sigma_{t+1|t}$, the Kalman filter algorithm:

- At time 0, initial guess is $X_0 \sim (\mu_{0|0}, \Sigma_{0|0})$
- At time t, 
    - prediction updates:
    $$\mu_{t+1|t} = A_t \mu_{t|t}$$
    $$\Sigma_{t+1|t} = A_t \Sigma_{t|t}A_t ^T + Q_t$$
    - Measurement updates:
      $$K_{t+1} = \Sigma_{t+1|t}H_{t+1}^T \left(H_{t+1} \Sigma_{t+1|t}H_{t+1}^T + R_{t+1}  \right)^{-1}, \text{the Kalman gain}$$
      $$\mu_{t+1|t+1} = \mu_{t+1|t} + K_{t+1} \left(y_{t+1} - H_{t+1}\mu_{t+1|t} \right) =\left(I -  K_{t+1}H_{t+1}\right)\mu_{t+1|t} + K_{t+1} y_{t+1}, \text{Innovation}$$
      $$\Sigma_{t+1|t+1} = \left(I -  K_{t+1}H_{t+1}\right) \Sigma_{t+1|t}$$
   
   
### Simulated Race Car Location Data 

Suppose a race car is on the track with following equations:

$$x=2\cos(t), y=\sin(3t), t\ge 0$$

 <img src="/img/simulate_motion.jpg" width="700" >



