# Döngü  Fonkisyonları 




##  Komut Satırında Döngü Oluşturma

Programlama yaparken `for` ve `while` döngüleri yazmak yararlıdır, ancak komut satırında etkileşimli olarak çalışırken özellikle kolay değildir. Küme parantezleri içeren çok satırlı ifadeleri komut satırında çalışırken sıralamak o kadar kolay değildir. R, hayatınızı kolaylaştırmak için döngüleri kompakt bir biçimde uygulayan bazı fonksiyonlara sahiptir.

- `lapply()`: Bir liste üzerinde döngü ve her öğe üzerinde bir işlevi değerlendirme 

- `sapply()`: `lapply` ile aynıdır ancak sonucu basitleştirmeye çalışın

- `apply()`: Bir dizinin kenar boşlukları üzerinde bir işlev uygulama

- `tapply()`: Bir vektörün alt kümeleri üzerinde bir fonksiyon uygulama

- `mapply()`: `lapply` nin çok değişkenli versiyonu

Yardımcı bir işlev olan `split` de özellikle `lapply` ile birlikte kullanışlıdır.

## `lapply()`


lapply()` fonksiyonu aşağıdaki basit işlemler dizisini gerçekleştirir:

1. Bir liste üzerinde döngü yaparak, listedeki her bir öğe üzerinde yineleme yapar
2. Listenin her bir öğesine bir *fonksiyon* uygular (sizin belirlediğiniz bir fonksiyon)
3. ve bir liste döndürür (`l` "liste" içindir). 

Bu fonksiyon üç argüman alır: (1) bir liste `X`; (2) bir fonksiyon (veya bir fonksiyonun adı) `FUN`; (3) `...` argümanı aracılığıyla diğer argümanlar. Eğer `X` bir liste değilse, `as.list()` kullanılarak bir listeye zorlanacaktır.

lapply()` fonksiyonunun gövdesi burada görülebilir.


```r
> lapply
function (X, FUN, ...) 
{
    FUN <- match.fun(FUN)
    if (!is.vector(X) || is.object(X)) 
        X <- as.list(X)
    .Internal(lapply(X, FUN))
}
<bytecode: 0x00000215f198a3b8>
<environment: namespace:base>
```


Girdinin sınıfından bağımsız olarak `lapply()` fonksiyonunun her zaman bir liste döndürdüğünü unutmamak önemlidir.

İşte `mean()` fonksiyonunun bir listenin tüm elemanlarına uygulanmasına bir örnek. Orijinal listede isimler varsa, isimler çıktıda korunacaktır.



```r
> x <- list(a = 1:5, b = rnorm(10))
> lapply(x, mean)
$a
[1] 3

$b
[1] 0.1322028
```

Burada `mean()` fonksiyonunu `lapply()` fonksiyonuna bir argüman olarak aktardığımıza dikkat edin. R'deki fonksiyonlar bu şekilde kullanılabilir ve tıpkı diğer nesneler gibi argüman olarak ileri geri aktarılabilir. Bir fonksiyonu başka bir fonksiyona aktardığınızda, bir fonksiyonu *çağırırken* yaptığınız gibi `()` açık ve kapalı parantezlerini eklemeniz gerekmez.

İşte `lapply()` kullanımına başka bir örnek.



```r
> x <- list(a = 1:4, b = rnorm(10), c = rnorm(20, 1), d = rnorm(100, 5))
> lapply(x, mean)
$a
[1] 2.5

$b
[1] 0.248845

$c
[1] 0.9935285

$d
[1] 5.051388
```

Bir fonksiyonu her biri farklı bir argümanla birden çok kez değerlendirmek için `lapply()` fonksiyonunu kullanabilirsiniz. Aşağıda, `runif()` fonksiyonunu (düzgün dağılımlı rastgele değişkenler üretmek için) her seferinde farklı sayıda rastgele sayı üreterek dört kez çağırdığım bir örnek yer almaktadır.



```r
> x <- 1:4
> lapply(x, runif)
[[1]]
[1] 0.02778712

[[2]]
[1] 0.5273108 0.8803191

[[3]]
[1] 0.37306337 0.04795913 0.13862825

[[4]]
[1] 0.3214921 0.1548316 0.1322282 0.2213059
```

Bir fonksiyonu `lapply()` fonksiyonuna aktardığınızda, `lapply()` listenin elemanlarını alır ve bunları uyguladığınız fonksiyonun *ilk argümanı* olarak geçirir. Yukarıdaki örnekte, `runif()` fonksiyonunun ilk argümanı `n`dir ve bu nedenle `1:4` dizisinin tüm elemanları `runif()` fonksiyonunun `n` argümanına aktarılır.

lapply()` fonksiyonuna aktardığınız fonksiyonlar başka argümanlara sahip olabilir. Örneğin, `runif()` fonksiyonunun bir `min` ve `max` argümanı da vardır. Yukarıdaki örnekte `min` ve `max` için varsayılan değerleri kullandım. Bunun için `lapply()` bağlamında farklı değerleri nasıl belirtebilirsiniz?

İşte burada `lapply()` fonksiyonunun `...` argümanı devreye girer. ...` argümanına yerleştirdiğiniz tüm argümanlar, listenin öğelerine uygulanan fonksiyona aktarılacaktır.

Burada, `min = 0` ve `max = 10` argümanları her çağrıldığında `runif()` fonksiyonuna aktarılır.


```r
> x <- 1:4
> lapply(x, runif, min = 0, max = 10)
[[1]]
[1] 2.263808

[[2]]
[1] 1.314165 9.815635

[[3]]
[1] 3.270137 5.069395 6.814425

[[4]]
[1] 0.9916910 1.1890256 0.5043966 9.2925392
```

Yani artık rastgele sayılar 0 ile 1 arasında (varsayılan) olmak yerine, hepsi 0 ile 10 arasındadır.

