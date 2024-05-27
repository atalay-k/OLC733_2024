
#  2pl Verisinin Üretimi 

İki Kategorili Madde Puanları üretimi Rasch modele göre

 p <- 1/(1+exp(-(theta-bb))

```r
b <- rnorm(20, 0, 1)
theta <- rnorm(1000, 0, 1)
madde_sayisi <- length(b)
kisi_sayisi <- length(theta)
replikasyon <- 10
```


```r
b.mat <- matrix(b, kisi_sayisi, madde_sayisi, byrow=T)
head(b.mat)
```

```
##            [,1]        [,2]       [,3]      [,4]       [,5]        [,6]
## [1,] -0.2828662 0.003356358 -0.2106352 0.9273176 -0.3333155 -0.08588651
## [2,] -0.2828662 0.003356358 -0.2106352 0.9273176 -0.3333155 -0.08588651
## [3,] -0.2828662 0.003356358 -0.2106352 0.9273176 -0.3333155 -0.08588651
## [4,] -0.2828662 0.003356358 -0.2106352 0.9273176 -0.3333155 -0.08588651
## [5,] -0.2828662 0.003356358 -0.2106352 0.9273176 -0.3333155 -0.08588651
## [6,] -0.2828662 0.003356358 -0.2106352 0.9273176 -0.3333155 -0.08588651
##            [,7]       [,8]       [,9]     [,10]      [,11]      [,12]     [,13]
## [1,] -0.5408991 -0.3003944 -0.4626559 0.2836625 -0.2102257 -0.3623701 0.1313088
## [2,] -0.5408991 -0.3003944 -0.4626559 0.2836625 -0.2102257 -0.3623701 0.1313088
## [3,] -0.5408991 -0.3003944 -0.4626559 0.2836625 -0.2102257 -0.3623701 0.1313088
## [4,] -0.5408991 -0.3003944 -0.4626559 0.2836625 -0.2102257 -0.3623701 0.1313088
## [5,] -0.5408991 -0.3003944 -0.4626559 0.2836625 -0.2102257 -0.3623701 0.1313088
## [6,] -0.5408991 -0.3003944 -0.4626559 0.2836625 -0.2102257 -0.3623701 0.1313088
##         [,14]     [,15]     [,16]     [,17]     [,18]     [,19]     [,20]
## [1,] 1.228999 0.4530929 0.1595247 -1.239957 0.5889015 0.9720307 -1.715354
## [2,] 1.228999 0.4530929 0.1595247 -1.239957 0.5889015 0.9720307 -1.715354
## [3,] 1.228999 0.4530929 0.1595247 -1.239957 0.5889015 0.9720307 -1.715354
## [4,] 1.228999 0.4530929 0.1595247 -1.239957 0.5889015 0.9720307 -1.715354
## [5,] 1.228999 0.4530929 0.1595247 -1.239957 0.5889015 0.9720307 -1.715354
## [6,] 1.228999 0.4530929 0.1595247 -1.239957 0.5889015 0.9720307 -1.715354
```


```r
theta.mat <- matrix(theta, kisi_sayisi, madde_sayisi)
head(theta.mat)
```

```
##           [,1]      [,2]      [,3]      [,4]      [,5]      [,6]      [,7]
## [1,] -1.989110 -1.989110 -1.989110 -1.989110 -1.989110 -1.989110 -1.989110
## [2,] -1.058355 -1.058355 -1.058355 -1.058355 -1.058355 -1.058355 -1.058355
## [3,]  2.406102  2.406102  2.406102  2.406102  2.406102  2.406102  2.406102
## [4,] -1.326026 -1.326026 -1.326026 -1.326026 -1.326026 -1.326026 -1.326026
## [5,] -1.070747 -1.070747 -1.070747 -1.070747 -1.070747 -1.070747 -1.070747
## [6,] -2.589518 -2.589518 -2.589518 -2.589518 -2.589518 -2.589518 -2.589518
##           [,8]      [,9]     [,10]     [,11]     [,12]     [,13]     [,14]
## [1,] -1.989110 -1.989110 -1.989110 -1.989110 -1.989110 -1.989110 -1.989110
## [2,] -1.058355 -1.058355 -1.058355 -1.058355 -1.058355 -1.058355 -1.058355
## [3,]  2.406102  2.406102  2.406102  2.406102  2.406102  2.406102  2.406102
## [4,] -1.326026 -1.326026 -1.326026 -1.326026 -1.326026 -1.326026 -1.326026
## [5,] -1.070747 -1.070747 -1.070747 -1.070747 -1.070747 -1.070747 -1.070747
## [6,] -2.589518 -2.589518 -2.589518 -2.589518 -2.589518 -2.589518 -2.589518
##          [,15]     [,16]     [,17]     [,18]     [,19]     [,20]
## [1,] -1.989110 -1.989110 -1.989110 -1.989110 -1.989110 -1.989110
## [2,] -1.058355 -1.058355 -1.058355 -1.058355 -1.058355 -1.058355
## [3,]  2.406102  2.406102  2.406102  2.406102  2.406102  2.406102
## [4,] -1.326026 -1.326026 -1.326026 -1.326026 -1.326026 -1.326026
## [5,] -1.070747 -1.070747 -1.070747 -1.070747 -1.070747 -1.070747
## [6,] -2.589518 -2.589518 -2.589518 -2.589518 -2.589518 -2.589518
```



```r
dir.create("Rasch")
```

```
## Warning in dir.create("Rasch"): 'Rasch' already exists
```

```r
for (r in 1:replikasyon){
  logit <- (theta.mat-b.mat)
  P <- 1/(1+exp(-logit))
  head(P)
  rand <- matrix(runif(kisi_sayisi*madde_sayisi), kisi_sayisi, madde_sayisi)
  res <- ifelse(P>rand, 1, 0)
  write.table(res, file=paste("Rasch/Rasch_rep",r,".txt",sep=""), sep=",", 
              row.names=F, col.names=F, na=" ", quote=F)
}
```

İki Kategorili Madde Puanları üretimi 2pl modele göre

p <- 1/(1+exp(-((aa)*(theta-bb))))


```r
set.seed(21)
a <- round(rlnorm(20, meanlog=0.000, sdlog=0.200), 3)
b <- round(rnorm(20, mean=0.000, sd=1.000), 3)
k <- length(b)

set.seed(41)
birey <- rnorm(400, mean=0.500, sd=0.750)
n <- length(birey)
```


```r
theta <- rep(birey, k)
aa <- rep(a, each=n)
bb <- rep(b, each=n)
p <- 1/(1+exp(-((aa)*(theta-bb))))
head(round(p, 3))
```

```
## [1] 0.361 0.574 0.732 0.779 0.715 0.636
```


```r
rr <- runif(n*k, 0, 1)
head(round(rr, 3))
```

```
## [1] 0.339 0.154 0.363 0.315 0.627 0.177
```


```r
puan <- ifelse(p>rr, 1, 0)
puan <- matrix(puan, ncol=k)
```



```r
madde2PL <- matrix(scan("import/madde2PL.dat"), byrow=TRUE, ncol=3)
set.seed(41)
birey <- rnorm(400)
puan2PL <- function(madde, birey){
  a <- madde[, 1]
  b <- madde[, 2]
  k <- length(b)
  n <- length(birey)
  theta <- rep(birey, k)
  aa <- rep(a, each=n)
  bb <- rep(b, each=n)
  p <- 1/(1+exp(-((aa)*(theta-bb))))
  rr <- runif(n*k, 0, 1)
  puan <- ifelse(p>rr, 1, 0)
  puan <- matrix(puan, ncol=k)
  return(puan)
}
```





## Çok Kategorili Madde Puanları Verisinin Üretimi 

- madde ve birey parametrelerinin değerlerinin belirlenmesi 



- Aşamalı tepki modeli (ATM) için, her bir bireyin **i** maddesini **x** kategorisinde  veya **x** kategorisinin üstünde yanıtlama olasılığı **p** hesaplanır.



- Daha sonra hesaplanan olasılık değerleriyle karşılaştırmak üzere 0 ile 1 aralığında tek biçimli (uniform) rastgele değerler (u) üretilir.


- Üretilen her bir değer ilgili olasılık değeriyle karşılaştırılır. 



- Rastgele değer, **x** kategorisinde hesaplanan olasılık değerinden küçük ancak **x+1** kategorisinde hesaplanan olasılık değerinden büyük ise madde puanı olarak **x-1** değeri atanır. 


- a parametresi

```r
set.seed(26)
a <- round(rlnorm(8, meanlog=0.000, sdlog=0.200), 3)
a
```

```
## [1] 0.653 1.258 0.907 1.180 0.921 1.030 1.026 1.201
```

- b1 parametresi


```r
b1 <- seq(from=-2.500, to=-0.750, by=0.250)
```

- diger b parametreleri


```r
b2 <- b1+1.250
b3 <- b2+1.250
b4 <- b3+1.250
cbind(b1,b2,b3,b4)
```

```
##         b1    b2   b3   b4
## [1,] -2.50 -1.25 0.00 1.25
## [2,] -2.25 -1.00 0.25 1.50
## [3,] -2.00 -0.75 0.50 1.75
## [4,] -1.75 -0.50 0.75 2.00
## [5,] -1.50 -0.25 1.00 2.25
## [6,] -1.25  0.00 1.25 2.50
## [7,] -1.00  0.25 1.50 2.75
## [8,] -0.75  0.50 1.75 3.00
```



- Madde sayısının nesneye atanması

```r
k <- length(a)
k
```

```
## [1] 8
```



```r
set.seed(46)
birey <- rnorm(400)
n <- length(birey)
n
```

```
## [1] 400
```


$$P^*_{ix}(\theta) = \frac{exp[a_i(\theta-b_{im})]}{1+exp[a_i(\theta-b_{im})]} = \frac{1}{1+exp{-[a_i(\theta-b_{im})]}}$$
- $P^*_{ix}$:  $θ$ yetenek düzeyinde bir birey için $i$ maddesini $x$ kategorisinde veya $x$ kategorisinin üstünde yanıtlama olasılığıdır.

- $a_i$, $i$ maddesi $a$ parametresidir.

- $b_im$ $i$ maddesi için $m$ (m=x-1) kategori eşiği ile ilişkili kategori eşik parametresidir. 


- Formüle karşılık gelen her bir kategori için olasılık değerlerine ilişkin R komutları aşağıdaki gibidir: 

- p1 <- 1/(1+exp(-((aa)*(theta-bb1))))
- p2 <- 1/(1+exp(-((aa)*(theta-bb2))))
- p3 <- 1/(1+exp(-((aa)*(theta-bb3))))
- p4 <- 1/(1+exp(-((aa)*(theta-bb4))))

- Her bir bireyin her bir maddeye ilişkin belirli bir kategorinin üstünde yanıt verme olasılıklarını (p1, p2, p3 ve p4) hesaplamak için birey parametresinin madde sayısı kadar tekrar etmesi gerekmektedir. 



- Yetenek bireyin tekrarlanması


```r
theta <- rep(birey, k)
```

- Madde parametrelerinin tekrarlanması

 

```r
aa  <- rep(a, each=n)
bb1 <- rep(b1, each=n)
bb2 <- rep(b2, each=n)
bb3 <- rep(b3, each=n)
bb4 <- rep(b4, each=n)
```


:::: {style="display: flex; justify-content: space-between;"}

::: {style="flex: 1; margin-right: 1cm;"}


```r
p1 <- 1/(1+exp(-((aa)*(theta-bb1))))
p2 <- 1/(1+exp(-((aa)*(theta-bb2))))
p3 <- 1/(1+exp(-((aa)*(theta-bb3))))
p4 <- 1/(1+exp(-((aa)*(theta-bb4))))
```

:::

::: {style="flex: 1; margin-right: 1cm;"}

```r
par <- cbind(p1,p2,p3,p4)
head(par)
```

```
##             p1        p2        p3        p4
## [1,] 0.7399113 0.5570647 0.3573252 0.1973021
## [2,] 0.8545847 0.7220738 0.5345751 0.3367685
## [3,] 0.7607382 0.5843073 0.3832516 0.2155112
## [4,] 0.9197751 0.8352146 0.6914261 0.4976362
## [5,] 0.9165028 0.8291341 0.6820595 0.4867537
## [6,] 0.7731207 0.6010321 0.3997558 0.2274559
```
:::

::::



```r
rr <- runif(n*k, 0, 1)
```




```r
head(par)
```

```
##             p1        p2        p3        p4
## [1,] 0.7399113 0.5570647 0.3573252 0.1973021
## [2,] 0.8545847 0.7220738 0.5345751 0.3367685
## [3,] 0.7607382 0.5843073 0.3832516 0.2155112
## [4,] 0.9197751 0.8352146 0.6914261 0.4976362
## [5,] 0.9165028 0.8291341 0.6820595 0.4867537
## [6,] 0.7731207 0.6010321 0.3997558 0.2274559
```


```r
head(rr, 6)
```

```
## [1] 0.3757090 0.4476721 0.7699587 0.7494361 0.9873121 0.9012236
```




```
##      [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8]
## [1,]    2    1    3    0    0    2    0    0
## [2,]    3    4    3    3    2    2    1    4
## [3,]    0    2    2    1    1    2    1    1
## [4,]    2    3    2    4    3    3    1    2
## [5,]    0    4    1    2    0    0    1    2
## [6,]    0    2    1    1    2    2    1    2
```

 
- Madde Puanlarının Atanması 


```r
puan <- 0
for (j in 1:(k*n)){
  if((rr[j]>p1[j]))puan[j] <- 0 
  else if((rr[j]<p1[j]&rr[j]>p2[j]))puan[j] <- 1 
  else if((rr[j]<p2[j]&rr[j]>p3[j]))puan[j] <- 2 
  else if((rr[j]<p3[j]&rr[j]>p4[j]))puan[j] <- 3 
  else puan[j] <- 4 
}

puan <- matrix(puan, ncol=k)
```


- "puan" matrisinin ilk 2 satırının seçilmesi

```r
puan[1:2,]
id <- matrix(1:n, ncol=1)

data <- cbind(id, puan)
```

```
##      [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8]
## [1,]    2    1    3    0    0    2    0    0
## [2,]    3    4    3    3    2    2    1    4
```




```r
maddeAT <- cbind(a,b1,b2,b3,b4)
birey <- rnorm(400)
```







```r
library(mirt)
```

```
## Loading required package: stats4
```

```
## Loading required package: lattice
```

```r
a <- matrix(rlnorm(20,.2,.3))
diffs <- t(apply(matrix(runif(20*4, .3, 1), 20), 1, cumsum))
diffs <- -(diffs - rowMeans(diffs))
d <- diffs + rnorm(20)
dat <- simdata(a, d, 500, itemtype = 'graded')
head(dat)
```

```
##      Item_1 Item_2 Item_3 Item_4 Item_5 Item_6 Item_7 Item_8 Item_9 Item_10
## [1,]      4      2      4      0      1      4      0      4      4       3
## [2,]      1      0      3      2      0      3      0      4      3       1
## [3,]      0      0      4      0      0      4      0      4      1       1
## [4,]      3      0      2      1      1      4      4      2      0       0
## [5,]      3      0      4      0      3      4      0      2      1       0
## [6,]      4      4      4      4      4      4      0      4      4       4
##      Item_11 Item_12 Item_13 Item_14 Item_15 Item_16 Item_17 Item_18 Item_19
## [1,]       4       4       2       4       4       4       3       3       4
## [2,]       2       2       3       4       0       0       2       0       0
## [3,]       0       4       2       2       0       0       0       0       2
## [4,]       0       1       0       0       0       0       0       0       0
## [5,]       0       4       3       2       0       1       0       0       2
## [6,]       4       4       4       4       4       4       4       4       4
##      Item_20
## [1,]       3
## [2,]       2
## [3,]       0
## [4,]       3
## [5,]       0
## [6,]       2
```


<!-- xaringanBuilder::build_pdf("ATM.Rmd") -->
