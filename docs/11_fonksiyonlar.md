---
editor_options: 
  markdown: 
    wrap: sentence
---

# Fonksiyonlar



En sık kullandığımız fonksiyonlar

+--------------+-----------------------------------------------------------------------------------------------------------+
| oluşturma    | c(), rep(), seq(), numeric(), character(), factor(), logical(), matrix(), array(), data.frame(), list()   |
+--------------+-----------------------------------------------------------------------------------------------------------+
| Kaydetme     | save(), load()                                                                                            |
+--------------+-----------------------------------------------------------------------------------------------------------+
| okuma/yazma  | read*(),write()*                                                                                          |
+--------------+-----------------------------------------------------------------------------------------------------------+
| dönüştürme   | as.numeric(),as.character(),as.factor(),as.logical(), as.matrix(), as.array(), as.data.frame(), as.list() |
+--------------+-----------------------------------------------------------------------------------------------------------+
| isimlendirme | names(), clonames(), rownames()                                                                           |
+--------------+-----------------------------------------------------------------------------------------------------------+
| indeksleme   | [ i ] (vektör için), [ i, j] (matris ve data frame için),                                                 |
|              |                                                                                                           |
|              | [ i, j, k, …] (dizi için), [ [ k ] ] (liste için)                                                         |
|              |                                                                                                           |
|              | j, k, tam sayı, karakter ya da mantıksal ifade olabilir                                                   |
+--------------+-----------------------------------------------------------------------------------------------------------+
| birleştirme  | c(), paste(), cbind(), rbind(), merge()                                                                   |
+--------------+-----------------------------------------------------------------------------------------------------------+
| sıralama     | order(), arrange()                                                                                        |
+--------------+-----------------------------------------------------------------------------------------------------------+
| tur          | class(), length(), dim(), nrow(), ncol() ………                                                              |
+--------------+-----------------------------------------------------------------------------------------------------------+

Fonksiyon yazmak, bir R programcısının temel faaliyetlerinden biridir.
Sadece bir "kullanıcıdan" R için yeni fonksiyonlar yaratan bir geliştiriciye geçişin temel adımını temsil eder.
Fonksiyonlar genellikle, belki de biraz farklı koşullar altında birçok kez yürütülmesi gereken bir dizi ifadeyi kapsüllemek için kullanılır.
Fonksiyonlar ayrıca genellikle kodun başkalarıyla veya kamuyla paylaşılması gerektiğinde yazılır.

Bir fonksiyonun yazılması, bir geliştiricinin koda bir dizi parametre ile açıkça belirtilen bir arayüz oluşturmasına olanak tanır.
Bu arayüz, potansiyel kullanıcılara kodun bir soyutlamasını sağlar.
Bu soyutlama kullanıcıların hayatını kolaylaştırır çünkü onları kodun nasıl çalıştığına dair her ayrıntıyı bilmek zorunda bırakmaz.
Buna ek olarak, bir arayüzün oluşturulması, geliştiricinin kullanıcıya kodun önemli veya en alakalı yönlerini iletmesine olanak tanır.

## R'da Fonkisyonlar

R'deki fonksiyonlar "birinci sınıf nesnelerdir", yani diğer R nesneleri gibi ele alınabilirler.
Daha da önemlisi,

-   Fonksiyonlar diğer fonksiyonlara argüman olarak aktarılabilir.
    Bu, `lapply()` ve `sapply()` gibi çeşitli döngü fonksiyonları için çok kullanışlıdır.

-   Fonksiyonlar iç içe geçebilir, böylece bir fonksiyonu başka bir fonksiyonun içinde tanımlayabilirsiniz

## İlk fonksiyon

Fonksiyonlar `function()` kullanılarak tanımlanır ve diğer her şey gibi R nesneleri olarak saklanır.
Özellikle, "function" sınıfının R nesneleridirler.