lapply()` fonksiyonu ve arkadaşları _anonim_ fonksiyonları yoğun bir şekilde kullanır. Anonim fonksiyonların isimleri yoktur. Bu fonksiyonlar siz `lapply()` fonksiyonunu kullanırken "anında" oluşturulur. lapply()` çağrısı tamamlandığında, fonksiyon kaybolur ve çalışma alanında görünmez.

Burada iki matris içeren bir liste oluşturuyorum.


```r
> x <- list(a = matrix(1:4, 2, 2), b = matrix(1:6, 3, 2)) 
> x
$a
     [,1] [,2]
[1,]    1    3
[2,]    2    4

$b
     [,1] [,2]
[1,]    1    4
[2,]    2    5
[3,]    3    6
```

Listedeki her matrisin ilk sütununu çıkarmak istediğimi varsayalım. Şöyle yazabilirim
her matrisin ilk sütununu çıkarmak için anonim bir fonksiyon.


```r
> lapply(x, function(elt) { elt[,1] })
$a
[1] 1 2

$b
[1] 1 2 3
```

Dikkat ederseniz `function()` tanımını doğrudan `lapply()` çağrısının içinde. Bu tamamen kabul edilebilir bir durumdur. Keyfi olarak karmaşık bir fonksiyon tanımını `lapply()` içine koyabilirsiniz, ancak daha karmaşık olacaksa, fonksiyonu ayrı olarak tanımlamak muhtemelen daha iyi bir fikirdir. 

Örneğin, aşağıdakileri yapabilirdim.


```r
> f <- function(elt) {
+         elt[, 1]
+ }
> lapply(x, f)
$a
[1] 1 2

$b
[1] 1 2 3
```

Artık fonksiyon anonim değildir; adı `f`dir. Anonim bir fonksiyon mu kullanacağınız yoksa önce bir fonksiyon mu tanımlayacağınız bağlamınıza bağlıdır. Eğer `f` fonksiyonunun kodunuzun diğer bölümlerinde çok ihtiyaç duyacağınız bir şey olduğunu düşünüyorsanız, onu ayrıca tanımlamak isteyebilirsiniz. Ancak sadece bu `lapply()` çağrısı için kullanacaksanız, muhtemelen anonim bir fonksiyon kullanmak daha basittir.


## `sapply()`

sapply()` fonksiyonu `lapply()` fonksiyonuna benzer şekilde davranır; tek gerçek fark dönüş değerindedir. sapply()` mümkünse `lapply()` sonucunu basitleştirmeye çalışacaktır. Esasen, `sapply()` girdisi üzerinde `lapply()` çağırır ve ardından aşağıdaki algoritmayı uygular:

- Eğer sonuç her elemanın uzunluğu 1 olan bir liste ise, o zaman bir vektör döndürülür

- Sonuç, her elemanı aynı uzunlukta (> 1) bir vektör olan bir liste ise, bir matris döndürülür.

- İşleri çözemezse, bir liste döndürülür

İşte `lapply()` çağrısının sonucu.


```r
> x <- list(a = 1:4, b = rnorm(10), c = rnorm(20, 1), d = rnorm(100, 5))
> lapply(x, mean)
$a
[1] 2.5

$b
[1] -0.251483

$c
[1] 1.481246

$d
[1] 4.968715
```

lapply()` işlevinin (her zamanki gibi) bir liste döndürdüğüne, ancak listenin her bir öğesinin uzunluğunun 1 olduğuna dikkat edin.

İşte aynı liste üzerinde `sapply()` çağrısının sonucu.


```r
> sapply(x, mean) 
        a         b         c         d 
 2.500000 -0.251483  1.481246  4.968715 
```

lapply()` işlevinin sonucu, her öğesinin uzunluğu 1 olan bir liste olduğundan, `sapply()` işlevi çıktıyı, genellikle bir listeden daha kullanışlı olan sayısal bir vektöre daraltmıştır.



## `split()`


split()` fonksiyonu bir vektörü veya diğer nesneleri alır ve bunları bir faktör veya faktörler listesi tarafından belirlenen gruplara böler.

split()` fonksiyonunun argümanları şunlardır


```r
> str(split)
function (x, f, drop = FALSE, ...)  
```

nerede

- x` bir vektör (veya liste) veya veri setidir
- f` bir faktör (veya bir faktöre zorlanmış) veya bir faktörler listesi
- `drop` boş faktör seviyelerinin bırakılıp bırakılmayacağını belirtir

split()` ve `lapply()` veya `sapply()` gibi bir fonksiyonun kombinasyonu R'de yaygın bir paradigmadır. Temel fikir, bir veri yapısını alıp başka bir değişken tarafından tanımlanan alt kümelere bölebilmeniz ve bu alt kümeler üzerinde bir fonksiyon uygulayabilmenizdir. Bu fonksiyonun alt kümeler üzerinde uygulanmasının sonuçları daha sonra harmanlanır ve bir nesne olarak döndürülür. Bu işlem dizisi bazen başka bağlamlarda "map-reduce" olarak adlandırılır.

Burada bazı verileri simüle ediyoruz ve bir faktör değişkenine göre bölüyoruz. Bir faktör değişkeninde "seviyeler oluşturmak" için `gl()` fonksiyonunu kullandığımıza dikkat edin.



```r
> x <- c(rnorm(10), runif(10), rnorm(10, 1))
> f <- gl(3, 10)
> split(x, f)
$`1`
 [1]  0.3981302 -0.4075286  1.3242586 -0.7012317 -0.5806143 -1.0010722
 [7] -0.6681786  0.9451850  0.4337021  1.0051592

$`2`
 [1] 0.34822440 0.94893818 0.64667919 0.03527777 0.59644846 0.41531800
 [7] 0.07689704 0.52804888 0.96233331 0.70874005

$`3`
 [1]  1.13444766  1.76559900  1.95513668  0.94943430  0.69418458  1.89367370
 [7] -0.04729815  2.97133739  0.61636789  2.65414530
