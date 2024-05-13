

# Kontrol Yapıları

R'deki kontrol yapıları, bir dizi R ifadesinin yürütme akışını kontrol etmenize olanak tanır. Temel olarak kontrol yapıları, her zaman aynı R kodunu çalıştırmak yerine kod satırlarında mantığımızı kullanmamızı sağlar. 

Kontrol yapıları, girdilere veya verilerin özelliklerine yanıt vermenize ve buna göre farklı R ifadeleri yürütmenize olanak tanır.

Yaygın olarak kullanılan kontrol yapıları:

-   `if` ve `else`: bir koşulu test etmek ve ona göre hareket etmek

-   `for`: bir döngüyü sabit sayıda çalıştırma

-   `while`: bir koşul doğru iken bir döngü yürütmek

-   `repeat`: sonsuz bir döngü yürütmek (durdurmak için `break` gerekir)

-   `break`: bir döngünün yürütülmesini keser

-   `next`: bir döngü arasını atlama

Çoğu kontrol yapısı etkileşimli oturumlarda değil, daha ziyade fonksiyonlar veya daha uzun ifadeler yazarken kullanılır. Ancak, bu yapılar fonksiyonlarda kullanılmak zorunda değildir ve progralama öğrenmek için bu yapılara aşina olmak gereklidir.

-   Döngüler diğer bütün programa dillerinde sıklıkla kullanılan **akış kontrolü (flow control)** mekanizmasının bir parçasıdır.

-   Her ne kadar R vektörel elementler üzerine kurulmuş olsa da bazı durumlarda döngülerin kullanılması gerekebilir.

-   Örneğin, simulasyon çalışmaları genellikle iterasyonel ve tekrar eden süreçleri içermektedir.

-   Döngüler sonuç elde etmek yerine süreçteki işlemleri dikkate aldığından, simulasyon çalışmalarında kullanılır.


-   **for()** döngüsü ile belirlenen sayıda işlem tekrarı yapılırken **while()** ya da **repeat()** döngülerinde bir sayaç ya da bir dizin ile kontrol sağlanarak işlemlerin tekrarlı yapılmasını sağlar.

-   **for()** bir vektör, liste ya da matris içindeki her bir elemanın bir değişken yardımıyla belirlenen komutu veya kodu sırasıyla yapması için oluşturulan bir döngüdür.

## `if`-`else`

`if`-`else` kombinasyonu muhtemelen R'de (veya belki de herhangi bir dilde) en sık kullanılan kontrol yapısıdır. Bu yapı, bir koşulu test etmenize ve doğru ya da yanlış olmasına bağlı olarak ona göre hareket etmenize olanak tanır.

Öncelilke `if` koşullu ifadesinin kullanımını gösterelim:

```r
if(<koşul>) {
        ## kodlar
} 
```

Yukarıdaki kod, koşul yanlışsa hiçbir şey yapmaz. Koşul yanlış olduğunda yürütmek istediğiniz bir eyleminiz varsa, o zaman bir `else` cümlesine ihtiyacınız vardır.

```r
if(<koşul>) {
        ## kodlar
} 
else {
        ## kodlar
}
```

`if`i herhangi bir `if` ile takip ederek bir dizi test yapabilirsiniz. `else if` kullanabilirsiniz.

```r
if(<kosul1>) {
        ## kodlar
} else if(<kosul2>)  {
        ## kodlar
} else {
        ## kodlar
}
```

İşte geçerli bir if/else yapısına bir örnek.


```r
##  bir rastgele sayı oluşturun
x <- runif(1, 0, 10)
if(x > 3) {
        y <- 10
} else {
        y <- 0
}
x;y
[1] 7.617612
[1] 10
```

`y` değeri `x > 3` olup olmamasına bağlı olarak ayarlanır. Bu ifade eşdeğer bir şekilde de yazılabilir.


```r
y <- if(x > 3) {
        10
} else { 
        0
}
```

Bu ifadeyi yazmanın hiçbir yolu diğerinden daha doğru değildir. Hangisini kullanacağınız sizin  tercihlerinize bağlıdır

Elbette `else` cümlesi gerekli değildir. Kendi koşulları doğruysa her zaman çalıştırılan bir dizi if cümlesine sahip olabilirsiniz.

```r
if(<kosul1>) {

}

if(<kosul2>) {

}
```
### Örnekler


