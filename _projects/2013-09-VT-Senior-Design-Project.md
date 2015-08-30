---
layout: project
title: Virginia Tech Bio-Inspired Robotic Seagull
date: September 2013 - May 2014
image: http://i1290.photobucket.com/albums/b525/Josh_Marino/VT_bird_zps9wb7lz2f.png
---

## Overview
As my senior design project, I was on the bio-inspired robotic seagull team with 10 others. This was a continuation project from previous years' teams. When we started work on the robotic bird, the previous year's team had created a design for the exoskeleton. We redesigned the exoskeleton by greatly increasing the strength profile, selecting a new motor, and increasing the efficiency of the motor by adding rotational springs. FEA work was performed for designing the airfoils used on the wings, as well as determining the necessary amplitude of the wing tip cycle. With these results, the controls team (of which I was part of) helped determine the flap cycle as a function of the rotational DOF joints. We then used an Arduino Mega2560 to send PWM commands to have the wing and wing tip follow these fuctions ot maximize lift and thrust.
 

### Robotic Model
I was responsible early on for creating a robotic model of a seagull, with 5 DOF, using Denavit-Hartenburg convention, located below. The wings included a 2 DOF rotational joint in the vertical/x-direction attached to a 2 DOF rotational joint in the vertical direction and horizontal direction (backsweep angle), lastly attached to another rotational joint to control the wing tip rotation about the x-axis. This model was used for preliminary calculation of motor torque required, and to optimize the wing tip trajectory compared to a paper presented by Dr. Liu.

![robotic_model](https://raw.githubusercontent.com/JoshMarino/VT_Robotic_Bird/master/VT_bird_robotic_model.png)


### Wiring Harness
A wiring harness was created from an empty breadboard to convert power from a battery pack to be accessible with the Arduino Mega2560, brushless DC motor, and PWM control of 6 servomotors. Located below is a picture of the harness created. Located at the top is the power from the LiPo battery pack after being seperated for the breadboard and Arduino Mega2560. This is then passed through a capacitor and 5V regulators, which powers the 6 servomotors (2 on each regulator).

<img src="https://raw.githubusercontent.com/JoshMarino/VT_Robotic_Bird/master/VT_bird_wiring_harness.png" width="768">


### Wing Tip Trajectory Optimization
Initial optimization was performed using the robotic model while tuning the amplitude of the DOF's to optimize the wing tip trajectory compare to Dr. Liu's model. The results from tuning the maplitude was given to the FEA team as a starting point for optimizing the lift and wing tip torsion while reducing the drag on the upstroke. Next, we worked with the mechanical design team to tune the phases of the coupled 4-bar linkages in order to replicate the results fro FEA. Last, a spline was fit to the curve from the mechanical design team for controlling the servo PWM commands.

### Controls
Controls was a huge portion of our efforts during the last semester. A teammate wrote code for controlling the speed of the motor using PID control with an encoder we purchased for the back of the brushless DC motor. The next main responsibility was figuring out how to correspond the flap cycle with the motor rate. Our solution was adding an absolute magnetic encoder with less than one degree resolution, in which we knew the starting position. Once turned on, nothing was performed until the motor passed the starting position. Then, with the splines we fit to the rotational DOF curves, we sent PWM signals to control the servomotors actuating the approriate joints.

### Prototype
A prototype was constructed with the revisions from the mechanical design team, while implementing the controls work. Revisions were further implemented to reduce weight and increase strength. One major drawback from this progress was the slow turn-over rate of parts being produced. We eventually gave up on the machine shop and outsourced our work to another company. Most of the parts came in 2 weeks before the term ended, while some never came in (eventually during the summer sessions). Due to this, we were never able to fully construct our prototype. However, the controls portion was implemented on the prototype we had and worked beautifully.

## Github Repository
Included is a [GitHub repository](https://github.com/JoshMarino/VT_Robotic_Bird) in which I uploaded a few pictures and final report. Feel free to browse through them.