```

Yaygın bir deyim `split` ve ardından `lapply`dir.


```r
> lapply(split(x, f), mean)
$`1`
[1] 0.07478098

$`2`
[1] 0.5266905

$`3`
[1] 1.458703
```

## Veri Setini Bölme


```r
> library(datasets)
> head(airquality)
```

<div class="kable-table">

| Ozone| Solar.R| Wind| Temp| Month| Day|
|-----:|-------:|----:|----:|-----:|---:|
|    41|     190|  7.4|   67|     5|   1|
|    36|     118|  8.0|   72|     5|   2|
|    12|     149| 12.6|   74|     5|   3|
|    18|     313| 11.5|   62|     5|   4|
|    NA|      NA| 14.3|   56|     5|   5|
|    28|      NA| 14.9|   66|     5|   6|

</div>


Her ay için ayrı alt veri çerçevelerimiz olması için `airquality` veri çerçevesini `Month` değişkenine göre bölebiliriz.


```r
> s <- split(airquality, airquality$Month)
> str(s)
List of 5
 $ 5:'data.frame':	31 obs. of  6 variables:
  ..$ Ozone  : int [1:31] 41 36 12 18 NA 28 23 19 8 NA ...
  ..$ Solar.R: int [1:31] 190 118 149 313 NA NA 299 99 19 194 ...
  ..$ Wind   : num [1:31] 7.4 8 12.6 11.5 14.3 14.9 8.6 13.8 20.1 8.6 ...
  ..$ Temp   : int [1:31] 67 72 74 62 56 66 65 59 61 69 ...
  ..$ Month  : int [1:31] 5 5 5 5 5 5 5 5 5 5 ...
  ..$ Day    : int [1:31] 1 2 3 4 5 6 7 8 9 10 ...
 $ 6:'data.frame':	30 obs. of  6 variables:
  ..$ Ozone  : int [1:30] NA NA NA NA NA NA 29 NA 71 39 ...
  ..$ Solar.R: int [1:30] 286 287 242 186 220 264 127 273 291 323 ...
  ..$ Wind   : num [1:30] 8.6 9.7 16.1 9.2 8.6 14.3 9.7 6.9 13.8 11.5 ...
  ..$ Temp   : int [1:30] 78 74 67 84 85 79 82 87 90 87 ...
  ..$ Month  : int [1:30] 6 6 6 6 6 6 6 6 6 6 ...
  ..$ Day    : int [1:30] 1 2 3 4 5 6 7 8 9 10 ...
 $ 7:'data.frame':	31 obs. of  6 variables:
  ..$ Ozone  : int [1:31] 135 49 32 NA 64 40 77 97 97 85 ...
  ..$ Solar.R: int [1:31] 269 248 236 101 175 314 276 267 272 175 ...
  ..$ Wind   : num [1:31] 4.1 9.2 9.2 10.9 4.6 10.9 5.1 6.3 5.7 7.4 ...
  ..$ Temp   : int [1:31] 84 85 81 84 83 83 88 92 92 89 ...
  ..$ Month  : int [1:31] 7 7 7 7 7 7 7 7 7 7 ...
  ..$ Day    : int [1:31] 1 2 3 4 5 6 7 8 9 10 ...
 $ 8:'data.frame':	31 obs. of  6 variables:
  ..$ Ozone  : int [1:31] 39 9 16 78 35 66 122 89 110 NA ...
  ..$ Solar.R: int [1:31] 83 24 77 NA NA NA 255 229 207 222 ...
  ..$ Wind   : num [1:31] 6.9 13.8 7.4 6.9 7.4 4.6 4 10.3 8 8.6 ...
  ..$ Temp   : int [1:31] 81 81 82 86 85 87 89 90 90 92 ...
  ..$ Month  : int [1:31] 8 8 8 8 8 8 8 8 8 8 ...
  ..$ Day    : int [1:31] 1 2 3 4 5 6 7 8 9 10 ...
 $ 9:'data.frame':	30 obs. of  6 variables:
  ..$ Ozone  : int [1:30] 96 78 73 91 47 32 20 23 21 24 ...
  ..$ Solar.R: int [1:30] 167 197 183 189 95 92 252 220 230 259 ...
  ..$ Wind   : num [1:30] 6.9 5.1 2.8 4.6 7.4 15.5 10.9 10.3 10.9 9.7 ...
  ..$ Temp   : int [1:30] 91 92 93 93 87 84 80 78 75 73 ...
  ..$ Month  : int [1:30] 9 9 9 9 9 9 9 9 9 9 ...
  ..$ Day    : int [1:30] 1 2 3 4 5 6 7 8 9 10 ...
```

Daha sonra her bir alt veri çerçevesi için `Ozone`, `Solar.R` ve `Wind` sütun ortalamalarını alabiliriz.


```r
> lapply(s, function(x) {
+         colMeans(x[, c("Ozone", "Solar.R", "Wind")])
+ })
$`5`
   Ozone  Solar.R     Wind 
      NA       NA 11.62258 

$`6`
    Ozone   Solar.R      Wind 
       NA 190.16667  10.26667 

$`7`
     Ozone    Solar.R       Wind 
        NA 216.483871   8.941935 

$`8`
   Ozone  Solar.R     Wind 
      NA       NA 8.793548 

$`9`
   Ozone  Solar.R     Wind 
      NA 167.4333  10.1800 
```

Daha okunabilir bir çıktı için burada `sapply()` kullanmak daha iyi olabilir.


```r
> sapply(s, function(x) {
+         colMeans(x[, c("Ozone", "Solar.R", "Wind")])
+ })
               5         6          7        8        9
