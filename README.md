# DIS_project_2022
Decomposition using Spark

## I. Introduction

When $a \ne 0$, there are two solutions to $(ax^2 + bx + c = 0)$ and they are 
$$ x = {-b \pm \sqrt{b^2-4ac} \over 2a} $$

## II. Homogeneity Function

The homogeneity  of a dataset D is a metric $Hom(D)$ that indicates how homogeneous (similar) the elements of the dataset are. 
With decomposition(clustering), the homogeinty function decute to $\sum_i(Hom(D_i))$, where $D_i$ s are the clusters.

We used a generalized cosine similarity as the homogeneity function in this project, as we are using cosine similarity as the distance metrics. The [Cosine Similarity](https://en.wikipedia.org/wiki/Cosine_similarity) $Cos()$ measures the distance between two vectors(or sequence of values), 2 orthogonal vectors have a cosine similarity of 1, 2 parallel vectors have similarity of 1, and 2 opposite vectors have -1. The cosine similarity is always used only in the positive space, therefore, it is ranged from 0 to 1.

$$
Cos(x, y) = \frac{x}{|x|} \cdot \frac{y}{|y|}
$$

For the Generalized Cosine Similarity, we firstly concatenate the vector into a matrix $X$, with each column is a normalized vector $x_i$ . After that, we compute the singular value decomposition(SVD)  of $X$, 

$$
X = USV^T
$$

Where the matrix $S$ is diagonal and the singular values $\sigma_i$s are non-negative(it's zero if and only if there is multicollinearity in the dataset) and arranged in decreasing order. Once we get the SVD decomposition, the similarity can be calculated as,


$$
SVDsim(Y) = \frac{\sum_{k}^K \sigma_k^2}{||Y||^{2}_F}
$$

where $||Y||^2_F$ is the [Frobenius norm](https://en.wikipedia.org/wiki/Matrix_norm#Frobenius_norm) of $Y$.

The idea behind SVD is that the SVD gives a lower rank approximation of $X$, as the cosine similiarity can be consider as the loss of using one vector to represent the 2 vectors, the SVD similairty can be defined as using $k(k<n)$ vectors to represent the whole datasets. The default value of $k$ is 1. 

Note that the above similarity score is the $\frac{\sqrt{1 + Cos(x, y)}}{2}$ when there is only 2 vectors in the dataset, to make it more consistant, we scaled it to a more interesting form.


$$
SVDsim = 2 \times (\frac{\sum_{k}^K \sigma_k^2}{||Y||^{2}_F})^2 - 1
$$