İşte hiçbir argüman almayan ve hiçbir şey yapmayan basit bir fonksiyon.


```r
> f <- function() {
+         ## Bu boş bir fonksiyondur
+ }
> ## Fonksiyonların kendi sınıfları vardır
> class(f)  
> ## Bu işlevi çalıştırın
> f()       
[1] "function"
NULL
```

Çok ilginç değil ama bu da bir başlangıç.
Yapabileceğimiz bir sonraki şey, aslında önemsiz olmayan bir *fonksiyon gövdesine* sahip bir fonksiyon oluşturmaktır.


```r
> f <- function() {
+         cat("Merhaba!\n")
+ }
> f()
Merhaba!
```

Temel bir fonksiyonun son unsuru *fonksiyon argümanlarıdır*.
Bunlar, kullanıcıya belirtebileceğiniz ve kullanıcının açıkça ayarlayabileceği seçeneklerdir.
Bu temel fonksiyon için, konsola kaç kez "Merhaba!" yazdırılacağını belirleyen bir argüman ekleyebiliriz.


```r
> f <- function(num) {
+         for(i in seq_len(num)) {
+                 cat("Merhaba!\n")
+         }
+ }
> f(3)
Merhaba!
Merhaba!
Merhaba!
```

Açıkçası, aynı etki için sadece `cat("Merhaba!\n")` üç kere kesip yapıştırabilirdik.
Ama o zaman programlama yapmıyor olurduk.
Ayrıca, kodunuzu bir başkasına vermeniz ve onu kodu istediği kadar kesip yapıştırmaya zorlamanız da iyi olmayacaktır:) "Merhaba!".

> Genel olarak, kendinizi çok fazla kesme ve yapıştırma yaparken bulursanız, bu genellikle bir fonksiyon yazmanız gerekebileceğine dair iyi bir işarettir.

R'da uzmanlaştıkça ve yapılan işler karmaşıklaştıkça fonksiyon yazma ihtiyacı duyulmaktadır.
Fonksiyon yazma gereksinimi özellikle tekrarlı işlemler yapılması gerektiği durumda ortaya çıkmaktadır.
Fonksiyon yazmak

\- pratiklik kazandırır (ekonomiktir)

\- Paylaşılmasını koylaştırır.

\- Tekrar kullanılabilirlik sağlar.

Tekrarlı işlemlerde hatalardan kurtulmanın yolu fonksiyon kullanmaktır.
Fonksiyonlar, koşullu önermeler ve döngüler ile kullanılarak çok sayıda komut ile yapılabilecek olan işlemler tek bir komut satırı ile yapılabilir hale gelmektedir

Son olarak, yukarıdaki fonksiyon *hiçbir şey döndürmez*.
Sadece konsola `tekrar` sayıda "Merhaba!" yazdırır ve sonra çıkar.
Ancak bir fonksiyonun, belki de kodun başka bir bölümüne beslenebilecek bir şey döndürmesi genellikle yararlıdır.

Sıradaki fonksiyon konsola yazdırılan toplam karakter sayısını döndürür.


```r
> f <- function(tekrar) {
+         Merhaba <- "Merhaba!\n"
+         for(i in seq_len(tekrar)) {
+                 cat(Merhaba)
+         }
+         chars <- nchar(Merhaba) * tekrar
+         chars
+ }
> f(3)
Merhaba!
Merhaba!
Merhaba!
[1] 27
```

Yukarıdaki fonksiyonda, fonksiyonun karakter sayısını döndürmesi için özel bir şey belirtmemiz gerekmedi.
R'de, bir fonksiyonun geri dönüş değeri her zaman değerlendirilen en son ifadedir.
Bu fonksiyonda değerlendirilen son ifade `chars` değişkeni olduğu için, fonksiyonun dönüş değeri de bu olur.

Bir fonksiyondan açık bir değer döndürmek için kullanılabilecek bir `return()` fonksiyonu olduğunu unutmayın, ancak nadiren kullanılır.

