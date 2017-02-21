# Introduction
This project is done as a part of the Nanodegree - *Self-Driving Car Engineer* provided by Udacity. The scope of this project is finding and marking lane lines on a given, recorded track. The recorded raw video and some used sample codes are provided by Udacity.

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
- Some code is taken out the material of the lectures of the Nanodegree *Self-Driving Car Engineer* provided by Udacity

# 2. Files
- TODO
- README.md - Explains the structure of the software and the approach to solve the problem 

# 3. Camera Calibration
When a camera takes pictures in the real world (3D) it transforms the images to 2D. This transformation is not perfect most of the time. The quality may vary (quality of camera, lenses, area within the image, ...) but there is always distortion. In this project, disturbed images would produce a wrong localization of the vehicle.

There are different types of distortion like radial distortion or tangential distortion.

To get undistorted images, first correction coefficients have to be calculated.

To do this, we use a set of chessboard images (provided by Udacity), which are taken with the same camera as in the car. This images from different views are analysed and the corners are marked.

The following image shows an example:
![chessboard](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/chessboard_corners.png)

The code for this functionality can be found in **CODE CELL 2** of the Jupyter notebook P4.jpynb.
Functions `get_calibration_data` and `cal_undistort`:
- A list of object points is created, which represent the x,y,z-coordinates in the real world. The chessboard doesn't move, z is fixed and only x and y may vary.
- For all chessboard-images (provided by Udacity) `cv2.findChessboardCorners(gray, (9,6), None)` is used to find a set of corners (x, y-coordinates) within the chessboard. The used images of the chessboard have 9 corners in x-dimension and 6 corners in y-dimension.
- Theese corner coordinates are added to a list of imgage-points, which represent the 2d points in image plane.
- The corner coordinates are added to a list of object-points too, which represent the 3d points in real world space.
- At all images, where the right number of corners have been detected, the corners are marked and one augmented example-image (with `cv2.drawChessboardCorners(img, (9,6), corners, ret)`) is shown above.
- The object-points and image-points are used for the undistortion of the images
- Finally `cv2.calibrateCamera(objpoints, imgpoints, gray.shape[::-1],None,None)` is used to get the camera matrix *mtx* and the distortion coeffcients *dist*.

# 4. Undistort Images
To undistor images the following steps are done. The code for this functionality can be found in **CODE CELL 2** and **CODE CELL 3** of the Jupyter notebook P4.jpynb.
Function `cal_undistort`:

- Usage of `cv2.undistort(img, mtx, dist, None, mtx)` to undistort.

The following image shows an chessboard-example: 
![chessboard_undistorted](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/undistorted_image_chessboard.png)

The following image shows an example (taken with the same camera) how this is applied to the lane-line-images:
![lane_lines_undistorted](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/undistorted_image_lane_lines.png)

# 5. Binary Threshold
To get a binary mask of the images the following steps are done. The code for this functionality can be found in **CODE CELL 4**, **CODE CELL 5**, **CODE CELL 6** and **CODE CELL 9** of the Jupyter notebook P4.jpynb.

Two different approches have been combined to get a properly working solution for the binary mask:
- Apply a color mask to a given image to filter out yellow and white parts of the image
- Use Sobel Gradients in x- and y-direction

### Color Mask
The code for this functionality can be found in **CODE CELL 4** of the Jupyter notebook P4.jpynb. Function `get_color_mask`:

- Convert image to HSV color space
- Define thresholds for white areas
- Mask out white areas
- Define thresholds for yellow areas
- Mask out yellow areas
- Combine both mask to detect yellow AND white areas within the image

The following image shows an example (taken with the same camera) how this is applied to the lane-line-images:
![Color_Mask](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/color_mask.png)

### Sobel Gradient
The code for this functionality can be found in **CODE CELL 5** of the Jupyter notebook P4.jpynb. Functions `abs_sobel_thresh`, `get_sobel_mask` and `get_combined_sobel_mask`:

