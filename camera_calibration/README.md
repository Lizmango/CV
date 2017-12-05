## Camera Calibration Tutorials

### Goal

- Learn about distortions in camera, intrinsic and extrinsic parameters of camera etc.
- Learn to find these parameters, undistort images etc.


### Basics

- Opencv 3.0 + Python3
- checkerboard 7*9

### Distortion

Pinhole cameras introduce a lot of distortion to images, Two major distortions are radial distortion and tangential distortion.

- Radial distortion
Radial distortion concludes barrel distortion, pincushion distortion and mustache distortion. Due to radial distortion, straight lines will appear curved as shown below:
![](./assets/20160330155136491.png)

Radial distortion can be solved as follows:
$$
\begin{aligned}
x_{corrected = x(1+k_1r^2 + k_2r^4 + k_3r^6)} \\
y_{corrected = y(1+k_1r^2 + k_2r^4 + k_3r^6)} 
\end{aligned}
$$

- Tangential distortion
Tangential distortion occurs because of image takeing lense is not aligned perfectly parallel to the imaging plane. So some areas in image may look nearer than expected. It is solved below:

$$
\begin{aligned}
x_{corrected = x(2p_1xy+p_2(r^2+2x^2))} \\
y_{corrected = y(p_1(r^2+2y^2)+2p_2xy)} 
\end{aligned}
$$

In short, we need to find five parameters, known as distortion coefficients given by:
$$ {Distortion} \quad {coefficients}= (k_1,k_2,p_1,p_2,k_3)$$

### Calibration

#### Coordinates System

There are four basic coordinate system:
- world coordinate system $ (X_w,Y_w,Z_w)$
- camera coordinate system $(X_c, Y_c, Z_c)$
- image coordinate system$ (x,y)$
- pixel plane coordinate system $ (u,v)$

calibration has three stage:
- Convert 3D points from world coordinate system to camera coordinate system. It involved camera extrinsic parameters R,t etc.
- Convert 3D points in camera coordinate system to 2D points in image coordinate system.
- Convert 2D points from image coordinate to pixel plane coordinate


#### Coordinates Conversion

##### pixel plane coordinate and image coordinate

The relationship between pixel plane coordinate and image coordinate is shown as below. Pixel plane and image plane are at the same plane, but there center are different. Besides, the pixel plane measures the length in pixel, while the image coordinate are  measured the length in meters.The coordinates' conversion can be expressed in mathematical formula.

![](./assets/20160125212048663.png)

$$
\begin{aligned}
 u=\frac{x}{dx}+u_0\\
 v=\frac{y}{dy}+v_0
 \end{aligned}
$$

The matrix expression is:

$$
\begin{bmatrix}
u\\
v\\
1
\end{bmatrix}
= \begin{bmatrix}
\frac{1}{dx}&0&u_0\\
0&\frac{1}{dy}&v_0\\
0&0&1
\end{bmatrix} 
\begin{bmatrix}
x\\
y\\
1
\end{bmatrix}
$$
  
##### camera coordinate and world coordinate

Three coordinates' relationships are shown as below:
![](./assets/20160329185310419.png)

- C : camera centre
- Z : principle axis
- the distance from C to P is focal length 
- image plane is the location of image coordinate 
- $ (u_0,v_0)$is the optical center of pixel plane
- $ dx , dy $ are act actual width and length of pixel.

The conversion of camera coordinate and world coordinate can be expressed as follows:
R is rotate transform matrix, t is translation matrix

$$
\begin{bmatrix}
X_c\\
Y_c\\
Z_c
\end{bmatrix}=
\begin{bmatrix}
R&t\\
0^T&1
\end{bmatrix}
\begin{bmatrix}
X\\
Y\\
Z\\
1
\end{bmatrix}
$$


##### Camera coordinate and image coordinate



 According to the triangle similarity principle:

 $$
 \begin{aligned}
 \frac{x}{f}=\frac{X_c}{Z_c}\\
 \frac{y}{f}=\frac{Y_c}{Z_c}
 \end{aligned}
 =>
 \begin{aligned}
 Z_c\cdot x = f\cdot X_c\\
 Z_c\cdot y = f\cdot Y_c
 \end{aligned}
$$ 
$ =>$ 
$$
 Z_c
 \begin{bmatrix}
 x\\
 y\\
 1
 \end{bmatrix}
 =\begin{bmatrix}
f&0&0&0\\
0&f&0&0\\
0&0&1&0
\end{bmatrix}
\begin{bmatrix}
X_c\\
Y_c\\
Z_c\\
1
\end{bmatrix}
$$




##### Camera parameters

In conclusion,
$$
 Z_c
 \begin{bmatrix}
 u\\
 v\\
 1
 \end{bmatrix}
 =\begin{bmatrix}
\frac{1}{dx}&0&u_0\\
0&\frac{1}{dy}&v_0\\
0&0&1
\end{bmatrix} 
 \begin{bmatrix}
f&0&0&0\\
0&f&0&0\\
0&0&1&0
\end{bmatrix}
\begin{bmatrix}
R&t\\
0^T&1
\end{bmatrix}
\begin{bmatrix}
X\\
Y\\
Z\\
1
\end{bmatrix}
$$



Thus, the intrinsic parameters matrix is :

$$
 \begin{bmatrix}
f_x&0&u_0&0\\
0&f_y&v_0&0\\
0&0&1&0
\end{bmatrix}=
\begin{bmatrix}
\frac{1}{dx}&0&u_0\\
0&\frac{1}{dy}&v_0\\
0&0&1
\end{bmatrix} 
 \begin{bmatrix}
f&0&0&0\\
0&f&0&0\\
0&0&1&0
\end{bmatrix}
$$

And the extrinsic parameters matrix is :$ \begin{bmatrix}
R&t\\
0^T&1
\end{bmatrix}$



### Undistort
For stereo applications, camera distortion need to be corrected first. To find all camera parameters, what we have to do is to provide some sample images of a well defined pattern(eg,chess board).We find some specific points in it(square corners in chess board). We know its coordinates in real world space and we know its coordinate in image. With these data,








