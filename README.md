gram_savitzky_golay
==

C++ Implementation of Savitzky-Golay filtering based on Gram polynomials, as described in 
- [General Least-Squares Smoothing and Differentiation by the Convolution (Savitzky-Golay) Method](http://pubs.acs.org/doi/pdf/10.1021/ac00205a007)

Example
==

```cpp
#include <gram_savitzky_golay/gram_savitzky_golay.h>

// Window size is 2*m+1
const size_t m = 3;
// Polynomial Order
const size_t n = 2;
// Initial Point Smoothing (ie evaluate polynomial at first point in the window)
// Points are defined in range [-m;m]
const size_t t = m;
// Derivate? 0: no derivation, 1: first derivative...
const int d = 0;



// Real-time filter (filtering at latest data point)
SavitzkyGolayFilter filter(m, t, n, d);
// Filter some data
std::vector<double> data = {.1, .7, .9, .7, .8, .5, -.3};
double result = filter.filter(data);



// Real-time derivative filter (filtering at latest data point)
// Use first order derivative
d=1;
SavitzkyGolayFilter first_derivative_filter(m, t, n, d);
// Filter some data
std::vector<double> values = {.1, .2, .3, .4, .5, .6, .7};
// Should be =.1
double derivative_result = first_derivative_filter.filter(values);
```


Filtering Rotations
==

```cpp
#include <gram_savitzky_golay/spatial_filters.h>

gram_sg::SavitzkyGolayFilterConfig sg_conf(50, 50, 2, 0);
RotationFilter filter(sg_conf);

filter.reset(Eigen::Matrix3d::Zero());

// Add rotation matrices to the filter
filter.add(Eigen::Matrix3d ...)
filter.add(Eigen::Matrix3d ...)
filter.add(Eigen::Matrix3d ...)

// Filter rotation matrices (weighted average of rotation matrices followed by an orthogonalization)
// See Peter Cork lecture here:
// https://www.cvl.isy.liu.se/education/graduate/geometry2010/lectures/Lecture7b.pdf
const Eigen::Matrix3d res = filter.filter();
```

This header also contains a filter for homogeneous transformations defined as `Eigen::Affine3d`, and a generic filter for eigen vectors. 