- Convert image to HLS color space
- Get the L-Channel of the image
- Calculate Sobel Gradients in x- and y direction
- Define both gradients to own maks for the L-Channel
- Get the S-Channel of the image
- Calculate Sobel Gradients in x- and y direction
- Define both gradients to own maks for the S-Channel
- Combine the masks for L- and S-Channel

The following image shows an example of usage of Sobel Gradients:
![Sobel_Mask](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/sobel_filter.png)

### Combine all Masks
The code for this functionality can be found in **CODE CELL 6** and **CODE CELL 9** of the Jupyter notebook P4.jpynb. Functions `plot_image_with_color_mask_and_sobel_HLS` and `get_comb_filter`:

- Get the color mask for yellow and white
- Get the mask with sobel gradients for L- and S-Channel
- Combine both masks

The following image shows an example of usage of Sobel Gradients and Color Masks for a non-wraped image:
![Sobel_and Color Mask](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/color_and_soble.png)

The following image shows an example of usage of Sobel Gradients and Color Masks for a wraped image:
![Sobel_and Color Mask_wraped](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/comb_filter_wraped.png)

# 6. Image Transformation - Bird Eye View
One task within this project is to measure the curvature of the lane lines. To do this the camera images have to be transformed to have a top-down view (Bird Eye View).

The code for this functionality can be found in **CODE CELL 7** and **CODE CELL 8** of the Jupyter notebook P4.jpynb. Functions `warp_image` and `warp_image_int`:

- Points in the source images are defined
- Points in the destination image are defined
- A polynom is drawn with this points to show how this transformation is managed.
- `cv2.getPerspectiveTransform(points_src_float, points_dst_float)` is used to define the transformation matrix (M) from source source points to destination points
- `cv2.warpPerspective(image, M, image_size, flags=cv2.INTER_LINEAR)` is used to do the transformation
- `cv2.getPerspectiveTransform(points_dst_float, points_src_float)` is used to define the inverted transformation matrix (Minv) back from destination points to source points

The following tables show the source and destination points, which have been used for transformation:
![Transformation_points](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/points.png)

The following image shows an example for a straight street segment: 
![street_straight_warped](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/straight_street_warped.png)

The following image shows an example for a curved street segment: 
![street_curved_warped](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/curved_street_warped.png)

# 7. Detect Lane Lines - Fit Polynomial
After applying calibration, undistortion, thresholding, and a perspective transform to a street image we have a binary image where the lane lines stand out clearly. Now we have to decide explicitly which pixels are part of the lines and which belong to the left line and which belong to the right line.

The following image shows a binary image example with its histogramm:
![bin_histogramm](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/mask_and_histo.png)

To find the lane lines two different approches are implemented:
- Sliding Windows
  - Used to find the lane lines when it is not known where they are (e.g.: first frame or after losing search windows)
- Searching in a defined area
  - Used to find the lane lines when it is already known where to search (e.g.: in frames where with a robust fit in the previous frame)

The code for *Sliding Window Search* can be found can be found in **CODE CELL 10** of the Jupyter notebook P4.jpynb. Function `find_first`:
- Code is mostly taken form the Udacity sample code and modfied when it was neccessary
- 9 sliding windows are used over the hole height of the images
- Window margin of 100 is used
- At least 50 pixels have to be found to recenter the searching window
- `np.polyfit(lefty, leftx, 2)` and `np.polyfit(righty, rightx, 2)` are used to define a polynom 2nd order for both lane lines
  - done in the *pixel-world* and in *real-world (m)* 
- To flatten the polynoms over more the one video-frame a list was defined for taking the everage

The following image shows an example:
![find_first](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/find_first.png)