Ozone         NA        NA         NA       NA       NA
Solar.R       NA 190.16667 216.483871       NA 167.4333
Wind    11.62258  10.26667   8.941935 8.793548  10.1800
```

Ne yazık ki, verilerde `NA`lar vardır, bu nedenle bu değişkenlerin ortalamalarını alamayız. Ancak, `colMeans` fonksiyonuna ortalamayı hesaplamadan önce `NA`ları kaldırmasını söyleyebiliriz.


```r
> sapply(s, function(x) {
+         colMeans(x[, c("Ozone", "Solar.R", "Wind")], 
+                  na.rm = TRUE)
+ })
                5         6          7          8         9
Ozone    23.61538  29.44444  59.115385  59.961538  31.44828
Solar.R 181.29630 190.16667 216.483871 171.857143 167.43333
Wind     11.62258  10.26667   8.941935   8.793548  10.18000
```



## tapply

`tapply()` bir vektörün alt kümeleri üzerinde bir fonksiyon uygulamak için kullanılır. Sadece vektörler için `split()` ve `sapply()` fonksiyonlarının bir kombinasyonu olarak düşünülebilir. Bana `tapply()`'deki "t "nin "table" anlamına geldiği söylendi, ancak bu doğrulanmadı.


```r
> str(tapply)
function (X, INDEX, FUN = NULL, ..., default = NA, simplify = TRUE)  
```

tapply()` işlevinin argümanları aşağıdaki gibidir:

- X` bir vektördür
- `INDEX` bir faktör ya da faktörler listesidir (ya da faktörlere zorlanırlar) 
- FUN` uygulanacak bir işlevdir
- ... geçirilecek diğer argümanları içerir `FUN`
- `basitleştir`, sonucu basitleştirmeli miyiz?

Sayılardan oluşan bir vektör verildiğinde, basit bir işlem grup ortalamalarını almaktır.


```r
> ## veri üret
> x <- c(rnorm(10), runif(10), rnorm(10, 1))
> ## factor değişken
> f <- gl(3, 10)   
> f
> tapply(x, f, mean)
 [1] 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 3 3 3 3 3 3 3 3 3 3
Levels: 1 2 3
        1         2         3 
0.1457707 0.4659058 1.2320306 
```

Sonucu sadeleştirmeden grup ortalamalarını da alabiliriz, bu da bize bir liste verecektir. Tek bir değer döndüren fonksiyonlar için genellikle istediğimiz bu değildir, ancak yapılabilir.


```r
> tapply(x, f, mean, simplify = FALSE)
$`1`
[1] 0.1457707

$`2`
[1] 0.4659058

$`3`
[1] 1.232031
```


Tek bir değerden daha fazlasını döndüren fonksiyonları da uygulayabiliriz. Bu durumda, `tapply()` sonucu basitleştirmeyecek ve bir liste döndürecektir. İşte her bir alt grubun aralığını bulmak için bir örnek.


```r
> tapply(x, f, range)
$`1`
[1] -1.024548  1.512213

$`2`
[1] 0.06490054 0.85750154

$`3`
[1] -0.8697888  2.4970410
```


## `apply()`


apply()` fonksiyonu, bir dizinin kenar boşlukları üzerinde bir fonksiyonu (genellikle anonim bir fonksiyon) değerlendirmek için kullanılır. Çoğunlukla bir matrisin (sadece 2 boyutlu bir dizi) satırlarına veya sütunlarına bir fonksiyon uygulamak için kullanılır. Ancak, örneğin bir dizi matrisin ortalamasını almak gibi genel dizilerde de kullanılabilir. apply()` kullanmak bir döngü yazmaktan gerçekten daha hızlı değildir, ancak tek satırda çalışır ve oldukça kompakttır.



```r
> str(apply)
function (X, MARGIN, FUN, ..., simplify = TRUE)  
```

apply()` işlevinin argümanları şunlardır

- X` bir dizidir
- MARGIN` hangi kenar boşluklarının "tutulması" gerektiğini gösteren bir tamsayı vektörüdür. 
- FUN` uygulanacak bir fonksiyondur
- ...`, `FUN`a aktarılacak diğer argümanlar içindir


Burada 20'ye 10'luk bir Normal rastgele sayılar matrisi oluşturuyorum. Daha sonra her bir sütunun ortalamasını hesaplıyorum.


```r
> x <- matrix(rnorm(200), 20, 10)
> apply(x, 2, mean)  ## Her sütunun ortalamasını alın
 [1] -0.05285811 -0.07607188 -0.14416325  0.26788937 -0.04893217 -0.31775109
 [7]  0.04588463 -0.02798894  0.07064680 -0.23878366
```

Ayrıca her satırın toplamını da hesaplayabilirim.


```r
> apply(x, 1, sum)   ## Her satırın ortalamasını alın
 [1] -3.81001275  0.28069148 -2.84131594 -0.34383521  3.35432798 -1.41790398
 [7]  4.16348869 -3.12614772 -5.05668423  5.08399986 -0.48483448  5.33222301
[13] -3.33862932 -1.39998450  2.37859098  0.01082604 -6.29457190 -0.26287700
[19]  0.71133578 -3.38125293
```

Her iki `apply()` çağrısında da dönüş değerinin bir sayı vektörü olduğuna dikkat edin.

Muhtemelen ikinci argümanın, satır istatistikleri mi yoksa sütun istatistikleri mi istediğimize bağlı olarak 1 veya 2 olduğunu fark etmişsinizdir. Peki `apply()` fonksiyonunun ikinci argümanı tam olarak nedir?

MARGIN` argümanı esasen `apply()` fonksiyonuna dizinin hangi boyutunu korumak veya saklamak istediğinizi belirtir. Yani her bir sütunun ortalamasını alırken şunu belirtirim


```r
> apply(x, 2, mean)
```

çünkü ortalamayı alarak ilk boyutu (satırları) daraltmak istiyorum ve sütun sayısını korumak istiyorum. Benzer şekilde, satır toplamlarını istediğimde


```r
> apply(x, 1, mean)
```

çünkü sütunları (ikinci boyut) daraltmak ve satır sayısını (ilk boyut) korumak istiyorum.


## Sütun/Satır Toplamları ve Ortalamaları