Ölçme açısından bakılacak olursa koşul bir ölçütü, durum
cümlesi ise değerlendirmeyi gösterilebilir.
Örneğin, yapılan bir sınavda geçme notu 60 olarak belirlendiğinde, 75 alan bir öğrencinin durumu aşağıdaki `if()` durum cümlesiyle belirlenebilmektedir.


```r
x <-75
if(x>=65){
print("Basarılı")
}
[1] "Basarılı"
```

Ancak kontrol durumu çoğunlukla tek önermeye bağlı değildir. 
 - Aşağıdaki kod çıktı vermeyecektir

```r
x <-60
if(x>=65){
print("Basarılı")
}
```

- else kullanımı ile çıktı alabiliriz

```r
x <-60
# Başarılı Durum
if(x>=65){
print("Basarılı")
}else{
print("Basarisiz")
}
[1] "Basarisiz"
```

Koşul her zaman iki kategori ile tanımlanamayabilir. Bu durumda kullanımı
`else if()` ile destekleyebiliriz


```r
x <- 75 # Başarılı Durum
if(x>=90){
print("AA")
}else if(x>=80){
print("BA")
}else if(x>=70){
print("BB")
}else if(x>=65){
print("CB")
}else if(x>=60){
print("CC")
}else if(x>=50){
print("DD")
}else if(x>=30){
print("FD")
}else{
print("FF")
}
[1] "BB"
```
### Sıra sizde

1. a sayısının çarpmaya göre tersi 1/a'dir. Ancak bu durum 0 için tanımsızdır.
`if()` durum cümlesi kullanarak bu durumu kodlayınız. x <- 5ve x<-0 için 
için test ediniz.


```
[1] "5'in carpmaya gore tersi 1/5"
```

`x <- 0`için test ediniz.


```
[1] "1/0 tanımsızdır."
```

2. -2 ile 2 arasinda sayilar üretip, bunu x değişkenine atayalım.


```r
x <- rnorm(1)
x
[1] 0.653815
```

Random olarak üretilen sayının 1'den büyük olması durumunda çıktı "1'den büyük" -1 ile 1 arasında olması durumunda "-1 ile +1 arasında" -1'den küçük olması durumunda ise "-1'den küçük" çıktısı versin.


```
[1] 0.04369764
[1] "sayı -1 ile +1 arasında"
```

## if() & all()

Her ne kadar `if()` önermesi bir elemanlı vektörlerde çıktı verirken `if()` önermesi içinde kullanılabilen `all` fonkisyonu ile vektörün tüm elemanları için kosul test edilebilir.


```r
x <- c(1,2,-3,4)
if(all(x>0)){
  
  print("tum sayilar 0'dan buyuktur")
  
} else{
  
  print("tum sayilar 0'dan buyuk degildir")
}
[1] "tum sayilar 0'dan buyuk degildir"
```

## if() & any()

Bir vektörde icinde yer alan her hangi bir elemana dair test ise `if()` fonksiyonu içinde `any()` fonksiyonu ile sağlanabilir.


```r
x <- c(1,2,-3,4)
if(any(x<0)){
  
  print("nesne en az bir negatif sayi icerir")
  
} else{
  
  print("nesne negatif sayi icermez")
}
[1] "nesne en az bir negatif sayi icerir"
```

## if() çoklu islem


```r
x <- 2
if(x == 2) {
  
  goster3 <- "Dogru"  
  goster3b <- c(1,2,3)
  goster3c <- sample(1:1000,4)
} else  {
  
  goster3 <- "Yanlis"  
  goster3b <- c(3,2,1)
  goster3c <- 10000 + sample(1:1000,4)
  
}

goster3
goster3b
goster3c
[1] "Dogru"
[1] 1 2 3
[1] 384 780 977 738
```


## ifelse()

`ifelse()` durum cümlesi, `if()` durum cümlelerinde vektörlerin kullanımından kaynaklı sıkıntılara çözüm sunar. Bu bakımdan `ifelse()`, `if()` durum cümlelerinin vektörler için kullanılabilir halidir.

`ifelse()` durum cümlesinin genel kullanımı aşağıdaki gibidir.

**`ifelse(koşul, Doğru İfade, Yanlış İfade)`**


```r
x <- 20
ifelse(x>= 65, "Başarılı" ,"Başarısız")
[1] "Başarısız"
```


`ifelse()` eksik veri atamakiçin de kullanılabilir. Eksik verinin 99 ile gösterildiği bir vektörde eksik veri yerine NA atama örneği


