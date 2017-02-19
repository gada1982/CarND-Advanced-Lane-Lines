# Introduction
This project is done as a part of the Nanodegree - *Self-Driving Car Engineer* provided from Udacity. The outcome of the projct is, ...

The goals / steps of this project are the following:
- Compute the camera calibration matrix and distortion coefficients given a set of chessboard images
- Apply a distortion correction to raw images
- Use color transform, gradients, etc., to create a thresholded binary image
- Apply a perspective transform to rectify binary image ("birds-eye view")
- Detect lane pixels and fit to find the lane boundry
- Determine the curvature of the lane and vehicle position with respect to center
- Warp the detected lane boundries back onto the original image
- Output visual display of the lane boundries and numerical estimation of lane curvature and vehicle  position

# Outline
1. Requirements
2. Files
3. Camera Calibration
4. Undistort Images
5. Binary Threshold
6. Image Transformation - Bird Eye View
7. Detect Lane Lines - Fit Polynomial
8. Lane Curvature and Vehicel Offset
9. Pipeline for Single Images
10. Pipeline for Video
11. Conclusion

# 1. Requirements
- Python 3.5
- Environment [CarND-Term1-Starter-Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) provided by Udacity
- [Project Data] (https://github.com/udacity/CarND-Advanced-Lane-Lines) provided by Udacity

# 2. Files
- TODO
- README.md - Explains the structure of the software and the approach to solve the problem 

# 3. Camera Calibration
When a camera takes pictures in the real world (3D) it transforms the images to 2D. This transformation is not perfect. The quality may very (quality of camera, lenses, area within the image, ...) but there is always distortion. Disturbed images would produce a wrong localization of the vehilce in this project.

There are different types of distortion like radial distortion or tangential distortion.

To get undistorted images, first Correction Coefficients have to be calculated.

To do this we use a set of chessboard images (provided by Udacity), which are taken with the same camera as in the car. This images from different views are analysed and the corners are marked.

TODO Insert Image with chessboard corners marked

The code for the code is in the second code cell of the iPyhton notebook P4.jpynb.

- Prepare "object points", which will be the (x, y, z) coordinates of the chessboard corners. It is assumed the chessboard is fixed on (x, y) plane at z=0, such that the object points are the same for each calibration image.
TODO more
- Use the output *objpoints and *imgpoints to compute the camera calibration and distortion coefficients by using *cv2.calibrateCamera(). 
- Apply distortion correction to the own sample image by using *cv2.undistort() and show the result

# 4. Undistort Images


# 5. Binary Threshold


# 6. Image Transformation - Bird Eye View


# 7. Detect Lane Lines - Fit Polynomial


# 8. Lane Curvature and Vehicel Offset


# 9. Pipeline for Single Images

The images for camera calibration are stored in the folder called `camera_cal`.  The images in `test_images` are for testing your pipeline on single frames.  To help the reviewer examine your work, please save examples of the output from each stage of your pipeline in the folder called `ouput_images`, and include a description in your writeup for the project of what each image shows.

# 10. Pipeline for Video

The video called `project_video.mp4` is the video your pipeline should work well on.  

The `challenge_video.mp4` video is an extra (and optional) challenge for you if you want to test your pipeline under somewhat trickier conditions.  The `harder_challenge.mp4` video is another optional challenge and is brutal!

# 11. Conclusion