Matrislerin sütun/satır toplamları ve sütun/satır ortalamalarının özel durumları için bazı kullanışlı kısayollarımız vardır.

- `rowSums` = `apply(x, 1, sum)`
- `rowMeans` = `apply(x, 1, mean)`
- `colSums` = `apply(x, 2, sum)`
- `colMeans` = `apply(x, 2, mean)`

Kısayol fonksiyonları yoğun bir şekilde optimize edilmiştir ve bu nedenle çok daha hızlıdır, ancak büyük bir matris kullanmadığınız sürece muhtemelen fark etmeyeceksiniz. Bu fonksiyonların bir başka güzel yönü de biraz daha açıklayıcı olmalarıdır. Kodunuzda `colMeans(x)` yazmak, `apply(x, 2, mean)` yazmaktan muhtemelen daha anlaşılırdır.

## Diğer apply lar

`apply()` fonksiyonu ile toplamları ve ortalamaları almaktan daha fazlasını yapabilirsiniz. Örneğin, `quantile()` fonksiyonunu kullanarak bir matrisin satırlarının niceliklerini hesaplayabilirsiniz.


```r
> x <- matrix(rnorm(200), 20, 10)
> ## çeyreklikler
> apply(x, 1, quantile, probs = c(0.25, 0.75))    
          [,1]       [,2]       [,3]       [,4]       [,5]         [,6]
25% -1.1168291 -0.2023664 -0.6802383 -1.3551031 -0.1823252 -1.267431359
75%  0.4138139  1.0892341  0.6240771 -0.4557305  1.3744778  0.002908451
          [,7]       [,8]       [,9]      [,10]      [,11]      [,12]     [,13]
25% -0.9954289 -0.2107765 -0.8557544 -0.7000363 -1.0884151 -0.6693040 0.2908481
75%  0.2281247  1.1643644  0.5043939  0.3575873  0.1843547  0.8210295 1.3667301
         [,14]      [,15]       [,16]      [,17]      [,18]      [,19]
25% -0.4602083 -1.0432010 -1.12773555 -1.4571706 -0.2406991 -0.3226845
75%  0.4424153  0.3571219  0.03653687 -0.1705336  0.6504486  1.1460854
        [,20]
25% -0.329898
75%  1.247092
```

Dikkat ederseniz `probs = c(0.25, 0.75)` argümanını `quantile()` fonksiyonuna `...` argümanı aracılığıyla `apply()` fonksiyonuna aktarmak zorunda kaldım.

Daha yüksek boyutlu bir örnek için, $2\times2$ matrislerinden oluşan bir dizi oluşturabilir ve dizideki matrislerin ortalamasını hesaplayabilirim.


```r
> a <- array(rnorm(2 * 2 * 10), c(2, 2, 10))
> apply(a, c(1, 2), mean)
            [,1]        [,2]
[1,]  0.08796671 -0.04719386
[2,] -0.15271298  0.07811452
```

Buradaki `apply()` çağrısında, `MARGIN` argümanı aracılığıyla birinci ve ikinci boyutları korumak ve ortalamayı alarak üçüncü boyutu daraltmak istediğimi belirttim.

Bu özel işlemi `colMeans()` fonksiyonu aracılığıyla yapmanın daha hızlı bir yolu vardır.


```r
> rowMeans(a, dims = 2)    ## daha hızlı
            [,1]        [,2]
[1,]  0.08796671 -0.04719386
[2,] -0.15271298  0.07811452
```


Bu durumda, `rowMeans()` kullanımının daha az okunabilir olduğunu iddia edebilirim, ancak büyük dizilerde önemli ölçüde daha hızlıdır.

## `mapply()`


`mapply()` fonksiyonu, bir dizi argüman üzerinde paralel olarak bir fonksiyon uygulayan bir tür çok değişkenli uygulamadır. lapply()` ve arkadaşlarının yalnızca tek bir R nesnesi üzerinde yineleme yaptığını hatırlayın. Peki ya birden fazla R nesnesi üzerinde paralel olarak yineleme yapmak isterseniz? İşte `mapply()` bunun içindir.


```r
> str(mapply)
function (FUN, ..., MoreArgs = NULL, SIMPLIFY = TRUE, USE.NAMES = TRUE)  
```

mapply()` işlevinin argümanları şunlardır

- FUN` uygulanacak bir işlevdir
- ...` üzerine uygulanacak R nesnelerini içerir
- `MoreArgs` `FUN` için diğer argümanların bir listesidir.
- SIMPLIFY` sonucun basitleştirilip basitleştirilmeyeceğini belirtir

mapply()` fonksiyonu, `lapply()` fonksiyonundan farklı bir argüman sırasına sahiptir, çünkü üzerinde yinelenecek nesne yerine uygulanacak fonksiyon önce gelir. Fonksiyonu uyguladığımız R nesneleri `...` argümanında verilir, çünkü keyfi sayıda R nesnesi üzerinde uygulama yapabiliriz.

Örneğin, aşağıdakileri yazmak sıkıcıdır

`list(rep(1, 4), rep(2, 3), rep(3, 2), rep(4, 1))`

Bunun yerine `mapply()` ile şunları yapabiliriz


```r
>  mapply(rep, 1:4, 4:1)
[[1]]
[1] 1 1 1 1

[[2]]
[1] 2 2 2

[[3]]
[1] 3 3

[[4]]
[1] 4
```

Bu, `rep()`in ilk argümanına `1:4` dizisini ve ikinci argümanına `4:1` dizisini geçirir.


İşte randon Normal değişkenleri simüle etmek için başka bir örnek.


```r
> noise <- function(n, mean, sd) {
+       rnorm(n, mean, sd)
+ }
> ## 5 random sayı
> noise(5, 1, 2)        
> 
> ##  sadece 1 sayı kümesini simüle ediyor, 5 değil
> noise(1:5, 1:5, 2)    
[1]  0.3206461 -2.3295293  2.8577035  3.8336537  0.8745584
[1] -0.9618047  4.1743005  3.2786541  3.2274558  7.2471708
```

