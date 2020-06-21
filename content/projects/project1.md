---
title: "Project1"
date: 2020-06-15T20:25:16-04:00
draft: false
title: "A model for covid-19"
---
This a python project that I did to modelize the COVID-19 epidemic.
This work is based on the Scipy Documentation.
Feel free to check the [Github Repo](https://github.com/nic001a/Model-COVI19-with-Python).
Without further due, let's jump in it!

### The SIR MODEL
This a quick disclaimer for you dear reader, as it's name indicate this is a perfect mathematical and simple model which means that the results will **NOT** match the real results, however it's a cool and useful aproximation. 
Ok now let's dive into the soupe.

SIR: Susceptible, Infectious, or Recovered
> This model is based on the Compartmental models, that simplify the mathematical approche hence    inderstanding of infectious diseases by creating compartments or blocks. Each compartements refers to a part of the population since the goal is to visualize the potential results and impact of the infectious disease.
[More about compartemental models](https://en.wikipedia.org/wiki/Compartmental_models_in_epidemiology)

 

### 1. We need to improt the libraries we will work with 

```python3
import numpy as np
from scipy.integrate import odeint
import matplotlib.pyplot as plt
```
We import the numpy package and bind it to the local variable np.


### 2. Set the variables

```python3
# Total population, N.
N = 1000
# Initial number of infected and recovered individuals, I0 and R0.
I0, R0 = 1, 0
# Everyone else, S0, is susceptible to infection initially.
S0 = N - I0 - R0
# Contact rate, beta, and mean recovery rate, gamma, (in 1/days).
beta, gamma = 2.2, 1./10 
# A grid of time points (in days)
t = np.linspace(0, 160, 160)
```
As mentionned below, the SIR model is based on 3 variables:

S: Suceptible 

I: Infected 

R: Recovered 

β: Describes the effective contact rate (The number of individuals that can one person have a contact with  in a periode of time).

γ: The number of individuals that a person can infecte.

### 3. Diffrential equations AKA favorite physicist mathematical tool.

```python3
# The SIR model differential equations.
def deriv(y, t, N, beta, gamma):
    S, I, R = y
    dSdt = -beta * S * I / N
    dIdt = beta * S * I / N - gamma * I
    dRdt = gamma * I
    return dSdt, dIdt, dRdt
```



### 4. Calculations 

```python3
# Initial conditions vector
y0 = S0, I0, R0
# Integrate the SIR equations over the time grid, t.
ret = odeint(deriv, y0, t, args=(N, beta, gamma))
S, I, R = ret.T
```

### 5. Plot the results 

```python3
# Plot the data on three separate curves for S(t), I(t) and R(t)
fig = plt.figure(facecolor='w')
ax = fig.add_subplot(111, axisbelow=True)
ax.plot(t, S/1000, 'b', alpha=0.5, lw=2, label='Susceptible')
ax.plot(t, I/1000, 'r', alpha=0.5, lw=2, label='Infected')
ax.plot(t, R/1000, 'g', alpha=0.5, lw=2, label='Recovered with immunity')
ax.set_xlabel('Time /days')
ax.set_ylabel('Number (1000s)')
ax.set_ylim(0,1.2)
ax.yaxis.set_tick_params(length=0)
ax.xaxis.set_tick_params(length=0)
ax.grid(b=True, which='major', c='w', lw=2, ls='-')
legend = ax.legend()
legend.get_frame().set_alpha(0.5)
for spine in ('top', 'right', 'bottom', 'left'):
    ax.spines[spine].set_visible(False)
plt.show()
```