```r
(x <- c(1,2,3,4,99,5))
ifelse(x==99, NA, x)
[1]  1  2  3  4 99  5
[1]  1  2  3  4 NA  5
```
### Sıra Sizde

1. Elimizdeki bir nesnede yer alan sayıların tek ya da çift olduğunu yazdırma



```r
set.seed(41)
sayilar <- sample(50:90,27)
sayilar
 [1] 89 84 54 81 57 78 55 71 80 62 87 67 70 83 82 61 66 53 50 69 79 64 85 51 73
[26] 74 88
```



```
 [1] "Tek Sayi"  "Cift Sayi" "Cift Sayi" "Tek Sayi"  "Tek Sayi"  "Cift Sayi"
 [7] "Tek Sayi"  "Tek Sayi"  "Cift Sayi" "Cift Sayi" "Tek Sayi"  "Tek Sayi" 
[13] "Cift Sayi" "Tek Sayi"  "Cift Sayi" "Tek Sayi"  "Cift Sayi" "Tek Sayi" 
[19] "Cift Sayi" "Tek Sayi"  "Tek Sayi"  "Cift Sayi" "Tek Sayi"  "Tek Sayi" 
[25] "Tek Sayi"  "Cift Sayi" "Cift Sayi"
```


2. Elimizdeki bir nesnede yer alan sayıların 0, pozitif ya negatif oldugu belirleme



```r
set.seed(987)
sayilar <- sample(-10:10,27,replace=TRUE)
sayilar
 [1]   4   3   4   2   1   7 -10   5   6  -8   7  -3   9   7  -9  10   4  -1  -8
[20]   8  -3   0   4   5   8   1   3
```


```
 [1] "Pozitif" "Pozitif" "Pozitif" "Pozitif" "Pozitif" "Pozitif" "Negatif"
 [8] "Pozitif" "Pozitif" "Negatif" "Pozitif" "Negatif" "Pozitif" "Pozitif"
[15] "Negatif" "Pozitif" "Pozitif" "Negatif" "Negatif" "Pozitif" "Negatif"
[22] "Sıfır"   "Pozitif" "Pozitif" "Pozitif" "Pozitif" "Pozitif"
```


3. Finalden 50 ve üzeri alan ve en az 11 derse devam edem öğrencilerin geçme notları finalin %60 ve vizenin %40 alınarak hesaplansın, 11'den az derse devam eden öğrencilerin geçme notu final notunun %60' olarak alınsın. 


```r
vize <- c(60,70,80,90,55)
final <- c(45,65,70,50,80)
devam <- c(14,10,13,12,11)
```


# `for`

Zaman zaman diğer döngü türlerine ihtiyaç duysanız da, for döngüsünün yeterli olmadığı nadir durum vardır.
R'de for döngüleri bir ara değişken alır ve ona bir dizi ya da vektörden ardışık değerler atar. For döngüleri en yaygın olarak bir nesnenin (liste, vektör, vb.) elemanları üzerinde yineleme yapmak için kullanılır.


```r
for(i in 1:10) {
        print(i)
}
[1] 1
[1] 2
[1] 3
[1] 4
[1] 5
[1] 6
[1] 7
[1] 8
[1] 9
[1] 10
```

Bu döngü `i` değişkenini alır ve döngünün her iterasyonunda ona 1, 2, 3, ..., 10 değerlerini verir, küme parantezleri içindeki kodu çalıştırır ve ardından döngüden çıkar.

Aşağıdaki üç döngünün hepsi aynı davranışa sahiptir.


```r
x <- c("a", "b", "c", "d")

for(i in 1:4) {
        ## 'x'in her bir öğesini yazdırır
        print(x[i])  
}
[1] "a"
[1] "b"
[1] "c"
[1] "d"
```

`seq_along()` fonksiyonu genellikle bir nesnenin (bu durumda `x` nesnesi) uzunluğuna bağlı olarak bir tamsayı dizisi oluşturmak için for döngüleriyle birlikte kullanılır.


```r
## 'x' uzunluğuna göre bir dizi oluşturun
for(i in seq_along(x)) {   
        print(x[i])
}
[1] "a"
[1] "b"
[1] "c"
[1] "d"
```

İndeks değişken kullanmak gerekli değildir.


```r
for(letter in x) {
        print(letter)
}
[1] "a"
[1] "b"
[1] "c"
[1] "d"
```

Bir satırlık döngüler için, küme parantezleri kesinlikle gerekli değildir.


