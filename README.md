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

The following image shows an example:
![chessboard](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/chessboard_corners.png)

The code for this functionality can be found in the second code cell of the iPyhton notebook P4.jpynb.
Functions `get_calibration_data` and `cal_undistort`:
- A list of object points is created, which represent x,y,z-coordinates in the real world. The chessboard doesn't move (z is fixed) and only x and y may vary.
- For all chessboard-images (provided by Udacity) `cv2.findChessboardCorners(gray, (9,6), None)` is used to find a set of corners (x, y-coordinates) within the chessboard. The used images of the chessboard have 9 corners in x-dimension and 6 corners in y-dimension.
- Theese corner coordinates are added to a list of imgage-points, which represent the 2d points in image plane.
- The corner coordinates are added to a list of object-points too, which represent the 3d points in real world space.
- At all images, where to right number of corners have been detected, the corners are marked and the augmented images (with `cv2.drawChessboardCorners(img, (9,6), corners, ret)`) are stored in the folder *camera_cal_corners_found* TODO richtig?
- The object-points and image-points are used for undistortion of images
- Finally `cv2.calibrateCamera(objpoints, imgpoints, gray.shape[::-1],None,None)` is used to get the camera matrix *mtx* and the distortion coeffcients *dist*.

# 4. Undistort Images
To undistor images to following steps are done. The code for this functionality can be found in the second code cell of the iPyhton notebook P4.jpynb in the function `cal_undistort`:

- Usage of `cv2.undistort(img, mtx, dist, None, mtx)` to undistort.

The following image shows an chessboard-example: 
![chessboard_undistorted](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/undistorted_image_chessboard.png)

The following image shows an example (taken with the same camera) how this is applied to the lane-line-images:
![lane_lines_undistorted](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/undistorted_image_lane_lines.png)




# 5. Binary Threshold
TODO


# 6. Image Transformation - Bird Eye View
One task within this project is to measure the curvature of the lane lines. To do this the camera images have to be transformed to have a top-down view (Bird Eye View).

The code for this functionality can be found in the TODO ????? code cell of the iPyhton notebook P4.jpynb in the functions `warp_image` and `warp_image_int`:

- Points in the source images are defined
- Points in the destination image are defined
- A polynom is drawn with this points to show how this transformation is managed.
- `cv2.getPerspectiveTransform(points_src_float, points_dst_float)` is used to define the transformation matrix (M) from source source points to destination points
- `cv2.warpPerspective(image, M, image_size, flags=cv2.INTER_LINEAR)` is used to do the transformation
- `cv2.getPerspectiveTransform(points_dst_float, points_src_float)` is used to define the inverted transformation matrix (Minv) back from destination points to source points

The following image shows an example for a straight street segment: 
![street_straight_warped](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/straight_street_warped.png)

The following image shows an example for a curved street segment: 
![street_curved_warped](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/curved_street_warped.png)

# 7. Detect Lane Lines - Fit Polynomial


# 8. Lane Curvature and Vehicel Offset


# 9. Pipeline for Single Images

The images for camera calibration are stored in the folder called `camera_cal`.  The images in `test_images` are for testing your pipeline on single frames.  To help the reviewer examine your work, please save examples of the output from each stage of your pipeline in the folder called `ouput_images`, and include a description in your writeup for the project of what each image shows.

# 10. Pipeline for Video

The video called `project_video.mp4` is the video your pipeline should work well on.  

The `challenge_video.mp4` video is an extra (and optional) challenge for you if you want to test your pipeline under somewhat trickier conditions.  The `harder_challenge.mp4` video is another optional challenge and is brutal!

# 11. Conclusion

