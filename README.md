# **Finding Lane Lines on the Road** 

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./CarND-LaneLines-P1-master/test_images/output_solidWhiteCurve.jpg "Lines"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 steps, as described below:
1.  The select_white_yellow(image) function uses the HSL colorspace to filter for white and yellow lines.
2.  Next, the image was grayscaled by the grayscale(white_yellow) function
3.  A Gaussian filter was applied to the image with the gaussian_blur(gray, 3) function, to blur the image slightly, accentuating the lines.
4.  Edges were drawn on the grayscaled image with the canny(gray, 50, 150) function.
5.  A Region of Interest ROI was select with vertices using percentages of the image.shape: np.array([[(.51 * imshape[1], imshape[0] * .58), (.21 * imshape[1], imshape[0] * .78), (0, imshape[1]), (imshape[1], imshape[0])]], dtype = np.int32), and applied as such: region_of_interest(edges, vertices)
6. Hough was run on the edge detected image using the hough_lines(file, masked_edges, 1.3, np.pi/180, 35, 5, 2) function, which also calls the draw lines function to draw the Hough lines on the image or frame.
7.  Lastly, the weighted_img(line_image,image) function draws the lines on the original image.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by using some averaging and weights to get rid of "jitter" in the videos. Each line segment is classified as right and left lines based on their slopes. 
The average slope and intercept for each line was found, using the lengths as the weights so that there is bias toward longer segments.
To eliminate frame jitter in the drawn lines in videos, line information from the previous frame is used to calculate a weighted average line. A distinction is made between image and video files based on the file extesions.

![image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when lighting is low. and when there is no left line.

Another shortcoming could be not accommodating cases where there is no left line.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to include HSV colorspace filtering for even better line detection.
