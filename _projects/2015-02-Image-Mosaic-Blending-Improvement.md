---
layout: project
title: Image Mosaic - Blending Improvement
date: March 2015
image: https://raw.githubusercontent.com/JoshMarino/droplet_manipulation/master/period_2_bouncing.png
---

## Overview

Image mosaic, or image stitching, is the process of combining a set of images that contain overlapping fields of view into one image, often called a panorama. After applying image stitching to a set of images, it was found the transition between overlapping images was noticeable and we set out to improve upon this. Three methods were attempted at blending the overlapping field of view between a pair of images: (1) average weighted sum blend, (2) pyramid blending, and (3) distance function weighted sum blending. The third method proved best in which each image was multiplied by a weighting function which decreases monotonically across its border. The resulting images are then summed to form the mosaic.


### Image Stitching - Correspondences, Homograph Transformation, Warping

The first step in image stitching is finding correspondences between pairs of images. There are two methods for finding correspondences, correlation-based or feature-based. Correlation-based is checking if one location in an image appears to be the same in another image, while the latter employ pre-processing stages to extract features such as corners, edges or blobs [1]. Feature-based matching is less computationally expensive due to containing a much lower subset of the image. Two common methods for feature-based matching are Scale-Invariant Feature Transform (SIFT) and Speeded Up Robust Features (SURF). SURF relies on integral images for image convolutions; by building on the strengths of the leading existing detectors and descriptors (in casu, using a Hessian matrix-based measure for the detector, and a distribution-based descriptor); and by simplifying these methods to the essential. This leads to a combination of novel detection, description, and matching steps [2].

Once the correspondences have been matched for a pair of images, a homograph transformation needs to be found. This homography transforms the matches in one image to the corresponding matches in the other image. Random Sampling Consensus (RANSAC) can be used to compute the homograph transformation by selecting 4 pairs of correspondences at random until maximize number of inliers (other correspondences well fit by the homography) [3,4]. After the optimal homograph transform has been selected, one of the images is warped to the other image using homography H, the homograph transformation from warped image to base image.

![normal_stitching](https://raw.githubusercontent.com/JoshMarino/image-stitching/master/normal_stitching.jpg)

In order to blend the two images for a more seamless transition between images, the overlapping field of view had to first be determined. This method included warping the two images together to make a preliminary panorama. Once the preliminary panorama was created, we used the homograph transformation to determine the overlapping field of view. Blending is now possible with both images containing only the overlapping field of view.

![overlapping_field_of_view](https://raw.githubusercontent.com/JoshMarino/image-stitching/master/overlapping_field_of_view.png)


#### Average Weighted Sum Blend

This was performed using the following equation: 

```
Image_blend= α*Image_left+β*Image_right
```

where α and β range from [0,1], and they were both selected as 0.5 for an averaging effect. The result of a few tests using the average weighted sum blend is shown below. Apparent from the blended results is that an average weighted sum blend is not enough to create a seamless transition from one image to the next. A few reasons for this are because any minor differences in matching correspondences were enhanced and became more apparent, blending along the edges of the images created lines in the blended image, and clarity was lost in the blended image compared to the original images.

![average_weighted_sume_blend](https://raw.githubusercontent.com/JoshMarino/image-stitching/master/average_weighted_sum.png)


#### Pyramid Blending

Pyramidal blending for the overlapping field of view in the panorama did not work as we had expected it to. The sample code provided in [6] for Python using the module OpenCV2 required a square image, same width as height. Due to the nature of image stitching, a square image for the overlapping field of view was very rare. Preliminary attempts were made to convert the overlapping field of view square, but without any results. Numerous online searches suggested resizing the image to a square by finding the largest area of the source image while maintaining the same aspect ratio, and then cropping off any excess area. However, cropping the overlapping field of view would render the panorama incomplete. Due to the short time period of this project, we decided to not continue down this road any further.


#### Distance Function Weighted Sum Blending

Another method for blending is a variation of the average weighted sum blend. Rather than using constant values for alpha and beta, an improved blending function could be used. The weighted average method [8] was used in which each image was multiplied by a weighting function which decreases monotonically across its border. The resulting images are then summed to form the mosaic. The region of interest is the overlapping field of view, T. Approximation of the weighted average function in this region T was done with the error function. However, the error function ranges from [-1,1] in the y-direction, and of concern is a weight from [0,1]. Simple scaling of the error function produced the appropriate weighting function. Represented in Figure 10 are the unaltered error function and the scaled error function. The scaled error function is as follows:
 
```
β(i) =  1/2*(erf⁡[4*(i/T)-2]+ 1).
α(i) = 1 - β(i).
```

where erf [] is the error function, T is the overlapping field of view in the x-direction (width), and i is iterated from 0 to T. Increments of i should be chosen based on the resolution of blending required; i was chosen to be one pixel for the panoramas created.

Results from using the weighted average function are much better visually than those from the average weighted sum blend. Apparent from the resulting blend compared to the average weighted sum blend are less noticeable lines from the edges of the individual images, as well as less noise present in the blended images from matching correspondences being misaligned by a few pixels.

![distance_function_weighted_sum_blending](https://raw.githubusercontent.com/JoshMarino/image-stitching/master/weigthed_average_function.png)


### Further Improvements

Possible areas of improvement for blending between the overlapping field of view include resizing to a square image for pyramid blending or improving upon the weighted average function blending. One opportunity to improve upon the latter would be comparing the overlapping field of view after cropping both left and right images for a bounding region. This bounding region would include a polygon that contains the area of maximum overlap. Once the bounding region has been selected, blending would occur using weighted average function blending, and the remainder of each image would be added directly back in. A possibility for selecting this polygon would be finding the edges of each image by looking at the gradient of black surrounding the image.


### References
[1] L. S. Shapiro and J. M. Brady, “Feature-based correspondence: An eigenvector approach,” Image Vision Comput.10(5), 1992, 283–288.
[2] Herbert Bay, Tinne Tuytelaars, and Luc Van Gool, "Speeded Up Robust Features", ETH Zurich, Katholieke Universiteit Leuven
[3] Martin A. Fischler and Robert C. Bolles (June 1981). "Random Sample Consensus: A Paradigm for Model Fitting with Applications to Image Analysis and Automated Cartography". Comm. of the ACM 24 (6): 381–395. doi:10.1145/358669.358692
[4] Snavely, N. (2006, January 1). Lecture 13: Homographies and RANSAC. Retrieved March 15, 2015.
[5] Matthew Brown and David G. Lowe, "Automatic panoramic image stitching using invariant features," International Journal of Computer Vision, 74, 1 (2007), pp. 59-73.
[6] Mordvintsev, A., & K., A. (2013, January 1). Image Pyramids. Retrieved March 15, 2015.
[7] Singh, C. (n.d.). Panoramic Image Mosaic. Retrieved March 15, 2015, from http://pages.cs.wisc.edu/~csverma/CS766_09/ImageMosaic/imagemosaic.html
[8] Burt, P. J. and Adelson, E. H., “A multiresolution spline with applications to image mosaics,” ACM Transactions on Graphics, 42(4), October 1983, 217-236.
