# Search and Sample Return

## Notebook Analysis
1) The criteria was addressed by changing the `color_thresh` function to be more versatile. The revised version allows for a range of values instead of just a greater than RGB value. This was needed when creating color threshold profiles for identifying rocks and obstacles. As seen in the3rd step of the `process_image` function, the different values for the thresholds were used to determine the rocks. Other than that, all other functions remained the same.

2) The `process_image` function was modified by first determining source values for the perspective transformation to use. From testing, it was determined that the original values provided were accurate enough to give a good transformed view. Then, the transformed image was filtered through the color threshold for each type of object. Initially in the notebook, the obstacles were given the exact opposite pixel locations as the navigable pixels. However, from testing, a custom color profile was created from test runs as shown in the 3rd step of the `process_image` function. Each of the thresholded pixels were converted to world coordinates and accordingly updated the map. The 7th step was left untouched, only removing the text for clarity.

## Autonomous Navigation and Mapping
1) The `perception_step` function was modeled after the `process_image` function in the notebook with a few modifications. Instead of using the `Databucket` object, the `Rover` instance was used to extract data from the current state of the robot. As noted above, the obstacles were given a custon color threshold as seen on line 101. Once the worldmap was updated, the distance and angles of the robot's new trajectory were calculated by averaging the navigable pixel locations together. The angles given to the Rover were skewed 15 degrees to the left. This was done to create a "wall crawler" effect by biasing the rover's movements to the left. This way, the rover would closely hug the left wall and map the whole area thoroughly.

    The `decision.py` was left untouched.

2) ### Settings
    Resolution: 1024 x 768
    Graphics Quality: Good
    FPS: 24


