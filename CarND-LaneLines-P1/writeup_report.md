# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/original.jpg "Original"
[image2]: ./test_images_output/gray.jpg "Grayscale"
[image3]: ./test_images_output/blur_gray.jpg "Blur Grayscale"
[image4]: ./test_images_output/edges.jpg "Edges"
[image5]: ./test_images_output/masked_edges.jpg "Masked Edges"
[image6]: ./test_images_output/broken_line_image.jpg "Broken Line"
[image7]: ./test_images_output/line_image.jpg "Line"
[image8]: ./test_images_output/lines_edges.jpg "Lines on Original"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 steps. 

Step 1. Read image file (or read a frame from video clip).
![alt text][image1]

Step 2. Convert image to grayscale.
![alt text][image2]

Step 3. Apply Gaussian smoothing to blur grayscale image.
![alt text][image3]

Step 4. Apply Canny to detect edges.
![alt text][image4]

Step 5. Apply masked image to get edges in the region of interest.
 - The region of interest is the area where the lanes are exist in front of the car.
![alt text][image5]

Step 6. Apply Hough transform. 
 - Since the Hough transform is calculated based on edges detected by Step 5, the output is multiple segmented lines. 
 - For example, if the original image has white broken lines, the `draw_lines()` function draws a line per each broken lines. See below image.
![alt text][image6]

 - For Self-driving car, we want to draw a single solid line on the left and right lanes. In order to do it, I modified the `draw_lines()` function with simple math.
 - The Linear equation is `y = slope * x + y_intersept`. 
 - Since the Hough transform returns a pair of two points, (x1, y1) and (x2, y2), I could calculate `slope` and `y_intersept`. 
    If `slope` is positive, it's for left lane, otherwise right lane. 
 - After calculating the average of `slope` and `y_intersept` for the left and right lanes, I draw a single solid lines on the left and right lanes. 
![alt text][image7]

Step 7. Synthesize lines on the original image.
![alt text][image8]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when lanes are curved. The `draw_lines()` function is based on Linear equation. If lanes are curved, each slope of edge segements is very different. When a solid line is drawn based on the average of slopes, that'd be wrong.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use the Quadratic equation to draw lines, instead of the Linear equation. The Quadratic equation covers Curve as well as Line, so that would be more appropriate.