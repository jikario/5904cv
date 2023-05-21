# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

This project is part of Udacity's Self-Driving car Nanodegree.

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Provide a reflection on the work in a written report
---

### Reflection

### 1. Pipeline description

The pipeline consisted of six steps. One of the test images will be used to describe the pipeline, and it is show below.

<img src="test_images/solidYellowCurve2.jpg" alt="test image"/>

First, the `color_focus` function is used to keep only shades of yellow and of white in the image, with the rest in black.

<img src="test_images_output/image3color_focused.png" alt="test image after color detection"/>

This image is then converted to grayscale using the `grayscale` function,
<img src="test_images_output/image3grayed.png" alt="grayscale image"/>

and then blurred with the `gaussian_blur` function, in order to remove uncessesary edges. This is shown below.

<img src="test_images_output/image3blurred.png" alt="blurred grayscale image"/>


The result is then passed onto the canny edge detection function, `canny`, which returns an image mask with the edges in white and the rest in black, as seen below. 

<img src="test_images_output/image3cannyed.png" alt="image after canny edge detection"/>

The next step would be to determine the region of interest, where the lanes are most of the time. In order to do that, vertices were created, and a polygon was drawn to show to suitability of the vertices, as seen below.

<img src="test_images_output/image3highlighted.png" alt="the region of interest"/>

This drawn region of interest was found to be suitable, and therefore, the process can resume.

The region of interest was applied to the canny edge detection output image using the `region_of_interest` function, and the output was as follows.

<img src="test_images_output/image3focused.png" alt="edges in the region of interest"/>

The `draw_lines` function was used to draw the lines on the image above, and the result was overlayed on the initial image. It is important to notice that this is the same function that was provided, that simply draws the lines passed by the `hough_lines` function, after being filtered by the `filter_lines` function. The `filter_lines` function takes the lines from the `hough_lines` output, removes horizontal and vertical, as well as lines with other abnormal slopes and vertical offsets, and returns either the filtered lines in a list or the mean slopes and vertical offsets for the right and left lanes, depending on what is passed as the `summary_required` parameter. This is shown below. 

<img src="test_images_output/image3with_lines.png" alt="Image after drawing the line segments"/>

A Hough tranform for line detection is then applied again on the image above the previous one, using the original `cv2.HoughTranformP`, with the returned lines being passed to the `draw_lanes` function. This is the function responsible for the final step of the pipeline, which is to draw the lane lines on the image of the road. This function takes the lines, calls the `filter_lines` function and receives the mean slopes and vertical offsets for the right and left lanes. With this, it calculates the x, y points required to draw the two lane lines in the region of interest, with the result shown below.

<img src="test_images_output/image3final_result.png" alt="Image after drawing the line segments"/>

In this image, purple is used for better contrast with yellow lines.

This pipeline is then used to draw the lanes on the videos. 


There is also the `addFrameN` function, that puts the frame number on the videos, in order to make it easier to find and fix the video rames with problems.

This pipeline was found to work quite well with all the videos, including the challenge video.
[<video src="test_videos_output/challenge.mp4" alt="challenge video" width="100%" />](test_videos_output/challenge.mp4)

However, it is not perfect, as seen in the following section.

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when at least on one side of the lanes, there are no lines with acceptable slopes, or the lines are too short. This would give errors, and the frame would be black.

Another shortcoming is that on some frames of the `challenge` video, the calculated line slopes would be far from the real one in the video.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to make the lane lines oscillate less from one video frame to another.

Another potential improvement could be to add better error handling.

It would also be better if the region of interest could extend a little bit further forward on the road.
