<script src='https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML' async></script>

\page added_mass Added Mass 

When a body accelerates through a fluid, it neccessarily accelerates some of that fluid along with it. Accelerating this surrounding fluid introduces an additional inertia force that would not be present if the body was accelerating in a vaccuum.  For many terrestiral robots, this additional inertia can be neglected as insignificant because the density of the robot components is much larger than the surrounding air.  However, whenever the density of the body is comparable to the density of the surrounding fluid, these added inertial forces are important.  Typical examples are underwater vehicles, and ballons and airships.

As of the Garden version of Gazebo, the ability to include a characterization of these added inertial forces is included.  This tutorial explains how these terms are specified, and provides some examples illustrating the effects of added-mass.

# Added Mass Description
As indicated above, added mass phenomenon is a close analog to the mass of a rigid body, and includes translational terms (added mass), and rotatational terms (added mass-moment-of-inertia).  An important differenc however is that unlike inertia due to the mass and mass-distribution of a body, inertia due to the surrounding fluid is a function of the geometry of the body only.  Additionally, translational added mass can have different values depending on the direction of motion of the body.  Further, cross-coupling terms can be present in which acceleration in one direction results in an inertial force in any other degree of freedom.

The characterizations of the added mass, and added mass-moment-of-inertia in the equations of motion for a single body appears as a addition 6X6 matrix $\bf{\mu}$ that is added to the usual mass matrix $\bf M$

$$ (\bf{M} + \bf{\mu})   \ddot{\bf x} = \sum{F({\bf x}, t)} $$

where \\(\bf{M}\\) is the body mass inertia matrix, \\(\mu\\) is the fluid added mass matrix,
\\(\sum{F}\\) is the sum of all forces applied to the body, and \\(\ddot{\bf x}\\) is the resulting acceleration, with:

$$
    {\bf x}^T
    =
    \begin{bmatrix}
      x           & y           & z           & p           &  q         & r
    \end{bmatrix}
$$

where,

* $x$ : Position in X axis
* \\(y\\) : Position in Y axis
* \\(z\\) : Position in Z axis
* \\(p\\) : Rotation about X axis
* \\(q\\) : Rotation about Y axis
* \\(r\\) : Rotation about Z axis



$$
    M
    =
    \begin{bmatrix}
      m           & 0           & 0           & 0           &  m z\_{CoM} & -m y\_{CoM} \\\
      0           & m           & 0           & -m z\_{CoM} & 0           &  m x\_{CoM} \\\
      0           & 0           & m           &  m y\_{CoM} & -m x\_{CoM} & 0           \\\
      0           & -m z\_{CoM} &  m y\_{CoM} & I\_{xx}     & I\_{xy}     & I\_{xz}     \\\
       m z\_{CoM} & 0           & -m x\_{CoM} & I\_{xy}     & I\_{yy}     & I\_{yz}     \\\
      -m y\_{CoM} &  mx\_{CoM}  & 0           & I\_{xz}     & I\_{yz}     & I\_{zz}
    \end{bmatrix}
$$

where

* \\(m\\) : Body's mass
* \\(I\_{xx}\\) : Principal mass moment of inertia about the X axis
* \\(I\_{xy}\\) : Product mass moment of inertia about the X and Y axes, and vice-versa
* \\(I\_{xz}\\) : Product mass moment of inertia about the X and Z axes, and vice-versa
* \\(I\_{yy}\\) : Principal mass moment of inertia about the Y axis
* \\(I\_{yz}\\) : Product mass moment of inertia about the Y and Z axes, and vice-versa
* \\(I\_{zz}\\) : Principal mass moment of inertia about the Z axis
* \\(x\_{CoM}\\) : Center of mass X coordinate
* \\(y\_{CoM}\\) : Center of mass Y coordinate
* \\(z\_{CoM}\\) : Center of mass Z coordinate


