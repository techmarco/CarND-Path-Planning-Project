#**Path Planning Project**

#### Reflection - Model Documentation

##### The code model for generating paths is described in detail.

To generate safe paths, the code developed had to meet the following valid trajectory criteria:

* The car is able to drive at least 4.32 miles without incident.
* The car drives according to the speed limit.
* Max Acceleration and Jerk are not Exceeded.
* Car does not have collisions.
* The car stays in its lane, except for the time between changing lanes.
* The car is able to change lanes.

Each of the criterion and the respective code is detailed below for how the code contributes to meeting the criterion.

* The car is able to drive at least 4.32 miles without incident.

To drive 4.32 miles without incident the track wraparound is handled in line 176 with the definition of **max_s ** as **6972.38**. End of track wrapping is handled by adjusting computed distances in lines 280 through 286.

* The car drives according to the speed limit.

To drive according to the speed limit, the maximum is target velocity is set to **49.1 MPH** in line 204. Speed control is handled in lines 401 through 410 where the speed is reduced by 1.6% when a vehicle occupying our lane ahead is too close. During lane changes, a decrement of 0.05 MPH is applied if the vehicle in our departure lane slows down as we attempt to pass. At low speeds below 30 MPH, the speed is increased by 0.8 MPH and speed is increased by 1.6% otherwise.

* Max Acceleration and Jerk are not Exceeded.

To ensure that max acceleration and jerk are not exceed the previously described speed changes are used along with a spline trajectory generation in lines 412 through 492.

* Car does not have collisions.

To prevent collisions the occupancy of all three forward traffic lines is computed in lines 316 through 350. Lane occupancy is computed as a lane without a vehicle present with distances 50% of the speed behind our vehicle and distances 90% of the speed ahead of our vehicle. This provides enough buffer room to safely navigate the track.

* The car stays in its lane, except for the time between changing lanes.

To have the car stay in its lane when not changing lanes, the generated spline trajectory is mapped along the map waypoints provided. Additionally, to ensure that multiple lane changes are not attempted, a lane change is only initiated when the vehicle is driving near the center of our desired lane in line 378.

* The car is able to change lanes.

The car changes lanes by identifying the optimal lane in lines 288 through 314 and lines 354 through 356. Optimality is determined as the lane with the maximum unoccupied path up to 200 meters ahead.

##### Results & Future Work

The best performance achieved using the model described above is illustrated in the figure below with a 2 hour runtime comprising 83.28 incident free miles representing an average vehicle speed of 41.64 MPH (83.28% of 50 MPH speed limit).

#![two_hour_runtime.png][image1]{width=100%}

To improve the model further, additional special handling for exceptional cases could be added. Two examples of extraordinary cases witness during testing included:

* Computer vehicle overshoots lane change to middle lane and sideswipes our vehicle through no fault of our own.

* On-coming traffic computer vehicle overshoots lane change to opposing left lane, causes a collision, and then during recovery drives on our left lane causing a head-on collision.

* Computer vehicle performs lane change immediately in front of us resulting in a collision unless the use of emergency braking is permitted.

The above cases could be handled, but would likely result in excessive jerk and acceleration as is usually the case during extreme collision avoidance maneuvers.

[//]: # (Image References)

[image1]: /home/marco.nogueira/CarND-Path-Planning-Project/doc/two_hour_runtime.png "PID Controller Error"
