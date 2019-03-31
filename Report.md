# Advanced Lane Finding

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

The notebook corresponding to this project is `ALF.ipynb`

## Camera Calibration

1. Calculated a `nx*ny` 2D grid and then converted it into a 3D `object_point` with `z=0`.
2. Used `cv2.findChessboardCorners()` to find the corners of the chessboard image and made it into `image_point`.
3. Used `cv2.calibrateCamera()` to find the Camera Matrix and Distortion coefficients.

Then looped through all the images to further enhance the CameraMatrix and to have better calibration.
Used `cv2.undistort()` to undistort the images. Example provided below:

## Perspective Transform

`src = np.array([[585,460],[203,720],[1127,720],[695,460]],dtype=np.float32)` </br>
`dst = np.array([[320,0],[320,720],[960,720],[960,0]],dtype=np.float32)`

Used the above points to find the Perspective Transform Matrix and the Inverse Perspective Transform Matrix.
An example of perspective transformed image provided below:

## Thresholding

Used absolute direction gradient threshold, total gradient magnitude threshold, direction threshold and S-Channel threshold.
The exact threshold values and the function used to create a binary threshold has been provided in the notebook.

### Find Lane pixels

Initialised `fact = 2`
Takes the `(1 - 1/fact)` part of the image from bottom and finds lane pixels based on window search, </br>
shifting mean window position to new point `if pixels found in window > minpix`.

Before using window search, to initialise the centres of left and right window it draws a histogram and </br>
then uses the maximum column in each of the left and right half of the image for it.

The problem with the above approach was found in the example below, where the some non-lane pixels got activated </br>
and the actual lane pixels were in the top half of the image.

So, a thresholding of 800 has been used around the found column within a window of 100 for asserting it to be a </br>
starting position. If this condition is not met, then we do `fact = fact*2`, i.e increase our search space.

### Line Class

A line class has been created to keep track of various attributes of the line during lane identification in a video </br>
to reduce computation of everything again from scratch.

