$$
    \bf{\mu}
    =
    \begin{bmatrix}
      xx & xy & xz & xp & xq & xr \\\
      xy & yy & yz & yp & yq & yr \\\
      xz & yz & zz & zp & zq & zr \\\
      xp & yp & zp & pp & pq & pr \\\
      xq & yq & zq & pq & qq & qr \\\
      xr & yr & zr & pr & qr & rr
    \end{bmatrix}
$$

where

* \\(xx\\) : added mass in the X axis due to linear acceleration in the X axis
* \\(xy\\) : added mass in the X axis due to linear acceleration in the Y axis, and vice-versa
* \\(xz\\) : added mass in the X axis due to linear acceleration in the Z axis, and vice-versa
* \\(xp\\) : added mass in the X axis due to angular acceleration about the X axis, and vice-versa
* \\(xq\\) : added mass in the X axis due to angular acceleration about the Y axis, and vice-versa
* \\(xr\\) : added mass in the X axis due to angular acceleration about the Z axis, and vice-versa
* \\(yy\\) : added mass in the Y axis due to linear acceleration in the Y axis
* \\(yz\\) : added mass in the Y axis due to linear acceleration in the Z axis, and vice-versa
* \\(yp\\) : added mass in the Y axis due to angular acceleration about the X axis, and vice-versa
* \\(yq\\) : added mass in the Y axis due to angular acceleration about the Y axis, and vice-versa
* \\(yr\\) : added mass in the Y axis due to angular acceleration about the Z axis, and vice-versa
* \\(zz\\) : added mass in the Z axis due to linear acceleration in the Z axis
* \\(zp\\) : added mass in the Z axis due to angular acceleration about the X axis, and vice-versa
* \\(zq\\) : added mass in the Z axis due to angular acceleration about the Y axis, and vice-versa
* \\(zr\\) : added mass in the Z axis due to angular acceleration about the Z axis, and vice-versa
* \\(pp\\) : added mass moment about the X axis due to angular acceleration about the X axis
* \\(pq\\) : added mass moment about the X axis due to angular acceleration about the Y axis, and vice-versa
* \\(pr\\) : added mass moment about the X axis due to angular acceleration about the Z axis, and vice-versa
* \\(qq\\) : added mass moment about the Y axis due to angular acceleration about the Y axis
* \\(qr\\) : added mass moment about the Y axis due to angular acceleration about the Z axis, and vice-versa
* \\(rr\\) : added mass moment about the Z axis due to angular acceleration about the Z axis

The unit of each matrix element matches its corresponding element on the mass matrix \\(\bf{M}\\).
That is, elements on the top-left 3x3 corner are in \\(kg\\), the bottom-right ones are in
\\(kg\cdotm^2\\), and the rest are in \\(kg\cdotm\\).


![Coordinate Systems](files/added_mass/CoordSys.png)

# Specification in Gazebo
```xml
<inertial>
  <mass>10</mass>
  <pose>1 1 1 0 0 0</pose>
  <inertia>
    <ixx>0.16666</ixx>
    <ixy>0</ixy>
    <ixz>0</ixz>
    <iyy>0.16666</iyy>
    <iyz>0</iyz>
    <izz>0.16666</izz>
  </inertia>
  <fluid_added_mass>
    <xx>1.0</xx>
    <xy>0.0</xy>
    <xz>0.0</xz>
    <xp>0.0</xp>
    <xq>0.0</xq>
    <xr>0.0</xr>
    <yy>1.0</yy>
    <yz>0.0</yz>
    <yp>0.0</yp>
    <yq>0.0</yq>
    <yr>0.0</yr>
    <zz>1.0</zz>
    <zp>0.0</zp>
    <zq>0.0</zq>
    <zr>0.0</zr>
    <pp>0.1</pp>
    <pq>0.0</pq>
    <pr>0.0</pr>
    <qq>0.1</qq>
    <qr>0.0</qr>
    <rr>0.1</rr>
  </fluid_added_mass>
</inertial>
```
#  Example

# Summary