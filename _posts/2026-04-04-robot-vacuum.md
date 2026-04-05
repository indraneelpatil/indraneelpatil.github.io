---
layout: post
title: (RoboVac) Building a home robot to autonomously clean our house
date: 2026-04-04 21:01:00
description: Building and deploying navigation algorithms on a home robot vacuum cleaner
tags: robot ml behavior cloning vacuum cleaner
categories: robotics
thumbnail: assets/img/robo_vac/robo_vac_top.jpeg
---

**Team: Bruce Kim, Indraneel Patil**

Bruce and I moved in together and were discussing buying a robot vacuum, but then decided to build one instead! Some design principles:

1. Buy as many off the shelf parts as possible
2. Keep the budget less than 500$
3. It should have a minimum battery life to be able to navigate around the house and vacuum the floor, so that we only have to charge it once a week

### Hardware

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/robo_vac/robo_vac_build.jpeg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/robo_vac/robo_vac_build2.jpeg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/robo_vac/robo_vac_vacuum.jpeg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/robo_vac/robo_vac_circuit.jpeg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/robo_vac/robo_vac_table.jpeg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/robo_vac/robo_vac_sketch.jpeg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/robo_vac/robo_vac_top.jpeg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/robo_vac/robo_vac_front.jpeg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/robo_vac/robo_vac_side.jpeg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>


### Software
Since we dont have any compute on the robot to do inference we decided to stream image frames from the robot to a laptop and then run inference on the robot and return result back to the robot for navigation. We teleoperated the robot around the house to collect tuples of image frame and a discrete navigation action. Our discrete navigation action set was: 

```python
COMMANDS = {
    keyboard.Key.up: "FORWARD\n",
    keyboard.Key.down : "REVERSE\n",
    keyboard.Key.left: "TURN_CCW\n",
    keyboard.Key.right: "TURN_CW\n",
    keyboard.Key.space: "STOP\n"
}

```
Then we trained a simple CNN with behavior cloning to learn from these expert trajectories.

##### Observations:

<div class="row mt-3">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/robo_vac/camera_frame1.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/robo_vac/camera_frame2.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
* Lots of false positive reversing, like for these there was no need to reverse so maybe it doesnt have a good perception of depth? -> It doesnt need a good perception of depth it only needs to worry about the ground plane in front of it
* In the kitchen seems to just keep oscillating between forward and reverse -> This could be lack of good data
* Should have created a validation / training split

<div class="row mt-3">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/robo_vac/camera_frame3.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
* It kept getting STOP for this, where clearly there is space ahead for it to move into

<div class="row mt-3">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/robo_vac/camera_frame_4.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
* Kept getting FORWARD for this, when clearly an obstacle can be seen
* Dont think it is trained super well
* There is some motion blur but most images are really nice
<div class="row mt-3">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/robo_vac/camera_frame5.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
* STOP action can get the robot stuck in front of an obstacle, is that really needed? Can we remove STOP? <a href="https://proceedings.neurips.cc/paper_files/paper/2020/file/2c75cf2681788adaca63aa95ae028b22-Paper.pdf">This paper<a> only use the STOP action when robot has reached the goal
* I feel like what its learnt is quite random but it has learnt to back up when very close to an obstacle or its about to hit something which is really nice
* What it hasnt learnt is how to determine there is free space in front of you
* Also doesnt know how to navigate out of tough spots
* There is a signal in the image for it to learn forward and reverse but there is no signal in the image for it to learn clock wise and counter clockwise

##### Training Experiments:

**Calculating Validation Loss**

We split the data into training and validation sets and then we found that the network has a very low validation loss.
<div class="row mt-3 justify-content-center">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid 
          loading="eager" 
          path="assets/img/robo_vac/training_graph.png" 
          class="img-fluid rounded z-depth-1" zoomable=true
        %}
    </div>
</div>

So there could be two explanations:
1. Network is overfitting training data
2. There is not enough signal in the data to learn

We can test the first explanation through data augmentation or pre training the network on a large dataset and see if that improves validation loss.

**Data Augmentation**

```python
 augmentations = {
        "grayscale": T.Grayscale(num_output_channels=3),
        "gaussianblur": T.GaussianBlur(kernel_size = (3,5), sigma = (2,9)),
        "noise:": addnoise,
        "colourjitter": T.ColorJitter(brightness=0.5, contrast=0.5, saturation=0.5, hue=0.1),
        "original": lambda x: x,
    }

```
I wrote a script to apply these augmentations on the original dataset of 600 image-action tuples to get a data of size of about 30000.

<div class="row mt-3 justify-content-center">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid 
          loading="eager" 
          path="assets/img/robo_vac/training_graph3.png" 
          class="img-fluid rounded z-depth-1" zoomable=true
        %}
    </div>
</div>

Validation loss is still high and does not follow the training loss curve.


**Pre-training network on ImageNet**

I followed this <a href="https://docs.pytorch.org/tutorials/beginner/transfer_learning_tutorial.html">tutorial<a> to pre train the network on image net.

<div class="row mt-3 justify-content-center">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid 
          loading="eager" 
          path="assets/img/robo_vac/training_graph2.png" 
          class="img-fluid rounded z-depth-1" zoomable=true
        %}
    </div>
</div>

Validation loss still does not converge which means that the network is not overffitting, the dataset just does not have enough signal to learn.


**Future work**

1. Add history of image frames, instead of a single frame
2. Collect more and better data: Our robot teleoperation wasnt always consistent and had some randomness in it.
3. Go through the data, eliminate noisy data and so on.


### Summary

This was a lot of fun! we built a very capable robot. Since we were working on the project only part time, on evenings and weekends it took us about 3 months to build the robot and another month to write the software. We were able to achieve the budget requirement and then robot costs about 300$ only.

Some limitations:
1. The vacuum isnt super strong and I wish we had spent more time just designing the vacuum, the inlet and its power specs (Cant believe this is the second time I made this mistake)
2. We had to use two separate batteries, one to power the vacuum and the other to power the rest of the system because the vacuum motor power on causes a voltage drop in the circuit
3. No autonomous charging
4. Robot isnt completely autonomous and gets stuck in places so needs baby sitting while it is cleaning.
