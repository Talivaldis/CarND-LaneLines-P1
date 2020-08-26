# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

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
grayscale->gaussian_blur->canny->region_of_interest->hough_lines->weighted_img

My pipeline consisted of 6 steps. First, I converted the images to grayscale, then I used Gaussian blur.

I tried to use all available helper functions, and the appropriate time to use blur is after converting to grayscale.

This smooths the image, hopefully reducing the number of points thought to be edges in the following step. We hope to reduce noise.

I used the canny function for canny edge detection. This should give us a nice picture with lots of points where edges were detected.

I use the region of interest function to mask my area that I'm interested in.

I call hough_lines to get the image of just the lines drawn.

This is then combined with the initial input image with the weighted_img function.


draw_lines() function modification:


In order to draw a single line on the left and right lanes, I modified the draw_lines() function by parsing the lines.

I parsed the lines given by the houghLines fed in as input, A.K.A lines. I seperated them by slope.

Negative slope is the left lane and positive slope is right lane.

Our graph image if flipped twice. Once on each axis. In case of confusion. Blame the way images are stored and read.

After parsing left and right lanes. I use polyfit to fit a line to the data. I get the slope and intercept.

I get my y values from the size of the image. I want the bottom and a little less than half of the image for my y values.

My y is basically my height. These do not change, and I use them to calculate my x values for each line.

Using my y values along with my slope and intercept. I calculate my x values.

I use openCV to draw the lines. There are some saftey checks. 


y = mx+b is used to derive: x = (y-b)/m

I do this for both points on the bottom and roughly half of the image.


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when curves.

Another shortcoming could be faded lines.

This pipeline is only good for very well mantained roads with painted lines with viewing lighting conditions.

Also, roads that are straight. This pipeline has a lot of shortcommings with any deviation to conditions.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to not rely on slope, if one wanted to detect curved roads.

left lane may become positive and the right lane suffers a similiar issue in the other direction.

Another potential improvement could be to make canny detection ignore horizontal lines.

Techniques that reduce noise like thresholding would help in that challenge video.

The shadows need noise reduction of edges. I may have looked ahead, so I know sliding window is mentined.

Image transformations to help make it easier for our image processing to parse details(Bird view).

Using slope of the previous frame for each respective lane, for comparison.

Could throw out anything that deviates too much from previous slope. Lane shouldn't deviate heavily from frame to frame.

Keeping in mind this is for straight lanes.

Could throw out heavy outliers from detected lines.

Oh, applying the color selection with thresholding would probably improve it.

Those are some ideas. 