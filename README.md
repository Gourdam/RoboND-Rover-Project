# Search and Sample Return

**The goals / steps of this project are the following:**  

**Training / Calibration**  

* Download the simulator and take data in "Training Mode"
* Test out the functions in the Jupyter Notebook provided
* Add functions to detect obstacles and samples of interest (golden rocks)
* Fill in the `process_image()` function with the appropriate image processing steps (perspective transform, color threshold etc.) to get from raw images to a map.  The `output_image` you create in this step should demonstrate that your mapping pipeline works.
* Use `moviepy` to process the images in your saved dataset with the `process_image()` function.  Include the video you produce as part of your submission.

**Autonomous Navigation / Mapping**

* Fill in the `perception_step()` function within the `perception.py` script with the appropriate image processing functions to create a map and update `Rover()` data (similar to what you did with `process_image()` in the notebook).
* Fill in the `decision_step()` function within the `decision.py` script with conditional statements that take into consideration the outputs of the `perception_step()` in deciding how to issue throttle, brake and steering commands.
* Iterate on your perception and decision function until your rover does a reasonable (need to define metric) job of navigating and mapping.  

[//]: # (Image References)

[image1]: ./misc/rover_image.jpg
[image2]: ./misc/obstacle_image.jpg
[image3]: ./misc/obstacle_thresh.jpg
[image4]: ./misc/rock_image.jpg
[image5]: ./misc/rock_thresh.jpg

## [Rubric](https://review.udacity.com/#!/rubrics/916/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.

## Notebook Analysis
1) I addressed the criteria by changing the `color_thresh` function to be more versatile. The revised version allowed for a range of values instead of just a greater than RGB value. This was needed when creating color threshold profiles for identifying rocks and obstacles. As seen in the 3rd step of the `process_image` function, the different values for the thresholds were used to determine the rocks. Other than that, all other functions remained the same.

![alt text][image4]

Rock image


![alt text][image5]

Thresholded rock image

![alt text][image2]

Obstacle image


![alt text][image3]

Thresholded obstacle image

2) I modified the `process_image` function by first determining source values for the perspective transformation to use. From testing, I  determined that the original values provided were accurate enough to give a good transformed view. Then, the transformed image was filtered through the color threshold for each type of object. Initially in the notebook, the obstacles were given the exact opposite pixel locations as the navigable pixels. However, from testing, a custom color profile was created from test runs as shown in the 3rd step of the `process_image` function. Each of the thresholded pixels were converted to world coordinates and accordingly updated the map. The 7th step was left untouched, only removing the text for clarity. The video produced from the notebook can be seen in `./output/test_mapping.mp4`.

## Autonomous Navigation and Mapping
1) The `perception_step` function was modeled after the `process_image` function in the notebook with a few modifications. Instead of using the `Databucket` object, the `Rover` instance was used to extract data from the current state of the robot. As noted above, the obstacles were given a custon color threshold as seen on line 101. Once the worldmap was updated, the distance and angles of the robot's new trajectory were calculated by averaging the navigable pixel locations together. The angles given to the Rover were skewed 15 degrees to the left. This was done to create a "wall crawler" effect by biasing the rover's movements to the left. This way, the rover would closely hug the left wall and map the whole area thoroughly.

    The `decision.py` was left untouched.

###Settings

**Resolution:** 1024 x 768
**Graphics Quality:** Good
**FPS:** 22

2a) The results I had was 40% of the map was mapped in 170 seconds with 65% fidelity. The robot successfully identified navigable terrain, obstacles, and rocks on the map with reasonable accuracy. When left alone, the robot managages to usually map at least 70% of the map. Due to the added 15 degrees in the robot's trajectory, there was an increase in the amount of area mapped but because it favored the left side, the robot tended to get stuck on rough sides of the terrain. This caused the robot to lose its orientation and map the environment inaccurately, losing fidelity. Another issue the basic wall crawler strategy introduced was that the rover was not as fast as it tended to veer to the left rather than taking the direct path to map the next area of the environment.

2b) There are many improvement that could be done for the robot. One suggestion would be to incorporate the yaw values emitted from the RobotState. If the absolute value of the yaw exceeded a certain value, the data collected from the camera would be ignored for that portion until the rover was in a better position. Another suggestion would be to implement a slight bias away from obstacles. Because of the skewed 15 degrees introduced before, the robot would still be able to create a detailed map but the bias away from obstacles would limit the issue of the robot constantly bumping into walls. Finally, the `decision.py` would be modified to increase the speed of the robot when the obstacle pixels sensed were low and the navigable terrain pixels are high. This would enable the rover to quickly navigate through its environment in a much quicker fashion.