```r
for(i in 1:4) print(x[i])
[1] "a"
[1] "b"
[1] "c"
[1] "d"
```

Bununla birlikte, tek satırlık döngüler için bile küme parantezleri kullanmayı seviyorum, çünkü bu şekilde döngüyü birden fazla satıra genişletmeye karar verirseniz, küme parantezleri eklemeyi unuttuğunuz için hata alamazsınız.




```r
for(i in 1:10) {
 print(i)
}
[1] 1
[1] 2
[1] 3
[1] 4
[1] 5
[1] 6
[1] 7
[1] 8
[1] 9
[1] 10
```




-   Döngüde indeks değişkeni herhangi bir nesne ile tanımlanabilir. Örneğin **i**,  ayrıca indeks değerinin başlangıcı 1 olmak zorunda **değildir.**



```r
for(i in 5:8) {
 print(i)
}
[1] 5
[1] 6
[1] 7
[1] 8
```




-   karakter yazımında indeks **i** sadece tekrar amaçlı kullanılır.



```r
for(i in 5:10){
  print("Merhaba")
}
[1] "Merhaba"
[1] "Merhaba"
[1] "Merhaba"
[1] "Merhaba"
[1] "Merhaba"
[1] "Merhaba"
```



-   Aşağıdaki çıktıyı sağlayacak kodu yazınız.



```
1  +   1  =  2 
2  +   2  =  4 
3  +   3  =  6 
4  +   4  =  8 
5  +   5  =  10 
6  +   6  =  12 
7  +   7  =  14 
8  +   8  =  16 
9  +   9  =  18 
10  +   10  =  20 
```



-   Döngüdelerde bir degişken yeniden tanımlanacak ise mutlaka döngü öncesi o değişken tanımlanmalıdır.

-   Oluşturulan bir matrisin satırlarında yer alan sayıların toplamını başka bir nesneye atama



```r
(X <- cbind(a = 1:5, b=2:6))
Y <- array()
for(i in 1:nrow(X)) {
Y[i] <- X[i,1] + X[i,2]
}
Y
     a b
[1,] 1 2
[2,] 2 3
[3,] 3 4
[4,] 4 5
[5,] 5 6
[1]  3  5  7  9 11
```



-   `cat()`, `paste()` gibi fonksiyonları uzun bir döngüde, döngünün durumunu görmek için de kullanabilirsiniz .



```r
islem.kontrol <- array()
for(i in 1:10){
  islem.kontrol[i] <- paste("Islem ", i, " tamamlandi", sep="")
}
islem.kontrol
 [1] "Islem 1 tamamlandi"  "Islem 2 tamamlandi"  "Islem 3 tamamlandi" 
 [4] "Islem 4 tamamlandi"  "Islem 5 tamamlandi"  "Islem 6 tamamlandi" 
 [7] "Islem 7 tamamlandi"  "Islem 8 tamamlandi"  "Islem 9 tamamlandi" 
[10] "Islem 10 tamamlandi"
```



-   Döngülerde her zaman `i` indeksini kullanmak zorunda **değiliz.**



```r
set.seed(10)
x <- sample(1:10000,100)

sayac <- 0
for (val in x) {
  if(val %% 2 == 0){
sayac = sayac+1
  }
}
print(sayac)
[1] 46
```





## **for()** Döngüsü ve Kontrol

Her zaman işlemi tüm elemanlara uygulamak istemeyebiliriz. Bunun önlemek icin akış kontrolü yapmak gerekir.

Kontrol mantıksal operatörlerle ya da koşul cümleleri ile sağlanabilir.


```r
for(i in 1:3){
  if (i==2) cat("indeks cift sayidir:","\n")
  else cat(i,"\n")
}
1 
indeks cift sayidir: 
3 
```



```r
for(i in 1:3){
  if (i==2) {
cat("indeks degeri ikidir:",i,"\n") 
  }else{cat("indeks degeri iki degildir","\n")}
}
indeks degeri iki degildir 
indeks degeri ikidir: 2 
indeks degeri iki degildir 
```

-   Döngünün indeksi her zaman bir tam sayı olmak zorunda değildir.  Liste, veri seti, matris de olabilir.

-   **if** sadece numerik değer ve vektörlerle çalışmaz. Aynı zamanda veri seti, matris ve listelerle de çalışabilir.



```r
d <- data.frame(a = 1:5, b=2:6)
d
for(x in d) {
  cat("sutun toplamlari:", sum(x), "\n")
}
```

<div class="kable-table">

