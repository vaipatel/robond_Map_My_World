# Submissions/Notes for Udacity RoboND 2020 - Map My World

## What is the Localization problem?

Localization is about estimating $p(x_{1:t}|u_{1:t},z_{1:t},m)$, where

* $x_t$ is the robot's state at time $t$; $x_{1:t}$ is the robot's trajectory
from time $1$ to time $t$.
* $u_t$ is the action taken by the robot at time $t$; $u_{1:t}$ are all the
actions taken by the robot from time $1$ to time $t$.
* $z_t$ is the noisy measurement made by the robot at time $t$; $z_{1:t}$ are 
all the noisy measurements made by the robot from time $1$ to time $t$.
* $m$ is perfect information about the map of the robot's
environment. An example of perfect info $m$ could be exact locations of 
landmarks in an environment.

So in localization we want to estimate the robot's state given the actions,
measurements and map.

## What is the Mapping problem?

As it turns out, in a lot of cases, we do not have an exact map $m$. Here, we
can think about tackling two problems given the data we have.

* Mapping with known poses: If we have data about the robot's trajectory
$x_{1:t}$, we can try to estimate $p(m|x_{1:t}, z_{1:t})$.
* Mapping with unknown poses or SLAM: If we don't even have data about the
robot's trajectory, we're in a bit of a chicken-egg sich. We need to
simultaneously develop an understanding of where the robot is and where the
environment is from the only things the robot knows - it's actions and it's
noisy measurements. That is, we try to estimate
$p(x_{1:t}, m|u_{1:t}, z_{1:t})$.

## Mapping challenges

Mapping is of course a very challenging inverse problem.

* The hypothesis space is huge. To understand this, imagine the map has 100 3d
landmarks. Simply enumerating the possibilites means we have a 300 dimensional
hypothesis space of maps, any point of which can be our true map.

  Realize that in most scenarios our map will be much more complicated than a
simple cluster of points - it will have continuous structures like curved walls
etc. It's not even clear (to me) how we would parameterize such hypothesis
spaces and what we would do if we could even achieve some parameterization.
* To get a sense of the environment, we would have to have the robot explore and
sense it. This exploratory sensing could itself be quite a time/energy consuming
affair for large environments.
* The mapping process is obviously prone to the same kinds of noise that we have
to deal with in localization like sensor noise, actuator noise etc. So
measurements obtained during mapping cannot be used in creating a map estimate
without accounting for the dynamics/measurement noise.
* Perceptual ambiguity is the name given to problems arising when the size of
the map is bigger than the robot's perceptual range. Imagine an environment has
multiple similar rooms and the robot's radar range is limited to the size of
just one room - in this case, the robot might end up incorrectly hypothesizing
that there is indeed just one room.
* The robot might end up cycling in an area when mapping, accumulating odometry
data and noise without really exploring anything new.