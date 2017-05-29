# **Finding Lane Lines on the Road**

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consists of 5 steps. First, I converted the images to grayscale,
then I apply gaussian blur to smooth the image. The next step is to obtain edges using canny edge detection.
After obtaining edges, I apply a mask to clean up the edges that are not in the interests region.
With remaining edges, I use hough transformation to fit lanes to the image. The last step is to draw fitted lanes onto the raw image.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by replacing
the points pair with provided by hough transformation with points pair extends to the edge of the image. Given
image has height $h$, and original points pair $(x_1, y_1)$, $(x_2, y_2)$. The extrapolated points can be calculated as:

$$
k = (y_2 - y_1)/(x_2 - x_1)
$$

$$
b = y_2 - k * x_2
$$


Then:
$$
x_1' = (0 - b)/k
$$


$$
y_1' = 0
$$


$$
x_2' = (h - b)/k
$$


$$
y_2' = h
$$

Then I put the extrapolated points pair into the cv.line function. After drawing the lanes,
I apply the mask again the get of the lane that extends beyond the mask.

Below is an example for land detection:
<img src="/test_images/outfile.jpg" width="480" alt="Combined Image" />


### 2. Identify potential shortcomings with your current pipeline
One potential shortcoming would be what would happen when the there are mutiple different road marks.
Our canny edge detection can identify feature like pedstrain lanes and other stuff.
Those would introduce a lot of noise to our system.

Another shortcoming could be this pipeline heavily relies on mask.
Since masked region is predefined, it cannot be changed when the image structure dramtically changed.
For example, this will cause trouble when car is making a big turn.


### 3. Suggest possible improvements to your pipeline
A possible improvement would be to filter out fitted lanes that are not orientated in vertical direction.
This can help out reduce a lot of noises in a complex scene.
