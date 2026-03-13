# class07
Paige Contrersas (A16850883)

## Background

Today we will begin our exploration of some important machine learning
methods, namely **clustering** and **dimensionality reduction**.

Let’s make up some input data for clustering where we know what the
natural “clusters” are.

The function `rnorm()` can be useful here. \## K-means clustering

``` r
hist( rnorm(5000) )
```

![](class07_files/figure-commonmark/unnamed-chunk-1-1.png)

> Q. Generate 30 random numbers centered at +3 and another 30 centered
> at -3

``` r
rnorm(30, mean = 3)
```

     [1] 2.723532 3.261522 2.534410 3.353386 2.838602 2.847074 1.991159 4.305638
     [9] 3.210332 3.210749 2.930113 2.056106 2.625664 3.293327 3.233725 3.232507
    [17] 2.918116 2.774017 2.284953 1.927727 1.950199 2.598374 2.962690 2.818210
    [25] 2.239249 2.918572 4.685384 3.315832 4.487750 5.429186

``` r
rnorm(30, mean = -3)
```

     [1] -4.5642812 -2.5208086 -1.6769302 -2.1514637 -2.1231808 -3.7774185
     [7] -1.8504283 -2.1150750 -2.5319243 -1.6193824 -1.7255608 -3.9948285
    [13] -2.7051794 -3.0158474 -3.9130494 -3.6944221 -2.8100062 -1.4892181
    [19] -1.1744037 -2.6474515 -1.9775158 -1.9702881 -1.5335114 -3.9279127
    [25] -4.4288345 -3.5374013 -0.7956935 -3.1129882 -2.6799682 -2.9710771

``` r
tmp <- c(rnorm(30, mean = 3),
rnorm(30, mean =-3) )

cbind(x=tmp, y =rev(tmp))
```

                   x          y
     [1,]  2.3352128 -3.5071933
     [2,]  2.4100615 -2.8690682
     [3,]  5.0005263 -2.1115064
     [4,]  1.3710957 -2.7738903
     [5,]  2.9710466 -3.5934649
     [6,]  2.0359491 -3.9272989
     [7,]  3.8280580 -1.6992426
     [8,]  1.9869728 -2.9717345
     [9,]  3.2388237 -1.7218173
    [10,]  0.8562546 -4.5509985
    [11,]  2.8227551 -2.5977699
    [12,]  4.0829836 -2.8445475
    [13,]  3.4133951 -2.9678159
    [14,]  3.3830829 -3.7673170
    [15,]  2.1173331 -2.4398973
    [16,]  2.9671846 -1.7773149
    [17,]  2.3307272 -3.6445934
    [18,]  3.7914382 -2.2260545
    [19,]  2.6657605 -3.9118829
    [20,]  2.2767449 -4.6048721
    [21,]  2.6146341 -2.5661864
    [22,]  3.4625961 -2.9878548
    [23,]  3.1157297 -2.5276555
    [24,]  2.0105485 -3.7083924
    [25,]  3.7814997 -4.6132415
    [26,]  2.4095337 -3.4401396
    [27,]  3.9797047 -2.3406314
    [28,]  3.4230426 -3.9319705
    [29,]  3.9575141 -3.3589091
    [30,]  2.5165553 -1.9681716
    [31,] -1.9681716  2.5165553
    [32,] -3.3589091  3.9575141
    [33,] -3.9319705  3.4230426
    [34,] -2.3406314  3.9797047
    [35,] -3.4401396  2.4095337
    [36,] -4.6132415  3.7814997
    [37,] -3.7083924  2.0105485
    [38,] -2.5276555  3.1157297
    [39,] -2.9878548  3.4625961
    [40,] -2.5661864  2.6146341
    [41,] -4.6048721  2.2767449
    [42,] -3.9118829  2.6657605
    [43,] -2.2260545  3.7914382
    [44,] -3.6445934  2.3307272
    [45,] -1.7773149  2.9671846
    [46,] -2.4398973  2.1173331
    [47,] -3.7673170  3.3830829
    [48,] -2.9678159  3.4133951
    [49,] -2.8445475  4.0829836
    [50,] -2.5977699  2.8227551
    [51,] -4.5509985  0.8562546
    [52,] -1.7218173  3.2388237
    [53,] -2.9717345  1.9869728
    [54,] -1.6992426  3.8280580
    [55,] -3.9272989  2.0359491
    [56,] -3.5934649  2.9710466
    [57,] -2.7738903  1.3710957
    [58,] -2.1115064  5.0005263
    [59,] -2.8690682  2.4100615
    [60,] -3.5071933  2.3352128

