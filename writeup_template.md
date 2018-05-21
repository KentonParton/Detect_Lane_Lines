# **Finding Lane Lines on the Road** 

## Writeup Template

**Finding Lane Lines on the Road**

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on the project


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Pipeline Description

Steps Involved
1. import colored image
2. transform colored image to gray scale
3. set kernal size
4. apply gaussian blur
5. set low and high thresholds
6. apply canny transform
7. select region of interest
8. Set params for hough transform & draw lines
9. return AddWeighted


The main approached in this project was to detect lane lines on a road. This was acheived primarily through OpenCv's Canny Edge Detection, and Hough Line Transform. 

First, a color image was transformed to grayscale and blurred. Using an upper and lower threshold, Canny Edge Detection could then be used to detect the changes is color gradients; therefore, detecting the edges needed. Next, a mask was created by specifying the region of the image that we are interested in; which is the section containing the lines of the road. After the edges and mask had been merged, the following variables were created for the Hough Line Transform function:
* rho, theta, threshold, min_line_len, max_line_len

Within hough_lines() a function called draw_lines() was responsible for drawing two lines. One for the left lane and another for the right lane. First, the points(X, Y) returned by hough_lines(), were catergorized into a left or right lane property based on their gradient.

Then using the formula y = mx + c, the slope(m), y-intercept(b), y-value(y), and x-value(x) were calculated in order to draw lines over the left & right lanes on the image. 


### 2. Potential Shortcomings of my Pipeline() Implementation

One of the issues of this approach is that it performs poorly when the road curves as the line being drawn is straight. This results in a very inaccurate representation of the road lanes. 

Another shortcoming is that changes in road color can have a negative impact on identifying the edges of the lane lines. This will negatively impact the calculated gradient for the lane line. An example of this would be when the road changes from black to grey or when road signs are present such as ones indicating two lanes merging.

Furthermore, this approach does not take lane changes into account as the region of interest selects the area in-front of the car where the lane lines are present. If the car did change lanes, lines would either be poorly drawn or not at all.


### 3. Suggested Improvements for my Pipeline() Implementation

A possible improvement would be to use color detection and color ranges to create a mask, then apply Canny Edge Detection. This would allow for better lane detection. However, this still leaves the issue of curved lines when the road turns. One possible way to reduce the error would be to order the points gathered by hough_lines() then split the data into a series of sections. Then calculate the same y = mx + c line, find its mid-point and store it. Using the series of mid-points, you would then be able to draw a polynomial best fit line.

A particular performance bottleneck in the code is the “for loop” in draw_lane_lines(). This could be improved by creating a numpy implementation, as it is run in C, which is significantly fasted.

