# Hata Ayıklama/Debugging



## Yanlış bir şeyler var!


R, size bir şeylerin yolunda gitmediğini göstermek için çeşitli yollara sahiptir. Sadece bildirimden ölümcül hataya kadar kullanılabilecek farklı gösterge seviyeleri vardır. R'de herhangi bir fonksiyonun çalıştırılması aşağıdaki *koşullarla* sonuçlanabilir.

- `message()` fonksiyonu tarafından üretilen genel bir bildirim/teşhis mesajı; fonksiyonun yürütülmesi devam eder
- `warning`: Bir şeylerin yanlış gittiğine dair bir gösterge, ancak ölümcül olması gerekmez; işlevin yürütülmesi devam eder. Uyarılar `warning()` fonksiyonu tarafından oluşturulur
- `error`: Ölümcül bir sorun oluştuğunu ve işlevin yürütülmesinin durduğunu gösteren bir gösterge. Hatalar `stop()` fonksiyonu tarafından üretilir.
- `condition`: Beklenmedik bir şeyin meydana geldiğini gösteren genel bir kavramdır; programcılar isterlerse kendi özel koşullarını oluşturabilirler.


İşte R'yi kullanırken alabileceğiniz bir uyarı örneği.


```r
> log(-1)
Warning in log(-1): NaNs produced
[1] NaN
```

Bu uyarı, negatif bir sayının logunu almanın `NaN` değeriyle sonuçlandığını bilmenizi sağlar, çünkü negatif sayıların logunu alamazsınız. Bununla birlikte, R bir hata vermez, çünkü döndürebileceği yararlı bir değere, `NaN` değerine sahiptir. Uyarı sadece beklenmedik bir şey olduğunu size bildirmek için vardır. Ne programladığınıza bağlı olarak, kodun başka bir bölümüne geçmek için kasıtlı olarak negatif bir sayının logunu almış olabilirsiniz.

İşte girdisinin niteliğine bağlı olarak konsola bir mesaj yazdırmak üzere tasarlanmış başka bir fonksiyon.



```r
> printmessage <- function(x) {
+         if(x > 0)
+                 print("x  sifirdan büyüktür")
+         else
+                 print("x sifirdan küçük ve ya eşittir")
+         invisible(x)        
+ }
```