Burada `mapply()` fonksiyonunu kullanarak `1:5` dizisini ayrı ayrı `noise()` fonksiyonuna aktarabiliriz, böylece her biri farklı uzunluk ve ortalamaya sahip 5 rastgele sayı kümesi elde edebiliriz.



```r
> mapply(noise, 1:5, 1:5, 2)
[[1]]
[1] -0.5196913

[[2]]
[1] 4.2979182 0.3150475

[[3]]
[1] 3.7828267 4.7827545 0.3294826

[[4]]
[1] 4.796247 3.776826 5.351488 2.422804

[[5]]
[1] 4.826027 7.764568 5.336980 6.646382 4.558211
```

Yukarıdaki `mapply()` çağrısı aşağıdaki ile aynıdır


```r
> list(noise(1, 1, 2), noise(2, 2, 2),
+      noise(3, 3, 2), noise(4, 4, 2),
+      noise(5, 5, 2))
[[1]]
[1] -1.058783

[[2]]
[1]  1.9781486 -0.4499823

[[3]]
[1] -2.1922228  5.3382452  0.8261824

[[4]]
[1] 0.347834 5.990564 3.976276 2.800743

[[5]]
[1] 4.644104 4.148037 6.993318 6.455321 1.546739
```

## Bir Fonksiyonu Vektörleştirme

`mapply()` fonksiyonu bir fonksiyonu otomatik olarak "vektörleştirmek" için kullanılabilir. Bunun anlamı, tipik olarak yalnızca tek argüman alan bir fonksiyonu almak ve vektör argümanları alabilen yeni bir fonksiyon oluşturmak için kullanılabileceğidir. Bu genellikle fonksiyonları çizmek istediğinizde gereklidir.

İşte bazı veriler, bir ortalama parametre ve bir standart sapma verildiğinde kareler toplamını hesaplayan bir fonksiyon örneği. Formül $\sum_{i=1}^n(x_i-\mu)^2/\sigma^2$ şeklindedir.


```r
> sumsq <- function(mu, sigma, x) {
+         sum(((x - mu) / sigma)^2)
+ }
```

Bu fonksiyon bir ortalama `mu`, bir standart sapma `sigma` ve bir vektör `x` içinde bazı veriler alır.

Birçok istatistiksel uygulamada, en uygun `mu` ve `sigma` değerlerini bulmak için kareler toplamını minimize etmek isteriz. Bunu yapmadan önce, birçok farklı `mu` veya `sigma` değeri için fonksiyonu değerlendirmek veya çizmek isteyebiliriz. Ancak, bir `mu` veya `sigma` vektörü geçmek bu fonksiyonla çalışmayacaktır çünkü vektörleştirilmemiştir.


```r
> x <- rnorm(100)       ## veri üret
> sumsq(1:10, 1:10, x)  ## İstediğimiz bu değil
[1] 109.7211
```

`sumsq()` çağrısının 10 değer yerine yalnızca bir değer ürettiğine dikkat edin.

Ancak, yapmak istediğimizi `mapply()` kullanarak yapabiliriz.


```r
> mapply(sumsq, 1:10, 1:10, MoreArgs = list(x = x))
 [1] 209.3372 126.2175 111.1558 105.9960 103.6587 102.4167 101.6844 101.2198
 [9] 100.9086 100.6913
```

Hatta R'de `Vectorize()` adında, fonksiyonunuzun vektörleştirilmiş bir versiyonunu otomatik olarak oluşturabilen bir fonksiyon bile vardır. Böylece aşağıdaki gibi tamamen vektörleştirilmiş bir `vsumsq()` fonksiyonu oluşturabiliriz.


```r
> vsumsq <- Vectorize(sumsq, c("mu", "sigma"))
> vsumsq(1:10, 1:10, x)
 [1] 209.3372 126.2175 111.1558 105.9960 103.6587 102.4167 101.6844 101.2198
 [9] 100.9086 100.6913
```

Çok havalı, değil mi?


## Özet

* R'deki döngü fonksiyonları çok güçlüdür çünkü kompakt bir form kullanarak veriler üzerinde bir dizi işlem yapmanıza olanak tanır

* Bir döngü fonksiyonunun çalışması, bir R nesnesi (örneğin bir liste, vektör veya matris) üzerinde yinelemeyi, nesnenin her bir öğesine bir fonksiyon uygulamayı ve sonuçları harmanlayıp harmanlanmış sonuçları döndürmeyi içerir.

* Döngü fonksiyonları, döngü fonksiyonunun ömrü boyunca var olan ancak hiçbir yerde saklanmayan anonim fonksiyonları yoğun bir şekilde kullanır

* split()` fonksiyonu, bir R nesnesini başka bir değişken tarafından belirlenen ve daha sonra döngü fonksiyonları kullanılarak üzerinde döngü yapılabilen alt kümelere bölmek için kullanılabilir.


# apply ailesi 


- **apply()** fonksiyonu matris ve dizilerde satır ve sütundaki değerlerin 
belirlenen fonksiyona göre değerlerini özetlemek için kullanılır. 

- **apply()** fonksiyonunun genel kullanımı


```r
> # apply(x, margin, FUN, …)
```

- margin argümanı satır (**1**), sütun (**2**) veya her ikisini (**c(1,2)**)
FUN argümanı ise uygulanacak fonksiyonu belirtmektedir.


```r
> X = matrix(c(1:9), nr= 3, byrow = T)
> X
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    4    5    6
[3,]    7    8    9
```


```r
> #1 satirlar icin, 
> apply(X, 1, mean) # satir ortalamaları
[1] 2 5 8
```


```r
> #2 sutunlar icin
> apply(X, 2, mean) # sutun ortalamaları
[1] 4 5 6
```


- Ortalaması 50, standart sapması 5 olan normal dağılıma sahip 100 elemanlı
"S1" vektöründen 20 satırlı ve 5 sütunlu matrisin oluşturulması


```r
> set.seed(12)
> S1 <- sample(rnorm(10000, 50, 5), 100, replace=TRUE)
> Matris1 <- matrix(S1, nrow=20, ncol=5)
```

- `mean()` fonksiyonunun "Matris1" nesnesinin her bir sütununa uygulanarak  sütunların ortalamasının alınması


```r
> apply(Matris1, 2, mean) # Fonksiyonun ikinci girdisi olan 2  sütun elamanlarını temsil etmektedir.
[1] 48.20485 52.13701 49.38658 50.61689 48.60479
```


- `summary()` fonksiyonunun "Matris1" nesnesinin her bir sütununa uygulanması


```r
> apply(Matris1, 2, summary)
            [,1]     [,2]     [,3]     [,4]     [,5]