|  a|  b|
|--:|--:|
|  1|  2|
|  2|  3|
|  3|  4|
|  4|  5|
|  5|  6|

</div>

```
sutun toplamlari: 15 
sutun toplamlari: 20 
```







```r
X <- cbind(1:5, 21:25)
X
     [,1] [,2]
[1,]    1   21
[2,]    2   22
[3,]    3   23
[4,]    4   24
[5,]    5   25
```



Aşağıdaki çıktıyı elde etmek için gerekli kodu yazınız.



```
1 satirdaki degerlerin carpimi 21 olarak hesaplanmistir. 
2 satirdaki degerlerin carpimi 44 olarak hesaplanmistir. 
3 satirdaki degerlerin carpimi 69 olarak hesaplanmistir. 
4 satirdaki degerlerin carpimi 96 olarak hesaplanmistir. 
5 satirdaki degerlerin carpimi 125 olarak hesaplanmistir. 
```



## `next()` ve `break()`

-   `next()` ve `break()` fonksiyonları döngülerde kontrol mekanizmasıdır. Döngüyü sadece belirli bir koşulda çalıştırmak istemezseniz `next()` fonksiyonunu kullanabilirsiniz.



```r
for(i in 1:6){
  if(i==3){
next
  }
  print (i)}
[1] 1
[1] 2
[1] 4
[1] 5
[1] 6
```




-   Döngüyü sadece belirli bir koşulda durdurmak isterseniz `break()` fonksiyonunu kullanabilirsiniz.




```r
for(i in 1:12){
  if(i==3){
break
  }
  print (i)}
[1] 1
[1] 2
```


-   Döngüler uzun zamanda çalışır. ilk olarak başlangıç noktasını belirleyelim



```r
set.seed(853)
y<-matrix(rnorm(1000000),nrow=1000)
z<-0*y
time1<-as.numeric(Sys.time())
```

-  işlemi döngü ile yapalım.

```r
#loop:
time2 <- system.time(
  for(i in 1:1000){
  for(j in 1:1000){
z[i,j]<-y[i,j]^2
  }
})

time2
   user  system elapsed 
   0.08    0.02    0.09 
```

- ayni islemi dongusuz yapma

```r

time3 <- system.time(z<-y^2)
time3
   user  system elapsed 
      0       0       0 
```


## İçiçe `for` döngüleri

`for` döngüler birbirinin içinde yuvalanabilir.

``` r
x <- matrix(1:6, 2, 3)

for(i in seq_len(nrow(x))) {
        for(j in seq_len(ncol(x))) {
                print(x[i, j])
        }   
}
```

İç içe döngüler genellikle çok boyutlu veya hiyerarşik veri yapıları (örn. matrisler, listeler) için gereklidir. Yine de iç içe geçme konusunda dikkatli olun. 2-3 seviyeden fazla iç içe geçme genellikle kodun okunmasını/anlaşılmasını zorlaştırır. Çok sayıda iç içe döngüye ihtiyaç duyuyorsanız, fonksiyonları kullanarak döngüleri parçalamak isteyebilirsiniz (daha sonra tartışılacaktır).

- sıra sizde

-   Bazen döngüler iç içe kullanılabilir 5X5 bir matrisin her bir elemanı satır ve sütun indeksleri çarpımı olsun orneğin m[2,5]=10 olsun. Bu işlemi yapmak için öncelikle boş bir matris oluştumak lazım.



```r
m2 <- matrix(0,nrow=5,ncol=5)
m2
     [,1] [,2] [,3] [,4] [,5]
[1,]    0    0    0    0    0
[2,]    0    0    0    0    0
[3,]    0    0    0    0    0
[4,]    0    0    0    0    0
[5,]    0    0    0    0    0
```


-   Aşağıdaki çıktıyı elde edecek kodu oluşturmaya çalışın



```
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    2    3    4    5
[2,]    2    4    6    8   10
[3,]    3    6    9   12   15
[4,]    4    8   12   16   20
[5,]    5   10   15   20   25
```


## Ödev-1

-   Kullanıcı tarafından belirlenen **nxn** boyutunda bir matris oluşturulsun. **nxn** bir matrisin her bir elemanı satır ve sütun indeksleri çarpımı olsun. örneğin m[2,5]=10 olsun.

-   Eger matrisin boyutları 10x10'dan büyükse sadece 10 satırını yazsın eğer matrisi boyutları 10x10'dan küçükse hepsini yazsın.