Son olarak, yukarıdaki fonksiyonda, kullanıcı `tekrar` argümanının değerini belirtmelidir.
Eğer kullanıcı tarafından belirtilmezse, R bir hata verecektir.


```r
> f()
Error in f(): argument "tekrar" is missing, with no default
```

Bu davranışı `tekrar` argümanı için bir *varsayılan değer* belirleyerek değiştirebiliriz.
Belirtmek isterseniz, herhangi bir fonksiyon argümanının varsayılan bir değeri olabilir.
Bazen, argüman değerleri nadiren değiştirilir (özel durumlar hariç) ve bu argüman için bir varsayılan değer ayarlamak mantıklıdır.
Bu, kullanıcıyı fonksiyon her çağrıldığında bu argümanın değerini belirtme zorunluluğundan kurtarır.

Örneğin, burada `tekrar` için varsayılan değeri 1 olarak ayarlayabiliriz, böylece fonksiyon `tekrar` argümanı açıkça belirtilmeden çağrılırsa, konsola bir kez "Merhaba!" yazdırır.


```r
> f <- function(tekrar = 1) {
+         Merhaba <- "Merhaba!\n"
+         for(i in seq_len(tekrar)) {
+                 cat(Merhaba)
+         }
+         chars <- nchar(Merhaba) * tekrar
+         chars
+ }
> f()    ## 'tekrar' için varsayılan değeri kullan
> f(2)   ## Kullanıcı tarafından belirtilen değeri kullan
Merhaba!
[1] 9
Merhaba!
Merhaba!
[1] 18
```

Fonksiyonun hala konsola yazdırılan karakter sayısını döndürdüğünü unutmayın.

Bu noktada, bir fonksiyon yazdık

-   fonksiyonunun `tekrar` adında ve *varsayılan değeri* 1 olan bir *formal argümanı* vardır.
    *formal argümanlar* fonksiyon tanımına dahil edilen argümanlardır.
    `formals()` fonksiyonu bir fonksiyonun tüm biçimsel argümanlarının bir listesini döndürür

-   "Merhaba!" mesajını `tekrar` argümanıyla belirtilen sayıda konsola yazdırır

-   \*konsola yazdırılan karakter sayısını döndürür

Fonksiyonlar, isteğe bağlı olarak varsayılan değerlere sahip olabilen *isimli argümanlara* sahiptir.
Tüm fonksiyon argümanlarının adları olduğundan, bunlar adları kullanılarak belirtilebilir.


```r
> f(tekrar = 2)
Merhaba!
Merhaba!
[1] 18
```

Bir fonksiyonun çok sayıda argümanı varsa ve hangi argümanın belirtildiği her zaman net olmayabilirse, bir argümanı adıyla belirtmek bazen yararlıdır.
Burada, fonksiyonumuzun yalnızca bir argümanı vardır, bu nedenle herhangi bir karışıklık olmaz.

## Argüman Eşleştirme

Bir R fonksiyonunu argümanlarla çağırmak çeşitli şekillerde yapılabilir.
Bu ilk başta kafa karıştırıcı olabilir, ancak komut satırında etkileşimli çalışma yaparken gerçekten kullanışlıdır.
R fonksiyonları argümanları *konumsal olarak* veya isme göre eşleştirilebilir.
Konumsal eşleştirme, R'nin ilk değeri ilk argümana, ikinci değeri ikinci argümana vb.
atadığı anlamına gelir.
Yani aşağıdaki `rnorm()` çağrısında


```r
> str(rnorm)
> mydata <- rnorm(100, 2, 1)              ## Bazı veriler oluşturun
function (n, mean = 0, sd = 1)  
```

100, `n` argümanına, 2 `ortalama` argümanına ve 1 `sd` argümanına atanır, hepsi de konum eşleştirmesi ile yapılır.

