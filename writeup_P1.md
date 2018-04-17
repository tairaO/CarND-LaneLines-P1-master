# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/writeup_example1.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I read an image and converted the images to grayscale, then I blured the gray scale image so that unimportant edges are not extracted in the next step. Next, I used Canny edge detection algorithm in order to detect good edges, and then masked unimportant area in the image. Finally, I used Hough transform so that I can detect only the lane lines.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function. The detail of the modifications is below.
First, I checked the slope of each line, and separate them to right or left. For example, if the slope value is less than zero, the line belongs to the left line group. However, sometimes wrong lines are detected, then it influenced badly in the average step below. So, I added new condition to detect lines. It is the x position condition. For instance, if the line belongs to left, the x value must be less than half of the image width. To sum up, I use slope and x value condition to group lines to left or right. Next, I used average and extrapolation algorithm to draw single lines. I used np.polyfit and np.poly1d which can calculate the least-squared line from the points. In my algorithm, the points are the vertices of each line. Finally I used cv2.line to draw single line.

The picture below is the result of my algorithm. As you can see, it detects lane lines correctly.


![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline

There are several shortcomings in my algorithm. One potensial shortcoming would be the misdetection of lines of the straight object in unmasked area, like shadows, sidewalls and so on. My algorithm cannot remove those lines. 

Another shortcoming is that my algorithm cannot deal with the camera position or zoom rate changes. Successful line detection of my algorithm heavily depends on masking process. For example, like challenge.mp4, when the car's bumper is always on the image, its lines are always detected and my algorithm cannot get correct lines.

Also, when the algorithm detects no line in each side, my algorithm returns error. This is because np.polyfit needs at least 2 points.



### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use the history. The line that is too far from the lane line before would be penalized.

Another potential improvement could be using majority rule. For example, the lines whose slope is much different from the others are removed, and I could draw average line only from majority lines.   