The code for the second search method (place where to expect the lane lines known) can be found in **CODE CELL 11** of the Jupyter notebook P4.jpynb. Function `find_next`:
- Code is mostly taken form the Udacity sample code and modfied when it was neccessary
- If it was not possible to get good fits in the preferred searching area, `find_first` is used again (to use the sliding window approach)
- `np.polyfit(lefty, leftx, 2)` and `np.polyfit(righty, rightx, 2)` are used to define a polynom 2nd order for both lane lines
  - done in the *pixel-world* and in *real-world (m)* 
- To flatten the polynoms over more the one video-frame a list was defined for taking the everage
  - Average is taken over the last 15 frames (if available)
- The polynoms for the left and right lane line is marked yellow in the output image to use it later on in the augmented image

The following image shows an example:
![find_next](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/find_next.png)

# 8. Lane Curvature and Vehicel Offset
The code for calculation of the lane curvature and the vehicel offset can be found in **CODE CELL 12** and **CODE CELL 13** of the Jupyter notebook P4.jpynb. Functions `calculate_curvature_pix` and `calculate_curvature_rw`:

`calculate_curvature_pix` is used to calculate the data in pixels, which is only done for testing. `calculate_curvature_rw` is used to calculate the data for the real world (m). To prepare the data for the real world, code is included in the functions `find_first` and `find_next`. In theese two functions a polynom with coefficients for the real world dimensions is calculated with the following conversion rates:
- ym_per_pix = 30/720 # meters per pixel in y dimension
- xm_per_pix = 3.7/700 # meters per pixel in x dimension

The curvature of a polynome is calculated the following way (A, B, C) are defined by `np.polyfit()`:
![calc_curv](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/calc_curv.png)

The curvature for the left and right lane are calculated seperately and the mean is taken for the final output.

To calculate the offset of the vehicle, the position where the left and right lane polynom cut the bottom of the camera image (nearest point) are taken as referance and are compared to the center of the image in x-direction (camera mounting point).

Theese information is printed on every frame of the augmented video.

# 9. Pipeline for Single Images
Finally a pipeline had to be built which take all these helper functions together. The pipeline takes a single raw image, does the calculation and augmentation and returns the augmented image.

The code for this functionality can be found in **CODE CELL 14** of the Jupyter notebook P4.jpynb. Function `generate_processed_image`:

- Undistort image
- Apply perspective transformation
- Get the final combined binary mask
  - Color mask for yellow and white parts
  - Sobel gradient for L- and S-Channel in HLS color space (x- and y-direction)
- Decide if sliding-window-search has to be used or the optimized search is possible
- Draw a polynom on the part of the image where the used lane lines is located
- Draw the left (red) and right (blue) lane line on part of the image where the single lane lines are located
- Augment original image with data about curvature and vehicle offset
- Augment original image with the small images of the sub-steps of the pipeline
  - Bird-Eye-View on the used lane line
  - Combined binary mask
  - Output of the search algorithm

The following image shows an example:
![final_image](https://github.com/gada1982/CarND-Advanced-Lane-Lines/blob/master/info_for_readme/final_image.png)

# 10. Pipeline for Video
Finally the pipeline has to be tested with videos provided by Udacity.

The video called `project_video.mp4` is the video the pipeline has to work properly to pass the project review.

The code for this functionality can be found in **CODE CELL 15** of the Jupyter notebook P4.jpynb.

[Project Video](https://youtu.be/dzjb8t82Jf8)

The videos `challenge_video.mp4` and `harder_challenge.mp4` are extra (and optional) challenges but these don't have to be used to pass the project review.

# 11. Conclusion
The most challenging part of the project was to try different approaches for defining binary mask which works properly. Different tries have been made.
- Color spaces (RGB, HSV, HLS)
- Magnitude of the gradients
- Direction of the gradients
- Sobel gradients
- And different combinations of the mentioned methods

A big improvement was possible by averaging over various frames from the video (15 taken). 

The choosen solution works well on the mandatory `project_video.mp4` but not at all at the optional videos `challenge_video.mp4` and `harder_challenge.mp4`.

Futher work has to be invested to get the optional part working in a satisfying way.