Bu fonksiyon basittir---x`in sıfırdan büyük mü yoksa sıfırdan küçük mü ya da sıfıra eşit mi olduğunu belirten bir mesaj yazdırır. Ayrıca girdisini *görünmez* olarak döndürür, bu da "print" fonksiyonlarında yaygın bir uygulamadır. Bir nesneyi görünmez olarak döndürmek, fonksiyon çağrıldığında dönüş değerinin otomatik olarak yazdırılmayacağı anlamına gelir.

Yukarıdaki fonksiyona dikkatlice bakın ve herhangi bir hata ya da sorun tespit edip edemeyeceğinize bakın.

Fonksiyonu aşağıdaki gibi çalıştırabiliriz.


```r
> printmessage(1)
[1] "x  sifirdan büyüktür"
```

İşlev bu noktada iyi çalışıyor gibi görünüyor. Hata, uyarı veya mesaj yok.


```r
> printmessage(NA)
Error in if (x > 0) print("x  sifirdan büyüktür") else print("x sifirdan küçük ve ya eşittir"): missing value where TRUE/FALSE needed
```

Ne oldu?

Fonksiyonun yaptığı ilk şey `x > 0` olup olmadığını test etmektir. Ancak `x` bir `NA` veya `NaN` değeri ise bu testi yapamazsınız. R bu durumda ne yapacağını bilemez, bu yüzden ölümcül bir hata ile durur.

Bu sorunu `NA` değerlerinin olasılığını öngörerek ve `is.na()` fonksiyonu ile girdinin `NA` olup olmadığını kontrol ederek çözebiliriz.


```r
> printmessage2 <- function(x) {
+         if(is.na(x))
+                 print("x in eksik degeri var!")
+         else if(x > 0)
+                 print("x 0'dan buyuk")
+         else
+                 print("x sifirdan küçük ve ya eşittir")
+         invisible(x)
+ }
```

Şimdi aşağıdakileri çalıştırabiliriz.


```r
> printmessage2(NA)
[1] "x in eksik degeri var!"
```
Ve her şey yolunda.

Peki ya aşağıdaki durum.


```r
> x <- log(c(-1, 2))
Warning in log(c(-1, 2)): NaNs produced
```

Burada bazı `NaN`lar bekliyoruz çünkü negatif bir sayının logunu almak mantıklı değildir.

```r
> printmessage2(x)
Error in if (is.na(x)) print("x in eksik degeri var!") else if (x > 0) print("x 0'dan buyuk") else print("x sifirdan küçük ve ya eşittir"): the condition has length > 1
```

Şimdi ne olacak? Neden bu hatayı alıyoruz?  

Buradaki sorun, `printmessage2()` fonksiyonuna 1 yerine 2 uzunluğunda bir `x` vektörü vermiş olmamdır. `printmessage2()` fonksiyonunun gövdesi içinde `is.na(x)` ifadesi, `if` deyiminde test edilen bir vektör döndürür. Ancak, `if` vektör argümanları alamaz, bu nedenle bir hata alırsınız (R'nin önceki sürümlerinde yalnızca bir uyarı alırdınız). Buradaki temel sorun `printmessage2()` ifadesinin *vektörleştirilmemiş* olmasıdır.

Bu sorunu iki şekilde çözebiliriz. Birincisi basitçe vektör argümanlarına izin vermemektir. Diğer yol ise `printmessage2()` fonksiyonunu vektör argümanları almasına izin verecek şekilde vektörleştirmektir.

İlk yol için, basitçe girdinin uzunluğunu kontrol etmemiz gerekir.


```r
> printmessage3 <- function(x) {
+         if(length(x) > 1L)
+                 stop("'x' vektörünün uzunluğu 1'den büyüktür")
+         if(is.na(x))
+                 print("x'in eksik değeri var")
+         else if(x > 0)
+                 print("x 0'dan büyük")
+         else
+                 print("x sifirdan küçük ve ya eşittir")
+         invisible(x)
+ }
```

Şimdi `printmessage3()` fonksiyonuna bir vektör verdiğimizde bir hata almamız gerekir.


```r
> printmessage3(1:2)
Error in printmessage3(1:2): 'x' vektörünün uzunluğu 1'den büyüktür
```

Fonksiyonun vektörleştirilmesi `Vectorize()` fonksiyonu ile kolayca gerçekleştirilebilir.



```r
> printmessage4 <- Vectorize(printmessage2)
> out <- printmessage4(c(-1, 2))
[1] "x sifirdan küçük ve ya eşittir"
[1] "x 0'dan buyuk"
```

Şimdi doğru mesajların herhangi bir uyarı veya hata olmadan yazdırıldığını görebilirsiniz. printmessage4()` fonksiyonunun geri dönüş değerini `out` adında ayrı bir R nesnesinde sakladığıma dikkat edin. Bunun nedeni, `Vectorize()` fonksiyonunu kullandığımda artık dönüş değerinin görünmezliğini korumamasıdır.

## Neyin Yanlış Olduğunu Anlamak

Herhangi bir R kodunda hata ayıklamanın birincil görevi, sorunun ne olduğunu doğru bir şekilde teşhis etmektir. Kodunuzla (veya bir başkasının koduyla) ilgili bir sorunu teşhis ederken, öncelikle ne olmasını beklediğinizi anlamanız önemlidir. Daha sonra neyin *olduğunu* ve beklentilerinizden nasıl saptığını belirlemeniz gerekir. Sormanız gereken bazı temel sorular şunlardır

- Girdiniz neydi? Fonksiyonu nasıl çağırdınız?
- Ne bekliyordunuz? Çıktı, mesajlar, diğer sonuçlar? 
- Ne elde ettiniz?
- Elde ettiğiniz şey beklediğinizden nasıl farklıydı? 
- Beklentileriniz ilk etapta doğru muydu?
- Sorunu (tam olarak) yeniden üretebilir misiniz?

Bu soruları yanıtlayabilmek sadece kendi iyiliğiniz için değil, aynı zamanda sorunu ayıklamak için başka birinden yardım istemeniz gerekebilecek durumlarda da önemlidir. Tecrübeli programcılar size tam olarak bu soruları soracaktır.


## R'de Hata Ayıklama Araçları