``` r
cbind(letters, rev(letters))
```

          letters    
     [1,] "a"     "z"
     [2,] "b"     "y"
     [3,] "c"     "x"
     [4,] "d"     "w"
     [5,] "e"     "v"
     [6,] "f"     "u"
     [7,] "g"     "t"
     [8,] "h"     "s"
     [9,] "i"     "r"
    [10,] "j"     "q"
    [11,] "k"     "p"
    [12,] "l"     "o"
    [13,] "m"     "n"
    [14,] "n"     "m"
    [15,] "o"     "l"
    [16,] "p"     "k"
    [17,] "q"     "j"
    [18,] "r"     "i"
    [19,] "s"     "h"
    [20,] "t"     "g"
    [21,] "u"     "f"
    [22,] "v"     "e"
    [23,] "w"     "d"
    [24,] "x"     "c"
    [25,] "y"     "b"
    [26,] "z"     "a"

``` r
x <- cbind(x = tmp, y=rev(tmp))
plot(x)
```

![](class07_files/figure-commonmark/unnamed-chunk-5-1.png)

## K means clustering

The main function in “base R” for K-means clustering is called
`kmeans()`:

``` r
km <- kmeans(x, centers = 2)
```

> Q. What component of the results object details the cluster sizes

``` r
km$size
```

    [1] 30 30

> Q. What component of the results object dtails the cluster centers?

> What component of the results object details the cluster membership
> vector (i.e. our main result of which points lie in which cluster?)

``` r
km$cluster
```

     [1] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1
    [39] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1

> Q. Plot our clustering results with points colors by cluster and also
> add the cluster centers as new points colored blue?

``` r
plot(x, col=km$cluster )
points(km$centers, col="blue", pch = 15)
```

![](class07_files/figure-commonmark/unnamed-chunk-9-1.png)

> Q. Run `kmeans()` again and this time produce 4 clusters (and call
> your result object `k4`) and make a results figure like above?

``` r
k4 <- kmeans(x, centers = 4)
plot(x, col=k4$cluster )
points(k4$centers, col="blue", pch = 15)
```

![](class07_files/figure-commonmark/unnamed-chunk-10-1.png)

the meteric

``` r
km$tot.withinss
```

    [1] 88.46049

``` r
k4$tot.withinss
```

    [1] 48.0586

``` r
ans <- NULL
for(i in 1:30) {
ans <- c(ans, kmeans(x, centers = i)$tot.withinss)
}


ans
```

     [1] 1157.785375   88.460488   68.259545   61.758902   57.789840   36.410805
     [7]   34.132988   50.400085   25.374385   19.998379   17.584037   16.253289
    [13]   14.990841   13.139224   13.811816   11.067882    9.561577    8.942365
    [19]    7.387526    6.948600    8.466480    8.077726    6.681661    6.216426
    [25]    4.257067    5.218452    4.489939    3.386489    2.883191    2.974720

``` r
plot(ans, typ = "o")
```

![](class07_files/figure-commonmark/unnamed-chunk-13-1.png)

**Key-point:** K-means will impose a clustering structure on your data
even if it is not there - it will always give you the answer you asked
for even if that anwer is silly!

\##Hierarchical Clustering

The main function for Hierarchical Clustering is called `hclust()`
unlike `kmeans()` (which does all the work for you) you can’t just pass
`hclust()` our raw input data. It needs a “distance matrix” like the one
returned from the `dist()` function.

``` r
d <- dist(x)
hc <- hclust(d)
plot(hc)
```

![](class07_files/figure-commonmark/unnamed-chunk-14-1.png)

To extract our cluster membership vector from a `hclust()` result object
we have to “cut” our tree at a given height to yield separate
“groups”/“branches”.

``` r
plot(hc)
abline(h=8, col="pink", lty=)
```

![](class07_files/figure-commonmark/unnamed-chunk-15-1.png)

to do this we use the `cutree()` function on our `hclust()` object:

