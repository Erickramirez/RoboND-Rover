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

[image1]: ./misc/rover_image.jpg
[image2]: ./calibration_images/example_grid1.jpg
[image3]: ./calibration_images/example_rock1.jpg 

![alt text][image1]
![alt text][image2]

### Notebook Analysis
#### 1. Run the functions provided in the notebook on test images (first with the test data provided, next on data you have recorded). Add/modify functions to allow for color selection of obstacles and rock samples.
##### 1.Calibration Data
Read in and display example grid and rock sample calibration images.
##### 2.Perspective Transform 
it is using a function called *perspect_transform*, this function takes as inputs an image, as well as source (src) and destination . After that it will apply the cv2.warpPerspective function with the return of the perspective transform and the image. 
##### 3.Color Thresholding
It is using the *color_thresh()* function which is used for   out by color_thresh() which shows the navigable terrain. The next task is to understand how to identify rocks. Using one of the rock sample images, I performed a pixel analysis to determine the appropriate thresholds for Red, Green, and Blue pixels that create the yellow of the rock.

##### 4.Coordinate Transformations
##### 5.process_image

First I took a series of images from the simulator and stored them in dataset/IMG/. I then stepped through the notebook process on the test data given and then on the images I took.

The first modifications were in perspect_transform, simply using the process learned in class.

Next, I added two functions color_match and color_inverse_thresh. Color match took the input color and compared the difference against a threshold. Simple but it worked for this example. One problem with it is that the color should be recognized whether it is bright or dark, so it should have some kind of normalization. I added color_inverse_threshold to check that a color is less than the given color. It is used to detect walls. I could not use the given color_thresh because it does not distinguish between black and white, both having similar rgb profiles.


#### 2. Populate the `process_image()` function with the appropriate analysis steps to map pixels identifying navigable terrain, obstacles and rock samples into a worldmap.  Run `process_image()` on your test data using the `moviepy` functions provided to create video output of your result. 
And another! 


### Autonomous Navigation and Mapping
#### Check the following video about result:
#### How to improve the result obtained:


