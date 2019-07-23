## Color-based Image Segmentation using K-Means clustering

**Color quantization** is a process that reduces the number of distinct colors used in an image, usually intended to still retain a visual similarity to the original image but with reduced number of colored channels. It becomes a critical process on devices that can only display a limited number of colors, thus enabling efficient compression of certain types of images. Color quantization is largely used in GIF and PNG images. GIF, for a long time the most popular lossless and animated bitmap formaton the World Wide Web, only supports 256 colors, thus crying out for quantization. This can be viewed as a challenge to be tackled by a clustering algorithm to iron out redundancies from the image which is what we set out to achieve in this project. We aim to perform color quantization (or image segmentation based on colors) using a very ubiquitous unsupervised learning algorithm - **K-Means clustering**. We further aim to do better than the standard clustering algorithm by tweaking its initialization and also record comparisons between the two approaches. 

### Standard K-Means clustering

K-Means clustering is a vector quantization algorithm that partitions *n* observations into *k* clusters. In simpler terms, it maps an observation to one of the *k* clusters based on the squared (Euclidean) distance of the obseravtion from the cluster centroids. Once the mapping is done, the observation point can simply be represented by the centroid of its respective cluster. Applying this on an image would imply reducing it to an image with *k* colors. This is an iterative refinement procedure that updates the cluster centroids in each iteration. The algorithm terminates when a certain convergence point is reached. The most commonly chosen point of convergence is when the cluster centroids no longer move or get updated.

The bare bones of this algorithm are as follows :

![Alt Text](https://giphy.com/gifs/spot-dashee87githubio-scikit-12vVAGkaqHUqCQ/giphy.gif)

### Modified K-Means clustering