-   Kullancı üç girdiğinde oluşacak çıktı:


```
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    2    4    6
[3,]    3    6    9
```



# `while` döngüsü

While döngüleri bir koşulu test ederek başlar. Koşul doğruysa, döngü gövdesini çalıştırır. Döngü gövdesi yürütüldükten sonra, koşul tekrar test edilir ve koşul yanlış olana kadar bu şekilde devam eder, ardından döngüden çıkılır.


```r
count <- 0
while(count < 10) {
        print(count)
        count <- count + 1
}
[1] 0
[1] 1
[1] 2
[1] 3
[1] 4
[1] 5
[1] 6
[1] 7
[1] 8
[1] 9
```

While döngüleri düzgün yazılmazsa sonsuz döngülere neden olabilir. Dikkatli kullanın!

Bazen testte birden fazla koşul olabilir.


```r
z <- 5
set.seed(1)

while(z >= 3 && z <= 10) {
        coin <- rbinom(1, 1, 0.5)
        
        if(coin == 1) {  ## rastgele çalışır
                z <- z + 1
        } else {
                z <- z - 1
        } 
}
print(z)
[1] 2
```

Koşullar her zaman soldan sağa doğru değerlendirilir. Örneğin, yukarıdaki kodda `z` 3'ten küçük olsaydı, ikinci test değerlendirilmezdi.

## `repeat` Döngüler


`repeat` başlangıçtan itibaren sonsuz bir döngü başlatır. Bunlar istatistiksel veya veri analizi uygulamalarında yaygın olarak kullanılmaz, ancak kullanım alanları vardır. Bir `repeat` döngüsünden çıkmanın tek yolu `break` çağrısı yapmaktır.

Olası bir paradigma, bir çözüm arıyor olabileceğiniz ve çözüme ulaşana kadar durmak istemediğiniz yinelemeli bir algoritmada olabilir.


```r
x0 <- 1
tol <- 1e-8

repeat {
        x1 <- computeEstimate()
        
        if(abs(x1 - x0) < tol) {  ## Yeterince yakın mı?
                break
        } else {
                x0 <- x1
        } 
}
```

Yukarıdaki kodun `computeEstimate()` fonksiyonu tanımlanmamışsa çalışmayacağını unutmayın (bunu sadece bu gösterimin amaçları için uydurdum).

Yukarıdaki döngü biraz tehlikelidir çünkü duracağının garantisi yoktur. `x0` ve`x1` değerlerinin ileri geri salındığı ve asla yakınsamadığı bir duruma girebilirsiniz. Bir `for` döngüsü kullanarak iterasyon sayısına sabit bir sınır koymak ve ardından yakınsamanın sağlanıp sağlanmadığını rapor etmek daha iyidir.



## Özet

-   `if`,`while`ve`for` gibi kontrol yapıları bir R programının akışını kontrol etmenizi sağlar

-   Sonsuz döngülerden, teorik olarak doğru olduklarına inansanız bile, genellikle kaçınılmalıdır.

-   Burada bahsedilen kontrol yapıları öncelikle program yazmak için kullanışlıdır; komut satırı etkileşimli çalışmalar için "apply" fonksiyonları daha kullanışlıdır.




##  Ödev-1

Fibonacci dizisinin elemanlari **1 1 2 3 5 8 13 21 34 55 89 ...** dizinin elemanlarını `for()` ve/ve ya `while()` döngüsü ile oluşturmaya çalışınız.






## Ödev-2


