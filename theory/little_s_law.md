# Little's Law for Single Server Queue

Let <img src="https://render.githubusercontent.com/render/math?math=\lambda"> and <img src="https://render.githubusercontent.com/render/math?math=\mu"> be the arrival rate and the service rate, respectively.

### View the system as queue + server

<img src="https://render.githubusercontent.com/render/math?math=n = \lambda T">

where n is the average number of jobs in the system and T is the average time in the system.


### View the system as queue only

<img src="https://render.githubusercontent.com/render/math?math=L = \lambda W">

where L is the average queue length and W is the average waiting time.


### View the system as server only

<img src="https://render.githubusercontent.com/render/math?math=\rho=\frac{\lambda}{\mu}"> 

where <img src="https://render.githubusercontent.com/render/math?math=\rho"> is the server utilization (i.e., the probability that the server is busy).

Combining the above two results gives:

<img src="https://render.githubusercontent.com/render/math?math=n = L \plus \rho">

Note that this requires the arrival rate is no greater than the service rate; otherwise, the queue grows indefinitely.
