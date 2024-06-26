# Fisheye-Camera-Calibration 
*Performing fish eye camera calibration by obtaining images from simulating a fisheye camera in blender.* 



Fisheye cameras deviate from the traditional pinhole camera model in terms of their optics and resulting image projection. Instead of using a small aperture and relying on perspective projection, fisheye cameras typically use wide-angle lenses with a highly curved field of view. This results in a distinct fisheye effect where straight lines in the scene are heavily distorted, appearing curved in the image. The adantages of using lens in cameras are wider field of view, more light exposure. However, they also introduce additional distortion in images such barrel distortion and pincusion distortion. Thus, we have to find the distortion coefficients to correct the distortion.

![alt text](New_folder/image-1.png)

### Intrinsic parameters:

These parameters characterize the internal geometry and optical properties of the camera. The primary intrinsic parameters include:

Focal Length (f): The distance from the camera's optical center (the pinhole or lens) to the image sensor or film plane when focused at infinity. It determines the scale of the scene in the image.

Principal Point (u₀, v₀): The coordinates of the principal point, which is the intersection point of the optical axis with the image plane. It represents the offset of the image sensor's origin from the top-left corner of the image.


### Extrinsic parameters:
These parameters define the transformation from the camera's local coordinate system to the global coordinate system. The primary extrinsic parameters include:

Rotation Matrix (R): A 3x3 matrix representing the rotation of the camera with respect to the global coordinate system. It describes how the camera's axes are oriented relative to the world axes.

Translation Vector (t): A 3x1 vector representing the translation of the camera's optical center from the origin of the global coordinate system. It specifies the position of the camera in 3D space.



### Lens Distortion: 
Distortions caused by imperfections in the camera's lens system, including radial distortion (barrel distortion or pincushion distortion) and tangential distortion. Distortion parameters are often represented using radial and tangential distortion coefficients.
![alt text](New_folder/image-2.png)

# Method


A model of a car is downloaded from the web. A fish-eye camera is mounted on the model mounted . The camera has a focal length of 10.50 mm and a field of view of 120 degrees.

A model of a chessboard is downloaded from the web and placed in the field of view of the camera. An image of the view is captured. The chessboard is moved in the field of view of the camera to obtain numerous images from different angles. The images obtained are preset in the repository.

In order to perform the camera calibration and distortion correction, we need to obtain camera matrix, distortion coefficients, translation matrix and rotation matrix.




![Alt Text](New_folder/Capture-1.png)



## Camera Calibration

The code performs camera calibration using a checkerboard pattern. It utilizes OpenCV's camera calibration functions to estimate camera intrinsic and extrinsic parameters, as well as to undistort images captured by the camera.


### Object Points Preparation

Object points, representing the coordinates of corners on the checkerboard pattern in the real-world space, are generated.

### Image Points Collection

Image points, representing the coordinates of detected corners on the checkerboard pattern in the image plane, are collected from a set of images.

### Camera Calibration

The collected object points and image points are used to calibrate the camera, resulting in camera matrix (`mtx`), distortion coefficients (`dist`), rotation vectors (`rvecs`), and translation vectors (`tvecs`).

### Undistortion

The obtained camera calibration parameters are used to undistort images captured by the camera. The `undistort` function is applied to remove lens distortion, followed by cropping to remove black borders.




----------------------------------------------------------------

To calculate the size of the camera sensor (d) when the vertical field of view (FOV) is 120 degrees and fy (the focal length in the vertical direction) is 552.7, we can use a similar approach as before, but this time considering the vertical direction. 

The formula relating FOV, d, and fy is:

FOV = 2 × atan ( d / (2fy) )

Rearranging this formula to solve for d:

d = 2 × fy × tan ( FOV / 2 )

Given that FOV = 120 degrees and fy = 552.7, we can plug these values into the formula to find d:

d = 2 × 552.7 × tan ( 120° / 2 )

d = 2 × 552.7 × tan(60°)

d = 2 × 552.7 × tan(π/3)

d = 2 × 552.7 × √3

d ≈ 1905.92

So, when the vertical field of view is 120 degrees and fy is 552.7, the size of the camera sensor (d) is approximately **1905.92.**

### Replicating a Leopard camera model *LI-VENUS-OX03D4C-GM2A-124H* witha field of view of 124 degrees(H) and reslution of 1920 (H) x 1536 (V)
---
required parameters of focal length:

Given:
- d = 1905.92
- FOV_horizontal = 124°


FOV_vertical:

aspect ratio = To find the vertical field of view (FOV) of the camera, you can use the aspect ratio of the resolution along with the horizontal FOV.

The aspect ratio of the resolution is given by:

Aspect Ratio = Width / Height

For the given resolution of 1920 (H) x 1536 (V), the aspect ratio is:

Aspect Ratio = 1920 / 1536 ≈ 1.25

Now, since the horizontal FOV is 124 degrees, we can use the aspect ratio to find the vertical FOV:

Vertical FOV = Horizontal FOV / Aspect Ratio

Vertical FOV = 124 / 1.25 ≈ 99.2 degrees

So, the vertical field of view of the camera is approximately 99.2 degrees.

---
## Now find new fx and fy 
Using the formulas:

f_x = d / (2 * tan(FOVx/ 2))
f_y = d / (2 * tan(FOVy/ 2))

f_x ≈ 509.3
f_y ≈ 770.5

So, when d = 1905.92 and both horizontal and vertical FOVx and FOVy are changed to 124° and 99.2° respectively, the focal lengths f_x and f_y are approximately 509.3 and 770.5 respectively.