```r
set.seed(1786)
ornek<-exp(matrix(rnorm(2000),nrow=100))
index1.temp<-sample(1:100,10)
index2.temp<-sample(1:20,10)
for(i in 1:10){
  ornek[index1.temp[i],index2.temp[i]]<--1
}
 head(ornek,6)
          [,1]      [,2]      [,3]      [,4]      [,5]      [,6]     [,7]
[1,] 0.5549525 0.3247338 0.5236032 0.3821027 0.4187483 0.1588847 5.226161
[2,] 0.5671734 1.2431592 0.8812069 2.6695443 0.6984453 1.0838792 1.079946
[3,] 4.8068457 0.3449856 0.6079096 0.9194116 1.5361330 1.9082522 0.671977
[4,] 1.3509234 2.3569582 0.1931423 4.0707377 0.3527276 2.3498825 1.198514
[5,] 0.9012032 0.2310683 0.2317487 1.3809955 0.9168741 0.6237213 1.609403
[6,] 1.2331483 1.1066056 0.3546027 0.3705946 0.9002303 0.2528151 3.337512
          [,8]      [,9]     [,10]     [,11]     [,12]     [,13]     [,14]
[1,] 2.6280057 1.2251526 0.4760966 5.2379018 1.4782655 1.3761338 1.0202608
[2,] 2.2087385 0.5195551 0.3757409 0.9004808 0.7409205 2.0543842 0.3668661
[3,] 1.5310016 0.6735007 2.2069776 0.5060078 0.7171477 1.2378655 0.3651527
[4,] 2.5592899 1.8205257 1.2624052 0.1524106 0.3828322 1.2406799 0.7954326
[5,] 1.1005990 1.0619758 2.1047783 2.7816902 1.4010878 0.6140937 0.5136842
[6,] 0.9799103 2.7520425 2.5407624 1.3889136 0.4346808 1.0637950 0.1859157
         [,15]     [,16]      [,17]     [,18]    [,19]     [,20]
[1,] 0.1437680 4.1807643  1.7389423 3.0760640 1.550557 4.4838291
[2,] 3.8674407 1.9349214  0.6333922 0.4862532 5.275571 0.1161029
[3,] 1.4724240 0.5971116 11.5869157 0.7580736 4.755297 1.0583051
[4,] 0.1243085 0.8376231  1.3723291 2.0884571 2.506128 1.2094517
[5,] 6.2971803 0.8422164  1.5335222 0.3079718 2.729447 1.7164885
[6,] 3.8052219 2.1611055  0.3280288 2.7773368 1.726558 1.3193446
```



-   **ornek** veri setinde i. satırda negatif sayı yok ise çıktıda **i. satırın ortalaması....dir** yazsin.

-   Eğer veri setinde her hangi bir satırda negatif sayı var ise **satır i negatif sayı bulunmaktadır.**

-   veri setindeki satırlardaki toplam negatif sayı toplamı üçü geçerse çktıda **cok sayıda negatif sayı** yazsın ve döngü çalışmayı durdursun.


