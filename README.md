
<!-- README.md is generated from README.Rmd. Please edit that file -->

# scclusteval

The goal of scclusteval is to evaluate the single cell clustering
stability by boostrapping the cells.

for Theory behind the method, see Christian Henning, “Cluster-wise
assessment of cluster stability,” Research Report 271, Dept. of
Statistical Science, University College London, December 2006)

The process is as follows (paraphased from Stephen Eichhorn in Xiaowei
Zhuang lab) :

1.  Performing the clustering at many different K values on the full
    data set.

2.  We then sample without replacement a subset of the data set
    (e.g. 80% of the cells in the full data set), and then repeat the
    clustering procedure on just this subset of data (so repeating all
    aspects of clustering, including calling variable genes, calculating
    PCs, building the neighbor graph, etc), and we do this n (default
    20) times.

3.  So for each K value, we have 1 clustering outcome for the full data
    set, and 20 clustering outcomes for subsampled portions of the data
    set. From this we identify the cluster in the first subsample
    clustering that is most similar to the full cluster 1 cells (the one
    that gives the maximum Jaccard coefficient) and record that value.
    If this maximum Jaccard coefficient is less than 0.5, the original
    cluster is considered to be dissolved-it didn’t show up in the new
    clustering. A cluster that’s dissolved too often is probably not a
    “real” cluster.

4.  Repeat this for all subsample clustering outcomes, and then the
    stability value of a cluster is the median or mean Jaccard
    coefficient. If it’s greater than 0.5 we say it’s stable, otherwise
    it’s unstable. So for a given K value this gives you a
    stable/unstable assignment for each cluster. We choose the k value
    to select for clustering the data by looking at which k value
    yielded the largest number of stable clusters while still having
    most of the cells from the data set in a stable cluster. This second
    condition is important because as you decrease K, you will always
    get more clusters, and many of them will be stable, but there will
    also be a huge number of unstable clusters, so a small fraction of
    the data set will actually be described in stable clusters are these
    values.

5.  So what we typically see is that as K decreases, we get more stable
    clusters and no unstable clusters, then you reach a point where you
    decrease K and you get more stable clusters but also some unstable
    ones, so ~95% of the data is present in stable clusters, as you go
    down in K further you get more stable clusters but now something
    like 70% of the data is present in stable clusters. I choose the k
    value where at least 90% of the data was in a stable cluster, and
    the one that yielded the most stable clusters.

6.  This is almost always the smallest K value wherein at least 90% of
    the data was stable, but occasionally you see idiosyncratic
    behaviors from a particular k value, so it wouldn’t be unheard of to
    see something where k=10 and k=8 were both \>90% with respect to
    data in stable clusters, but k=10 had more stable clusters. In
    general we end up selecting k values around 8-12 in most cases.

As a rule of thumb, clusters with a stability value less than 0.6 should
be considered unstable. Values between 0.6 and 0.75 indicate that the
cluster is measuring a pattern in the data, but there isn’t high
certainty about which points should be clustered together. Clusters with
stability values above about 0.85 can be considered highly stable
(they’re likely to be real clusters). \#\# Installation

You can install the released version of scclusteval from
[CRAN](https://CRAN.R-project.org) with:

``` r
install.packages("scclusteval")
```

## Example

This is a basic example which shows you how to solve a common problem:

``` r
## basic example code
```

What is special about using `README.Rmd` instead of just `README.md`?
You can include R chunks like so:

``` r
summary(cars)
#>      speed           dist       
#>  Min.   : 4.0   Min.   :  2.00  
#>  1st Qu.:12.0   1st Qu.: 26.00  
#>  Median :15.0   Median : 36.00  
#>  Mean   :15.4   Mean   : 42.98  
#>  3rd Qu.:19.0   3rd Qu.: 56.00  
#>  Max.   :25.0   Max.   :120.00
```

You’ll still need to render `README.Rmd` regularly, to keep `README.md`
up-to-date.

You can also embed plots, for example:

<img src="man/figures/README-pressure-1.png" width="100%" />

In that case, don’t forget to commit and push the resulting figure
files, so they display on GitHub\!

## Acknowledgements

Thanks to Tim Sackton and Catherin Dulac for their supervision and
support.  
Thanks to Yasin Kaymaz in Sackton group for fruitful discussion. Thanks
to Stephen Eichhorn in Xiaowei Zhuang lab for the idea and sharing the
python code working on [Scanpy](https://github.com/theislab/scanpy)
object.  
Thanks to Sophia(Zhengzheng) Liang in Dulac lab for initial sharing
data.

## Why this package?

fpc package `clusterboot`.

read this blog post
<http://www.win-vector.com/blog/2015/09/bootstrap-evaluation-of-clusters/>