Aşağıdaki `sd()` fonksiyonu (bir sayı vektörünün ampirik standart sapmasını hesaplar) çağrılarının tümü eşdeğerdir.
sd()`fonksiyonunun iki argümanı olduğunu unutmayın: x` sayı vektörünü gösterir ve `na.rm` eksik değerlerin kaldırılıp kaldırılmayacağını belirten bir mantıksaldır.


```r
> ## Konumsal eşleşme ilk argüman, na.rm için varsayılan
> sd(mydata)                     
> ## 'x' argümanını isimle belirtin, varsayılan 'na.rm'
> sd(x = mydata)                 
> ## Her iki argümanı da adla belirtin
> sd(x = mydata, na.rm = FALSE)  
[1] 0.8887614
[1] 0.8887614
[1] 0.8887614
```

Fonksiyon argümanlarını isimle belirtirken, bunları hangi sırada belirttiğiniz önemli değildir.
Aşağıdaki örnekte, fonksiyon tanımında tanımlanan ilk argüman `x` olmasına rağmen, önce `na.rm` argümanını, ardından `x` argümanını belirtiyoruz.


```r
> ## Her iki argümanı da adla belirtin
> sd(na.rm = FALSE, x = mydata)     
[1] 0.8887614
```

Konumsal eşleştirme ile ada göre eşleştirmeyi karıştırabilirsiniz.
Bir argüman isme göre eşleştirildiğinde, argüman listesinden "çıkarılır" ve kalan isimsiz argümanlar fonksiyon tanımında listelendikleri sırayla eşleştirilir.


```r
> sd(na.rm = FALSE, mydata)
[1] 0.8887614
```

Burada, `mydata` nesnesi `x` argümanına atanır, çünkü henüz belirtilmemiş tek argüman budur.

Aşağıda, bir veri kümesine doğrusal modeller uyduran `lm()` fonksiyonunun argüman listesi yer almaktadır.


```r
> args(lm)
function (formula, data, subset, weights, na.action, method = "qr", 
    model = TRUE, x = FALSE, y = FALSE, qr = TRUE, singular.ok = TRUE, 
    contrasts = NULL, offset, ...) 
NULL
```

Aşağıdaki iki kod satırı eşdeğerdir.

``` r
lm(data = mydata, y ~ x, model = FALSE, 1:100)
lm(y ~ x, mydata, 1:100, model = FALSE)
```

Bu işlem güvenli olsa da, bazı karışıklıklara yol açabileceğinden, argümanların sırası ile çok fazla uğraşmanızı önermem.

Çoğu zaman, adlandırılmış argümanlar komut satırında uzun bir argüman listeniz olduğunda ve listenin sonuna yakın bir argüman dışında her şey için varsayılanları kullanmak istediğinizde kullanışlıdır.
Adlandırılmış argümanlar, konumunu değil, argüman adını hatırlayabiliyorsanız da yardımcı olur.
Örneğin, çizim fonksiyonları genellikle özelleştirmeye izin vermek için çok sayıda seçeneğe sahiptir, ancak bu, argüman listesindeki her argümanın konumunu tam olarak hatırlamayı zorlaştırır.

Varsayılan bir değer belirtmemenin yanı sıra, bir argümanın değerini `NULL` olarak da ayarlayabilirsiniz.

``` r
f <- function(a, b = 1, c = 2, d = NULL) {

}
```

Bir R nesnesinin `NULL` olup olmadığını `is.null()` fonksiyonu ile kontrol edebilirsiniz.
Bazen bir argümanın `NULL` değerini almasına izin vermek yararlıdır, bu da fonksiyonun belirli bir işlem yapması gerektiğini gösterebilir.

