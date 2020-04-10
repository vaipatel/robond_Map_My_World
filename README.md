# Submissions/Notes for Udacity RoboND 2020 - Map My World

- [What is the Localization problem?](#what-is-the-localization-problem)
- [What is the Mapping problem?](#what-is-the-mapping-problem)
- [Mapping challenges](#mapping-challenges)

## What is the Localization problem?

Localization is about estimating ![$p(x_{1:t}|u_{1:t},z_{1:t},m)$](https://render.githubusercontent.com/render/math?math=%24p(x_%7B1%3At%7D%7Cu_%7B1%3At%7D%2Cz_%7B1%3At%7D%2Cm)%24), where

* ![$x_t$](https://render.githubusercontent.com/render/math?math=%24x_t%24) is the robot's state at time ![$t$](https://render.githubusercontent.com/render/math?math=%24t%24); ![$x_{1:t}$](https://render.githubusercontent.com/render/math?math=%24x_%7B1%3At%7D%24) is the robot's trajectory
from time ![$1$](https://render.githubusercontent.com/render/math?math=%241%24) to time ![$t$](https://render.githubusercontent.com/render/math?math=%24t%24).
* ![$u_t$](https://render.githubusercontent.com/render/math?math=%24u_t%24) is the action taken by the robot at time ![$t$](https://render.githubusercontent.com/render/math?math=%24t%24); ![$u_{1:t}$](https://render.githubusercontent.com/render/math?math=%24u_%7B1%3At%7D%24) are all the
actions taken by the robot from time ![$1$](https://render.githubusercontent.com/render/math?math=%241%24) to time ![$t$](https://render.githubusercontent.com/render/math?math=%24t%24).
* ![$z_t$](https://render.githubusercontent.com/render/math?math=%24z_t%24) is the noisy measurement made by the robot at time ![$t$](https://render.githubusercontent.com/render/math?math=%24t%24); ![$z_{1:t}$](https://render.githubusercontent.com/render/math?math=%24z_%7B1%3At%7D%24) are 
all the noisy measurements made by the robot from time ![$1$](https://render.githubusercontent.com/render/math?math=%241%24) to time ![$t$](https://render.githubusercontent.com/render/math?math=%24t%24).
* ![$m$](https://render.githubusercontent.com/render/math?math=%24m%24) is perfect information about the map of the robot's
environment. An example of perfect info ![$m$](https://render.githubusercontent.com/render/math?math=%24m%24) could be exact locations of 
landmarks in an environment.

So in localization we want to estimate the robot's state given the actions,
measurements and map.

## What is the Mapping problem?

As it turns out, in a lot of cases, we do not have an exact map ![$m$](https://render.githubusercontent.com/render/math?math=%24m%24). 
The Mapping problem concerns itself with finding an estimate of the map ![$m$](https://render.githubusercontent.com/render/math?math=%24m%24).

Depending on what information is available to estimate the map, we can
think about further classifying the Mapping problem into two problems.

* Mapping with known poses: If we have data about the robot's trajectory
![$x_{1:t}$](https://render.githubusercontent.com/render/math?math=%24x_%7B1%3At%7D%24), we can try to estimate ![$p(m|x_{1:t}, z_{1:t})$](https://render.githubusercontent.com/render/math?math=%24p(m%7Cx_%7B1%3At%7D%2C%20z_%7B1%3At%7D)%24).
* Mapping with unknown poses or SLAM: If we don't even have data about the
robot's trajectory, we're in a bit of a chicken-egg sich. We need to
simultaneously develop an understanding of where the robot is and where the
environment is from the only things the robot knows - it's actions and it's
noisy measurements. That is, we try to estimate
![$p(x_{1:t}, m|u_{1:t}, z_{1:t})$](https://render.githubusercontent.com/render/math?math=%24p(x_%7B1%3At%7D%2C%20m%7Cu_%7B1%3At%7D%2C%20z_%7B1%3At%7D)%24).

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