``` r
grps <- cutree(hc, h=8)
grps
```

     [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2
    [39] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2

``` r
table(grps, km$cluster)
```

        
    grps  1  2
       1  0 30
       2 30  0

## PCA of UK food data

Import the dataset of food consumption in the UK

``` r
url <- "https://tinyurl.com/UK-foods"
x <- read.csv(url)
x
```

                         X England Wales Scotland N.Ireland
    1               Cheese     105   103      103        66
    2        Carcass_meat      245   227      242       267
    3          Other_meat      685   803      750       586
    4                 Fish     147   160      122        93
    5       Fats_and_oils      193   235      184       209
    6               Sugars     156   175      147       139
    7      Fresh_potatoes      720   874      566      1033
    8           Fresh_Veg      253   265      171       143
    9           Other_Veg      488   570      418       355
    10 Processed_potatoes      198   203      220       187
    11      Processed_Veg      360   365      337       334
    12        Fresh_fruit     1102  1137      957       674
    13            Cereals     1472  1582     1462      1494
    14           Beverages      57    73       53        47
    15        Soft_drinks     1374  1256     1572      1506
    16   Alcoholic_drinks      375   475      458       135
    17      Confectionery       54    64       62        41

> Q1. How many rows and columns are in your new data frame named x? What
> R functions could you use to answer this questions?

``` r
dim(x)
```

    [1] 17  5

One solution to set the row names is to do it by hand…

``` r
rownames(x) <- x[,1]
```

To remove the first column I can use the minus index trick

``` r
x <- x[,-1]
x
```

                        England Wales Scotland N.Ireland
    Cheese                  105   103      103        66
    Carcass_meat            245   227      242       267
    Other_meat              685   803      750       586
    Fish                    147   160      122        93
    Fats_and_oils           193   235      184       209
    Sugars                  156   175      147       139
    Fresh_potatoes          720   874      566      1033
    Fresh_Veg               253   265      171       143
    Other_Veg               488   570      418       355
    Processed_potatoes      198   203      220       187
    Processed_Veg           360   365      337       334
    Fresh_fruit            1102  1137      957       674
    Cereals                1472  1582     1462      1494
    Beverages                57    73       53        47
    Soft_drinks            1374  1256     1572      1506
    Alcoholic_drinks        375   475      458       135
    Confectionery            54    64       62        41

A better way to do this is to set the row names to the first column by
arguing with `read.csv()`

``` r
x <- read.csv(url, row.names = 1)
x
```

                        England Wales Scotland N.Ireland
    Cheese                  105   103      103        66
    Carcass_meat            245   227      242       267
    Other_meat              685   803      750       586
    Fish                    147   160      122        93
    Fats_and_oils           193   235      184       209
    Sugars                  156   175      147       139
    Fresh_potatoes          720   874      566      1033
    Fresh_Veg               253   265      171       143
    Other_Veg               488   570      418       355
    Processed_potatoes      198   203      220       187
    Processed_Veg           360   365      337       334
    Fresh_fruit            1102  1137      957       674
    Cereals                1472  1582     1462      1494
    Beverages                57    73       53        47
    Soft_drinks            1374  1256     1572      1506
    Alcoholic_drinks        375   475      458       135
    Confectionery            54    64       62        41

> Q2. Which approach to solving the ‘row-names problem’ mentioned above
> do you prefer and why? Is one approach more robust than another under
> certain circumstances?

### Spotting major differences and trends

is difficult even in this wee 17d dataset…

``` r
barplot(as.matrix(x), beside=T, col=rainbow(nrow(x)))
```

![](class07_files/figure-commonmark/unnamed-chunk-23-1.png)

``` r
barplot(as.matrix(x), beside=F, col=rainbow(nrow(x)))
```

![](class07_files/figure-commonmark/unnamed-chunk-24-1.png)

``` r
pairs(x, col=rainbow(nrow(x)), pch=16)
```

![](class07_files/figure-commonmark/unnamed-chunk-25-1.png)

``` r
library(pheatmap)
pheatmap( as.matrix(x) )
```

![](class07_files/figure-commonmark/unnamed-chunk-26-1.png)

\##PCA to the rescue

The main PCA function in “base R” is called `prcomp()`. This function
wants the transpose of our food data as inout (i.e. the foods as columns
and the countries as rows).

``` r
pca <- prcomp( t(x) )
```

``` r
summary(pca)
```

    Importance of components:
                                PC1      PC2      PC3     PC4
    Standard deviation     324.1502 212.7478 73.87622 2.7e-14
    Proportion of Variance   0.6744   0.2905  0.03503 0.0e+00
    Cumulative Proportion    0.6744   0.9650  1.00000 1.0e+00

``` r
attributes(pca)
```

    $names
    [1] "sdev"     "rotation" "center"   "scale"    "x"       

    $class
    [1] "prcomp"

To make one of main PCA result figures we turn to `pca$x` the scores
along our new PCs. This is called “PC plot” or “score plot” or
“Ordienation plot”…

``` r
pca$x
```

                     PC1         PC2        PC3           PC4
    England   -144.99315   -2.532999 105.768945  1.612425e-14
    Wales     -240.52915 -224.646925 -56.475555  4.751043e-13
    Scotland   -91.86934  286.081786 -44.415495 -6.044349e-13
    N.Ireland  477.39164  -58.901862  -4.877895  1.145386e-13

``` r
my_cols <- c("peachpuff3", "red", "lightblue", "rosybrown1")
```

``` r
library(ggplot2)
ggplot(pca$x) +
aes(PC1, PC2,) +
geom_point(col=my_cols)
```

![](class07_files/figure-commonmark/unnamed-chunk-32-1.png)

The second major result figure is called a “loading plot” of “variable
contributions plot” or “weight plot”

``` r
ggplot(pca$rotation) +
aes(PC1, rownames(pca$rotation)) +
geom_col()
```

![](class07_files/figure-commonmark/unnamed-chunk-33-1.png)
