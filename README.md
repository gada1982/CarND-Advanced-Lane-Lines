# CarND-Advanced-Lane-Lines

This project is done as a part of the Nanodegree - Self-Driving Car Engineer provided from Udacity. The outcome of the project is, ...

The goals / steps of this project are the following:
- Compute the camera calibration matrix and distortion coefficients given a set of chessboard images
- Apply a distortion correction to raw images
- Use color transform, gradients, etc., to create a thresholded binary image
- Apply a perspective transform to rectify binary image ("birds-eye view")
- Detect lane pixels and fit to find the lane boundry
- Determine the curvature of the lane and vehicle position with respect to center
- Warp the detected lane boundries back onto the original image
- Output visual display of the lane boundries and numerical estimation of lane curvature and vehicle  position

# Rubric Points
### Camera Calibration

#### 1. Compute camera matrix and distortion coefficients and check on one of the calibration images as a test:

The code for the code is in the first code cell of the iPyhton notebook P4.jpynb.

- Prepare "object points", which will be the (x, y, z) coordinates of the chessboard corners. It is assumed the chessboard is fixed on (x, y) plane at z=0, such that the object points are the same for each calibration image.
TODO more
- Use the output *objpoints and *imgpoints to compute the camera calibration and distortion coefficients by using *cv2.calibrateCamera(). 
- Apply distortion correction to the own sample image by using *cv2.undistort() and show the result

### Pipeline (single image)
#### 1. Apply distortion correction to images:
TODO

#### 2. Create a binary image using color transfer and gradients:
TODO

#### 3. Apply perspective transform to rectify images:
TODO

#### 4. Identify lane line pixels in the rectified image and fit with a polynomial:
TODO

#### 5. Estimate the radius of the curvature of the road and the position of the vehicle with respect to center of lane:
TODO

### Pipeline (video)
#### 1. Use the pipeline established with a single image for the video:

### Discussion










## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)


In this project, your goal is to write a software pipeline to identify the lane boundaries in a video, but the main output or product we want you to create is a detailed writeup of the project.  Check out the [writeup template](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) for this project and use it as a starting point for creating your own writeup.  

Creating a great writeup:
---
A great writeup should include the rubric points as well as your description of how you addressed each point.  You should include a detailed description of the code used in each step (with line-number references and code snippets where necessary), and links to other supporting documents or external references.  You should include images in your writeup to demonstrate how your code works with examples.  

All that said, please be concise!  We're not looking for you to write a book here, just a brief description of how you passed each rubric point, and references to the relevant code :). 

You're not required to use markdown for your writeup.  If you use another method please just submit a pdf of your writeup.

The Project
---

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

The images for camera calibration are stored in the folder called `camera_cal`.  The images in `test_images` are for testing your pipeline on single frames.  To help the reviewer examine your work, please save examples of the output from each stage of your pipeline in the folder called `ouput_images`, and include a description in your writeup for the project of what each image shows.    The video called `project_video.mp4` is the video your pipeline should work well on.  

The `challenge_video.mp4` video is an extra (and optional) challenge for you if you want to test your pipeline under somewhat trickier conditions.  The `harder_challenge.mp4` video is another optional challenge and is brutal!

If you're feeling ambitious (again, totally optional though), don't stop there!  We encourage you to go out and take video of your own, calibrate your camera and show us how you would implement this project from scratch!
