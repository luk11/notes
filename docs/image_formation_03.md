### Recap on Digital Images [^1]
+ Image channels: RGB color space / YUV color space
+ Basic configuration: Light $\rightarrow$ Camera Lens $\rightarrow$  Aperture $\rightarrow$ Shutter $\rightarrow$  Camera Image Sensor
+ Camera response function: maps log-exposure value to the intensity levels in the input images
+ Human eye: pupil $\leftrightarrow$ aperture; photoreceptor cells in the retina $\leftrightarrow$ sensor

### Pinhole Camera [^2]
+ A thin converging lens focuses light onto the focal point $\rightarrow$ sensor
+ Relation between image and object
+ Perspective Effects: size inversely proportional to distance, vanishing point, vanishing direction
+ Image plane is usually represented in front of the lens, such that it preserves orientation (upright image)
+ Field of view (FOV): angular portion of 3D scene seen by the camera; inversely proportional to the focal length. Field of view $\theta$, image width $W$, focal length $f$: $tan(\theta/2)=\frac{W}{2f}$ 

### Perspective Projection [^3]

>[!info]- Homogeneous Coordinates
> Homogeneous coordinates are a mathematical concept used in projective geometry and computer graphics to represent points, lines, and transformations in a consistent and convenient way. They extend the Cartesian coordinate system by adding an extra coordinate, typically denoted as "w."
> 
> In Cartesian coordinates, a point in 2D space is represented by two coordinates (x, y), and in 3D space, by three coordinates (x, y, z). However, homogeneous coordinates add a fourth coordinate (x, y, z, w) to represent points in a higher-dimensional space.
> 
> The key idea behind homogeneous coordinates is that different tuples (x, y, z, w) can represent the same point in space. The point (x, y, z, w) is equivalent to the point (kx, ky, kz, kw) for any non-zero scalar value k. This property allows for convenient representation of points at infinity and the use of transformations such as translation, rotation, scaling, and perspective projection in a unified manner.
> 
> By convention, the fourth coordinate w is typically set to 1 for most points in Euclidean space. This normalization simplifies calculations and ensures that division by w yields the original Cartesian coordinates. However, points at infinity, such as the vanishing point in perspective projection, have w = 0.
> 
> Homogeneous coordinates also enable the representation of lines and other geometric objects. For example, a line in 2D space can be represented by a 3D homogeneous coordinate (a, b, c), where ax + by + cz = 0 is the equation of the line.
> 
> In computer graphics, homogeneous coordinates are commonly used to perform 3D transformations using matrices. By representing points as column vectors and transformations as matrices, you can perform transformations like translation, rotation, scaling, and perspective projection using matrix multiplication.
> 
> Overall, homogeneous coordinates provide a powerful mathematical framework for representing points, lines, and transformations, allowing for efficient and consistent manipulation in various geometric and graphical contexts.

Basic Knowledge: optical center C (in the lens), optical axis, principal point O (intersection of optical axis and image plane; on the sensor), axes of the camera frame $X_c, Y_c, Z_c$ in the lens.

Perspective Projection vs. Parallel Projection

Pixel coordinate system:
+ Given coordinates $(x,y)$ we compute the **pixel coordinates** $(u,v)$
$$
\begin{align}
u = u_0 + k_u x \quad &\rightarrow \quad u = u_0 + \frac{k_ufX_C}{Z_C}\\
v = v_0 + k_v y \quad &\rightarrow \quad v = v_0 + \frac{k_vfY_C}{Z_C}\\
\end{align}
$$
+ Focal lengths expressed in pixels: $k_uf = \alpha_u$ 
+ Express pixel coordinates as homogeneous coordinates: $\tilde{p}=\lambda[u,v,1]^T$
+ Intrinsic or calibration matrix with focal lengths expressed in pixels: not equal du to conversion factor. Skew factor (if principal point is not in the middle) is often assumed to be zero nowadays. For square pixels, it is $\alpha u = \alpha v$ 

$$
\lambda
\begin{bmatrix}
u\\v\\1
\end{bmatrix} =
\begin{bmatrix}
\alpha_u & 0 & u_0\\0 & \alpha_v & v_0\\ 0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
X_c\\Y_c\\Z_c
\end{bmatrix} = K \begin{bmatrix}
X_c\\Y_c\\Z_c
\end{bmatrix}
$$
With Intrinsic / calibration matrix $K$. 
World frame $\rightarrow$ camera frame $\rightarrow$ pixel coordinates (rigid transformation + perspective projection)
World frame and camera frame: $X_C = RX_W + t$ as a rigid transformation (extrinsic parameters): rotation matrix and translation vector $\rightarrow$ more compact form $[R|T]$ 

$$
\lambda
\begin{bmatrix}
u\\v\\1
\end{bmatrix} = K
\begin{bmatrix}
R | T
\end{bmatrix}
\begin{bmatrix}
X_w\\Y_w\\Z_w \\ 1
\end{bmatrix}
$$
With extrinsic parameters $[R|T]$ with *Projection matrix* $M=K[R|T]$ 

Computer Vision vs. Computer Graphics
+ transform the view volume, i.e. the pyramidal frustrum to the canonical view (normalized device coordinates NDC)
+ Eye coordinate is camera frame in computer vision
+ In computer vision only 2D points, in computer graphics we also store the depth information
+ Third coordinate is the depth information

##### Line projection
+ *Two-step computation method:*
	+ project points $A, B$ on the projection plane $\rightarrow$ $A' = KA$ with intrinsic matrix $K$ 
	+ Coordinates of 2D line: $l = A' \times B'$ 
+ *One-step computation method:* $l = \mathcal{K}n$ with $\mathcal{K}$ as a special intrinsic matrix

##### Relationship between Points, Lines and Planes

##### Normalized Image
Normalized image plane: virtual image plane with **focal length 1 and origin of the pixel coordinates at the principal point** 
$$
\begin{bmatrix}
\overline{u}\\\overline{v}\\1
\end{bmatrix} = K^{-1} 
\begin{bmatrix}
u\\v\\1
\end{bmatrix}
= \begin{bmatrix}
\frac{u-u_0}{\alpha}\\ \frac{v-v_0}{\alpha}\\1
\end{bmatrix}
$$


[^1]: Lecture May 03. 2023, slide 8 - 10
[^2]: Lecture May 03. 2023, slide 11 - 21
[^3]: Lecture May 03. 2023, slide 22 - 57