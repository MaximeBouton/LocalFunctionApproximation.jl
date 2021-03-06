# LocalFunctionApproximation

This package provides local function approximators that interpolate values across a space based on some locality metric.
Currently it supports multi-linear and simplex interpolations for multi-dimensional grids, and k-nearest-neighbor
interpolation. Two important dependencies are [GridInterpolations](https://github.com/sisl/GridInterpolations.jl/blob/master/src/GridInterpolations.jl)
and [NearestNeighbors](https://github.com/KristofferC/NearestNeighbors.jl).

## Installation and Usage

Start Julia and run the following:

```julia
Pkg.add("LocalFunctionApproximation")
using LocalFunctionApproximation
```

## Create Function Approximators

Create a rectangular grid for interpolation using `GridInterpolations` and create the function approximator
that uses it:

```julia
using GridInterpolations # Make the grid interpolations module available
grid = RectangleGrid([0., 0.5, 1.],[0., 0.5, 1.])      # rectangular grid
grid_values = [8., 1., 6., 3., 5., 7., 4., 9., 2.]     # corresponding values at each grid point
gifa = LocalGIFunctionApproximator(grid, grid_values)  # create the function approximator using the grid and values
```

Create a nearest neighbor tree using `NearestNeighbors` and create the corresponding approximator:

```julia
points = [SVector(0.,0.), SVector(0.,1.), SVector(1.,1.), SVector(1.,0.)]   # the 4 corners of the unit square
nntree = KDTree(points)                                                     # create a KDTree using the points
vals = [1., 1., -1., -1]                                                    # values corresponding to points
k = 2                                                                       # the k parameter for knn approximator
knnfa = LocalNNFunctionApproximator(nntree, points, k)
```


## Compute values at arbitrary points

```julia
point = rand(2)                                           # random 2D point
LocalFunctionApproximation.evaluate(gifa, point)         # Need to use namespace as Distances.jl also has evaluate
LocalFunctionApproximation.evaluate(knnfa, point)
```

A typical use case for this package is for Local Approximation Value Iteration, as shown [here](https://github.com/Shushman/LocalApproximationValueIteration.jl/blob/split_localfunctionapproximation/src/local_approximation_vi.jl).