Min.    39.00080 40.23309 39.04749 39.32974 37.74364
1st Qu. 45.21933 48.44165 45.57123 47.36401 43.71252
Median  49.31295 52.24410 49.49029 51.08794 47.62144
Mean    48.20485 52.13701 49.38658 50.61689 48.60479
3rd Qu. 52.40540 55.97719 52.70180 54.36235 53.32016
Max.    55.24910 63.33272 58.88203 59.93019 60.51715
```

- `summary()` fonksiyonunun "Matris1" nesnesinin her bir satırına uygulanması


```r
> apply(Matris1, 1, summary)
            [,1]     [,2]     [,3]     [,4]     [,5]     [,6]     [,7]     [,8]
Min.    45.82396 39.16789 51.63544 40.23309 39.04749 44.81304 39.73637 51.11418
1st Qu. 47.78055 39.32974 52.46878 43.82775 47.16408 47.46234 46.19462 51.96290
Median  48.36804 46.24689 53.43269 47.65095 49.56534 49.64774 49.12984 52.65739
Mean    50.47126 45.82933 54.50679 47.52181 48.65629 52.22224 50.10067 54.92558
3rd Qu. 54.95931 51.70256 56.11501 49.31343 52.65050 59.25790 55.94640 55.56069
Max.    55.42443 52.69959 58.88203 56.58380 54.85404 59.93019 59.49613 63.33272
            [,9]    [,10]    [,11]    [,12]    [,13]    [,14]    [,15]    [,16]
Min.    44.96852 39.00080 43.36682 48.42947 42.13211 42.73818 40.55680 41.37856
1st Qu. 48.34900 48.83882 52.38428 50.17014 48.46619 46.50319 43.21988 42.18138
Median  52.21976 53.65437 52.38428 51.40809 48.88713 50.98943 45.46715 47.83169
Mean    50.61489 50.35382 51.18599 53.07152 50.44334 49.60777 46.24742 47.67032
3rd Qu. 53.31388 54.20555 52.91266 54.83276 55.24910 51.36429 46.82348 50.54044
Max.    54.22331 56.06955 54.88190 60.51715 57.48218 56.44375 55.16980 56.41952
           [,17]    [,18]    [,19]    [,20]
Min.    40.53528 40.55680 37.74364 46.71473
1st Qu. 46.04637 44.03153 47.73063 49.31247
Median  47.98124 44.46635 49.30321 51.96828
Mean    48.55872 45.45876 47.52113 50.83282
3rd Qu. 49.85073 46.40143 50.16318 52.82962
Max.    58.37998 51.83770 52.66500 53.33901
```


###kisisel tanımlı fonksiyon ile kullanılması

-  Yazılan **bagil_degiskenlik()** fonksiyonunun "Matris1" nesnesinin 
her bir sütununa uygulanarak her bir değişkenin bağıl değişkenlik 
katsayısının hesaplanması


```r
> 
> bagil_degiskenlik <- function(x){
+ (sd(x)/mean(x))*100
+ }
> apply(Matris1, 2, bagil_degiskenlik)
[1] 11.24914 10.05771 11.02709 10.59998 12.97312
```


Adsız (anonymous) fonksiyonlar  ile kullanılması


```r
> 
> apply(Matris1, 2, function(x){(sd(x)/mean(x))*100})
> 
[1] 11.24914 10.05771 11.02709 10.59998 12.97312
```


## lapply() Fonksiyonu


- **lapply()** fonksiyonu **apply()** fonksiyonunun listeler (list-apply), 
vektörler ve veri setleri için özelleşmiş halidir. 

- Liste yapısında olduğu için satır veya sütuna ilişkin bir argüman kullanılmaz. 

- **lappy()** fonksiyonu veri setlerinde kullanıldığında, sütundaki her bir değişkeni listenin elemanı olarak alır.

---

## lapply() Fonksiyonu


`lappy()` fonksiyonuyla elde edilen çıktılar da liste şeklindedir. 
`lapply()` fonksiyonunun genel kullanımı 


```r
> # lapply(x, FUN, …)
```



```r
> # liste olusturma
> (mylist <- list(a=(1:10), b=(45:77)))
> 
$a
 [1]  1  2  3  4  5  6  7  8  9 10

$b
 [1] 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 69
[26] 70 71 72 73 74 75 76 77
```



```r
> # liste elemalarını toplama
> (resultlapply <- lapply(mylist, sum))
> 
> liste <- list(a=c(1,2,3),b=c(4,5,6))
> lapply(liste, function(x){x^(3/2)})
$a
[1] 55

$b
[1] 2013

$a
[1] 1.000000 2.828427 5.196152

