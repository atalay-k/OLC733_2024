---
title: "lavaan"
output: html_document
date: "2024-04-17"
---
# lavaan ve sem


Bundan sonraki bölümlerde kullanılacak olan lavaan paketinin `cfa` ve `sem` 
 ve semPlot paketinin `semPaths`fonksiyonlarının argümanları açıklanmıştır. 

## sem fonksiyonu


| Argüman	| Açıklama	| Değerleri| 
| ---|---|---|
| Model	  |YEM modeli tanımlanır.| 	|
| Data	  |Gözlenen değişkenlerin yer aldığı veri setidir.| | 	
|sampling.weights	| Örneklem ağırlıklandırması yapılacağı durumlarda tanımlanır. | 	Veri çerçevesinde ağırlıklandırma bilgisinin yer aldığı değişkenin adıdır.| 
|group	|Çoklu grup analizlerde grup değişkeni tanımlanır. |	Veri matrisinde grubu tanımlayan değişkenin adıdır. |
|cluster	|Çok düzeyli analizlerde düzey değişkeni tanımlanır. 	|Veri matrisinde düzeyi tanımlayan değişkenin adıdır. |
|constraints|	Modele eklenecek diğer sınırlandırmalar tanımlanır. |	|
|estimator	|Kestirim yöntemidir. |	“ML”, “GLS”, “WLS”, ”ULS”, ”DWLS” gibi|

| formul	| tur	|  
| ---|---|
| gizil değişken tanımlama	| =~	| 
| regresyon	| ~	| 
| kovaryans	| ~~	|  
| kesisim	| ~1	|



-   **cfa()** fonksiyonunun kullanımı aşağıdaki gibidir:


```r
cfa(model = NULL,
    data = NULL, 
    ordered = NULL, sampling.weights = NULL, 
    sample.cov = NULL, sample.mean = NULL, sample.th = NULL, 
    sample.nobs = NULL, group = NULL, cluster = NULL, 
    constraints = "", WLS.V = NULL, NACOV = NULL, ...)
```



## semPaths


| Argüman |	Açıklama |	Değerleri |
| ---|---|---|
|Object	|YEM modeli analiz çıktısını içeren nesnedir. |
|What	|Diyagramda hangi değerlerin gösterileceği tanımlanır. |	“path”, “diagram” ve “mod”: yalnızca diyagramı “est” ve “par” kestirilen; “stand” ve “std” standartlaştırılmış parametreler “eq” ve “cons” eşitlenen parametreler aynı renkle gösterilir. |
|whatLabels	|Yol çizgilerinde hangi değerlerin gösterileceği tanımlanır. |	what argümanıyla aynı değerleri alır. |
|Style	|Diyagramın biçimi tanımlanır.	“ram”, “mx”, “OpenMx”, “lisrel”|
|layout	|Diyagramın tasarımı tanımlanır. |	“tree”, “tree2”, “circle”, “circle2”, “spring”|
|title|	Çoklu grup analizlerde grup adlarının diyagram başlığı olarak tanımlanması sağlanır.|	|
|Reorder|	Faktör yüklerine göre sıralama yapılır.| 	TRUE, FALSE|
|edge.label.cex	|Yol çizgilerinde yer alan parametre kestirim değerlerinin font büyüklüğüdür.| 	Sayısal değer|
|color|	Diyagramdaki şekillerin renkleri tanımlanır. |	Liste: list(man=””, lat= “”, int=””) man: gözlenen, lat: gizil değişken, int: kesişim|
|rotation|	Diyagramın yönü belirlenir. |	1, 2, 3, 4|
|NCharNodes|	Değişken adlarının maksimum kaç karakter olacağı tanımlanır. |	Sayısal değer|
|SizeMan|	Gözlenen değişkene ilişkin dörtgen şeklinin büyüklüğü tanımlanır. |	Sayısal değer|
|sizeLat|	Gizil değişkene ilişkin daire şeklinin büyüklüğü tanımlanır. |	Sayısal değer |



## semPlot Paketi

-   **semPlot** paketi YEM analizlerine ilişkin diyagramların çizilmesine olanak sağlayan fonksiyonları içerir.

-   Paket içinde yer alan **semPaths()** fonksiyonu **lavaan** fonksiyonlarının çıktılarıyla doğrudan çalıştırılabildiğinden, YEM analizlerine ilişkin diyagramların çizilmesinde oldukça kullanışlıdır.

-   **semPaths()** fonksiyonunun diyagramların özelleştirilmesine ilişkin çok sayıda argümanı bulunmaktadır.



