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

### Summary

This was a lot of fun! we built a very capable robot. Since we were working on the project only part time, on evenings and weekends it took us about 3 months to build the robot and another month to write the software. We were able to achieve the budget requirement and then robot costs about 300$ only.

Some limitations:
1. The vacuum isnt super strong and I wish we had spent more time just designing the vacuum, the inlet and its power specs (Cant believe this is the second time I made this mistake)
2. We had to use two separate batteries, one to power the vacuum and the other to power the rest of the system because the vacuum motor power on causes a voltage drop in the circuit
3. No autonomous charging
4. Robot isnt completely autonomous and gets stuck in places so needs baby sitting while it is cleaning.

### Future Work

Some work needs to be done to improve the navigation software on the robot.


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/9.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/7.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    A simple, elegant caption looks good between image rows, after each row, or doesn't have to be there at all.
</div>

Images can be made zoomable.
Simply add `data-zoomable` to `<img>` tags that you want to make zoomable.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/8.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/10.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

The rest of the images in this post are all zoomable, arranged into different mini-galleries.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/11.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/12.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/7.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
