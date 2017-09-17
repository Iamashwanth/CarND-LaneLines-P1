# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

Overview of the pipeline to find lanes:
* Converting the image to gray scale.
* Applying gaussain blur to smoothen the image and remove spurious edges.
* Canny to detect the edges.
* Mask the edges over a region of interest.
* Run this image through hough tranform to detect the lines in region of interest.

Following are the changes I made to extrapolate the lines to a single left and right lane:
* Sorting out lanes into left and right ones based on their slopes.
* Computed a weighted sum over slopes and intercepts using the line lengths. Initially I was just computing a mean over slopes and intercepts, but that resulted in a skewed output because of the small outlier lines.
* Working through the optional section, I realized that maintaining running averages of mean and intercept could help stabilize the lines over time. But since the current value is given a weight of 0.1, during the intial few iterations - slopes and intercept values could be diminished and result in wrong lanes. So I added a bias term which cancels out the weight of 0.1 during the intial iterations and has a diminishing effect over time.

### 2. Shortcomings with the current pipeline

* Sorting lines based on slope values would not work if the lanes are curved.
* Current pipeline is not able to correctly handle a cross section of road with horizontal lines from the optional challenge video.
* Also the region of interest is not optimally configured to work with optional challenge video. Edges from the railing are contributing to the slopes resulting in skewed lines.


### 3. Possible improvements to the pipeline

* Since the lines tend to curve it would be better to fit a second order polynomial to the edges. I have seen people using hough curves to do this. Even the mask can be applied over a curved region of interest.
* inRange function can be used to filter out the white and yellow lanes in region of interest and add more weight to that part of image. This would diminish the contribution from horizontal lanes across the road.
