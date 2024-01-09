---
title: State Estimation in linear 1D problem 
summary: State Estimation of a linear one-dimensional robot driving back and forth in a straight line. 
tags:
  - Robotics
date: '2023-09-27T00:00:00Z'

# Optional external URL for project (replaces project detail page).
external_link: ''

image:
  caption: Photo by rawpixel on Unsplash
  focal_point: Smart

 
links:
  - icon: twitter
    icon_pack: fab
    name: Follow
    url: https://twitter.com/georgecushen
url_code: ''
url_pdf: ''
url_slides: ''
url_video: ''

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: example
 


---
For the assignment we’ll use the batch linear-Gaussian algorithm to estimate the robot’s position using both the odometry and laser measurements. You might wonder, why do we need to use both? In this simple example we could use either datastream to estimate the robot’s position. Figure 1.7 shows the errors we would obtain if we use each datastream individually. We see the odometry error grows without bound over time (probably due to a very small bias). The position error derived from range remains bounded, but in a real situation, we might not have so much range data; we’ll look at using only a small portion of this to effectively correct the odometry data.

## Heading 2
![screen reader text](icon.png "caption")

{{< gallery album="demo" resize_options="250x250" >}}
### Heading 3

#### Python Code Implementation
```python
import scipy.io
import numpy as np
from matplotlib import pyplot as plt

# Load matlab dataset
mat = scipy.io.loadmat('dataset1.mat')

# There are l, r, r_var, t, v, v_var, x_true in this matlab file.

size = mat['r'].shape[0]
x_c = mat['l']              # l: The scalar true position, x_c, of the cylinders center 1x1
r_k = mat['r']              # r: the range r_k between robot and cylinders center measured by laser range finder 12709x1
r_var = mat['r_var']        # r_var: the variance of the range readings
t = mat['t']                # t: data timestamps 12709x1
u_k = mat['v']              # v: Speed measrued by odometer, u_k
v_var = mat ['v_var']       # v_var: variance of the speed readings
x_true = mat['x_true']      # x_true : the true robot position x_k 12709x1, this will be used as benchmark
y_k = x_c - r_k             # Small transformation to the observation model to create sensor output


# Question 1: Compute the variance of process noise, and sensor noise.
sensor_var = 0.019155 ** 2
process_var = (0.047554/10) ** 2
print("The variance for process noise and sensor noise is", process_var, "m^2 and ", sensor_var, "m^2.")

# Question 4: Make a plot of the sparsity pattern of the left hand side(LHS) of the linear system
# whose solution minimize the objective function and comment on the pattern.

def lhs_plot(size):

    # We would need a representation of H and W matrix to get the LHS
    # From the definitions from the full Bayesian approach
    A = np.tri(size)            # A is a lower triangular matrix
    A_inv = np.linalg.inv(A)    # Inverse of A results in a simple form that is sparse
    C = np.eye(size)            # C is the lifted observation matrix diag(C0, C1.... Ck)
    Q = np.eye(size) * 2        # Q: diag(P_check, Q1, Q2....Qk) P_check
    R = np.eye(size)            # R: diag(R0, R1, R2,.... Rk)

    H = np.vstack((A_inv, C))   # Stacking A inverse, and C into equation 24
    W = scipy.linalg.block_diag(Q, R)
    # lhs = H.T @ np.linalg.inv(W) @ H
    lhs = np.dot(np.dot(H.T,np.linalg.inv(W)),H)

    if size == 10 :
        plt.matshow(lhs)
        plt.title("Zoomed in LHS plot for a 10 by 10 matrix")
        plt.show()
    else:
        plt.matshow(lhs)
        plt.title(" LHS plot for a 12709 by 12709 matrix")
        plt.show()
#Uncomment to see plots
lhs_plot(10)
# lhs_plot(12709)


# Question 5 Write a python script to solve for robot positions (mean and covariance) at all K timestamps

# Note:
# There will be 4 cases, where we use 1st, 10th, 100th, 1000th laser rangefinder readings.


#a) Plot the error() vs time as a solid line
A = 1
C = 1
P_0_check = r_var                # Initial state knowledge covariance
Q_k = process_var                 # Inputs process noise covariance
R_k = r_var                      # Measurement covariance matrix

P_f_check,x_f_check,K, P_f_hat,x_f_hat,x_hat,P_hat = np.zeros(size),np.zeros(size),np.zeros(size),np.zeros(size),np.zeros(size),np.zeros(size),np.zeros(size)



for i in {1,10,100,1000}: # We use 1st, 10th, 100th, 1000th laser rangefinder readings.
    # Forward pass
    for k in range(size): # k = 1......K

      if k == 0: # Initalization
          P_f_check[k] = P_0_check #Initialized according to txtbook
          x_f_check[k] = y_k[k]    #Initialized according to txtbook
          K[k] = P_f_check[k] * C * (C * P_f_check[k] * C + R_k)**-1
          P_f_hat[k] = (1 - K[k] * C) * P_f_check[k]
          x_f_hat[k] = x_f_check[k] + K[k] * (y_k[k] - C * x_f_check[k])

      else:

          C = 1 if k % i == 0 else 0
          P_f_check[k] = A * P_f_hat[k-1] * A + Q_k
          x_f_check[k] = A * x_f_hat[k-1] + u_k[k]*0.1
          K[k] = P_f_check[k] * C * (C * P_f_check[k] * C + R_k)**-1
          P_f_hat[k] = (1 - K[k] * C) * P_f_check[k]
          x_f_hat[k] = x_f_check[k] + K[k] * (y_k[k] - C * x_f_check[k])



    #Backward pass

    # Initialization outside the for loop
    P_hat[12708] = P_f_hat[12708]
    x_hat[12708] = x_f_hat[12708]

    for k in reversed(range(size)): # backward pass k = K....1
        if k == 0: #Avoid going out of range x-hat[-1]
            break
        x_hat[k-1] = x_f_hat[k-1] + (P_f_hat[k-1] * A * P_f_check[k] ** -1) * (x_hat[k] - x_f_check[k])
        P_hat[k-1] = P_f_hat[k-1] + (P_f_hat[k-1] * A * P_f_check[k] ** -1) * (P_hat[k] - P_f_check[k]) * (P_f_hat[k-1] * A * P_f_check[k]**-1).T

    # Compute the state estimation error by taking a difference
    error = x_hat-x_true[..., 0]
    std_d3 = 3 * np.sqrt(P_hat)
    negative_std_d3 = -std_d3
    # Plotting uncertainty envelope, and error vs time
    plt.rcParams['savefig.dpi'] = 300
    plt.plot(t,std_d3, 'r:', label='uncertainty envelope', linewidth=0.3)
    plt.plot(t,negative_std_d3,'r:',   linewidth=0.3)
    plt.plot(t, error,alpha=0.5, label='error', linewidth = 0.3)
    plt.legend()
    plt.title(f'Error vs timestep for every {i}th laser rangefinder reading')
    plt.xlabel("timestamps[s]")
    plt.ylabel("Estimation error [m]")
    plt.show()
    # plt.savefig(f"delta{i}.png")

    # b) Plot a histogram of errors for each value of delta

    plt.hist(error,  color='#86bf91', zorder=2, rwidth=0.6,bins=25)
    plt.axvline(error.mean(), color='k', linestyle='dashed', linewidth=1)
    plt.grid(color='grey',linestyle='-.', linewidth=0.5, alpha=0.6)
    plt.title(f'Histogram of estimation errors for every {i}th laser rangefinder reading')
    plt.ylabel("error count")
    plt.xlabel("Error[m]")
    plt.show()


```