Kullanışlı bir fonksiyon yazmak için mümkün olduğunca kısa isimler kullanılmalıdır; bununla birlikte bu isimler kullanıcıya yapılacak işlemi anlaşılırkılmalıdır.
Bunun yanında R'da özel anlamı olan `c,C,D,F,I,q,t,T` gibi tek harfl ik fonksiyon isimleri kullanmaktan ve R'da hazır olan fonksiyon isimlerini kişisel tanımlı fonksiyonlara vermekten kaçınılmalıdır.

## `...` argümanı

R'de `...` argümanı olarak bilinen ve genellikle diğer fonksiyonlara aktarılan değişken sayıda argümanı gösteren özel bir argüman vardır.
`...` argümanı genellikle başka bir fonksiyonu genişletirken kullanılır ve orijinal fonksiyonun tüm argüman listesini kopyalamak istemezsiniz

Örneğin, özel bir çizim fonksiyonu varsayılan `plot()` fonksiyonunu tüm argüman listesiyle birlikte kullanmak isteyebilir.
Aşağıdaki fonksiyon `type` argümanı için varsayılanı `type = "l"` değerine değiştirir (orijinal varsayılan `type = "p"` idi).

``` r
myplot <- function(x, y, type = "l", ...) {
        plot(x, y, type = type, ...)         ## '...'yi 'plot' işlevine geçirin
}
```

Jenerik fonksiyonlar, metotlara ekstra argümanlar aktarılabilmesi için `...` kullanır.


```r
> mean
function (x, ...) 
UseMethod("mean")
<bytecode: 0x000001bbe70ca418>
<environment: namespace:base>
```

Fonksiyona aktarılan argüman sayısı önceden bilinemediğinde `...` argümanı gereklidir.
Bu durum `paste()` ve `cat()` gibi fonksiyonlarda açıkça görülmektedir.


```r
> args(paste)
> args(cat)
function (..., sep = " ", collapse = NULL, recycle0 = FALSE) 
NULL
function (..., file = "", sep = " ", fill = FALSE, labels = NULL, 
    append = FALSE) 
NULL
```

Hem `paste()` hem de `cat()` birden fazla karakter vektörünü bir araya getirerek konsola metin yazdırdığından, bu fonksiyonların kullanıcı tarafından fonksiyona kaç karakter vektörü aktarılacağını önceden bilmesi imkansızdır.
Bu yüzden her iki fonksiyonun da ilk argümanı `...` şeklindedir.

## `...` argümanından sonra gelen argümanlar

`...` ile ilgili bir sorun, argüman listesinde _after_`...` olarak görünen herhangi bir argümanın açıkça adlandırılması gerektiği ve kısmen eşleştirilemeyeceği veya konum olarak eşleştirilemeyeceğidir.

`paste()` fonksiyonunun argümanlarına bir göz atın.


```r
> args(paste)
function (..., sep = " ", collapse = NULL, recycle0 = FALSE) 
NULL
```

 `paste()` fonksiyonu ile,`sep`ve`collapse` argümanları, varsayılan değerler kullanılmayacaksa, açıkça ve tam olarak adlandırılmalıdır.

Burada "a" ve "b "nin birlikte yapıştırılmasını ve iki nokta üst üste ile ayrılmasını istediğimi belirtiyorum.


```r
> paste("a", "b", sep = ":")
[1] "a:b"
```

Eğer `sep` argümanını tam olarak belirtmezsem ve kısmi eşleştirmeye güvenmeye çalışırsam, beklenen sonucu alamıyorum.


```r
> paste("a", "b", se = ":")
[1] "a b :"
```

## Yazım Aşamaları

Fonksiyon yazmak kadar iyi bir fonksiyon yazmak da önemlidir.
İyi bir fonksiyonun ilk özelliği doğru sonucu veriyor olmasıdır.

Bunu sağlayabilmek için fonksiyon yazmadan önce problemi iyi tanımlamakve problemin çözümünü komut satırları ile yazmak daha sonra bunufonksiyona dönüştürmek gereklidir.

