# Advanced Lane Line Project
## 1.Camera Calibration
The first step of this project is to calibrate the camera. I used the chessboard iamges provided to do the calibration. The mtx and dist arrays are defined to be global variables in python since we need to use them in the image processing pipeline later. An example of the original test image and the undistorted image are shown below:

![test_undist_image](https://raw.githubusercontent.com/junfeizhu/CarND-Advanced-Lane-Lines/master/output_images/calibration_image.png)

## 2.Color and gradient threshold
After lots of experiments, to find out the lanes, I applied two color filter and one x-direction sobel filter to the undistorted image. This idea is also inspired by this [excellent post](https://chatbotslife.com/advanced-lane-line-project-7635ddca1960). Since the lanes are usually yellow and white, I first converted the image to an HSV color space and applied one yellow color filter and one white color filter to the image. Then I applied the x-direction sobel filter to the image. Finally, I did a bitwise addition of these three images to get the final binary image:

![thre_image](https://raw.githubusercontent.com/junfeizhu/CarND-Advanced-Lane-Lines/master/output_images/thre_image.png)

## 3.Crop image
Next, I cropped the image to get rid of the parts not needed for lane detectation:

![cropped_image](https://raw.githubusercontent.com/junfeizhu/CarND-Advanced-Lane-Lines/master/output_images/cropped_image.png)

## 4.Perspective Transformation
Next I did a perspective transformation to the image. To do that, I need to choose four source point. By looking at the straight line image, I chose these four points:[585, 450], [205, 720], [1100, 720], [695, 450].

![straight](https://raw.githubusercontent.com/junfeizhu/CarND-Advanced-Lane-Lines/master/output_images/straight.png)

I applied the perspective transformation to the binary image and got the following bird eye image:

![birdeye](https://raw.githubusercontent.com/junfeizhu/CarND-Advanced-Lane-Lines/master/output_images/birdeye.png)

## 5. Lane detectation
Next I use **detect_lines_sliding_window()** function and **detect_lines()** function to detect the line. These two functions are almost the same as the functions provided in the course except that I added some codes to check whether there is a fit in the image or not. I added these code because when applied to the project video, I did have some cases that there are no fits for the image given. In this case, I will only return an empty array for further action later. One example was shown in the following image:

![final](https://raw.githubusercontent.com/junfeizhu/CarND-Advanced-Lane-Lines/master/output_images/final.png)

## 6.Curvature

Next, I computed the curvature for both left lane and right lane. I also computed the offset.

## 7. Final processor
Finally, I put every step together to make the final image processor. As metioned before, it is possible that the algorithm cannot get a polynomial fit for the given image. If this is the case, I will just use the fit from the previous image instead. One snapshot of the video is shown below:

![video_snapshot](https://raw.githubusercontent.com/junfeizhu/CarND-Advanced-Lane-Lines/master/output_images/video_snapshot.png)

## 8.Discussion
When doing this project, the most difficult part I have is to choose what kinds of filter to use to detect the lane and how to decide the thresholds for these filters. This needed lots of experiments. I also read lots of posts to get inspiration about these filters.

When applied this processor to the challenge video, it didn't work well:

![challenge](https://raw.githubusercontent.com/junfeizhu/CarND-Advanced-Lane-Lines/master/output_images/challenge.png)

Compared to the project video, the vehicle in the challenge video is very close to the isolation belt in the road. The processor may detect the belt and its shade as a lane. Thus it did not work well.