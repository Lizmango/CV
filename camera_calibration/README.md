## Camera Calibration Tutorials

### Goal

- Learn about distortions in camera, intrinsic and extrinsic parameters of camera etc.
- Learn to find these parameters, undistort images etc.
**original image**
![](assets/chess04.jpg)


  **undistort image**

  ![](assets/calirest.png)

### Basics

- **Opencv 3.0 + Python3**
- **checkerboard with 8x10 squares and 7x9 internal corners**
- **func : cv2.findChessboardCorners( )** , **cv2.undistort( )**

### Distortion

Pinhole cameras introduce a lot of distortion to images, Two major distortions are `radial distortion` and `tangential distortion`.

- **`Radial distortion`**
`Radial distortion` concludes `barrel distortion`, `pincushion distortion` and `mustache distortion`. Due to radial distortion, straight lines will appear curved 

- **`Tangential distortion`**
`Tangential distortion` occurs because of image takeing lense is not aligned perfectly parallel to the imaging plane. So some areas in image may look nearer than expected. 

In short, we need to find five parameters, known as **`distortion coefficients`** 
### Calibration

#### Coordinates System

There are four basic coordinate system:
- world coordinate system (X_w,Y_w,Z_w)
- camera coordinate system (X_c, Y_c, Z_c)
- image coordinate system (x,y)
- pixel plane coordinate system (u,v)

calibration has three stage:
- Convert 3D points from world coordinate system to camera coordinate system. It involved camera extrinsic parameters R,t etc.
- Convert 3D points in camera coordinate system to 2D points in image coordinate system.
- Convert 2D points from image coordinate to pixel plane coordinate


### Undistort


For stereo applications, camera distortion need to be corrected first. To find all camera parameters, what we have to do is to provide some sample images of a well defined pattern(eg,chess board).We find some specific points in it(square corners in chess board). We know its coordinates in real world space and we know its coordinate in image. With these data, some mathematical problem is solved in background to get the distortion coefficients. That is the summary of the whole story. For better results, we need at least 10 test pattern.



### Code


We take the chess board which has 7*9 grid as our test pattern(in the ` cali-img ` folder). These images points are the locations where two black squares touch each other in the chess boards.  For sake of camera calibration, we need important data of 3D real world points and its corresponding 2D image points. 


- 2D image points can be easily found from test image.  
- 3D points in real world can be get from chess board's square size.
 Because that these images are taken from a static camera and chess boards are placed at different locations and orientations. So we need to know (X,Y,Z) values. But for simplicity, we can say chess board was kept stationary at XY plane, (so Z=0 always) and camera was moved accordingly. This consideration helps us to find only X,Y values. Now for X,Y values, we can simply pass the points as (0,0), (1,0), (2,0), ... which denotes the location of points. In this case, the results we get will be in the scale of size of chess board square. But if we know the square size, (say 30 mm), and we can pass the values as (0,0),(30,0),(60,0),..., we get the results in mm. (In this case, we don’t know square size since we didn’t take those images, so we pass in terms of square size). 

 3D points are called **`Object points`** and image points are called **`image points `**





 ### Setup

In order to find pattern in chess board, we use the func  **`cv2.findChessboardCorners()`**. The test image we used is a chess board with 8x10 squares and 7x9 internal corners. It returns the corner points and retval which will be True if pattern is obtained. These corners will be placed in an order (from left-to-right, top-to-bottom).

**see also:** Instead of chess board, we can use some circular grid, but then use the function cv2.findCirclesGrid() to find the pattern. It is said that less number of images are enough when using circular grid.
