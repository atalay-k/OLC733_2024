---
bibliography: references.bib
---



# Klasik Test Teorisi

Düşünme İhtiyacı Ölçeğine (NFC Ölçeğine) ait verileri Klasik Test Teorisine (CTT) dayalı çeşitli psikometrik analizler yapmak ve bu aracın güvenilirlik ve geçerliliğinin nasıl değerlendirileceğini göstermek için kullanacağız. Örnek verilerin yer aldığı çalışma: (Thinking in action: Need for Cognition predicts Self-Control together with Action Orientation, Grass et al. 2019), NFC ile diğer gizli özellikler (örneğin, özkontrol) arasındaki ilişkiye odaklanmıştır.

NFC’nin gizil özelliğini ölçmek için Cacioppo ve Petty geliştirmiştir.

NFC (need for Cognition) bilişsel olarak zorlayıcı görevlere ve çaba gerektiren düşünmeye katılma arzusu olarak tanımlanan psikolojik bir gizli özelliktir.

Yüksek düzeyde NFC’ye sahip bireyler bilgiyi arama, edinme, üzerinde düşünme ve yansıtma eğilimindeyken, düşük düzeyde NFC’ye sahip bireyler dünya hakkında ayrıntılı bilgiden kaçınma ve bilişsel olarak karmaşık görevleri stresli bulma eğilimindedir.



<table class="table table-striped table-hover" style="font-size: 16px; margin-left: auto; margin-right: auto;border-bottom: 0;">
 <thead>
  <tr>
   <th style="text-align:right;"> Items </th>
   <th style="text-align:left;"> Description </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:left;"> Enjoyment of tasks that involve problem-solving </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:left;"> Preference for cognitive, difficult and important tasks </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:left;"> Tendency to strive for goals that require mental effort </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> Appeal of relying on one’s thought to be successful (R) </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5 </td>
   <td style="text-align:left;"> Satisfaction of completing important tasks that required thinking and mental effort </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:left;"> Preference for thinking about long-term projects (R) </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:left;"> Preference for cognitive challenges (R) </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:left;"> Satisfaction on hard and long deliberation (R) </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 9 </td>
   <td style="text-align:left;"> Attitude towards thinking as something one does primarily because one has to (R) </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:left;"> Appeal of being responsible for handling situations that require thinking (R) </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 11 </td>
   <td style="text-align:left;"> Attitude towards thinking as something that is fun (R) </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 12 </td>
   <td style="text-align:left;"> Anticipation and avoiding of situations that may require in-depth thinking (R) </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:left;"> Preference for puzzles to be solved </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 14 </td>
   <td style="text-align:left;"> Preference for complex over simple problems </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 15 </td>
   <td style="text-align:left;"> Preference for understanding the reason for an answer over simply knowing the answer without any background (R) </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 16 </td>
   <td style="text-align:left;"> Preference to know how something works over simply knowing that it works (R) </td>
  </tr>
</tbody>
<tfoot><tr><td style="padding: 0; " colspan="100%">
<span style="font-style: italic;">Note: </span> <sup></sup> Items marked with (R) were presented in an inverted form.</td></tr></tfoot>
</table>

Maddelere verilen yanıtlar 1 (hiç katılmıyorum) ile 7 (tamamen katılıyorum) arasında değişen 7 puanlık bir derecelendirme ölçeğine göre kaydedilmiştir.

Ancak, NFC Ölçeğindeki toplam puanları hesaplamak için, madde yanıtları -3 (tamamen katılmıyorum) ile +3 (tamamen katılıyorum) olarak yeniden kodlanmalıdır.

Grass et al. (2019 ) veri dosyalarını ve diğer materyalleri paylaşmıştır.

Aşağıdaki analiz için, NFC Ölçeğine verilen yanıtlar, demografik değişkenler ve Öz Denetim Ölçeği gibi ölçüt ölçümlerinden elde edilen ek puanları içeren orijinal verilerin bir alt kümesini kullanacağız. Bu veri seti 🔗 [import/nfc_data.csv](import/nfc_data.csv) indirilebilir.



## Veri İnceleme

