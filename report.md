# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps:

First, I converted the images to grayscale, using `grayscale` function.

Second, I used `gaussian_blur` function with `kernel_size = 5` is implemented to blur the image.

Third, I used `canny` function to do the edge detection

Fourth, `region_of_interest` is called to mask the image.

Fifth, I called `hough_lines` to do the hough transformation and it will output line segments.

At last, `weighted_img` is called to draw the detected line segments on the original image.


In order to draw a single line on the left and right lanes, I modified the draw_lines() function by sorting the line segments into groups (whether the line segment belong to left line or right line or none) with their gradient:

if the absolute value of the gradient of current line segment is less than 0.5, it is almost a horizontal line and we skip.

Otherwise...

if the gradient of current line segmenmt is less than 0, we append this line segment to left group 

if the gradient of current line segmenmt is greater than 0, we append this line segment to right group


We use `np.polyfit` to get the linear function parameter and use `np.poly1d` to construct the linear function. By setting up the vertex y coordinate, we can obtain the vertex x coordinates. 

After sorting them into an `np.array`, we can use `cv2.line` to draw the lines.


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the road is curved. Since we are fitting the lines into linear functions, the system could only detect straight lines. However, when the curvature of the road is huge, the lane line almost obeys a second or third order polynomial spline, there would potentially be a mismatch from the output of our project.

Another shortcoming could be a lack of flexibility. Even when we make it possible for the system to detect the curve line, the system still lacks the intelligence of distinguishing road conditions, i.e., it doesn't know when to switch to curve lane detection mode and when to switch to straight line detection mode.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to set up a scheme for the system to detect curve lanes.

Another potential improvement could be to provide pipelines to let the system distinguish different lane types.
