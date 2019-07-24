## Color-based Image Segmentation using K-Means clustering

**Color quantization** is a process that reduces the number of distinct colors used in an image, usually intended to still retain a visual similarity to the original image but with reduced number of colored channels. It becomes a critical process on devices that can only display a limited number of colors, thus enabling efficient compression of certain types of images. Color quantization is largely used in GIF and PNG images. GIF, for a long time the most popular lossless and animated bitmap formaton the World Wide Web, only supports 256 colors, thus crying out for quantization. This can be viewed as a challenge to be tackled by a clustering algorithm to iron out redundancies from the image which is what we set out to achieve in this project. We aim to perform color quantization (or image segmentation based on colors) using a very ubiquitous unsupervised learning algorithm - **K-Means clustering**. We further aim to do better than the standard clustering algorithm by tweaking its initialization and also record comparisons between the two approaches. 

### Standard K-Means clustering

K-Means clustering is a vector quantization algorithm that partitions *n* observations into *k* clusters. In simpler terms, it maps an observation to one of the *k* clusters based on the squared (Euclidean) distance of the obseravtion from the cluster centroids. The intuition here is to minimize the variance between points within the same cluster and maximize the variance amongst points from different clusters. Once the mapping is done, the observation point can simply be represented by the centroid of its respective cluster. Applying this on an image would imply reducing it to an image with *k* colors. This is an iterative refinement procedure that updates the cluster centroids in each iteration. The algorithm terminates when a certain convergence point is reached. The most commonly chosen point of convergence is when the cluster centroids no longer move or get updated.

The bare bones of this algorithm are as follows :

**(a) Initialization** - We randomly pick any *k* points from the set of observations as our initial guess as to what the *k* cluster centroids could be.

**(b) Assignment** - For each observation, find the cluster centroid closest to it in terms of the Euclidean distance. Once found, this observation will now simply be represented by that cluster centroid and is now said to belong to that particular cluster.

**(c) Update** - Once all observations have been assigned to a cluster, the new centroids for the next iteration are computed. The mean of all the observations mapped to one particular cluster gives us an updated centroid.

**(d) Repeat** - Steps (b) and (c) constitute a single iteration. We will continue with these iterations until the centroids no longer move. This is when we claim 'convergence' to have been achieved.

<p align="center">
  <img src = "https://github.com/sanjanprakash/Color-based-Image-Segmentation-using-K-Means-clustering/blob/master/kmeans.gif">
</p>

### Modified K-Means clustering

Graphically, the goal that we set to achieve with such clustering is to have the radial distance from a cluster centroid to be very similar for all points that fall under that cluster. And with this naive random initialization of cluster centroids, we are never guaranteed an optimal solution as we could easily converge to some local minimum. Further, it is never beyond the realm of possibility to have some of the initial cluster centroid choices to be closer to each other than desired. This undermines the purpose of redundancy reduction in images and opens up the possibility for an **alternative initialization strategy**.

We propose a strategy that takes inspiration from the well-documented K-Means++ algorithm and at the same time has lesser randomness than the K-Means++ approach to initialization and also the standard K-Means initialization. Here is how we proceed :

**(a) Seeding** - Start by randomly picking one of the points of observation as a cluster centroid. This stage is akin to the first stage of initialization in K-Means++ as well.

**(b) Assignment & Max-tracking** - Very similar to the assignment step in the standard K-Means algorithm. In addition, during each assignment, keep track of the observation point with the largest distance from its cluster centroid.

**(c) Update** - Same as the update step in the standard K-Means algorithm.

**(d) Another centroid** - Add the observation point that was found to have been the farthest from its cluster centroid to the set of centroids that will form our 'initial guess'.

**(e) Repeat** - Steps (b) to (d) are repeatedly performed until we have *k* initial centroids.

The intuition behind this approach is to ensure a good spread amongst the *k* initial cluster centroids. The greater the Euclidean distance between an observation point and its closest-lying cluster centroid, the greater are its odds of being chosen as an initial centroid. This approach to initialization leads to a more refined initial guess as to what the initial cluster centroids would be and also leads to a more assured convergence to a global minimum, albeit at the cost of performing a pseudo-K-Means clustering to get there which would cost us some pre-computation time. However, since the running time of the standard K-Means algorithm is super-polynomial in its input size, asymptotically, this extra bit of pre-computation will not greatly affect the overall running time.

### Analysis

<p align="center">
  <img src = "https://github.com/sanjanprakash/Color-based-Image-Segmentation-using-K-Means-clustering/blob/master/test_image.jpeg">
</p>

We chose to work with an image of the *Bridges of Amsterdam* painting by Leonid Afremov.  