Verileri özetlemek için genellikle hem istatistiksel hem de veri görselleştirme araçlarını kullanırız. Daha ayrıntılı bilgi için 🔗[sayfayı](https://okanbulut.github.io/bigdata/eda.html) inceleyebilirsiniz.


```r
nfc <- read.csv("import/nfc_data.csv", header = TRUE)
head(nfc)
```

<div class="kable-table">

| id| age|sex    |education | nfc01| nfc02| nfc03| nfc04| nfc05| nfc06| nfc07| nfc08| nfc09| nfc10| nfc11| nfc12| nfc13| nfc14| nfc15| nfc16| action_orientation| effortful_control| self_control|
|--:|---:|:------|:---------|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|------------------:|-----------------:|------------:|
|  1|  26|Male   |Abitur    |     5|     7|     5|     1|     6|     2|     2|     5|     2|     1|     2|     2|     6|     5|     2|     1|                 10|                11|            5|
|  4|  19|Female |Abitur    |     5|     5|     3|     2|     5|     3|     3|     3|     2|     4|     3|     2|     3|     2|     4|     4|                 11|                32|           16|
|  7|  23|Female |Abitur    |     5|     6|     5|     1|     7|     3|     2|     3|     1|     3|     3|     1|     6|     5|     1|     1|                 13|                10|           -2|
|  8|  24|Female |Abitur    |     5|     5|     4|     2|     5|     5|     2|     2|     2|     2|     4|     3|     2|     3|     1|     1|                  5|                 0|           -4|
| 11|  24|Female |Abitur    |     2|     3|     3|     5|     3|     5|     6|     1|     6|     6|     6|     6|     2|     1|     5|     5|                 18|                10|           11|
| 12|  20|Female |Abitur    |     6|     6|     6|     1|     6|     3|     1|     1|     1|     1|     1|     1|     6|     6|     2|     2|                 20|                24|           11|

</div>

Ayrıca fonksiyonunu ile veri setinin yapısını incelyebilirsiniz.


```r
str(nfc)
```

```
## 'data.frame':	1209 obs. of  23 variables:
##  $ id                : int  1 4 7 8 11 12 15 16 21 23 ...
##  $ age               : int  26 19 23 24 24 20 25 27 21 25 ...
##  $ sex               : chr  "Male" "Female" "Female" "Female" ...
##  $ education         : chr  "Abitur" "Abitur" "Abitur" "Abitur" ...
##  $ nfc01             : int  5 5 5 5 2 6 3 7 7 6 ...
##  $ nfc02             : int  7 5 6 5 3 6 4 4 5 7 ...
##  $ nfc03             : int  5 3 5 4 3 6 4 5 3 7 ...
##  $ nfc04             : int  1 2 1 2 5 1 1 6 1 1 ...
##  $ nfc05             : int  6 5 7 5 3 6 6 6 6 7 ...
##  $ nfc06             : int  2 3 3 5 5 3 5 3 6 2 ...
##  $ nfc07             : int  2 3 2 2 6 1 3 1 1 1 ...
##  $ nfc08             : int  5 3 3 2 1 1 2 2 2 2 ...
##  $ nfc09             : int  2 2 1 2 6 1 3 2 1 1 ...
##  $ nfc10             : int  1 4 3 2 6 1 3 2 2 2 ...
##  $ nfc11             : int  2 3 3 4 6 1 1 1 1 1 ...
##  $ nfc12             : int  2 2 1 3 6 1 1 1 1 1 ...
##  $ nfc13             : int  6 3 6 2 2 6 3 4 6 5 ...
##  $ nfc14             : int  5 2 5 3 1 6 3 4 4 5 ...
##  $ nfc15             : int  2 4 1 1 5 2 1 2 2 1 ...
##  $ nfc16             : int  1 4 1 1 5 2 2 2 2 2 ...
##  $ action_orientation: int  10 11 13 5 18 20 6 16 12 8 ...
##  $ effortful_control : int  11 32 10 0 10 24 -3 2 -6 21 ...
##  $ self_control      : int  5 16 -2 -4 11 11 -4 3 -10 5 ...
```

Veri kümesi 1209 satırdan (yani katılımcılar) ve 23 değişkenden (id, yaş, cinsiyet, eğitim, NFC Ölçeği maddelerine verilen yanıtları temsil eden nfc01 ila nfc16 ve ölçüt ölçümleri için üç puan) oluşmaktadır. DataExplorer paketindeki [@DataExplorer] `introduce()` ve `plot_intro()` fonksiyonlarını kullanarak veri seti hakkında biraz daha bilgi edinebiliriz:


```r
DataExplorer::introduce(nfc)
```

<div class="kable-table">

| rows| columns| discrete_columns| continuous_columns| all_missing_columns| total_missing_values| complete_rows| total_observations| memory_usage|
|----:|-------:|----------------:|------------------:|-------------------:|--------------------:|-------------:|------------------:|------------:|
| 1209|      23|                2|                 21|                   0|                    8|          1201|              27807|       126664|

</div>


```r
kbl(t(introduce(nfc)), 
    row.names = TRUE, col.names = "", 
    format.args = list(big.mark = ",")) %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;">  </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> rows </td>
   <td style="text-align:right;"> 1,209 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> columns </td>
   <td style="text-align:right;"> 23 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> discrete_columns </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> continuous_columns </td>
   <td style="text-align:right;"> 21 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> all_missing_columns </td>
   <td style="text-align:right;"> 0 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> total_missing_values </td>
   <td style="text-align:right;"> 8 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> complete_rows </td>
   <td style="text-align:right;"> 1,201 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> total_observations </td>
   <td style="text-align:right;"> 27,807 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> memory_usage </td>
   <td style="text-align:right;"> 126,664 </td>
  </tr>
</tbody>
</table>


```r
DataExplorer::plot_intro(nfc)
```

<img src="15_KTK_files/figure-html/unnamed-chunk-9-1.png" width="100%" style="display: block; margin: auto;" />

değişkenlerin çoğunun sürekli olduğunu (R Likert maddeleri de aslında sıralı olmalarına rağmen sürekli değişkenler olarak tanımlanır), kesikli (yani kategorik) değişkenler (yani cinsiyet ve eğitim) olduğunu göstermektedir. Ayrıca veri setindeki bazı değişkenlerin kayıp değerlere sahip olduğunu ancak kayıp veri oranının çok küçük olduğunu görüyoruz (sadece %0,029).


```r
DataExplorer::plot_missing(nfc)
```

<img src="15_KTK_files/figure-html/unnamed-chunk-10-1.png" width="100%" style="display: block; margin: auto;" />

Eksik değerlere daha yakından bakmak için, her bir değişken için eksiklik oranını görselleştirebiliriz. grafik, yaş ve cinsiyetin bazı kayıp değerlere sahip olduğunu ancak kayıp oranının çok küçük olduğunu (%1’den az) göstermektedir.


```r
DataExplorer::plot_bar(data = nfc[, c("education", "sex")])
```

<img src="15_KTK_files/figure-html/unnamed-chunk-11-1.png" width="100%" style="display: block; margin: auto;" />


```r
DataExplorer::plot_histogram(data = 
            nfc[, c("age", "self_control", 
                    "action_orientation", 
                    "effortful_control")])
```

<img src="15_KTK_files/figure-html/unnamed-chunk-12-1.png" width="100%" style="display: block; margin: auto;" />


```r
DataExplorer::plot_boxplot(data = nfc[!is.na(nfc$sex), # cinsiyet değişkeninde eksik verisi olmayanlar
c("sex", "self_control", 
  "action_orientation", 
  "effortful_control")],  
by = "sex") # Kategorik değişken düzeyleri için
```

<img src="15_KTK_files/figure-html/unnamed-chunk-13-1.png" width="100%" style="display: block; margin: auto;" />


```r
## # id değişkeni analizlere dahil edilmediği için çıkarıldı
nfc <- DataExplorer::drop_columns(nfc, "id")
#
# DataExplorer::create_report(data = nfc,
#                              report_title = "Veri On Inceleme",
#                              output_file = "oninceleme.html")
```

Tüm özet istatistikleri tek bir rapor halinde düzenlemek için create_report() fonksiyonunu kullanabiliriz. Bu fonksiyon DataExplorer içindeki çoğu fonksiyonu çalıştırır ve bir HTML rapor dosyası çıktısı verir.🔗 [oninceleme](oninceleme.html)

Tek bir analizde nfc veri setinin ayrıntılı bir özetini elde etmek için skimr paketindeki [@skimr] `skim()` fonksiyonunu kullanabiliriz. Çıktıdan da görebileceğiniz gibi, nfc veri setindeki değişkenler için benzer tanımlayıcı istatistikler elde edilir


```r
skimr::skim(nfc)
```


Table: (\#tab:unnamed-chunk-15)Data summary

|                         |     |
|:------------------------|:----|
|Name                     |nfc  |
|Number of rows           |1209 |
|Number of columns        |22   |
|_______________________  |     |
|Column type frequency:   |     |
|character                |2    |
|numeric                  |20   |
|________________________ |     |
|Group variables          |None |


**Variable type: character**

|skim_variable | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|sex           |         3|             1|   4|   6|     0|        2|          0|
|education     |         0|             1|   5|  15|     0|        4|          0|


**Variable type: numeric**

|skim_variable      | n_missing| complete_rate|  mean|    sd|  p0| p25| p50| p75| p100|hist  |
|:------------------|---------:|-------------:|-----:|-----:|---:|---:|---:|---:|----:|:-----|
|age                |         5|             1| 24.43|  3.93|  18|  22|  24|  26|   50|▇▅▁▁▁ |
|nfc01              |         0|             1|  5.54|  1.19|   1|   5|   6|   6|    7|▁▁▁▃▇ |
|nfc02              |         0|             1|  4.94|  1.39|   1|   4|   5|   6|    7|▁▂▃▆▇ |
|nfc03              |         0|             1|  4.44|  1.40|   1|   3|   5|   5|    7|▃▅▆▇▆ |
|nfc04              |         0|             1|  2.47|  1.47|   1|   1|   2|   3|    7|▇▂▁▁▁ |
|nfc05              |         0|             1|  5.82|  1.23|   1|   5|   6|   7|    7|▁▁▁▂▇ |
|nfc06              |         0|             1|  3.75|  1.50|   1|   3|   4|   5|    7|▇▆▇▅▅ |
|nfc07              |         0|             1|  2.70|  1.32|   1|   2|   2|   3|    7|▇▃▂▁▁ |
|nfc08              |         0|             1|  3.34|  1.55|   1|   2|   3|   4|    7|▇▆▅▃▂ |
|nfc09              |         0|             1|  2.45|  1.52|   1|   1|   2|   3|    7|▇▂▂▁▁ |
|nfc10              |         0|             1|  3.16|  1.52|   1|   2|   3|   4|    7|▇▅▃▂▂ |
|nfc11              |         0|             1|  2.75|  1.45|   1|   2|   2|   4|    7|▇▃▂▂▁ |
|nfc12              |         0|             1|  2.45|  1.40|   1|   1|   2|   3|    7|▇▂▁▁▁ |
|nfc13              |         0|             1|  4.10|  1.42|   1|   3|   4|   5|    7|▅▆▇▇▅ |
|nfc14              |         0|             1|  3.57|  1.47|   1|   2|   4|   4|    7|▇▆▇▅▃ |
|nfc15              |         0|             1|  2.25|  1.34|   1|   1|   2|   3|    7|▇▂▁▁▁ |
|nfc16              |         0|             1|  2.48|  1.40|   1|   1|   2|   3|    7|▇▃▁▁▁ |
|action_orientation |         0|             1|  9.84|  4.96|   0|   6|   9|  13|   24|▃▇▇▃▁ |
|effortful_control  |         0|             1|  6.84| 13.97| -45|  -2|   7|  16|   57|▁▂▇▃▁ |
|self_control       |         0|             1|  0.12|  8.36| -22|  -5|   0|   5|   24|▁▅▇▃▁ |

veriler için temel tanımlayıcı istatistikleri elde etmek için psych paketindeki [@psych] `describe()` fonksiyonu kullanılabilir.


```r
psych::describe(x = nfc) %>% 
  kable(digit=2)
```



|                   | vars|    n|  mean|    sd| median| trimmed|   mad| min| max| range|  skew| kurtosis|   se|
|:------------------|----:|----:|-----:|-----:|------:|-------:|-----:|---:|---:|-----:|-----:|--------:|----:|
|age                |    1| 1204| 24.43|  3.93|     24|   24.00|  2.97|  18|  50|    32|  1.54|     4.79| 0.11|
|sex*               |    2| 1206|  1.41|  0.49|      1|    1.39|  0.00|   1|   2|     1|  0.36|    -1.87| 0.01|
|education*         |    3| 1209|  1.04|  0.31|      1|    1.00|  0.00|   1|   4|     3|  7.59|    58.45| 0.01|
|nfc01              |    4| 1209|  5.54|  1.19|      6|    5.68|  1.48|   1|   7|     6| -1.18|     1.56| 0.03|
|nfc02              |    5| 1209|  4.94|  1.39|      5|    5.01|  1.48|   1|   7|     6| -0.49|    -0.31| 0.04|
|nfc03              |    6| 1209|  4.44|  1.40|      5|    4.51|  1.48|   1|   7|     6| -0.32|    -0.48| 0.04|
|nfc04              |    7| 1209|  2.47|  1.47|      2|    2.25|  1.48|   1|   7|     6|  1.16|     0.68| 0.04|
|nfc05              |    8| 1209|  5.82|  1.23|      6|    6.01|  1.48|   1|   7|     6| -1.33|     2.09| 0.04|
|nfc06              |    9| 1209|  3.75|  1.50|      4|    3.69|  1.48|   1|   7|     6|  0.24|    -0.60| 0.04|
|nfc07              |   10| 1209|  2.70|  1.32|      2|    2.57|  1.48|   1|   7|     6|  0.79|     0.20| 0.04|
|nfc08              |   11| 1209|  3.34|  1.55|      3|    3.27|  1.48|   1|   7|     6|  0.40|    -0.65| 0.04|
|nfc09              |   12| 1209|  2.45|  1.52|      2|    2.23|  1.48|   1|   7|     6|  1.03|     0.32| 0.04|
|nfc10              |   13| 1209|  3.16|  1.52|      3|    3.06|  1.48|   1|   7|     6|  0.61|    -0.30| 0.04|
|nfc11              |   14| 1209|  2.75|  1.45|      2|    2.61|  1.48|   1|   7|     6|  0.72|    -0.15| 0.04|
|nfc12              |   15| 1209|  2.45|  1.40|      2|    2.26|  1.48|   1|   7|     6|  0.93|     0.18| 0.04|
|nfc13              |   16| 1209|  4.10|  1.42|      4|    4.13|  1.48|   1|   7|     6| -0.13|    -0.54| 0.04|
|nfc14              |   17| 1209|  3.57|  1.47|      4|    3.54|  1.48|   1|   7|     6|  0.11|    -0.50| 0.04|
|nfc15              |   18| 1209|  2.25|  1.34|      2|    2.03|  1.48|   1|   7|     6|  1.25|     1.23| 0.04|
|nfc16              |   19| 1209|  2.48|  1.40|      2|    2.28|  1.48|   1|   7|     6|  1.06|     0.77| 0.04|
|action_orientation |   20| 1209|  9.84|  4.96|      9|    9.65|  4.45|   0|  24|    24|  0.33|    -0.40| 0.14|
|effortful_control  |   21| 1209|  6.84| 13.97|      7|    6.93| 13.34| -45|  57|   102| -0.08|     0.27| 0.40|
|self_control       |   22| 1209|  0.12|  8.36|      0|    0.08|  7.41| -22|  24|    46|  0.06|    -0.26| 0.24|

## Korelasyon

Madde analizine geçmeden önce, maddelerin birbirleriyle ne kadar güçlü bir şekilde ilişkili olduğunu incelemek için maddeler arasındaki korelasyonları da kontrol etmeliyiz. Maddelerin birbirleriyle belirli bir dereceye kadar ilişkili olmasını bekleriz çünkü maddelerin aynı örtük özelliği (bu örnekte NFC yapısı) ölçtüğünü varsayarız.

Buna ek olarak, NFC Ölçeğindeki bazı maddelerin olumsuz ifadeler içerdiğini ve dolayısıyla bu maddelere verilen yanıtların diğer maddelerle ters yönde olabileceğini biliyoruz. Örneğin, yüksek NFC’ye sahip bireylerin “Problem çözmeyi içeren görevlerden keyif alma” gibi olumlu ifadeler içeren bir madde için “7 = tamamen katılıyorum” seçeneğini işaretlemeleri beklenirken, “Derinlemesine düşünmeyi gerektirebilecek durumları öngörme ve bunlardan kaçınma” gibi olumsuz ifadeler içeren bir madde için “1 = tamamen katılmıyorum” seçeneğini işaretlemeleri beklenmektedir.


```r
matris <- dplyr::select(nfc, starts_with("nfc"))

head(matris)
```

<div class="kable-table">

| nfc01| nfc02| nfc03| nfc04| nfc05| nfc06| nfc07| nfc08| nfc09| nfc10| nfc11| nfc12| nfc13| nfc14| nfc15| nfc16|
|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|
|     5|     7|     5|     1|     6|     2|     2|     5|     2|     1|     2|     2|     6|     5|     2|     1|
|     5|     5|     3|     2|     5|     3|     3|     3|     2|     4|     3|     2|     3|     2|     4|     4|
|     5|     6|     5|     1|     7|     3|     2|     3|     1|     3|     3|     1|     6|     5|     1|     1|
|     5|     5|     4|     2|     5|     5|     2|     2|     2|     2|     4|     3|     2|     3|     1|     1|
|     2|     3|     3|     5|     3|     5|     6|     1|     6|     6|     6|     6|     2|     1|     5|     5|
|     6|     6|     6|     1|     6|     3|     1|     1|     1|     1|     1|     1|     6|     6|     2|     2|

</div>


```r
cormat <- psych::polychoric(x = matris)$rho

cormat %>% kbl(digits = 2) %>%
kable_styling(bootstrap_options = c("striped", "condensed"), font_size =
11)
```

<table class="table table-striped table-condensed" style="font-size: 11px; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;"> nfc01 </th>
   <th style="text-align:right;"> nfc02 </th>
   <th style="text-align:right;"> nfc03 </th>
   <th style="text-align:right;"> nfc04 </th>
   <th style="text-align:right;"> nfc05 </th>
   <th style="text-align:right;"> nfc06 </th>
   <th style="text-align:right;"> nfc07 </th>
   <th style="text-align:right;"> nfc08 </th>
   <th style="text-align:right;"> nfc09 </th>
   <th style="text-align:right;"> nfc10 </th>
   <th style="text-align:right;"> nfc11 </th>
   <th style="text-align:right;"> nfc12 </th>
   <th style="text-align:right;"> nfc13 </th>
   <th style="text-align:right;"> nfc14 </th>
   <th style="text-align:right;"> nfc15 </th>
   <th style="text-align:right;"> nfc16 </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> nfc01 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 0.49 </td>
   <td style="text-align:right;"> 0.42 </td>
   <td style="text-align:right;"> -0.31 </td>
   <td style="text-align:right;"> 0.37 </td>
   <td style="text-align:right;"> -0.21 </td>
   <td style="text-align:right;"> -0.44 </td>
   <td style="text-align:right;"> -0.37 </td>
   <td style="text-align:right;"> -0.33 </td>
   <td style="text-align:right;"> -0.42 </td>
   <td style="text-align:right;"> -0.40 </td>
   <td style="text-align:right;"> -0.33 </td>
   <td style="text-align:right;"> 0.48 </td>
   <td style="text-align:right;"> 0.37 </td>
   <td style="text-align:right;"> -0.32 </td>
   <td style="text-align:right;"> -0.36 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc02 </td>
   <td style="text-align:right;"> 0.49 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 0.56 </td>
   <td style="text-align:right;"> -0.35 </td>
   <td style="text-align:right;"> 0.43 </td>
   <td style="text-align:right;"> -0.17 </td>
   <td style="text-align:right;"> -0.50 </td>
   <td style="text-align:right;"> -0.41 </td>
   <td style="text-align:right;"> -0.30 </td>
   <td style="text-align:right;"> -0.34 </td>
   <td style="text-align:right;"> -0.35 </td>
   <td style="text-align:right;"> -0.29 </td>
   <td style="text-align:right;"> 0.43 </td>
   <td style="text-align:right;"> 0.43 </td>
   <td style="text-align:right;"> -0.34 </td>
   <td style="text-align:right;"> -0.34 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc03 </td>
   <td style="text-align:right;"> 0.42 </td>
   <td style="text-align:right;"> 0.56 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> -0.33 </td>
   <td style="text-align:right;"> 0.40 </td>
   <td style="text-align:right;"> -0.23 </td>
   <td style="text-align:right;"> -0.40 </td>
   <td style="text-align:right;"> -0.33 </td>
   <td style="text-align:right;"> -0.18 </td>
   <td style="text-align:right;"> -0.26 </td>
   <td style="text-align:right;"> -0.30 </td>
   <td style="text-align:right;"> -0.27 </td>
   <td style="text-align:right;"> 0.42 </td>
   <td style="text-align:right;"> 0.37 </td>
   <td style="text-align:right;"> -0.25 </td>
   <td style="text-align:right;"> -0.21 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc04 </td>
   <td style="text-align:right;"> -0.31 </td>
   <td style="text-align:right;"> -0.35 </td>
   <td style="text-align:right;"> -0.33 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> -0.35 </td>
   <td style="text-align:right;"> 0.18 </td>
   <td style="text-align:right;"> 0.43 </td>
   <td style="text-align:right;"> 0.38 </td>
   <td style="text-align:right;"> 0.33 </td>
   <td style="text-align:right;"> 0.29 </td>
   <td style="text-align:right;"> 0.35 </td>
   <td style="text-align:right;"> 0.37 </td>
   <td style="text-align:right;"> -0.24 </td>
   <td style="text-align:right;"> -0.21 </td>
   <td style="text-align:right;"> 0.29 </td>
   <td style="text-align:right;"> 0.22 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc05 </td>
   <td style="text-align:right;"> 0.37 </td>
   <td style="text-align:right;"> 0.43 </td>
   <td style="text-align:right;"> 0.40 </td>
   <td style="text-align:right;"> -0.35 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> -0.17 </td>
   <td style="text-align:right;"> -0.38 </td>
   <td style="text-align:right;"> -0.30 </td>
   <td style="text-align:right;"> -0.29 </td>
   <td style="text-align:right;"> -0.22 </td>
   <td style="text-align:right;"> -0.33 </td>
   <td style="text-align:right;"> -0.26 </td>
   <td style="text-align:right;"> 0.32 </td>
   <td style="text-align:right;"> 0.26 </td>
   <td style="text-align:right;"> -0.29 </td>
   <td style="text-align:right;"> -0.22 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc06 </td>
   <td style="text-align:right;"> -0.21 </td>
   <td style="text-align:right;"> -0.17 </td>
   <td style="text-align:right;"> -0.23 </td>
   <td style="text-align:right;"> 0.18 </td>
   <td style="text-align:right;"> -0.17 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 0.32 </td>
   <td style="text-align:right;"> 0.20 </td>
   <td style="text-align:right;"> 0.22 </td>
   <td style="text-align:right;"> 0.30 </td>
   <td style="text-align:right;"> 0.19 </td>
   <td style="text-align:right;"> 0.22 </td>
   <td style="text-align:right;"> -0.19 </td>
   <td style="text-align:right;"> -0.13 </td>
   <td style="text-align:right;"> 0.15 </td>
   <td style="text-align:right;"> 0.19 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc07 </td>
   <td style="text-align:right;"> -0.44 </td>
   <td style="text-align:right;"> -0.50 </td>
   <td style="text-align:right;"> -0.40 </td>
   <td style="text-align:right;"> 0.43 </td>
   <td style="text-align:right;"> -0.38 </td>
   <td style="text-align:right;"> 0.32 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 0.52 </td>
   <td style="text-align:right;"> 0.44 </td>
   <td style="text-align:right;"> 0.45 </td>
   <td style="text-align:right;"> 0.49 </td>
   <td style="text-align:right;"> 0.49 </td>
   <td style="text-align:right;"> -0.41 </td>
   <td style="text-align:right;"> -0.35 </td>
   <td style="text-align:right;"> 0.38 </td>
   <td style="text-align:right;"> 0.34 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc08 </td>
   <td style="text-align:right;"> -0.37 </td>
   <td style="text-align:right;"> -0.41 </td>
   <td style="text-align:right;"> -0.33 </td>
   <td style="text-align:right;"> 0.38 </td>
   <td style="text-align:right;"> -0.30 </td>
   <td style="text-align:right;"> 0.20 </td>
   <td style="text-align:right;"> 0.52 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 0.45 </td>
   <td style="text-align:right;"> 0.40 </td>
   <td style="text-align:right;"> 0.51 </td>
   <td style="text-align:right;"> 0.37 </td>
   <td style="text-align:right;"> -0.35 </td>
   <td style="text-align:right;"> -0.33 </td>
   <td style="text-align:right;"> 0.30 </td>
   <td style="text-align:right;"> 0.30 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc09 </td>
   <td style="text-align:right;"> -0.33 </td>
   <td style="text-align:right;"> -0.30 </td>
   <td style="text-align:right;"> -0.18 </td>
   <td style="text-align:right;"> 0.33 </td>
   <td style="text-align:right;"> -0.29 </td>
   <td style="text-align:right;"> 0.22 </td>
   <td style="text-align:right;"> 0.44 </td>
   <td style="text-align:right;"> 0.45 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 0.40 </td>
   <td style="text-align:right;"> 0.49 </td>
   <td style="text-align:right;"> 0.45 </td>
   <td style="text-align:right;"> -0.25 </td>
   <td style="text-align:right;"> -0.21 </td>
   <td style="text-align:right;"> 0.30 </td>
   <td style="text-align:right;"> 0.26 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc10 </td>
   <td style="text-align:right;"> -0.42 </td>
   <td style="text-align:right;"> -0.34 </td>
   <td style="text-align:right;"> -0.26 </td>
   <td style="text-align:right;"> 0.29 </td>
   <td style="text-align:right;"> -0.22 </td>
   <td style="text-align:right;"> 0.30 </td>
   <td style="text-align:right;"> 0.45 </td>
   <td style="text-align:right;"> 0.40 </td>
   <td style="text-align:right;"> 0.40 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 0.41 </td>
   <td style="text-align:right;"> 0.47 </td>
   <td style="text-align:right;"> -0.30 </td>
   <td style="text-align:right;"> -0.22 </td>
   <td style="text-align:right;"> 0.23 </td>
   <td style="text-align:right;"> 0.26 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc11 </td>
   <td style="text-align:right;"> -0.40 </td>
   <td style="text-align:right;"> -0.35 </td>
   <td style="text-align:right;"> -0.30 </td>
   <td style="text-align:right;"> 0.35 </td>
   <td style="text-align:right;"> -0.33 </td>
   <td style="text-align:right;"> 0.19 </td>
   <td style="text-align:right;"> 0.49 </td>
   <td style="text-align:right;"> 0.51 </td>
   <td style="text-align:right;"> 0.49 </td>
   <td style="text-align:right;"> 0.41 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 0.43 </td>
   <td style="text-align:right;"> -0.40 </td>
   <td style="text-align:right;"> -0.35 </td>
   <td style="text-align:right;"> 0.28 </td>
   <td style="text-align:right;"> 0.29 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc12 </td>
   <td style="text-align:right;"> -0.33 </td>
   <td style="text-align:right;"> -0.29 </td>
   <td style="text-align:right;"> -0.27 </td>
   <td style="text-align:right;"> 0.37 </td>
   <td style="text-align:right;"> -0.26 </td>
   <td style="text-align:right;"> 0.22 </td>
   <td style="text-align:right;"> 0.49 </td>
   <td style="text-align:right;"> 0.37 </td>
   <td style="text-align:right;"> 0.45 </td>
   <td style="text-align:right;"> 0.47 </td>
   <td style="text-align:right;"> 0.43 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> -0.30 </td>
   <td style="text-align:right;"> -0.20 </td>
   <td style="text-align:right;"> 0.30 </td>
   <td style="text-align:right;"> 0.28 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc13 </td>
   <td style="text-align:right;"> 0.48 </td>
   <td style="text-align:right;"> 0.43 </td>
   <td style="text-align:right;"> 0.42 </td>
   <td style="text-align:right;"> -0.24 </td>
   <td style="text-align:right;"> 0.32 </td>
   <td style="text-align:right;"> -0.19 </td>
   <td style="text-align:right;"> -0.41 </td>
   <td style="text-align:right;"> -0.35 </td>
   <td style="text-align:right;"> -0.25 </td>
   <td style="text-align:right;"> -0.30 </td>
   <td style="text-align:right;"> -0.40 </td>
   <td style="text-align:right;"> -0.30 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 0.60 </td>
   <td style="text-align:right;"> -0.23 </td>
   <td style="text-align:right;"> -0.25 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc14 </td>
   <td style="text-align:right;"> 0.37 </td>
   <td style="text-align:right;"> 0.43 </td>
   <td style="text-align:right;"> 0.37 </td>
   <td style="text-align:right;"> -0.21 </td>
   <td style="text-align:right;"> 0.26 </td>
   <td style="text-align:right;"> -0.13 </td>
   <td style="text-align:right;"> -0.35 </td>
   <td style="text-align:right;"> -0.33 </td>
   <td style="text-align:right;"> -0.21 </td>
   <td style="text-align:right;"> -0.22 </td>
   <td style="text-align:right;"> -0.35 </td>
   <td style="text-align:right;"> -0.20 </td>
   <td style="text-align:right;"> 0.60 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> -0.21 </td>
   <td style="text-align:right;"> -0.20 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc15 </td>
   <td style="text-align:right;"> -0.32 </td>
   <td style="text-align:right;"> -0.34 </td>
   <td style="text-align:right;"> -0.25 </td>
   <td style="text-align:right;"> 0.29 </td>
   <td style="text-align:right;"> -0.29 </td>
   <td style="text-align:right;"> 0.15 </td>
   <td style="text-align:right;"> 0.38 </td>
   <td style="text-align:right;"> 0.30 </td>
   <td style="text-align:right;"> 0.30 </td>
   <td style="text-align:right;"> 0.23 </td>
   <td style="text-align:right;"> 0.28 </td>
   <td style="text-align:right;"> 0.30 </td>
   <td style="text-align:right;"> -0.23 </td>
   <td style="text-align:right;"> -0.21 </td>
   <td style="text-align:right;"> 1.00 </td>
   <td style="text-align:right;"> 0.75 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc16 </td>
   <td style="text-align:right;"> -0.36 </td>
   <td style="text-align:right;"> -0.34 </td>
   <td style="text-align:right;"> -0.21 </td>
   <td style="text-align:right;"> 0.22 </td>
   <td style="text-align:right;"> -0.22 </td>
   <td style="text-align:right;"> 0.19 </td>
   <td style="text-align:right;"> 0.34 </td>
   <td style="text-align:right;"> 0.30 </td>
   <td style="text-align:right;"> 0.26 </td>
   <td style="text-align:right;"> 0.26 </td>
   <td style="text-align:right;"> 0.29 </td>
   <td style="text-align:right;"> 0.28 </td>
   <td style="text-align:right;"> -0.25 </td>
   <td style="text-align:right;"> -0.20 </td>
   <td style="text-align:right;"> 0.75 </td>
   <td style="text-align:right;"> 1.00 </td>
  </tr>
</tbody>
</table>

Maddeler arasındaki ilişkileri değerlendirmek için ggcorrplot paketini [@ggcorrplot] kullanarak bir korelasyon matrisi grafiği oluşturacağız. psych’den `corPlot()` kullanılarak da grafik oluşturualbilir.


```r
ggcorrplot::ggcorrplot(
  corr = cormat, # korelasyon matirisi
 type = "lower", # alt kösegen
 show.diag = TRUE, # kosegen
 lab = TRUE, # degerleri ekleme
 lab_size = 3) #
```

<img src="15_KTK_files/figure-html/unnamed-chunk-19-1.png" width="100%" style="display: block; margin: auto;" />

Ölçekteki birkaç maddenin (mavi/mor renkli kutulara bakınız) diğer maddelerle negatif korelasyona sahip olduğunu görüyoruz. Bunlar NFC Ölçeğindeki (R) işaretli maddelerdir (yani, negatif olarak ifade edilmiş maddeler). Tüm maddeleri aynı yöne koymak için bu maddelere verilen yanıtları ters kodlayacağız (yani, 1=tamamen katılıyorum ile 7=tamamen katılmıyorum). Bu işlem için psych’in `reverse.code()` fonksiyonunu kullanacağız.


```r
# ters kodlacak maddeler -1 ile belirtilmiştir.

nfc_key <- c(1,1,1,-1,1,-1,-1,-1,-1,-1,-1,-1,1,1,-1,-1)

ters_matris <- psych::reverse.code(
 keys = nfc_key, # ters kodlanacak maddeler
 items = matris, # veri
 mini = 1, # minumum deger
 maxi = 7) # maksimum deger
```


```r
cor_ters_matris <-
 psych::polychoric(ters_matris)$rho
 ggcorrplot::ggcorrplot(
 corr = cor_ters_matris,
 type = "lower",
 show.diag = TRUE,
 lab = TRUE,
 lab_size = 3) 
```

<img src="15_KTK_files/figure-html/unnamed-chunk-21-1.png" width="100%" style="display: block; margin: auto;" />


```r
# yeniden adlandırma
 colnames(ters_matris) <- colnames(matris)
 ters_matris <- as.data.frame(ters_matris)
 head(ters_matris)
```

<div class="kable-table">

| nfc01| nfc02| nfc03| nfc04| nfc05| nfc06| nfc07| nfc08| nfc09| nfc10| nfc11| nfc12| nfc13| nfc14| nfc15| nfc16|
|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|
|     5|     7|     5|     7|     6|     6|     6|     3|     6|     7|     6|     6|     6|     5|     6|     7|
|     5|     5|     3|     6|     5|     5|     5|     5|     6|     4|     5|     6|     3|     2|     4|     4|
|     5|     6|     5|     7|     7|     5|     6|     5|     7|     5|     5|     7|     6|     5|     7|     7|
|     5|     5|     4|     6|     5|     3|     6|     6|     6|     6|     4|     5|     2|     3|     7|     7|
|     2|     3|     3|     3|     3|     3|     2|     7|     2|     2|     2|     2|     2|     1|     3|     3|
|     6|     6|     6|     7|     6|     5|     7|     7|     7|     7|     7|     7|     6|     6|     6|     6|

</div>


```r
library("OpenMx")
 means_cor <-
 mean(vechs(cor_ters_matris))
 library(qgraph)
 qgraph(cor_ters_matris,
 cut=0, layout="spring",
 title=paste("Korelasyon matrisi,
 ortalama korelasyon = ",
 round(means_cor, digits=2),
 sep=" "))
```

<img src="15_KTK_files/figure-html/unnamed-chunk-23-1.png" width="100%" style="display: block; margin: auto;" />


```r
library(EGAnet); library(psychTools)

# Perform Unique Variable Analysis
EGA(matris)
```

<img src="15_KTK_files/figure-html/unnamed-chunk-24-1.png" width="100%" style="display: block; margin: auto;" />

```
## Model: GLASSO (EBIC with gamma = 0.5)
## Correlations: auto
## Lambda: 0.0753057457698569 (n = 100, ratio = 0.1)
## 
## Number of nodes: 16
## Number of edges: 75
## Edge density: 0.625
## 
## Non-zero edge weights: 
##      M    SD    Min   Max
##  0.049 0.119 -0.153 0.626
## 
## ----
## 
## Algorithm:  Walktrap
## 
## Number of communities:  3
## 
## nfc01 nfc02 nfc03 nfc04 nfc05 nfc06 nfc07 nfc08 nfc09 nfc10 nfc11 nfc12 nfc13 
##     1     1     1     2     1     2     2     2     2     2     2     2     1 
## nfc14 nfc15 nfc16 
##     1     3     3 
## 
## ----
## 
## Unidimensional Method: Louvain
## Unidimensional: No
## 
## ----
## 
## TEFI: -10.355
```

## Madde analizi

her bir madde için betimleyici istatistikler (örn. ortalama, standart sapma, minimum ve maksimum), madde düzeyinde istatistikler (örn. güçlük ve ayırt edicilik) ve bunların ölçek düzeyinde istatistiklerle (örn. güvenilirlik) ilişkileri R’de, ikili (yani, 0 veya 1) ve sıralı (örneğin, Likert ölçeği maddeleri) maddeler üzerinde madde analizi yapmak için çeşitli paketler bulunmaktadır.


```r
itemanalysis_ctt <- CTT::itemAnalysis(items = ters_matris, pBisFlag = .2,
 bisFlag = .2)
 itemanalysis_ctt$itemReport %>% kbl(digits = 2) 
```

<table>
 <thead>
  <tr>
   <th style="text-align:left;"> itemName </th>
   <th style="text-align:right;"> itemMean </th>
   <th style="text-align:right;"> pBis </th>
   <th style="text-align:right;"> bis </th>
   <th style="text-align:right;"> alphaIfDeleted </th>
   <th style="text-align:left;"> lowPBis </th>
   <th style="text-align:left;"> lowBis </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> nfc01 </td>
   <td style="text-align:right;"> 5.54 </td>
   <td style="text-align:right;"> 0.58 </td>
   <td style="text-align:right;"> 0.63 </td>
   <td style="text-align:right;"> 0.86 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc02 </td>
   <td style="text-align:right;"> 4.94 </td>
   <td style="text-align:right;"> 0.60 </td>
   <td style="text-align:right;"> 0.62 </td>
   <td style="text-align:right;"> 0.86 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc03 </td>
   <td style="text-align:right;"> 4.44 </td>
   <td style="text-align:right;"> 0.51 </td>
   <td style="text-align:right;"> 0.53 </td>
   <td style="text-align:right;"> 0.86 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc04 </td>
   <td style="text-align:right;"> 5.53 </td>
   <td style="text-align:right;"> 0.43 </td>
   <td style="text-align:right;"> 0.47 </td>
   <td style="text-align:right;"> 0.87 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc05 </td>
   <td style="text-align:right;"> 5.82 </td>
   <td style="text-align:right;"> 0.47 </td>
   <td style="text-align:right;"> 0.51 </td>
   <td style="text-align:right;"> 0.86 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc06 </td>
   <td style="text-align:right;"> 4.25 </td>
   <td style="text-align:right;"> 0.32 </td>
   <td style="text-align:right;"> 0.33 </td>
   <td style="text-align:right;"> 0.87 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc07 </td>
   <td style="text-align:right;"> 5.30 </td>
   <td style="text-align:right;"> 0.66 </td>
   <td style="text-align:right;"> 0.70 </td>
   <td style="text-align:right;"> 0.86 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc08 </td>
   <td style="text-align:right;"> 4.66 </td>
   <td style="text-align:right;"> 0.57 </td>
   <td style="text-align:right;"> 0.59 </td>
   <td style="text-align:right;"> 0.86 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc09 </td>
   <td style="text-align:right;"> 5.55 </td>
   <td style="text-align:right;"> 0.49 </td>
   <td style="text-align:right;"> 0.53 </td>
   <td style="text-align:right;"> 0.86 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc10 </td>
   <td style="text-align:right;"> 4.84 </td>
   <td style="text-align:right;"> 0.50 </td>
   <td style="text-align:right;"> 0.53 </td>
   <td style="text-align:right;"> 0.86 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc11 </td>
   <td style="text-align:right;"> 5.25 </td>
   <td style="text-align:right;"> 0.58 </td>
   <td style="text-align:right;"> 0.61 </td>
   <td style="text-align:right;"> 0.86 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc12 </td>
   <td style="text-align:right;"> 5.55 </td>
   <td style="text-align:right;"> 0.51 </td>
   <td style="text-align:right;"> 0.55 </td>
   <td style="text-align:right;"> 0.86 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc13 </td>
   <td style="text-align:right;"> 4.10 </td>
   <td style="text-align:right;"> 0.54 </td>
   <td style="text-align:right;"> 0.55 </td>
   <td style="text-align:right;"> 0.86 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc14 </td>
   <td style="text-align:right;"> 3.57 </td>
   <td style="text-align:right;"> 0.46 </td>
   <td style="text-align:right;"> 0.47 </td>
   <td style="text-align:right;"> 0.87 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc15 </td>
   <td style="text-align:right;"> 5.75 </td>
   <td style="text-align:right;"> 0.47 </td>
   <td style="text-align:right;"> 0.52 </td>
   <td style="text-align:right;"> 0.86 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> nfc16 </td>
   <td style="text-align:right;"> 5.52 </td>
   <td style="text-align:right;"> 0.46 </td>
   <td style="text-align:right;"> 0.49 </td>
   <td style="text-align:right;"> 0.87 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
</tbody>
</table>

### psych (Revelle 2021): alpha()































































































