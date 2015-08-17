---
layout: project
title: Baxter Dual-arm Manipulation
date: January-March 2015
image: http://i1290.photobucket.com/albums/b525/Josh_Marino/successful_iteration_zpsitpq2zu3.png
---

## Overview
The intention of this project was to gain some experience in manipulation tasks with the Baxter research robot. The task at hand was to pass an object from one gripper to the next in minimal time, for any initial poses of each end-effector. In order to solve this complex task, it was broken down into three steps:

1. Minimization problem for finding handoff pose for each end-effector, which are constrained to being a certain distance and rotation apart from one another
2. Using MoveIt! to generate paths for both end-effectors from initial pose to handoff pose, without being in collision
3. Performing the first and second steps numerous times in order to find the path that takes the minimal time

### Minimization Problem
In order to find a handoff pose, a minimization problem was set up which minimized the norm of the distance and rotation between the initial poses of each end-effector and the handoff pose. Python's scipy.optimize.minimize() function was used to solve this problem, using Sequential Least Squares Programming, SLSQP method. Also set as parameters for the minimization function were the jacobian (gradient) of the objective function, bounds, constraints, and tolerance for termination. Experimentation was used with the constraints and tolerance for termination to find a solution in the least amount of iterations and time, while still satisfying the constraints. After numerous experiments, I was able to solve the minimization problem succesfully in roughly 30 iterations and 1-2 seconds.

### MoveIt! used to Generate Paths
After finding a set of joint angles that minimized the objective function while satisfying the constraints, collision-free paths had to be generated to move the end-effectors. [MoveIt!](http://moveit.ros.org/baxter-research-robot/) was used to generate the collision-free paths by setting the [move_group](https://github.com/davetcoleman/moveit_commander/blob/hydro-devel/src/moveit_commander/move_group.py) joint value goal targets to the joint angles found from the minimization function. In order to ensure collision-free paths, the move_group 'both_arms' from the [MoveGroupCommander](http://docs.ros.org/indigo/api/moveit_commander/html/classmoveit__commander_1_1move__group_1_1MoveGroupCommander.html) was utilized. This allowed planning to occur for both end-effectors simultaneously, and collision-free planning was implemented.

### Performing Numerous Times to Find Minimal Time Path
Once the first two steps were working reliably, multiple minimizations were run with random initial guesses to find different sets of joint angles that satisfied the constraints. For each minimization solution, numerous paths were generated in MoveIt! to increase the probability of finding the least time trajectory. The reason behind running multiple minimizations was to better explore his workspace to find a solution that would allow MoveIt! to have a better chance of finding a trajectory that occurs in the least amount of time from the initial poses of each end-effector to the handoff location.

## Baxter Dual-arm Manipulation Repository
A Github repository was created for this project, located [here.](https://github.com/JoshMarino/baxter_dual_arm_manipulation)