Bir fonksiyonun doğru sonucu vermesi kadar diğer kullanıcılar tarafından anlaşılır olması da önemlidir.

-   Önce bir taslak oluşturun.
-   Taslağınızı içine komut satırlarınıza yapıştırın
-   Fonksiyonun argümanları belirleyin
-   Argüman isimlerinizi kullanacağınız değişkenlerle değiştirin


## Çoklu veri seti oluşturma ve dışarı aktarma

-   İstenilen sayıda veri seti oluşturan bir fonksiyon yazalım. Fonksiyonun ilk girdisi veri seti sayısı olmalı, varsayılan olarak bir veri seti oluşturalan fonksiyon taslağı oluşturalım.

``` r
fonksiyon_adi <- function(sayi=1){

}
```

-   Kullanıcı oluşturmak istediği her bir veri seti için satır ve sütun sayısını belirleyebilirsin. Satır ve sütun sayısını argüman olarak tanımlayalım. Örneğin oluşturduğu ilk veri setin 5 satır, 10 sütunlu ikincisi olsun. Bunun için argümana varsayalin değerler atayalım

``` r
fonksiyon_adi <- function(sayi=1,satir=c(5),sutun=c(10)){

}
```

-   Oluşturacak olan her bir veri setinin her bir sütunu standart normal dağılıma uygun olacak şekilde üretilsin. Oluşturulan veri setlerinden ilki "veri_1.xlsx" şeklinde çalışma alanına yazdırılsın


```r
> fonksiyon_adi <- function(sayi=1,satir=c(5),sutun=c(10)){
+ 
+     df <- data.frame(matrix(0,nrow=satir, ncol=sutun))
+      writexl::write_xlsx(df,"veri_1.xlsx")
+ }
> fonksiyon_adi(sayi=1,satir=c(5),sutun=c(10))
```

-   Şimdi ise bu fonksiyonu çoklu dosya yazımına uygun hale getirelim


```r
> 
> fonksiyon_adi <- function(sayi=3,satir=c(5,5,5),sutun=c(10,10,10)){
+     
+   df_list <- list() ## her bir veri setinin atanacağı yeni nesne
+     
+     for( i in 1:sayi){
+     df <- data.frame(matrix(0,nrow=satir[i], ncol=sutun[i])) # veri seti istenilen ozelliklerde olusturulur
+     }
+      for(j in 1:sutun[i]){
+      df[,j] <- round(rnorm(satir[i],0,1),2) #  her bir veri setinin her bir sütunu standart normal dağılıma uygun uretilir
+     }
+      df_list[[i]] <- df
+       df_list
+       writexl::write_xlsx(df_list[[i]],paste("veri",i,".xlsx", sep=""))
+ }
> fonksiyon_adi(sayi=3,satir=c(5,4,3),sutun=c(10,5,4))
```

### Sıra Sizde

Geometrik ortalamanın farklı hesaplama yolları bulunmaktadır.
Logaritma değerlerine dayalı olarak hesaplandığında, geometrik ortalama,gözlem değerlerinin logaritmalarının aritmetik ortalamasıdır.
Bir x vektorunun geometrik ortalamaasını logartimalara dayalı olarakhesaplayan bir fonsiyon yazıp, `x <- 1:100` için çalıştırınız.

## Özet

-   Fonksiyonlar `function()` direktifi kullanılarak tanımlanabilir ve diğer R nesneleri gibi R nesnelerine atanır

-   Fonksiyonlar adlandırılmış argümanlarla tanımlanabilir; bu fonksiyon argümanlarının varsayılan değerleri olabilir

-   Fonksiyon argümanları isme göre veya argüman listesindeki konuma göre belirtilebilir

-   Fonksiyonlar her zaman fonkisyon gövdesinde değerlendirilen son ifadeyi döndürür

-   Bir fonksiyon tanımında özel `...` argümanı kullanılarak değişken sayıda argüman belirtilebilir.
