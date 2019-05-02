

### Advance lane finding

---

`The output of the test images are present in output_images folder`

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Caliberation and Distortion correction
* Perspective Transform
* Color & Gradient Threshold
* Finding Lane pixels
* Calculating radius of curvature
* Pipelining



### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README


### Camera Calibration

#### 1.  The camera matrix and distortion coefficients

The code for this step is contained in the first code cell of the IPython notebook located in second code cell of P1.ipynb with function name `caliberation`.

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function. Please execute the cell block to see undistorted picture of `calibration1.png`.




#### 2. The color transforms, gradients 

I used a combination of color and gradient thresholds to generate a binary image, Initially I created a function `combine_threshold_gradient` to calculate combined threshold with S channel. But later move to combine color and gradients with `threshold_Channel` function.



#### 3.  Perspective transform 

The code for my perspective transform includes a function called `transform()`, which appears in 8th code cell of P1.ipynb.  The `transform()` function takes as inputs an image (`img`), as well as source (`src`), image size(`img_size`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
 src =  np.float32([[250,700],[1200,700],[550,450],[750,450]])
 dst = np.float32([[250,700],[1200,700],[300,50],[1000,50]])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 585, 460      | 320, 0        | 
| 203, 720      | 320, 720      |
| 1127, 720     | 960, 720      |
| 695, 460      | 960, 0        |


#### 4. Find lane-line pixels and their positions 


Then I create a function `find_lane_pixels` which runs in sliding histogram window which starts from the bottom of the binary image and upto the 3/4th part of the top. The function calculates the left pixels and right pixels along with the mid point vehicle from the direction left or right. These pixels are then paased into the function `lane_curve` which calculates the lines along with the curve by fitting the points into ploynomial fit of degree 2.

#### 5.  The radius of curvature of the lane 

The radius of curvature is calculated into function `lane_Area` which also plots the lane into the undistorted image. The radius is calulated with the help of `curve` calulated in `lane_curve` function.


#### 6. Pipelining

The pipelining includes undistorting the images then performing the perspective transformation which is then converted into binary image. The binary image is passed into the `find_lane_pixels` functions which precedes with `lane_curve` then `lane_Area`. Finally the image is plotted along with details.


### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The bit masking of the image require more work and error handling, as it is affecting other methods and faced various slicing issue after converting the images.