R, kodunuzda hata ayıklama yapmanıza yardımcı olacak bir dizi araç sağlar. R'de fonksiyonlarda hata ayıklamak için kullanılan başlıca araçlar şunlardır

- `traceback()`: bir hata oluştuktan sonra fonksiyon çağrı yığınını yazdırır; hata yoksa hiçbir şey yapmaz
- `debug()`: bir fonksiyonu "hata ayıklama" modu için işaretler, bu da bir fonksiyonun yürütülmesinde her seferinde bir satır adım atmanızı sağlar
- `browser()`: çağrıldığı her yerde bir işlevin yürütülmesini askıya alır ve işlevi hata ayıklama moduna geçirir
- `trace()`: bir fonksiyonun belirli yerlerine hata ayıklama kodu eklemenizi sağlar 
- `recover()`: fonksiyon çağrı yığınına göz atabilmeniz için hata davranışını değiştirmenize olanak tanır

Bu fonksiyonlar, bir fonksiyonun içinden geçmenizi sağlamak için özel olarak tasarlanmış etkileşimli araçlardır. Ayrıca, fonksiyona `print()` veya `cat()` deyimlerini eklemek gibi daha kör bir teknik de vardır.

## `traceback()` 


`traceback()` fonksiyonu, bir hata oluştuktan sonra *fonksiyon çağrı yığınını* yazdırır. Fonksiyon çağrı yığını, hata oluşmadan önce çağrılan fonksiyonlar dizisidir. 

Örneğin, bir `a()` fonksiyonunuz olabilir. Bu fonksiyonda  `c()` ve  `d()` fonksiyonlarını çağıran `b()` fonksiyonuna bağlı çalışabilir.  Bir hata oluşursa, hatanın hangi fonksiyonda oluştuğu hemen anlaşılamayabilir. `traceback()` fonksiyonu hata oluştuğunda kaç seviye derinde olduğunuzu gösterir.

```r
> mean(x)
Error in mean(x) : object 'x' not found
> traceback()
1: mean(x)
```
Burada, `x` nesnesi mevcut olmadığı için hatanın `mean()` fonksiyonunun içinde meydana geldiği açıktır. 

Bir hata oluştuktan hemen sonra `traceback()` fonksiyonu çağrılmalıdır. Başka bir fonksiyon çağrıldığında, geri izleme özelliğini kaybedersiniz.


Burada doğrusal modelleme için `lm()` fonksiyonunu kullanan biraz daha karmaşık bir örnek verilmiştir.

```r
> lm(y ~ x)
Error in eval(expr, envir, enclos) : object ’y’ not found
> traceback()
7: eval(expr, envir, enclos)
6: eval(predvars, data, env)
5: model.frame.default(formula = y ~ x, drop.unused.levels = TRUE)
4: model.frame(formula = y ~ x, drop.unused.levels = TRUE)
3: eval(expr, envir, enclos)
2: eval(mf, parent.frame())
1: lm(y ~ x)
```

Şimdi hatanın fonksiyon çağrı yığınının 7. seviyesine kadar atılmadığını görebilirsiniz, bu durumda `eval()` fonksiyonu `y ~ x` formülünü değerlendirmeye çalışmış ve `y` nesnesinin mevcut olmadığını fark etmiştir.

Geri izlemeye bakmak, hatanın kabaca nerede oluştuğunu anlamak için yararlıdır, ancak daha ayrıntılı hata ayıklama için kullanışlı değildir. Bunun için `debug()` fonksiyonuna başvurabilirsiniz.

##  `debug()`

