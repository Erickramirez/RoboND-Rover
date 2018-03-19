## RoboND-Rover
### Project: Search and Sample Return

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

### Notebook Analysis
#### 1. Run the functions provided in the notebook on test images (first with the test data provided, next on data you have recorded). Add/modify functions to allow for color selection of obstacles and rock samples.
##### 1.Data collection.
Read in and display example grid and rock sample calibration images. Note: in this case it is not necessary to 
##### 2.Perspective Transform 
it is using a function called *perspect_transform()*, this function takes as inputs an image, as well as source (src) and destination .  It is returning two values: warped (image transformed) and a mask.
* Define 4 source points, in this case, the 4 corners of a grid cell in the image above.
* Define 4 destination points (must be listed in the same order as source points!).
* warped = Use cv2.getPerspectiveTransform() to get M, the transform matrix.
* mask = Use cv2.warpPerspective() to apply M and warp your image to a top-down view.

##### 3.Color Thresholding
It is using the *color_thresh()* function. I modified this function in order to have a max and min threshold defined in rgb_thresh parameter. With the modification performed this function is useful to show the navigable terrain and the rocks.
For navigable terrain: `color_thresh(warped, ([160, 160, 160],[255,255,255]))` and for the rocks: `([110,110,0],[240,210,50]))`

The images processed are in RGB color model, in this case I performed an analysis on each color (Red, Green and Blue) for the appropriate thresholds.

The obstacles are only the rest of the values in found in the navigable terrain and it only applies to the mask (the area of interest obtained in the perspective transform) `obstacle = np.absolute(np.float32(threshed)-1)*mask`


##### 4.Coordinate Transformations
* `rover_coords()` is a function to convert from image coords to rover coords
* `to_polar_coords()` is a function to convert to radial coords in rover space
* `rotate_pix()`  is a function to perform rotation
* `translate_pix()` is a function to perform translation
* `pix_to_world()` Is a function to map rover space pixels to world space using the previous rotation and translation functions.


#### 2. Populate the `process_image()` function with the appropriate analysis steps to map pixels identifying navigable terrain, obstacles and rock samples into a worldmap.  Run `process_image()` on your test data using the `moviepy` functions provided to create video output of your result. 

##### 1.process_image
It has most of the changes performed, this section used the previous steps and the main aspects to get it right are:
* definition of source and destination points for perspective transform (I kept the same provided by Udacity)
* Apply perspective transform
* Define yaw, world_size and parameters. Most of this data comes from data = `Databucket()` and from `Rover` in the case of the file [perception.py](/code/perception.py)
* Apply color threshold to identify navigable terrain/obstacles/rock samples, it was applied according with the explanation of the step *3.Color Thresholding*, also. in some cases, the threshold wasn't good enought to return a valid value for terrain, then in the future it could be fixed with the following:
```
nav_pix = Rover.worldmap[:, :, 2] > 0  #if the current result is a valid terrain, then the obstacle has to be cleaned.
Rover.worldmap[nav_pix, 0] = 0 #it is cleaning the values on obstacles

```
Note: this also applies to [perception.py](/code/perception.py) and function `perception_step(Rover)`. The file [decision.py](/code/decision.py) wasn't modified 
The order for the data captured will be: rock samples, terrain and obstacles.
* Convert the rover-centric pixel values to world coords, it was performed with the Coordinate transform.
##### 2.final result from [Rover_Project_Test_Notebook.ipynb](/code/Rover_Project_Test_Notebook.ipynb)
* [![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/4OlmGjVkrZ4/0.jpg)](https://www.youtube.com/watch?v=4OlmGjVkrZ4)




### Autonomous Navigation and Mapping
#### Check the following video about result:
* [![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/AjGlZjWEh6Q/0.jpg)](https://www.youtube.com/watch?v=AjGlZjWEh6Q)
#### How to improve the result obtained:
* The steps to collect the samples and return them to the starting point wasn't implemented,
* for a better accuracy  of the model, I could test with other color model like HSV
* check the drive_rover.py for path planning, for instance it can get stuck on the rocks.



