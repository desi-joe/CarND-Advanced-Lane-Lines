#Advanced Lane Finding Project

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./imgs/chessboard-corners.png "chessboard corner detection"
[image2]: ./imgs/undistorted.png "distortion corrected image"
[image3]: ./imgs/warped.png "birds-eye-view"
[image4]: ./imgs/warped-binary.png "Binary Threshold"
[image5]: ./imgs/color_lane.png "hilighted lane"


### Writeup / README


#### 1. Camera Calibration

The code for this step is contained in the third code cell of the IPython notebook located in "./advanced-Lane-finding.ipynb".  


I defined the following parameters for the chessboard images:

* channels = 3
* chessboard_rows = 6
* chessboard_columns = 9

I created a calibration_helper(cal_image) function which in turn uses cv2.findChessboardCorners finds the chessboard corners. Corners for each image is appended to img_points. 


![alt text][image1]

#### 2. Distortion-corrected image.

Next, in my undistort(image) function I call cv2.calibrateCamera with the img_points and obj_points to generate the camera calibration matrix (mtx). 
mtx is passed to cv2.undistort alone with image to get a get an undistored image.

Here is an example of undistored image
![alt text][image2]

#### 3. perspective transform (birds-eye-view)

I transform the undistored image to birds-eye-view using cv2.getPerspectiveTransform.  

I am using the following source and distination point for perspetive transform. 

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 200, 720      | 350, 720       | 
| 570, 470      | 350, 0      |
| 720, 470     | 980, 0      |
| 1130, 720      | 980, 720        |


here is an example of the birds-eye-view:


![alt text][image3]

#### 4. Binary threshold transform

Next step in our pipeline is to get a binary threshold from the birds-eye-view image. In my binary_thresholds(image) function, I am taking the S channel of the HLS channels, L of the LUV channels and B of the RBG channels. All the channels are combined to produce the binary threshold.  

Here is an example of the binary threshold image: 
![alt text][image4]




#### 5a. Identifying lane-line pixels and fit their positions with a polynomial

I use a sliding window search on the binary threshold image to find the lane pixels for both left and right lane. Then using np.polyfit I find the left lane fit and right lane fit. This is done with the find_fit function in my code. find_fit_targeted is used when I have aready found the lane lines in previous frame. 


#### 5b. Radius of curvature of the lane and the position of the vehicle with respect to center.

In color_lane function I am using the left_fit and right_fit values from find_fit function to generate lane curvature and vehicale position in respect to center. 

I am also highlighting the lane area in this function. 

Here is an example of the highlighted portion of the lane and lane curvature and vehicle position in respect to center. 
![alt text][image5]



#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

In the color_lane function I am calling cv2.fillPoly to to highlight the lane. Then using cv2.getPerspectiveTransform to perspective transform the highlighted lane and then finally cv2.addWeighted to merge it with the original image. 

![alt text][image5]

---

### Pipeline (video)

#### 1. Here is a link to the final video.

Here's a [link to my video result](./output_videos/project_video_output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

For me the challenging part was to understand how the sliding window techniq works and then how we can identify the left and right lane line fit.  

I was not able finish the challenge video due to lack of time.  I needed to spent more time to make the sanity_check code robust. With a robust sanity check and use of the Lane class I should be able to process the challenge video.    