```
[1] "Satir 1 ortalamasi 0.986111423178787"
[1] "Satir 2 ortalamasi 1.66440473890558"
[1] "Satir 3 ortalamasi 1.86445460243509"
[1] "Satir 4 negatif sayi icermektedir."
[1] "Satir 5 negatif sayi icermektedir."
[1] "Satir 6 ortalamasi 2.18755744815693"
[1] "Satir 7 ortalamasi 2.42896783600747"
[1] "Satir 8 ortalamasi 1.11152186047931"
[1] "Satir 9 ortalamasi 1.28348082027049"
[1] "Satir 10 ortalamasi 1.49790135754768"
[1] "Satir 11 ortalamasi 1.00823845594998"
[1] "Satir 12 ortalamasi 1.84432161490249"
[1] "Satir 13 ortalamasi 2.30730516248531"
[1] "Satir 14 ortalamasi 1.32997520232501"
[1] "Satir 15 ortalamasi 1.40736423997693"
[1] "Satir 16 ortalamasi 0.930694377568197"
[1] "Satir 17 ortalamasi 1.09683802891735"
[1] "Satir 18 ortalamasi 1.34543057465283"
[1] "Satir 19 ortalamasi 1.91931890408157"
[1] "Satir 20 ortalamasi 1.46149447129439"
[1] "Satir 21 ortalamasi 1.48698773010654"
[1] "Satir 22 ortalamasi 2.50083591324982"
[1] "Satir 23 ortalamasi 2.49403230671112"
[1] "Satir 24 ortalamasi 2.03307899444367"
[1] "Satir 25 ortalamasi 1.47358418101605"
[1] "Satir 26 ortalamasi 1.77152589640626"
[1] "Satir 27 ortalamasi 1.25135003349089"
[1] "Satir 28 ortalamasi 1.33894076274636"
[1] "Satir 29 ortalamasi 1.82874224246664"
[1] "Satir 30 ortalamasi 1.23831471787453"
[1] "Satir 31 ortalamasi 1.82082600141082"
[1] "Satir 32 ortalamasi 1.12466160143214"
[1] "Satir 33 ortalamasi 1.32597664522914"
[1] "Satir 34 negatif sayi icermektedir."
[1] "Satir 35 ortalamasi 2.32162456679167"
[1] "Satir 36 ortalamasi 2.23274928866424"
[1] "Satir 37 negatif sayi icermektedir."
[1] "Satir 38 ortalamasi 2.275511227626"
[1] "Satir 39 ortalamasi 1.7921160361432"
[1] "Satir 40 ortalamasi 0.970509167208986"
[1] "Satir 41 ortalamasi 1.24765799189581"
[1] "Satir 42 ortalamasi 2.51234120817512"
[1] "Satir 43 ortalamasi 2.31828043397862"
[1] "Satir 44 negatif sayi icermektedir."
[1] "Satir 45 ortalamasi 1.95647545685842"
[1] "Satir 46 negatif sayi icermektedir."
[1] "Satir 47 ortalamasi 2.36551615481398"
[1] "Satir 48 ortalamasi 1.97786024664016"
[1] "Satir 49 ortalamasi 1.6393028512105"
[1] "Satir 50 ortalamasi 3.73629039983628"
[1] "Satir 51 ortalamasi 1.82116726064836"
[1] "Satir 52 ortalamasi 1.87732770333814"
[1] "Satir 53 ortalamasi 2.7020031804201"
[1] "Satir 54 ortalamasi 1.05164097984234"
[1] "Satir 55 ortalamasi 1.88981004324099"
[1] "Satir 56 ortalamasi 1.54248819505925"
[1] "Satir 57 ortalamasi 1.65731581957976"
[1] "Satir 58 ortalamasi 1.36890435340706"
[1] "Satir 59 negatif sayi icermektedir."
[1] "Satir 60 ortalamasi 2.22046851034413"
[1] "Satir 61 ortalamasi 1.0408644748318"
[1] "Satir 62 ortalamasi 1.72072095294252"
[1] "Satir 63 ortalamasi 1.53167534425738"
[1] "Satir 64 ortalamasi 1.72856879470484"
[1] "Satir 65 ortalamasi 1.37607074870477"
[1] "Satir 66 ortalamasi 1.42295571491744"
[1] "Satir 67 ortalamasi 0.88385039568476"
[1] "Satir 68 ortalamasi 2.35701379888311"
[1] "Satir 69 ortalamasi 1.35179926755423"
[1] "Satir 70 ortalamasi 1.28012686374286"
[1] "Satir 71 negatif sayi icermektedir."
[1] "Satir 72 ortalamasi 1.67406636870506"
[1] "Satir 73 ortalamasi 1.37691945587952"
[1] "Satir 74 ortalamasi 2.00099153014073"
[1] "Satir 75 negatif sayi icermektedir."
[1] "Satir 76 ortalamasi 1.60454610453076"
[1] "Satir 77 ortalamasi 2.0804975152321"
[1] "Satir 78 ortalamasi 1.67436426400702"
[1] "Satir 79 ortalamasi 2.04712004349156"
[1] "Satir 80 ortalamasi 1.2963699279751"
[1] "Satir 81 ortalamasi 2.06864424004881"
[1] "Satir 82 ortalamasi 2.18401195176334"
[1] "Satir 83 ortalamasi 2.38233635418165"
[1] "Satir 84 ortalamasi 1.65733160944781"
[1] "Satir 85 ortalamasi 1.53913327407787"
[1] "Satir 86 ortalamasi 1.5977866331596"
[1] "Satir 87 ortalamasi 1.53640423869466"
[1] "Satir 88 ortalamasi 1.4151688443321"
[1] "Satir 89 ortalamasi 1.65657353958559"
[1] "Satir 90 ortalamasi 1.09930366562984"
[1] "Satir 91 ortalamasi 2.04289262764082"
[1] "Satir 92 ortalamasi 1.49359077505866"
[1] "Satir 93 ortalamasi 1.59542242961016"
[1] "Satir 94 negatif sayi icermektedir."
[1] "Satir 95 ortalamasi 1.63562964801907"
[1] "Satir 96 ortalamasi 1.25826462716513"
[1] "Satir 97 ortalamasi 3.88578289773781"
[1] "Satir 98 ortalamasi 2.05151453891869"
[1] "Satir 99 ortalamasi 1.96874159472044"
[1] "Satir 100 ortalamasi 1.5918224514213"
```




Matris yazdırma 

```r

n<-as.numeric(readline(prompt = "Kare matriste satir/sutun sayisi olarak kullanilmak uzere bir sayi yaziniz: "))
matris<-matrix(0,n,n)
for(satir in 1:n){
  for(sutun in 1:n){
    matris[satir,sutun]<- satir*sutun
  }
}

if(nrow(matris)<=10){
  matris
}else{
  matris[1:10,1:10]
}

```

