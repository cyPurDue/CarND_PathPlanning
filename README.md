# CarND-Path Planning
Self-Driving Car Engineer Nanodegree Program

---
### 1. Compilation: The code compiles correctly.
The code has been succesfully compiled with no errors. All features are implemented in the main.cpp, plus a spline.h added from online source. After build, running the ./path_planning will start listening to the simulator.

### 2. Valid Trajectories
2.1 The car is able to drive at least 4.32 miles without incident.
I have attached 2x screen shots in the folder, showing running 4.80 and 5.08 miles in two separate different run times, without incident.

2.2 The car drives according to the speed limit.
The car did not run beyond speed limit.

2.3 Max Acceleration and Jerk are not Exceeded.
No max acceleration or jerk has been exceeded.

2.4 Car does not have collisions.
No collisions happened.

2.5 The car stays in its lane, except for the time between changing lanes.
The car stayed well in the lane.

2.6 The car is able to change lanes.
The car is able to switch lanes, if there is a slow car in front of it and available lanes are ready to switch. Then when the condition allows, it will switch back to the center lane.


### 3. Reflection: There is a reflection on how to generate paths.
First, I have followed the QA session in the class and added the spline.h in the src foler, and made the car easy to generate a spline, but only stay in the center lane. 

Then starting from line 261 in main.cpp, I have implemented additional codes to deal with sensor fusion. Basically, it checks conditions of three lanes (current, its left, its right). If any car exists on that lane within 30 meters ahead of our current position, then the corresponding flag ("too_close": center lane flag, "left_lane": left lane flag, "right_lane": right lane flag) becomes true. Especially for left and right lanes, since we need to consider the car behind on that lane when switching lanes, I have added to give -30 to +30 meters distance check to allow switch lane.

Following that, starting from line 306 in main.cpp, the state-machine-like switch strategy was implemented. The logic is like this: when the car is approaching close to the front car, first if left lane is available to switch and pass. If we are not on the first left lane (lane > 0) and left lane flag is clear, then switch to left lane (lane--). Then if left lane is not available, use the same strategy to check right lane. If neither of them are available, then slow down. When there is no car close ahead, and the car is not at the center lane (lane != 1), then switch back to center lane, if it is available (code line 320). If the speed is still below the limit, speed up then.

Following that it is the calculation of trajectory. It uses last two of previous trajectory and three future points to calculate a spline path, and then coordinates are converted to car coordinate. Also during generating the trajectory, a simple velocity limiting is added (code line 402-408) to ensure the reference velocity is within reasonable range.


