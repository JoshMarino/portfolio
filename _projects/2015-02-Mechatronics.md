---
layout: project
title: Mechatronics
date: February 2015
image: 
---

## Overview

Completed in the Mechatronics course was becoming familiar with the PIC32, writing C code for programming the PIC32, and uploading code to the PIC32. The course ended in a project in which we created a library of functions to control a bar's position on the end of a motor. Ultimately, the bar's angular position tracked a reference signal by using a PID controller.


## DC Motor Control Project

Control of the DC motor occurred through MATLAB, in which serial communication sent commands to the PIC32 to directly control the DC motor. Hardware used was quadrature decoder and counter chip, current sensor, H-bridge, as well as resistors and capacitors. We built up a library of functions before having a bar attached to the end of the motor follow a reference trajectory. These functions were: 

1. Read current sensor (ticks)
2. Read encoder (ticks)
3. Reset encoder
4. Set current gains
5. Set position gains
6. Tune current loop
7. Go to angle
8. Load cubic trajectory
9. Quit
10. Read current sensor (mA)
11. Read encoder (deg)
12. Set PWM
13. Get current gains
14. Get position gains
15. Set recording samples
16. Get state
17. Load step trajectory
18. Execute trajectory

Once functions 1-16 were working, we wrote code for 2 control loops: current control loop (5 kHz) and position control loop (200 Hz). The PI current controller makes the current sensor reading match a reference signal by adjusting the PWM signal and motor direction. The PID position control loop determines the error between the reference position and enconder angle, setting a desired current to follow for the current control loop. Tuning of both control loops was required.

The results of following three different reference trajectories are shown below: 1) square trajectory, 2) 90 degree sine curve, 3) 180 degree sine curve

![square_trajectory](https://raw.githubusercontent.com/JoshMarino/mechatronics_project/master/square_trajectory.png)
![90_degree_trajectory](https://raw.githubusercontent.com/JoshMarino/mechatronics_project/master/90_degree_trajectory.png)
![180_degree_trajectory](https://raw.githubusercontent.com/JoshMarino/mechatronics_project/master/180_degree_trajectory.png)


## Github Link
[Mechatronics Project](https://github.com/JoshMarino/mechatronics_project)