$b
[1]  8.00000 11.18034 14.69694
```

---
## sapply() Fonksiyonu

- **sapply()** fonksiyonu **lapply()** fonksiyonu gibi liste, 
listelerin özelleşmiş hali olan veri setleri ve vektörler
üzerinde çalışır. 

- **sapply()** fonksiyonunun temel amacı çıktıları basitleştirmektir.

- **lapply()** fonksiyonuyla elde edilen çıktılar liste biçimindeyken **sapply()** fonksiyonuyla elde edilen çıktılar daha çok vektör şeklindedir.


- **sapply()** fonksiyonu **apply()** fonksiyonuyla benzer çıktılar verir ancak **sapply()** fonksiyonunda margin değerleri bulunmaz. 
- `sapply()` fonksiyonunun genel kullanımı


```r
> 
> mylist <- list(a=(1:10), b=(45:77))
> 
> resultsapply <- sapply(mylist, sum)
> resultsapply
   a    b 
  55 2013 
```



```r
> 
> # lapply() fonksiyonunun liste veri türüne uygulanması
> lapply(list(a=c(1,2,3)), mean) # Liste çıktısı vermektedir.
> 
$a
[1] 2
```



```r
> 
> # lapply() fonksiyonunun çıktısının vektöre dönüştürülmesi
> unlist(lapply(list(a=c(1,2,3)), mean))
> 
a 
2 
```


```r
> 
> # sapply() fonksiyonunun liste veri türüne uygulanması
> sapply(list(a=c(1,2,3)), mean) # Çıktı adlandırılmış vektör şeklindedir.
> 
a 
2 
```

## tapply() Fonksiyonu


- **tapply()**  fonksiyonun temel görevi verileri belirlenen grup veya faktör değişkenine göre özetlemektir. 

- Fonksiyonda bulunan x argümanı vektör, veri seti ve liste şeklindeki nesneleri, index argümanı "x" nesnesinin altboyut, grup veya faktör değişkenini, FUN argümanı ise uygulanacak fonksiyonu belirtir. 

- $tapply(x, Index, FUN, …)$


- **tapply()** liste ve veri seti yapısındaki nesnelere uygulandığında, grup veya faktör değişkenine ilişkin fonksiyon değerlerini fonksiyon türüne gore vektör ya da liste şeklinde verir. 

- Eğer **tapply()** içinde kullanılan fonksiyon tek bir değer veriyorsa, çıktı vektör; birden fazla değer veriyorsa, çıktı liste yapısındadır.




```r
> 
> isim <- c("Ali","Elif","Su","Deniz","Aras","Berk","Can","Ece","Efe","Arda")
> boy <- c(160,165,170,155,167,162,169,158,160,164)
> kilo <- c(55,55,57,50,48,65,58,62,45,47)
> cinsiyet <- c("erkek","kadin","kadin","kadin","erkek",
+ "erkek","erkek","kadin","erkek","erkek")
> cinsiyet <- factor(cinsiyet)
> beden <- c("S","M","S","M","S","L","M","L","S","S")
> beden <- factor(beden)
> # tapply() fonksiyonunun liste veri yapısına uygulanması
> Liste <- list(isim=isim,boy=boy,cinsiyet=cinsiyet,beden=beden,kilo=kilo)
> 
> tapply(Liste$boy, Liste$cinsiyet, sort)
$erkek
[1] 160 160 162 164 167 169

$kadin
[1] 155 158 165 170
```



```r
> 
> tapply(Liste$boy, Liste$cinsiyet, sort, decreasing=TRUE)
$erkek
[1] 169 167 164 162 160 160

$kadin
[1] 170 165 158 155
```



```r
> 
> isim <- c("Ali","Elif","Su","Deniz","Aras","Berk","Can","Ece","Efe","Arda")
> boy <- c(160,165,170,155,167,162,169,158,160,164)
> kilo <- c(55,55,57,50,48,65,58,62,45,47)
> cinsiyet <- c("erkek","kadin","kadin","kadin","erkek",
+ "erkek","erkek","kadin","erkek","erkek")
> cinsiyet <- factor(cinsiyet)
> beden <- c("S","M","S","M","S","L","M","L","S","S")
> beden <- factor(beden)
> #Once veri seti olusturalım
> df <- data.frame(isim,boy,kilo,cinsiyet, beden)
```



```r
> 
> tapply(df$boy, Liste$cinsiyet, sort)
> 
> tapply(df$boy, Liste$cinsiyet, sort, decreasing=TRUE)
$erkek
[1] 160 160 162 164 167 169

$kadin
[1] 155 158 165 170

$erkek
[1] 169 167 164 162 160 160

$kadin
[1] 170 165 158 155
```



```r
> 
> tapply(df$boy, Liste$cinsiyet, mean)
> 
> tapply(df$boy, Liste$cinsiyet, mean)
   erkek    kadin 
163.6667 162.0000 
   erkek    kadin 
163.6667 162.0000 
```



## by() Fonksiyonu


```r
> 
> 
> by(df$boy, Liste$cinsiyet, sort)
> 
> by(df$boy, Liste$cinsiyet, sort, decreasing=TRUE)
> 
> by(df$boy, Liste$cinsiyet, mean)
> 
> by(df$boy, Liste$cinsiyet, mean)
Liste$cinsiyet: erkek
[1] 160 160 162 164 167 169
------------------------------------------------------------ 
Liste$cinsiyet: kadin
[1] 155 158 165 170
Liste$cinsiyet: erkek
[1] 169 167 164 162 160 160
------------------------------------------------------------ 
Liste$cinsiyet: kadin
[1] 170 165 158 155
Liste$cinsiyet: erkek
[1] 163.6667
------------------------------------------------------------ 
Liste$cinsiyet: kadin
[1] 162
Liste$cinsiyet: erkek
[1] 163.6667
------------------------------------------------------------ 
Liste$cinsiyet: kadin
[1] 162
```


```r
> 
> by(df$boy, Liste$cinsiyet, mean)
> 
> by(df$boy, Liste$cinsiyet, mean)
Liste$cinsiyet: erkek
[1] 163.6667
------------------------------------------------------------ 
Liste$cinsiyet: kadin
[1] 162
Liste$cinsiyet: erkek
[1] 163.6667
------------------------------------------------------------ 
Liste$cinsiyet: kadin
[1] 162
```