`debug()` işlevi, bir işlev için etkileşimli bir hata ayıklayıcı (R'de "tarayıcı" olarak da bilinir) başlatır. Hata ayıklayıcıyla, bir hatanın tam olarak nerede oluştuğunu saptamak için bir R işlevini her seferinde bir ifadeyle adımlayabilirsiniz.

`debug()` fonksiyonu ilk argüman olarak bir fonksiyon alır. Burada `lm()` fonksiyonunun hata ayıklamasına bir örnek verilmiştir. 

```r
> debug(lm)      
> lm(y ~ x)
debugging in: lm(y ~ x)
debug: {
    ret.x <- x
    ret.y <- y
    cl <- match.call()
    ...
    if (!qr)
        z$qr <- NULL 
    z
} 
Browse[2]>
```

Şimdi, `lm()` fonksiyonunu her çağırdığınızda etkileşimli hata ayıklayıcıyı başlatacaktır. Bu davranışı kapatmak için `undebug()` fonksiyonunu çağırmanız gerekir.

Hata ayıklayıcı, tarayıcıyı fonksiyon gövdesinin en üst seviyesinde çağırır. Buradan gövdedeki her bir ifade boyunca adım atabilirsiniz. Tarayıcıda çağırabileceğiniz birkaç özel komut vardır:

* `n` geçerli ifadeyi yürütür ve sonraki ifadeye geçer
* `c` fonksiyonun yürütülmesine devam eder ve bir hata oluşana ya da fonksiyondan çıkılana kadar durmaz
* `Q` tarayıcıdan çıkar

İşte `lm()` fonksiyonunu kullanan bir tarayıcı oturumu örneği.

```r
Browse[2]> n   ##  geçerli ifadeyi yürütür ve sonraki ifadeye geçer
debug: ret.x <- x
Browse[2]> n
debug: ret.y <- y
Browse[2]> n
debug: cl <- match.call()
Browse[2]> n
debug: mf <- match.call(expand.dots = FALSE)
Browse[2]> n
debug: m <- match(c("formula", "data", "subset", "weights", "na.action",
    "offset"), names(mf), 0L)
```

Tarayıcıdayken, normal bir oturumda kullanabileceğiniz diğer R işlevlerini çalıştırabilirsiniz. Özellikle, mevcut ortamınızda (fonksiyon ortamı) ne olduğunu görmek için `ls()` ve fonksiyon ortamındaki R nesnelerinin değerlerini yazdırmak için `print()` fonksiyonlarını kullanabilirsiniz.

`undebug()` fonksiyonu ile etkileşimli hata ayıklamayı kapatabilirsiniz.

```r
undebug(lm)    ## Unflag the 'lm()' function for debugging
```

## `recover()`

`recover()` fonksiyonu, bir hata oluştuğunda R'nin hata davranışını değiştirmek için kullanılabilir. Normalde, bir fonksiyonda hata oluştuğunda, R bir hata mesajı yazdırır, fonksiyondan çıkar ve diğer komutları beklemek üzere sizi çalışma alanınıza geri döndürür. 

`recover()` ile R'ye bir hata oluştuğunda, hatanın oluştuğu tam noktada yürütmeyi durdurması gerektiğini söyleyebilirsiniz. Bu size hatanın oluştuğu ortamı kurcalama fırsatı verebilir. Bu, bozulmuş veya yanlışlıkla değiştirilmiş herhangi bir R nesnesi veya verisi olup olmadığını görmek için yararlı olabilir.

```r
> options(error = recover)    ## Change default R error behavior
> read.csv("nosuchfile")      ## This code doesn't work
Error in file(file, "rt") : cannot open the connection
In addition: Warning message:
In file(file, "rt") :
  cannot open file ’nosuchfile’: No such file or directory
  
Enter a frame number, or 0 to exit

1: read.csv("nosuchfile")
2: read.table(file = file, header = header, sep = sep, quote = quote, dec =
3: file(file, "rt")

Selection:
```

`recover()` fonksiyonu, bir hata oluştuğunda ilk olarak fonksiyon çağrı yığınını yazdıracaktır. Daha sonra, çağrı yığınının etrafında atlamayı ve sorunu araştırmayı seçebilirsiniz. Bir kare numarası seçtiğinizde, tarayıcıya yerleştirileceksiniz (tıpkı `debug()` ile tetiklenen etkileşimli hata ayıklayıcı gibi) ve etrafı kurcalayabileceksiniz.

## ÖZet


- Bir sorunun/durumun üç ana göstergesi vardır: `message`, `warning`, `error`; sadece bir `error` ölümcüldür
- Problemli bir fonksiyonu analiz ederken, problemi yeniden üretebildiğinizden emin olun, beklentilerinizi ve çıktının beklentinizden nasıl farklı olduğunu açıkça belirtin
- Etkileşimli hata ayıklama araçları `traceback`, `debug`, `browser`, `trace` ve `recover` işlevlerdeki sorunlu kodları bulmak için kullanılabilir
- **Hata ayıklama araçları düşünmenin yerini tutmaz!**


<!-- https://bookdown.org/rdpeng/rprogdatascience/parallel-computation.html -->
