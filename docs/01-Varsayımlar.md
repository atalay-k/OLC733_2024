# Varsayımlar


-   Veri Dosyasındaki Verinin Doğruluğu

-   Kayıp Verinin Miktarı ve Dağılımı

-   Tek Değişkenli ve Çok Değişkenli Uç Değerler (Outliers)

-   Sayıltılar

-   Çoklu Bağlantı (Multicollinearity) ve Tekillik (Singularity)

## Veri İnceleme

-   Varsayımlar incelenirken ilk olarak yanlış girilmiş bir değer olup
olmadığına bakılmalıdır.

-   Bu bölümde 🔗 [SCREEN.sav](data\SCREEN.sav) adlı veri seti
kullanılmıştır. Bu veri setinde 20-59 yaşları arasında 465 kadının 6 değişkene
ilişkin bilgileri bulunmaktadır. Değişkenlerden timedrs, attdrug, atthouse ve 
income değişkenleri sürekli, mstatus ve race değişkenleriyse iki kategorili değişkenlerdir.

-   Bu veri seti **Tabachnick, B. G., & Fidell, L. S. (2012). Using Multivariate
Statistics (4rd ed.). New York: Harper Collins.** kitabının 4. bölümünde 
kullanılmaktadır.

- Veri incelemede birden fazla paket kullanılabilir. En temel fonksiyon `base`
paketin `summary()` fonksiyonudur. `psych` paketinde `describe`; 
`gtsummary` paketinde `describe`;`vtable` paketinde `sumtable` 
fonksiyonu da aynı amaçla kullanılabilir.
 



```r
library(haven)
screen <- read_sav("data/SCREEN.sav")
head(screen)
```

<div class="kable-table">

| subno| timedrs| attdrug| atthouse| income| mstatus| race|
|-----:|-------:|-------:|--------:|------:|-------:|----:|
|     1|       1|       8|       27|      5|       2|    1|
|     2|       3|       7|       20|      6|       2|    1|
|     3|       0|       8|       23|      3|       2|    1|
|     4|      13|       9|       28|      8|       2|    1|
|     5|      15|       7|       24|      1|       2|    1|
|     6|       3|       8|       25|      4|       2|    1|

</div>

-   Veri setindeki maksimum ve minumum değerleri belirlenmiştir.


```r
summary(screen)
```

```
##      subno          timedrs          attdrug          atthouse    
##  Min.   :  1.0   Min.   : 0.000   Min.   : 5.000   Min.   : 2.00  
##  1st Qu.:137.0   1st Qu.: 2.000   1st Qu.: 7.000   1st Qu.:21.00  
##  Median :314.0   Median : 4.000   Median : 8.000   Median :24.00  
##  Mean   :317.4   Mean   : 7.901   Mean   : 7.686   Mean   :23.54  
##  3rd Qu.:483.0   3rd Qu.:10.000   3rd Qu.: 9.000   3rd Qu.:27.00  
##  Max.   :758.0   Max.   :81.000   Max.   :10.000   Max.   :35.00  
##                                                    NA's   :1      
##      income         mstatus           race      
##  Min.   : 1.00   Min.   :1.000   Min.   :1.000  
##  1st Qu.: 2.50   1st Qu.:2.000   1st Qu.:1.000  
##  Median : 4.00   Median :2.000   Median :1.000  
##  Mean   : 4.21   Mean   :1.778   Mean   :1.088  
##  3rd Qu.: 6.00   3rd Qu.:2.000   3rd Qu.:1.000  
##  Max.   :10.00   Max.   :2.000   Max.   :2.000  
##  NA's   :26
```

-   Elde edilen değerlerin makul olduğu söylenebilir. Ancak bunu elde etmek için başka yollar da bulunmaktadır.  `psych` paketi ile inceleme daha ayrıntılı yapılabilir.


```r
library(psych)
```

```
## 
## Attaching package: 'psych'
```

```
## The following objects are masked from 'package:ggplot2':
## 
##     %+%, alpha
```

```r
describe(screen[,-1])
```

<div class="kable-table">

|         | vars|   n|      mean|         sd| median|   trimmed|    mad| min| max| range|       skew|   kurtosis|        se|
|:--------|----:|---:|---------:|----------:|------:|---------:|------:|---:|---:|-----:|----------:|----------:|---------:|
|timedrs  |    1| 465|  7.901075| 10.9484932|      4|  5.605898| 4.4478|   0|  81|    81|  3.2271914| 12.8786814| 0.5077242|
|attdrug  |    2| 465|  7.686021|  1.1560925|      8|  7.707775| 1.4826|   5|  10|     5| -0.1217206| -0.4660855| 0.0536125|
|atthouse |    3| 464| 23.540948|  4.4835244|     24| 23.623656| 4.4478|   2|  35|    33| -0.4542073|  1.5067335| 0.2081424|
|income   |    4| 439|  4.209567|  2.4188755|      4|  4.014164| 2.9652|   1|  10|     9|  0.5776184| -0.3808944| 0.1154466|
|mstatus  |    5| 465|  1.778495|  0.4157071|      2|  1.847185| 0.0000|   1|   2|     1| -1.3369785| -0.2129327| 0.0192780|
|race     |    6| 465|  1.088172|  0.2838503|      1|  1.000000| 0.0000|   1|   2|     1|  2.8954859|  6.3976109| 0.0131632|

</div>

🔗 [personality-project sayfasını](https://personality-project.org/r/psych/) daha fazla örnek için inceleyebilirsiniz.

-   `gtsummary` paketi ile inceleme


```r
library(gtsummary)
screen %>% select(2:6) %>%tbl_summary(statistic = all_continuous() ~ c(
"{min}, {max}"),missing ="always")
```

```{=html}
<div id="fvebvhlmwm" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>#fvebvhlmwm table {
  font-family: system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol', 'Noto Color Emoji';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#fvebvhlmwm thead, #fvebvhlmwm tbody, #fvebvhlmwm tfoot, #fvebvhlmwm tr, #fvebvhlmwm td, #fvebvhlmwm th {
  border-style: none;
}

#fvebvhlmwm p {
  margin: 0;
  padding: 0;
}

#fvebvhlmwm .gt_table {
  display: table;
  border-collapse: collapse;
  line-height: normal;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#fvebvhlmwm .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#fvebvhlmwm .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#fvebvhlmwm .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 3px;
  padding-bottom: 5px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#fvebvhlmwm .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#fvebvhlmwm .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#fvebvhlmwm .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#fvebvhlmwm .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#fvebvhlmwm .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#fvebvhlmwm .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#fvebvhlmwm .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#fvebvhlmwm .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#fvebvhlmwm .gt_spanner_row {
  border-bottom-style: hidden;
}

#fvebvhlmwm .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}

#fvebvhlmwm .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#fvebvhlmwm .gt_from_md > :first-child {
  margin-top: 0;
}

#fvebvhlmwm .gt_from_md > :last-child {
  margin-bottom: 0;
}

#fvebvhlmwm .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#fvebvhlmwm .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#fvebvhlmwm .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#fvebvhlmwm .gt_row_group_first td {
  border-top-width: 2px;
}

#fvebvhlmwm .gt_row_group_first th {
  border-top-width: 2px;
}

#fvebvhlmwm .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#fvebvhlmwm .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#fvebvhlmwm .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#fvebvhlmwm .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#fvebvhlmwm .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#fvebvhlmwm .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#fvebvhlmwm .gt_last_grand_summary_row_top {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: double;
  border-bottom-width: 6px;
  border-bottom-color: #D3D3D3;
}

#fvebvhlmwm .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#fvebvhlmwm .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#fvebvhlmwm .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#fvebvhlmwm .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#fvebvhlmwm .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#fvebvhlmwm .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#fvebvhlmwm .gt_left {
  text-align: left;
}

#fvebvhlmwm .gt_center {
  text-align: center;
}

#fvebvhlmwm .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#fvebvhlmwm .gt_font_normal {
  font-weight: normal;
}

#fvebvhlmwm .gt_font_bold {
  font-weight: bold;
}

#fvebvhlmwm .gt_font_italic {
  font-style: italic;
}

#fvebvhlmwm .gt_super {
  font-size: 65%;
}

#fvebvhlmwm .gt_footnote_marks {
  font-size: 75%;
  vertical-align: 0.4em;
  position: initial;
}

#fvebvhlmwm .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#fvebvhlmwm .gt_indent_1 {
  text-indent: 5px;
}

#fvebvhlmwm .gt_indent_2 {
  text-indent: 10px;
}

#fvebvhlmwm .gt_indent_3 {
  text-indent: 15px;
}

#fvebvhlmwm .gt_indent_4 {
  text-indent: 20px;
}

#fvebvhlmwm .gt_indent_5 {
  text-indent: 25px;
}
</style>
<table class="gt_table" data-quarto-disable-processing="false" data-quarto-bootstrap="false">
  <thead>
    <tr class="gt_col_headings">
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1" scope="col" id="&lt;strong&gt;Characteristic&lt;/strong&gt;"><strong>Characteristic</strong></th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="&lt;strong&gt;N = 465&lt;/strong&gt;&lt;span class=&quot;gt_footnote_marks&quot; style=&quot;white-space:nowrap;font-style:italic;font-weight:normal;&quot;&gt;&lt;sup&gt;1&lt;/sup&gt;&lt;/span&gt;"><strong>N = 465</strong><span class="gt_footnote_marks" style="white-space:nowrap;font-style:italic;font-weight:normal;"><sup>1</sup></span></th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="label" class="gt_row gt_left">Visits to health professionals</td>
<td headers="stat_0" class="gt_row gt_center">0, 81</td></tr>
    <tr><td headers="label" class="gt_row gt_left">    Unknown</td>
<td headers="stat_0" class="gt_row gt_center">0</td></tr>
    <tr><td headers="label" class="gt_row gt_left">Attitudes toward medication</td>
<td headers="stat_0" class="gt_row gt_center"><br /></td></tr>
    <tr><td headers="label" class="gt_row gt_left">    5</td>
<td headers="stat_0" class="gt_row gt_center">13 (2.8%)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">    6</td>
<td headers="stat_0" class="gt_row gt_center">60 (13%)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">    7</td>
<td headers="stat_0" class="gt_row gt_center">126 (27%)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">    8</td>
<td headers="stat_0" class="gt_row gt_center">149 (32%)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">    9</td>
<td headers="stat_0" class="gt_row gt_center">95 (20%)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">    10</td>
<td headers="stat_0" class="gt_row gt_center">22 (4.7%)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">    Unknown</td>
<td headers="stat_0" class="gt_row gt_center">0</td></tr>
    <tr><td headers="label" class="gt_row gt_left">Attitudes toward housework</td>
<td headers="stat_0" class="gt_row gt_center">2.0, 35.0</td></tr>
    <tr><td headers="label" class="gt_row gt_left">    Unknown</td>
<td headers="stat_0" class="gt_row gt_center">1</td></tr>
    <tr><td headers="label" class="gt_row gt_left">Income</td>
<td headers="stat_0" class="gt_row gt_center">1.00, 10.00</td></tr>
    <tr><td headers="label" class="gt_row gt_left">    Unknown</td>
<td headers="stat_0" class="gt_row gt_center">26</td></tr>
    <tr><td headers="label" class="gt_row gt_left">Whether currently married</td>
<td headers="stat_0" class="gt_row gt_center"><br /></td></tr>
    <tr><td headers="label" class="gt_row gt_left">    1</td>
<td headers="stat_0" class="gt_row gt_center">103 (22%)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">    2</td>
<td headers="stat_0" class="gt_row gt_center">362 (78%)</td></tr>
    <tr><td headers="label" class="gt_row gt_left">    Unknown</td>
<td headers="stat_0" class="gt_row gt_center">0</td></tr>
  </tbody>
  
  <tfoot class="gt_footnotes">
    <tr>
      <td class="gt_footnote" colspan="2"><span class="gt_footnote_marks" style="white-space:nowrap;font-style:italic;font-weight:normal;"><sup>1</sup></span> Range; n (%)</td>
    </tr>
  </tfoot>
</table>
</div>
```

-   🔗[Presentation-Ready Summary Tables] with gtsummary(https://education.rstudio.com/blog/2020/07/gtsummary)

-   `vtable` paketi ile inceleme


```r
library(vtable)
```

```
## Loading required package: kableExtra
```

```
## 
## Attaching package: 'kableExtra'
```

```
## The following object is masked from 'package:dplyr':
## 
##     group_rows
```

```r
sumtable(screen, summ=c('notNA(x)','min(x)','max(x)'))
```

<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:unnamed-chunk-5)Summary Statistics</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Variable </th>
   <th style="text-align:left;"> NotNA </th>
   <th style="text-align:left;"> Min </th>
   <th style="text-align:left;"> Max </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> subno </td>
   <td style="text-align:left;"> 465 </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> 758 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> timedrs </td>
   <td style="text-align:left;"> 465 </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 81 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> attdrug </td>
   <td style="text-align:left;"> 465 </td>
   <td style="text-align:left;"> 5 </td>
   <td style="text-align:left;"> 10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> atthouse </td>
   <td style="text-align:left;"> 464 </td>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:left;"> 35 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> income </td>
   <td style="text-align:left;"> 439 </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> 10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> mstatus </td>
   <td style="text-align:left;"> 465 </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> race </td>
   <td style="text-align:left;"> 465 </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> 2 </td>
  </tr>
</tbody>
</table>

-   🔗 [vtable paketi için örnekler](https://nickch-k.github.io/vtable/index.html)

-   sütun isimleri aşağıdaki gibi değiştirilebilir.


```r
sumtable(screen, summ = c('notNA(x)','min(x)','max(x)'),
         summ.names = c('Frekans'
,'Minimum','Maximum'))
```

<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:unnamed-chunk-6)Summary Statistics</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Variable </th>
   <th style="text-align:left;"> Frekans </th>
   <th style="text-align:left;"> Minimum </th>
   <th style="text-align:left;"> Maximum </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> subno </td>
   <td style="text-align:left;"> 465 </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> 758 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> timedrs </td>
   <td style="text-align:left;"> 465 </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 81 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> attdrug </td>
   <td style="text-align:left;"> 465 </td>
   <td style="text-align:left;"> 5 </td>
   <td style="text-align:left;"> 10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> atthouse </td>
   <td style="text-align:left;"> 464 </td>
   <td style="text-align:left;"> 2 </td>
   <td style="text-align:left;"> 35 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> income </td>
   <td style="text-align:left;"> 439 </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> 10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> mstatus </td>
   <td style="text-align:left;"> 465 </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> race </td>
   <td style="text-align:left;"> 465 </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> 2 </td>
  </tr>
</tbody>
</table>

-   `kable` paketi ile `psych` paketi çıktılarını düzenleme


```r
ozet <- describe(screen[,-1])
kable(ozet,format='markdown',
      caption="Betimsel İstatistikler",digits=2)
```



Table: (\#tab:unnamed-chunk-7)Betimsel İstatistikler

|         | vars|   n|  mean|    sd| median| trimmed|  mad| min| max| range|  skew| kurtosis|   se|
|:--------|----:|---:|-----:|-----:|------:|-------:|----:|---:|---:|-----:|-----:|--------:|----:|
|timedrs  |    1| 465|  7.90| 10.95|      4|    5.61| 4.45|   0|  81|    81|  3.23|    12.88| 0.51|
|attdrug  |    2| 465|  7.69|  1.16|      8|    7.71| 1.48|   5|  10|     5| -0.12|    -0.47| 0.05|
|atthouse |    3| 464| 23.54|  4.48|     24|   23.62| 4.45|   2|  35|    33| -0.45|     1.51| 0.21|
|income   |    4| 439|  4.21|  2.42|      4|    4.01| 2.97|   1|  10|     9|  0.58|    -0.38| 0.12|
|mstatus  |    5| 465|  1.78|  0.42|      2|    1.85| 0.00|   1|   2|     1| -1.34|    -0.21| 0.02|
|race     |    6| 465|  1.09|  0.28|      1|    1.00| 0.00|   1|   2|     1|  2.90|     6.40| 0.01|

-   🔗 [rmarkdown-cookbook](https://bookdown.org/yihui/rmarkdown-cookbook/kable.html)


## Kayıp Değerler

- Kayıp veri, veri analizindeki en yaygın problemlerden biridir.

- Kayıp verinin önemi kayıp verinin miktarına, örüntüsüne ve neden eksik olduğuna
bağlıdır.

- Bir değişkene ait beklenmeyen miktarda kayıp veri varsa, ilk olarak 
bununnedeni araştırılmalıdır. Daha sonra kayıp verinin örüntüsüne bakılarak,rastlantısal mı
yoksa sistematik bir örüntü mü gösterdiği belirlenmelidir.

    - Örneğin, 30 yaşın üstündeki birçok kadın yaş ile ilgili soruyu
cevaplamak istemezler.

- Genellikle kayıp verinin örüntüsü miktarından daha önemlidir. Rastlantısal 
dağılmayan kayıp veriler sonuçların genellenebilirliğini 
etkileyeceğinden miktarları az da olsa,
rastlantısal dağılan kayıp verilere oranla daha ciddi problemlere yol açarlar.

### Kayıp Veri Türleri

- Kayıp veri türleri arasındaki ayrım 1976 yılında Rubin tarafından yapılmıştır.
Rubin (1976) kayıp veriyi aşağıdaki şekilde sınıfl andırmıştır.

- Tamamen Rastlantısal Olarak Kayıp (TROK) - Missing Completely at Random MCAR

- Rastlantısal Olarak Kayıp (ROK) -  Missing at R andom (MAR)

- Rastlantısal Olmayarak Kayıp / İhmal Edilemez Kayıp (İEK) - Not Missing at Random
(NMAR)

- Kayıp veri en azından MAR türünde değilse, kayıp verinin
ihmal edilemeyeceği söylenir.Bu türdeki kayıp veri
rastlantısal olamyan kayıp veya ihmal edilemez kayıp olarak adlandırılır.

- Büyük bir veri setinde, verinin %5’i veya daha azı rastlantısal olarak kayıpsa
çok ciddi problemlerle karşılaşılmaz ve kayıp veri ile ilgili problemleri
çözmek için kullanılan herhangi bir yöntem benzer sonuçlar verir.
Halbuki küçük veya orta büyüklükteki bir veri setinde çok sayıda veri kaybı
varsa ciddi problemler ortaya çıkabilir.

- Eldeki bilgiden yararlanarak kayıp verideki örüntüler test edilebilir.

- Örneğin, kayıp verinin bulunduğu değişkene göre eksik değerlere sahip
bireyler ve tam değerlere sahip bireylerden iki grup oluşturulabilir. Sonra
analizde bu değişkenle ilgili olabilecek diğer değişkenlerde t testi ile iki
grup arasındaki ortalama farklara bakılabilir


### MAR

- MAR türünde veri gerçekte rastlantısal olarak kayıp değildir, veri kaybı veri
setindeki değişkenlerden bazılarına bağlıdır.

- MAR türünde bir veri noktasının kayıp olma eğilimi kayıp veriyle ilişkili
değildir ancak gözlenen verinin bir kısmıyla İlişkilidir.

- Rastlantısal olarak kayıp değerler ve gözlenen değerler arasında sistematik
farklılıkların olabileceği ancak bu farklılıkların diğer gözlenen değişkenlerle
tamamen açıklanabileceği anlamındadır.

- Bir değişkenin gözlemleri rastlantısal olarak kayıpsa, şartlı değişkenler
kontrol edilebilirse , rastlantısal küme elde edilebilir; kayıp ve gözlenen
değerler kontrol altına alınan gruplarda benzer dağılımlara sahip
olacaklardır.


- Kayıp veriyi incelemek ve kayıp veri ile baş etmek konusunda birden fazla
paket mevcuttur. Bu paketler arasında
- VIM
- missMethods
- Amelia
- naniar paketi sayılabilir.

Kayıp veriyi incelemek ve kayıp veri ile başetmek konusunda birden fazla paket mevcuttur. Bu paketler arasında **VIM**,**missMethods**,**Amelia**, **naniar** paketi sayılabilir. İlk örnekler **naniar** üzerinden gösterilmektedir.

- **herhangi bir eksik veri olup olmadığının kontrolu**


```r
library(naniar)

any_na(screen)
```

```
## [1] TRUE
```

- **toplam kaç eksik veri var**


```r
n_miss(screen)
```

```
## [1] 27
```

**eksik veri oranı ne?**


```r
prop_miss(screen)
```

```
## [1] 0.008294931
```

- **eksik veriler hangi sütunlarda**


```r
screen %>% is.na() %>% colSums()
```

```
##    subno  timedrs  attdrug atthouse   income  mstatus     race 
##        0        0        0        1       26        0        0
```

- **eksik veri tablosu, frekans ve oran**


```r
miss_var_summary(screen)
```

<div class="kable-table">

|variable | n_miss|  pct_miss|
|:--------|------:|---------:|
|income   |     26| 5.5913978|
|atthouse |      1| 0.2150538|
|subno    |      0| 0.0000000|
|timedrs  |      0| 0.0000000|
|attdrug  |      0| 0.0000000|
|mstatus  |      0| 0.0000000|
|race     |      0| 0.0000000|

</div>

- **değişkenlere göre eksik veri tablosu**


```r
miss_var_table(screen)
```

<div class="kable-table">

| n_miss_in_var| n_vars| pct_vars|
|-------------:|------:|--------:|
|             0|      5| 71.42857|
|             1|      1| 14.28571|
|            26|      1| 14.28571|

</div>

- **Hangi bireylerde/satırlarda eksik veri var**


```r
miss_case_summary(screen)
```

<div class="kable-table">

| case| n_miss| pct_miss|
|----:|------:|--------:|
|   52|      1| 14.28571|
|   64|      1| 14.28571|
|   69|      1| 14.28571|
|   77|      1| 14.28571|
|  118|      1| 14.28571|
|  135|      1| 14.28571|
|  161|      1| 14.28571|
|  172|      1| 14.28571|
|  173|      1| 14.28571|
|  174|      1| 14.28571|
|  181|      1| 14.28571|
|  196|      1| 14.28571|
|  203|      1| 14.28571|
|  236|      1| 14.28571|
|  240|      1| 14.28571|
|  253|      1| 14.28571|
|  258|      1| 14.28571|
|  304|      1| 14.28571|
|  321|      1| 14.28571|
|  325|      1| 14.28571|
|  352|      1| 14.28571|
|  378|      1| 14.28571|
|  379|      1| 14.28571|
|  409|      1| 14.28571|
|  419|      1| 14.28571|
|  421|      1| 14.28571|
|  435|      1| 14.28571|
|    1|      0|  0.00000|
|    2|      0|  0.00000|
|    3|      0|  0.00000|
|    4|      0|  0.00000|
|    5|      0|  0.00000|
|    6|      0|  0.00000|
|    7|      0|  0.00000|
|    8|      0|  0.00000|
|    9|      0|  0.00000|
|   10|      0|  0.00000|
|   11|      0|  0.00000|
|   12|      0|  0.00000|
|   13|      0|  0.00000|
|   14|      0|  0.00000|
|   15|      0|  0.00000|
|   16|      0|  0.00000|
|   17|      0|  0.00000|
|   18|      0|  0.00000|
|   19|      0|  0.00000|
|   20|      0|  0.00000|
|   21|      0|  0.00000|
|   22|      0|  0.00000|
|   23|      0|  0.00000|
|   24|      0|  0.00000|
|   25|      0|  0.00000|
|   26|      0|  0.00000|
|   27|      0|  0.00000|
|   28|      0|  0.00000|
|   29|      0|  0.00000|
|   30|      0|  0.00000|
|   31|      0|  0.00000|
|   32|      0|  0.00000|
|   33|      0|  0.00000|
|   34|      0|  0.00000|
|   35|      0|  0.00000|
|   36|      0|  0.00000|
|   37|      0|  0.00000|
|   38|      0|  0.00000|
|   39|      0|  0.00000|
|   40|      0|  0.00000|
|   41|      0|  0.00000|
|   42|      0|  0.00000|
|   43|      0|  0.00000|
|   44|      0|  0.00000|
|   45|      0|  0.00000|
|   46|      0|  0.00000|
|   47|      0|  0.00000|
|   48|      0|  0.00000|
|   49|      0|  0.00000|
|   50|      0|  0.00000|
|   51|      0|  0.00000|
|   53|      0|  0.00000|
|   54|      0|  0.00000|
|   55|      0|  0.00000|
|   56|      0|  0.00000|
|   57|      0|  0.00000|
|   58|      0|  0.00000|
|   59|      0|  0.00000|
|   60|      0|  0.00000|
|   61|      0|  0.00000|
|   62|      0|  0.00000|
|   63|      0|  0.00000|
|   65|      0|  0.00000|
|   66|      0|  0.00000|
|   67|      0|  0.00000|
|   68|      0|  0.00000|
|   70|      0|  0.00000|
|   71|      0|  0.00000|
|   72|      0|  0.00000|
|   73|      0|  0.00000|
|   74|      0|  0.00000|
|   75|      0|  0.00000|
|   76|      0|  0.00000|
|   78|      0|  0.00000|
|   79|      0|  0.00000|
|   80|      0|  0.00000|
|   81|      0|  0.00000|
|   82|      0|  0.00000|
|   83|      0|  0.00000|
|   84|      0|  0.00000|
|   85|      0|  0.00000|
|   86|      0|  0.00000|
|   87|      0|  0.00000|
|   88|      0|  0.00000|
|   89|      0|  0.00000|
|   90|      0|  0.00000|
|   91|      0|  0.00000|
|   92|      0|  0.00000|
|   93|      0|  0.00000|
|   94|      0|  0.00000|
|   95|      0|  0.00000|
|   96|      0|  0.00000|
|   97|      0|  0.00000|
|   98|      0|  0.00000|
|   99|      0|  0.00000|
|  100|      0|  0.00000|
|  101|      0|  0.00000|
|  102|      0|  0.00000|
|  103|      0|  0.00000|
|  104|      0|  0.00000|
|  105|      0|  0.00000|
|  106|      0|  0.00000|
|  107|      0|  0.00000|
|  108|      0|  0.00000|
|  109|      0|  0.00000|
|  110|      0|  0.00000|
|  111|      0|  0.00000|
|  112|      0|  0.00000|
|  113|      0|  0.00000|
|  114|      0|  0.00000|
|  115|      0|  0.00000|
|  116|      0|  0.00000|
|  117|      0|  0.00000|
|  119|      0|  0.00000|
|  120|      0|  0.00000|
|  121|      0|  0.00000|
|  122|      0|  0.00000|
|  123|      0|  0.00000|
|  124|      0|  0.00000|
|  125|      0|  0.00000|
|  126|      0|  0.00000|
|  127|      0|  0.00000|
|  128|      0|  0.00000|
|  129|      0|  0.00000|
|  130|      0|  0.00000|
|  131|      0|  0.00000|
|  132|      0|  0.00000|
|  133|      0|  0.00000|
|  134|      0|  0.00000|
|  136|      0|  0.00000|
|  137|      0|  0.00000|
|  138|      0|  0.00000|
|  139|      0|  0.00000|
|  140|      0|  0.00000|
|  141|      0|  0.00000|
|  142|      0|  0.00000|
|  143|      0|  0.00000|
|  144|      0|  0.00000|
|  145|      0|  0.00000|
|  146|      0|  0.00000|
|  147|      0|  0.00000|
|  148|      0|  0.00000|
|  149|      0|  0.00000|
|  150|      0|  0.00000|
|  151|      0|  0.00000|
|  152|      0|  0.00000|
|  153|      0|  0.00000|
|  154|      0|  0.00000|
|  155|      0|  0.00000|
|  156|      0|  0.00000|
|  157|      0|  0.00000|
|  158|      0|  0.00000|
|  159|      0|  0.00000|
|  160|      0|  0.00000|
|  162|      0|  0.00000|
|  163|      0|  0.00000|
|  164|      0|  0.00000|
|  165|      0|  0.00000|
|  166|      0|  0.00000|
|  167|      0|  0.00000|
|  168|      0|  0.00000|
|  169|      0|  0.00000|
|  170|      0|  0.00000|
|  171|      0|  0.00000|
|  175|      0|  0.00000|
|  176|      0|  0.00000|
|  177|      0|  0.00000|
|  178|      0|  0.00000|
|  179|      0|  0.00000|
|  180|      0|  0.00000|
|  182|      0|  0.00000|
|  183|      0|  0.00000|
|  184|      0|  0.00000|
|  185|      0|  0.00000|
|  186|      0|  0.00000|
|  187|      0|  0.00000|
|  188|      0|  0.00000|
|  189|      0|  0.00000|
|  190|      0|  0.00000|
|  191|      0|  0.00000|
|  192|      0|  0.00000|
|  193|      0|  0.00000|
|  194|      0|  0.00000|
|  195|      0|  0.00000|
|  197|      0|  0.00000|
|  198|      0|  0.00000|
|  199|      0|  0.00000|
|  200|      0|  0.00000|
|  201|      0|  0.00000|
|  202|      0|  0.00000|
|  204|      0|  0.00000|
|  205|      0|  0.00000|
|  206|      0|  0.00000|
|  207|      0|  0.00000|
|  208|      0|  0.00000|
|  209|      0|  0.00000|
|  210|      0|  0.00000|
|  211|      0|  0.00000|
|  212|      0|  0.00000|
|  213|      0|  0.00000|
|  214|      0|  0.00000|
|  215|      0|  0.00000|
|  216|      0|  0.00000|
|  217|      0|  0.00000|
|  218|      0|  0.00000|
|  219|      0|  0.00000|
|  220|      0|  0.00000|
|  221|      0|  0.00000|
|  222|      0|  0.00000|
|  223|      0|  0.00000|
|  224|      0|  0.00000|
|  225|      0|  0.00000|
|  226|      0|  0.00000|
|  227|      0|  0.00000|
|  228|      0|  0.00000|
|  229|      0|  0.00000|
|  230|      0|  0.00000|
|  231|      0|  0.00000|
|  232|      0|  0.00000|
|  233|      0|  0.00000|
|  234|      0|  0.00000|
|  235|      0|  0.00000|
|  237|      0|  0.00000|
|  238|      0|  0.00000|
|  239|      0|  0.00000|
|  241|      0|  0.00000|
|  242|      0|  0.00000|
|  243|      0|  0.00000|
|  244|      0|  0.00000|
|  245|      0|  0.00000|
|  246|      0|  0.00000|
|  247|      0|  0.00000|
|  248|      0|  0.00000|
|  249|      0|  0.00000|
|  250|      0|  0.00000|
|  251|      0|  0.00000|
|  252|      0|  0.00000|
|  254|      0|  0.00000|
|  255|      0|  0.00000|
|  256|      0|  0.00000|
|  257|      0|  0.00000|
|  259|      0|  0.00000|
|  260|      0|  0.00000|
|  261|      0|  0.00000|
|  262|      0|  0.00000|
|  263|      0|  0.00000|
|  264|      0|  0.00000|
|  265|      0|  0.00000|
|  266|      0|  0.00000|
|  267|      0|  0.00000|
|  268|      0|  0.00000|
|  269|      0|  0.00000|
|  270|      0|  0.00000|
|  271|      0|  0.00000|
|  272|      0|  0.00000|
|  273|      0|  0.00000|
|  274|      0|  0.00000|
|  275|      0|  0.00000|
|  276|      0|  0.00000|
|  277|      0|  0.00000|
|  278|      0|  0.00000|
|  279|      0|  0.00000|
|  280|      0|  0.00000|
|  281|      0|  0.00000|
|  282|      0|  0.00000|
|  283|      0|  0.00000|
|  284|      0|  0.00000|
|  285|      0|  0.00000|
|  286|      0|  0.00000|
|  287|      0|  0.00000|
|  288|      0|  0.00000|
|  289|      0|  0.00000|
|  290|      0|  0.00000|
|  291|      0|  0.00000|
|  292|      0|  0.00000|
|  293|      0|  0.00000|
|  294|      0|  0.00000|
|  295|      0|  0.00000|
|  296|      0|  0.00000|
|  297|      0|  0.00000|
|  298|      0|  0.00000|
|  299|      0|  0.00000|
|  300|      0|  0.00000|
|  301|      0|  0.00000|
|  302|      0|  0.00000|
|  303|      0|  0.00000|
|  305|      0|  0.00000|
|  306|      0|  0.00000|
|  307|      0|  0.00000|
|  308|      0|  0.00000|
|  309|      0|  0.00000|
|  310|      0|  0.00000|
|  311|      0|  0.00000|
|  312|      0|  0.00000|
|  313|      0|  0.00000|
|  314|      0|  0.00000|
|  315|      0|  0.00000|
|  316|      0|  0.00000|
|  317|      0|  0.00000|
|  318|      0|  0.00000|
|  319|      0|  0.00000|
|  320|      0|  0.00000|
|  322|      0|  0.00000|
|  323|      0|  0.00000|
|  324|      0|  0.00000|
|  326|      0|  0.00000|
|  327|      0|  0.00000|
|  328|      0|  0.00000|
|  329|      0|  0.00000|
|  330|      0|  0.00000|
|  331|      0|  0.00000|
|  332|      0|  0.00000|
|  333|      0|  0.00000|
|  334|      0|  0.00000|
|  335|      0|  0.00000|
|  336|      0|  0.00000|
|  337|      0|  0.00000|
|  338|      0|  0.00000|
|  339|      0|  0.00000|
|  340|      0|  0.00000|
|  341|      0|  0.00000|
|  342|      0|  0.00000|
|  343|      0|  0.00000|
|  344|      0|  0.00000|
|  345|      0|  0.00000|
|  346|      0|  0.00000|
|  347|      0|  0.00000|
|  348|      0|  0.00000|
|  349|      0|  0.00000|
|  350|      0|  0.00000|
|  351|      0|  0.00000|
|  353|      0|  0.00000|
|  354|      0|  0.00000|
|  355|      0|  0.00000|
|  356|      0|  0.00000|
|  357|      0|  0.00000|
|  358|      0|  0.00000|
|  359|      0|  0.00000|
|  360|      0|  0.00000|
|  361|      0|  0.00000|
|  362|      0|  0.00000|
|  363|      0|  0.00000|
|  364|      0|  0.00000|
|  365|      0|  0.00000|
|  366|      0|  0.00000|
|  367|      0|  0.00000|
|  368|      0|  0.00000|
|  369|      0|  0.00000|
|  370|      0|  0.00000|
|  371|      0|  0.00000|
|  372|      0|  0.00000|
|  373|      0|  0.00000|
|  374|      0|  0.00000|
|  375|      0|  0.00000|
|  376|      0|  0.00000|
|  377|      0|  0.00000|
|  380|      0|  0.00000|
|  381|      0|  0.00000|
|  382|      0|  0.00000|
|  383|      0|  0.00000|
|  384|      0|  0.00000|
|  385|      0|  0.00000|
|  386|      0|  0.00000|
|  387|      0|  0.00000|
|  388|      0|  0.00000|
|  389|      0|  0.00000|
|  390|      0|  0.00000|
|  391|      0|  0.00000|
|  392|      0|  0.00000|
|  393|      0|  0.00000|
|  394|      0|  0.00000|
|  395|      0|  0.00000|
|  396|      0|  0.00000|
|  397|      0|  0.00000|
|  398|      0|  0.00000|
|  399|      0|  0.00000|
|  400|      0|  0.00000|
|  401|      0|  0.00000|
|  402|      0|  0.00000|
|  403|      0|  0.00000|
|  404|      0|  0.00000|
|  405|      0|  0.00000|
|  406|      0|  0.00000|
|  407|      0|  0.00000|
|  408|      0|  0.00000|
|  410|      0|  0.00000|
|  411|      0|  0.00000|
|  412|      0|  0.00000|
|  413|      0|  0.00000|
|  414|      0|  0.00000|
|  415|      0|  0.00000|
|  416|      0|  0.00000|
|  417|      0|  0.00000|
|  418|      0|  0.00000|
|  420|      0|  0.00000|
|  422|      0|  0.00000|
|  423|      0|  0.00000|
|  424|      0|  0.00000|
|  425|      0|  0.00000|
|  426|      0|  0.00000|
|  427|      0|  0.00000|
|  428|      0|  0.00000|
|  429|      0|  0.00000|
|  430|      0|  0.00000|
|  431|      0|  0.00000|
|  432|      0|  0.00000|
|  433|      0|  0.00000|
|  434|      0|  0.00000|
|  436|      0|  0.00000|
|  437|      0|  0.00000|
|  438|      0|  0.00000|
|  439|      0|  0.00000|
|  440|      0|  0.00000|
|  441|      0|  0.00000|
|  442|      0|  0.00000|
|  443|      0|  0.00000|
|  444|      0|  0.00000|
|  445|      0|  0.00000|
|  446|      0|  0.00000|
|  447|      0|  0.00000|
|  448|      0|  0.00000|
|  449|      0|  0.00000|
|  450|      0|  0.00000|
|  451|      0|  0.00000|
|  452|      0|  0.00000|
|  453|      0|  0.00000|
|  454|      0|  0.00000|
|  455|      0|  0.00000|
|  456|      0|  0.00000|
|  457|      0|  0.00000|
|  458|      0|  0.00000|
|  459|      0|  0.00000|
|  460|      0|  0.00000|
|  461|      0|  0.00000|
|  462|      0|  0.00000|
|  463|      0|  0.00000|
|  464|      0|  0.00000|
|  465|      0|  0.00000|

</div>

- **tam ve eksik veri tablosu**


```r
miss_case_table(screen)
```

<div class="kable-table">

| n_miss_in_case| n_cases| pct_cases|
|--------------:|-------:|---------:|
|              0|     438| 94.193548|
|              1|      27|  5.806452|

</div>

- **Eksik verinin görselleştirilmesi**


```r
gg_miss_var(screen)
```

<img src="01-Varsayımlar_files/figure-html/unnamed-chunk-16-1.png" width="100%" style="display: block; margin: auto;" />

- **Eksik verinin görselleştirilmesi**


```r
vis_miss(screen) + theme(axis.text.x = element_text(angle=80))
```

<img src="01-Varsayımlar_files/figure-html/unnamed-chunk-17-1.png" width="100%" style="display: block; margin: auto;" />

```r
gg_miss_upset(screen)
```

```
## `geom_line()`: Each group consists of only one observation.
## ℹ Do you need to adjust the group aesthetic?
```

<img src="01-Varsayımlar_files/figure-html/unnamed-chunk-17-2.png" width="100%" style="display: block; margin: auto;" />


### Kayıp Veri Testi

Veri kaybının diğer değişkenlerle ilişkili olup olmadığının incelenmesi

### finalfit paketi


```r
# değişkeni kopyala
screen2 <- screen
screen2$income_m <- screen2$income

library(finalfit)

explanatory = c("timedrs", "attdrug", "atthouse")
dependent = "income_m"
screen2 %>% 
  missing_compare(dependent, explanatory) %>% 
    knitr::kable(row.names=FALSE, align = c("l", "l", "r", "r", "r"), 
        caption = "Eksik veriye sahip olan ve olmayan değişkenlerin ortalama karşılaştırması") 
```



Table: (\#tab:unnamed-chunk-18)Eksik veriye sahip olan ve olmayan değişkenlerin ortalama karşılaştırması

|Missing data analysis: Income  |          | Not missing|    Missing|     p|
|:------------------------------|:---------|-----------:|----------:|-----:|
|Visits to health professionals |Mean (SD) |  7.9 (11.1)|  7.6 (7.4)| 0.891|
|Attitudes toward medication    |Mean (SD) |   7.7 (1.2)|  7.9 (1.0)| 0.368|
|Attitudes toward housework     |Mean (SD) |  23.5 (4.5)| 23.7 (4.2)| 0.860|



```r
library(finalfit)
library(dplyr)

screen2 <- screen2 %>% 
  mutate(mstatus2 = 
    case_when(
      mstatus ==1 ~ "not married",
      mstatus ==2  ~ "married")) %>% 
  mutate_if(is.character,as.factor)

screen2 %>% 
  ff_glimpse()
```

```
## $Continuous
##                                   label var_type   n missing_n missing_percent
## subno                    Subject number    <dbl> 465         0             0.0
## timedrs  Visits to health professionals    <dbl> 465         0             0.0
## attdrug     Attitudes toward medication    <dbl> 465         0             0.0
## atthouse     Attitudes toward housework    <dbl> 464         1             0.2
## income                           Income    <dbl> 439        26             5.6
## mstatus       Whether currently married    <dbl> 465         0             0.0
## race            Ethnic group membership    <dbl> 465         0             0.0
## income_m                         Income    <dbl> 439        26             5.6
##           mean    sd min quartile_25 median quartile_75   max
## subno    317.4 194.2 1.0       137.0  314.0       483.0 758.0
## timedrs    7.9  10.9 0.0         2.0    4.0        10.0  81.0
## attdrug    7.7   1.2 5.0         7.0    8.0         9.0  10.0
## atthouse  23.5   4.5 2.0        21.0   24.0        27.0  35.0
## income     4.2   2.4 1.0         2.5    4.0         6.0  10.0
## mstatus    1.8   0.4 1.0         2.0    2.0         2.0   2.0
## race       1.1   0.3 1.0         1.0    1.0         1.0   2.0
## income_m   4.2   2.4 1.0         2.5    4.0         6.0  10.0
## 
## $Categorical
##             label var_type   n missing_n missing_percent levels_n
## mstatus2 mstatus2    <fct> 465         0             0.0        2
##                                         levels levels_count levels_percent
## mstatus2 "married", "not married", "(Missing)"     362, 103         78, 22
```

#### Bir değişkenin kategorilerinde inceleme


```r
miss_test <- screen2 %>%
  mutate(miss_income = is.na(income))
  
# evli olmayanlar için
notmarried <- miss_test %>%
  filter(mstatus2 == "not married") %>%
   pull(miss_income)
  
# Evliler için
married <- miss_test %>%
  filter(mstatus2 == "married") %>%
  pull(miss_income)
  
#c Oran
t.test(notmarried, married)
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  notmarried and married
## t = -0.95833, df = 198.7, p-value = 0.3391
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -0.06708191  0.02320485
## sample estimates:
##  mean of x  mean of y 
## 0.03883495 0.06077348
```


```r
gg_miss_fct(screen2, fct = mstatus2)
```

<img src="01-Varsayımlar_files/figure-html/unnamed-chunk-21-1.png" width="100%" style="display: block; margin: auto;" />

### MCAR test


```r
library(naniar)
mcar_test(data=screen[,2:5])
```

<div class="kable-table">

| statistic| df|   p.value| missing.patterns|
|---------:|--:|---------:|----------------:|
|  3.286262|  6| 0.7721547|                3|

</div>

### Kayıp veri ile başetme

Liste bazında silme işlemi **na.omit** ve **complete.cases** fonkisyonları ile sağlanabilir.


```r
na.omit(screen) 
screen[!complete.cases(screen),]
screen[complete.cases(screen),]
```

<div class="kable-table">

| subno| timedrs| attdrug| atthouse| income| mstatus| race|
|-----:|-------:|-------:|--------:|------:|-------:|----:|
|     1|       1|       8|       27|      5|       2|    1|
|     2|       3|       7|       20|      6|       2|    1|
|     3|       0|       8|       23|      3|       2|    1|
|     4|      13|       9|       28|      8|       2|    1|
|     5|      15|       7|       24|      1|       2|    1|
|     6|       3|       8|       25|      4|       2|    1|
|     7|       2|       7|       30|      6|       2|    1|
|     8|       0|       7|       24|      6|       2|    1|
|     9|       7|       7|       20|      2|       2|    1|
|    10|       4|       8|       30|      8|       1|    1|
|    11|      15|       9|       15|      7|       2|    1|
|    12|       0|       6|       22|      3|       2|    1|
|    13|       2|       6|       19|      5|       2|    1|
|    14|      13|       8|       25|      6|       2|    1|
|    15|       2|       5|       17|      1|       2|    1|
|    16|       2|       8|       19|      3|       2|    2|
|    21|       1|       8|       22|      1|       2|    1|
|    22|       2|       6|       21|      7|       1|    1|
|    23|       5|       8|       28|      2|       2|    1|
|    24|       5|      10|       25|      9|       2|    1|
|    25|       3|       6|       19|      4|       2|    1|
|    26|       4|       5|       31|      5|       2|    1|
|    27|       2|       8|       25|      2|       2|    1|
|    28|       0|       8|       26|      1|       2|    1|
|    29|      13|       9|       26|      2|       2|    1|
|    30|       7|       9|       33|      1|       2|    1|
|    31|       2|       8|       20|      5|       2|    1|
|    32|      12|       9|       26|      1|       2|    1|
|    33|       2|       6|       27|      3|       1|    1|
|    34|       5|       9|       30|      1|       2|    1|
|    35|       4|       7|       31|      4|       2|    1|
|    36|       6|       6|       25|      5|       2|    1|
|    37|       2|       9|       27|      5|       2|    1|
|    38|       3|       7|       23|      4|       2|    1|
|    39|      14|       9|       15|      4|       2|    1|
|    40|       7|       7|       32|      3|       2|    1|
|    45|       0|       8|       21|      9|       1|    2|
|    46|       1|       8|       28|      6|       2|    2|
|    47|       3|       8|       16|      1|       2|    1|
|    48|      60|       7|       24|      1|       2|    1|
|    49|       5|       8|       19|      1|       2|    1|
|    50|       3|       9|       23|      6|       2|    1|
|    51|       0|       7|       24|      8|       2|    1|
|    52|       3|       8|       31|      3|       2|    1|
|    53|       2|       6|       29|      1|       1|    1|
|    54|       1|       8|       23|      4|       2|    1|
|    55|       1|       9|       24|      4|       2|    1|
|    56|      13|       8|       25|      1|       2|    1|
|    57|       2|       7|       22|      2|       2|    1|
|    65|       5|       9|       26|      5|       1|    1|
|    66|      12|       6|       27|      5|       2|    1|
|    68|       1|       9|       27|      6|       2|    1|
|    69|      20|       9|       24|      6|       2|    1|
|    70|       0|       6|       23|      4|       2|    1|
|    71|       5|       7|       25|      4|       2|    1|
|    72|       0|       7|       22|      6|       2|    2|
|    73|       8|      10|       25|      9|       1|    2|
|    74|       9|       8|       15|      3|       2|    1|
|    75|      10|       8|       28|      6|       1|    1|
|    76|       1|      10|       25|      3|       2|    1|
|    77|       5|       8|       22|      7|       1|    1|
|    78|       5|       8|       21|      3|       2|    1|
|    80|       7|       6|       26|      1|       1|    2|
|    81|       1|       7|       23|      2|       2|    1|
|    82|      39|       7|       26|      2|       2|    1|
|    83|       2|       8|       21|      3|       1|    1|
|    85|       9|       7|       31|      5|       1|    1|
|    86|       0|       5|       35|      5|       2|    1|
|    87|       4|       8|       28|      5|       2|    1|
|    88|      16|      10|       28|      5|       2|    1|
|    89|       0|       7|       20|      5|       2|    1|
|    90|      16|       8|       24|      3|       2|    1|
|    91|      33|       9|       25|      1|       2|    1|
|    97|       2|       8|       16|      3|       2|    1|
|    98|      38|       8|       18|      2|       2|    1|
|    99|       8|       8|       23|      2|       2|    1|
|   101|       2|       8|       24|      9|       1|    1|
|   102|       2|       9|       29|     10|       1|    1|
|   103|       0|       9|       16|      1|       2|    2|
|   104|      15|       7|       22|      5|       2|    2|
|   105|       0|       7|       27|     10|       1|    1|
|   106|       1|       8|       23|      8|       1|    1|
|   107|       5|       7|       26|      1|       2|    1|
|   108|       7|       9|       30|      3|       2|    1|
|   109|      10|       6|       23|      3|       2|    1|
|   110|      22|       9|       26|      6|       1|    1|
|   111|      10|       7|       30|      3|       2|    1|
|   112|       0|       7|       25|      2|       2|    1|
|   113|      11|       8|       22|      1|       2|    1|
|   114|       9|       7|       21|      4|       2|    1|
|   115|       0|       8|       16|      4|       2|    1|
|   116|      34|       7|       23|      5|       2|    1|
|   117|       3|       9|       25|      4|       1|    1|
|   118|       1|       6|       20|      5|       1|    1|
|   119|       0|      10|       18|      1|       2|    2|
|   120|      10|       6|       23|      4|       2|    1|
|   121|       4|       8|       17|      4|       2|    2|
|   122|      27|       8|       23|      8|       1|    2|
|   123|       7|       9|       23|      5|       2|    2|
|   124|       1|       5|       22|      2|       2|    2|
|   125|       7|      10|       22|      6|       1|    1|
|   126|       3|       8|       28|      4|       2|    1|
|   127|       2|       6|       17|      2|       2|    1|
|   128|      11|       6|       27|      7|       2|    1|
|   129|       9|       6|       18|      4|       2|    1|
|   130|      11|       8|       26|      7|       2|    2|
|   131|      11|       8|       21|      7|       2|    1|
|   132|       8|       7|       21|      4|       2|    1|
|   133|       2|       9|       24|     10|       1|    1|
|   134|       4|       9|       26|      3|       2|    1|
|   135|       4|       8|       21|      3|       2|    1|
|   136|       6|       7|       18|      8|       2|    1|
|   137|      30|       5|       24|     10|       2|    2|
|   139|      15|       7|       26|      1|       2|    1|
|   140|       6|       7|       25|      8|       2|    2|
|   141|       7|       7|       23|      5|       2|    1|
|   142|       2|       9|       22|      3|       2|    1|
|   143|       1|       7|       19|      5|       2|    1|
|   144|       5|       7|       26|      3|       2|    1|
|   145|       1|       9|       29|      3|       2|    1|
|   146|      11|       6|       20|      4|       2|    1|
|   148|      16|      10|       20|      1|       2|    1|
|   149|       2|       7|       19|      5|       2|    1|
|   150|      14|       5|       24|      4|       2|    1|
|   151|       8|       7|       21|      3|       2|    1|
|   152|      17|       8|       32|      1|       2|    1|
|   153|       1|       8|       18|      3|       2|    1|
|   154|       0|       6|       22|      1|       2|    1|
|   155|       1|       9|       27|      8|       1|    2|
|   157|      10|       8|       19|      1|       1|    1|
|   158|       7|       7|       25|      1|       2|    1|
|   159|       2|       8|       17|      7|       1|    1|
|   160|       5|       6|       25|      3|       2|    1|
|   166|       6|       7|       31|      4|       1|    1|
|   167|       1|       8|       16|      7|       2|    1|
|   183|      16|      10|       17|      8|       2|    1|
|   184|       0|       5|       14|      8|       1|    1|
|   185|      17|       7|       30|      4|       2|    2|
|   187|       0|       8|       25|      6|       2|    1|
|   190|      10|       8|       28|      1|       2|    1|
|   192|       6|       7|       22|      1|       2|    1|
|   202|      20|       8|       24|      5|       2|    1|
|   203|       1|       8|       21|      2|       2|    1|
|   204|      25|       8|       25|      7|       1|    1|
|   205|       3|       9|       27|      7|       2|    1|
|   206|       6|       9|       20|      7|       1|    1|
|   208|       0|       7|       26|      1|       2|    1|
|   210|       8|       8|       25|      1|       2|    1|
|   212|       9|       9|       27|      3|       1|    1|
|   213|      19|      10|       23|      5|       2|    1|
|   214|       2|       8|       27|      3|       2|    1|
|   225|       2|       7|       21|      3|       2|    1|
|   226|       3|       7|       22|      5|       2|    1|
|   227|       7|       8|       19|      7|       2|    1|
|   229|       8|       6|       25|      4|       2|    1|
|   230|      49|       8|       34|      4|       2|    1|
|   231|       2|       8|       27|      1|       2|    2|
|   232|       1|       8|       22|      3|       2|    1|
|   233|       5|       6|       14|      3|       2|    1|
|   234|       5|       8|       29|      1|       2|    1|
|   235|      60|      10|       29|      4|       1|    1|
|   236|      10|       7|       26|      6|       2|    1|
|   237|      27|       8|       17|      7|       2|    1|
|   238|       7|       9|       22|      3|       2|    1|
|   242|       2|       7|       21|      1|       2|    1|
|   243|       4|       7|       22|      3|       2|    1|
|   244|       6|       5|       29|      3|       2|    1|
|   245|      27|       9|       22|      1|       2|    1|
|   246|       0|       8|       20|      9|       1|    1|
|   247|       9|      10|       30|      3|       2|    1|
|   249|       0|       8|       18|      5|       2|    1|
|   250|       2|       6|       19|      4|       2|    1|
|   251|       3|       8|       24|      3|       2|    1|
|   252|       5|       8|       22|      4|       2|    1|
|   253|       5|       9|       21|      8|       1|    2|
|   254|       1|       9|       29|      1|       2|    1|
|   255|       7|       9|       22|      1|       2|    1|
|   258|       4|       8|       26|      7|       2|    1|
|   259|       3|       7|       19|      6|       1|    1|
|   260|       7|       8|       23|      8|       1|    1|
|   261|       1|       7|       19|      9|       1|    1|
|   262|      52|       9|       31|      4|       2|    2|
|   263|       6|       7|       26|      2|       2|    1|
|   264|      18|       7|       23|      4|       2|    1|
|   266|       0|       7|       22|      4|       2|    1|
|   267|       8|       9|       29|      1|       2|    2|
|   268|      16|       8|       23|      7|       1|    1|
|   269|       2|       9|       17|      3|       2|    1|
|   270|      12|       8|       26|      1|       2|    1|
|   271|       3|       7|       25|      4|       2|    1|
|   273|       0|       9|       19|      4|       1|    1|
|   274|       0|       7|       16|      3|       2|    1|
|   276|      57|       9|       24|      2|       2|    1|
|   277|      11|       9|       29|      3|       2|    1|
|   278|       1|       8|       20|      6|       2|    1|
|   279|      18|       6|       25|      2|       2|    1|
|   280|       1|       5|       16|      6|       1|    1|
|   289|      11|       9|       35|      4|       2|    1|
|   290|       0|       8|       24|      7|       2|    1|
|   291|      52|       8|       19|      1|       2|    1|
|   292|      12|       7|       25|      4|       1|    1|
|   293|       6|       6|       29|      3|       2|    1|
|   294|       0|       7|       25|      3|       1|    1|
|   295|       2|       8|       29|      2|       2|    1|
|   299|       2|       8|       20|      6|       2|    1|
|   300|      13|      10|       25|      1|       1|    1|
|   301|       2|       7|       31|      4|       2|    1|
|   302|       3|       6|       33|      5|       2|    1|
|   303|       2|       9|       24|      2|       2|    1|
|   304|       2|       7|       26|      4|       2|    1|
|   305|       1|       7|       20|      7|       1|    1|
|   306|       2|       7|       17|      4|       2|    1|
|   307|       3|       8|       25|      7|       1|    1|
|   308|       1|       9|       24|      3|       2|    1|
|   309|       5|       5|       27|     10|       1|    1|
|   310|       6|       7|       25|     10|       1|    1|
|   311|       1|       7|       28|      7|       1|    1|
|   312|       2|       8|       29|      1|       2|    1|
|   313|       3|       6|       27|      4|       2|    1|
|   314|       7|      10|       24|      4|       2|    1|
|   315|       7|       6|       23|      7|       2|    1|
|   316|       0|       8|       19|      3|       2|    1|
|   318|       8|       7|       24|      3|       2|    1|
|   319|       9|       7|       21|      7|       2|    1|
|   320|       8|       6|       22|      3|       2|    1|
|   322|      14|       9|       30|      2|       2|    1|
|   323|       2|       9|       32|      1|       2|    1|
|   324|       4|       7|       21|      3|       2|    1|
|   325|       8|       8|       19|     10|       1|    1|
|   326|       3|       8|       24|      4|       2|    1|
|   327|      10|       8|       28|      1|       2|    1|
|   328|      21|      10|       28|     10|       1|    1|
|   329|       6|       7|       29|      7|       2|    1|
|   330|      58|       7|       29|      4|       2|    1|
|   335|      12|       8|       23|      4|       1|    1|
|   336|       5|       7|       26|      1|       2|    1|
|   337|       2|       9|       22|      1|       2|    1|
|   339|       4|       7|       25|      6|       2|    2|
|   340|       2|       7|       19|      5|       2|    2|
|   341|       5|       9|       22|      3|       2|    1|
|   342|       0|       9|       18|      5|       2|    1|
|   344|       7|       9|       23|      5|       2|    1|
|   345|       1|       9|       19|      8|       2|    2|
|   346|       2|       8|        2|      1|       1|    1|
|   347|       1|       6|       26|      5|       2|    1|
|   348|       4|       6|       24|      3|       2|    1|
|   349|       4|       7|       30|      4|       2|    1|
|   355|       4|       8|       30|      5|       2|    1|
|   357|       2|       8|       22|      3|       2|    1|
|   358|       0|       8|       26|      7|       2|    1|
|   359|      13|       6|       30|      4|       2|    1|
|   361|       3|       8|       25|      5|       2|    1|
|   362|       1|       8|       24|      5|       2|    1|
|   363|       4|       7|       23|      3|       2|    1|
|   365|       1|       8|       18|      4|       2|    1|
|   367|       3|       7|       23|      4|       2|    1|
|   369|       1|       6|       34|      4|       2|    1|
|   370|      57|       8|       23|      4|       2|    1|
|   372|       1|       8|       13|      1|       2|    2|
|   374|      17|       9|       27|      7|       2|    1|
|   378|      11|       7|       21|      3|       2|    1|
|   379|      43|       6|       28|      7|       1|    1|
|   380|       6|       9|       28|      7|       1|    1|
|   381|       6|       8|       20|      6|       1|    1|
|   382|       1|       9|       21|      1|       2|    1|
|   383|       0|       7|       25|      9|       1|    1|
|   384|      10|       6|       27|      9|       1|    1|
|   385|       3|       9|       23|     10|       2|    1|
|   386|      37|       9|       25|      6|       1|    1|
|   387|       6|       8|       22|      2|       2|    1|
|   392|      11|       7|       24|      9|       1|    1|
|   397|       4|       7|       15|      3|       2|    2|
|   398|      75|       9|       33|      9|       1|    1|
|   399|       7|       9|       24|      5|       2|    1|
|   400|       3|       6|       29|      7|       1|    2|
|   401|       3|       7|       21|      3|       2|    1|
|   402|       3|       7|       26|      2|       2|    1|
|   403|       4|       8|       29|      4|       1|    1|
|   404|       5|       9|       21|      1|       2|    1|
|   405|       2|      10|       21|      1|       2|    1|
|   406|       4|       7|       18|      5|       1|    1|
|   407|       2|       8|        2|      4|       1|    1|
|   413|      11|       9|       28|      2|       1|    1|
|   414|       0|       6|       20|      6|       2|    1|
|   417|       2|       7|       27|      4|       2|    1|
|   418|       4|       8|       23|      6|       2|    1|
|   421|       0|       8|       27|      5|       2|    2|
|   424|       2|       6|       25|      4|       2|    1|
|   425|       5|       6|       22|      4|       2|    1|
|   434|       2|       6|       29|      1|       2|    1|
|   435|      10|       9|       23|      7|       2|    1|
|   436|      29|       9|       14|      3|       2|    1|
|   437|       3|       8|       28|      8|       1|    1|
|   438|       0|       8|       29|      3|       2|    1|
|   439|      21|       8|       27|      7|       1|    1|
|   440|       0|       7|       23|      6|       1|    1|
|   441|       3|       8|       23|      5|       2|    1|
|   442|       1|       8|       20|      1|       2|    1|
|   443|       9|       9|       24|     10|       1|    1|
|   444|       3|       8|       26|      3|       2|    1|
|   445|       3|       7|       23|      1|       2|    1|
|   446|       5|       8|       25|      4|       1|    1|
|   448|       4|       9|       16|      4|       2|    1|
|   451|      16|       5|       29|      1|       1|    1|
|   452|       3|       9|       25|      9|       1|    1|
|   454|       3|       9|       29|      1|       2|    1|
|   455|       2|       6|       23|      3|       2|    1|
|   456|       2|       8|       26|      3|       2|    2|
|   457|       3|       7|       19|      5|       2|    2|
|   458|       7|       7|       27|      5|       1|    1|
|   459|       2|       8|       21|      4|       2|    2|
|   460|       2|       7|       18|      6|       2|    1|
|   461|       2|       7|       19|      4|       2|    1|
|   462|       2|       7|       23|      3|       2|    2|
|   463|       5|      10|       19|      7|       1|    2|
|   464|       3|       7|       21|      6|       2|    1|
|   465|       2|       6|       23|      5|       2|    1|
|   466|       4|       9|       28|      3|       2|    1|
|   467|       1|       7|       24|      6|       2|    1|
|   469|       3|       8|       27|      2|       2|    1|
|   472|       6|       9|       27|     10|       1|    1|
|   473|       4|       8|       25|      4|       1|    1|
|   474|      30|       8|       29|      4|       1|    1|
|   476|       0|       7|       19|      5|       2|    1|
|   479|      25|       7|       27|      3|       2|    1|
|   480|       0|       7|       31|      3|       1|    1|
|   481|       5|       8|       18|      2|       2|    1|
|   482|       3|       8|       23|      8|       1|    1|
|   483|       2|       9|       32|      4|       1|    1|
|   484|       2|       7|       20|      7|       1|    1|
|   485|       9|       8|       21|      3|       2|    1|
|   487|      13|       6|       21|      4|       2|    1|
|   488|       1|       8|       22|      3|       2|    1|
|   489|       4|       9|       29|      4|       2|    1|
|   490|       4|       7|       23|      1|       1|    1|
|   491|       3|       9|       20|      1|       2|    1|
|   492|       7|       9|       21|      1|       2|    1|
|   493|       7|       8|       26|      1|       2|    1|
|   494|      14|       6|       21|      2|       2|    1|
|   495|       4|       7|       24|      1|       2|    1|
|   496|      15|       6|       26|      6|       2|    1|
|   497|      37|       9|       30|      2|       2|    1|
|   498|       2|       7|       18|      2|       2|    1|
|   499|       4|       7|       22|      2|       2|    1|
|   500|       6|       9|       25|      4|       2|    1|
|   501|      10|       6|       21|      4|       2|    1|
|   502|      56|       8|       19|      3|       2|    1|
|   503|       3|      10|       23|      5|       2|    1|
|   504|       0|       9|       22|      1|       2|    1|
|   505|      18|       7|       21|      1|       2|    1|
|   506|       3|       9|       22|      3|       2|    1|
|   507|      18|       7|       24|      4|       2|    1|
|   508|       7|       8|       18|      4|       2|    1|
|   509|      29|       8|       18|      5|       2|    1|
|   510|       0|       5|       24|      4|       2|    1|
|   511|       5|       6|       25|      8|       1|    1|
|   514|       6|       7|       29|      2|       2|    1|
|   515|      21|       9|       22|      2|       2|    1|
|   516|       1|       8|       25|      1|       2|    1|
|   517|       3|       7|       27|      3|       2|    1|
|   518|       3|       8|       26|      6|       1|    2|
|   519|       0|       7|       28|      1|       2|    1|
|   520|      13|      10|       19|      2|       2|    1|
|   521|       5|       6|       23|      9|       1|    1|
|   522|       5|       8|       33|      4|       2|    1|
|   523|      37|       7|       24|     10|       1|    1|
|   524|       2|       8|       28|      4|       2|    1|
|   525|      11|       8|       28|      4|       2|    1|
|   526|      13|       7|       24|      9|       2|    1|
|   527|       2|       9|       21|      4|       2|    1|
|   528|       4|       9|       31|      3|       2|    1|
|   529|      21|       9|       23|      2|       2|    1|
|   530|       2|      10|       26|      4|       2|    1|
|   533|       4|       8|       26|      3|       2|    1|
|   534|       3|       8|       26|      3|       2|    2|
|   535|       3|       8|       24|      4|       2|    1|
|   536|      12|       8|       18|      3|       2|    1|
|   538|       1|       7|       21|      5|       1|    1|
|   539|       4|       7|       19|      2|       2|    1|
|   540|      13|       7|       17|      8|       1|    1|
|   547|       2|       7|       24|      1|       2|    1|
|   548|      81|       8|       24|      9|       1|    1|
|   549|      12|       7|       31|     10|       1|    1|
|   550|       2|       8|       19|      6|       2|    1|
|   551|      16|       7|       28|      4|       2|    1|
|   553|       2|       5|       26|      5|       2|    1|
|   554|       2|       7|       26|      2|       1|    1|
|   555|       8|       8|       18|      6|       2|    1|
|   556|       2|       8|       16|      4|       2|    1|
|   557|       4|       8|       29|      1|       2|    1|
|   558|       3|       8|       14|      4|       2|    1|
|   559|      19|       7|       23|      4|       2|    1|
|   560|       4|       9|       29|      1|       2|    1|
|   567|       1|       7|       19|      3|       2|    1|
|   569|      15|       6|       20|      1|       2|    1|
|   571|       4|       8|       21|      1|       2|    1|
|   572|      13|       8|       23|      6|       2|    1|
|   573|       6|       9|       26|      1|       2|    1|
|   574|       1|      10|       18|     10|       1|    1|
|   575|       3|       9|       18|      1|       2|    1|
|   576|       3|       9|       28|      4|       2|    1|
|   577|       0|       9|       31|      2|       2|    1|
|   578|      22|       7|       22|      2|       2|    1|
|   579|       4|       8|       28|      3|       2|    1|
|   580|      14|       7|       20|      2|       1|    1|
|   581|       6|       9|       20|      7|       1|    1|
|   582|      16|       7|       26|      7|       2|    2|
|   583|       6|       6|       27|      1|       2|    1|
|   585|       8|       6|       26|      7|       2|    1|
|   586|       0|       7|       27|      6|       2|    1|
|   587|       4|       7|       16|      4|       2|    1|
|   588|       5|       6|       12|      4|       2|    1|
|   589|       2|       8|       20|      3|       2|    1|
|   590|       6|       8|       19|      6|       1|    1|
|   591|      11|       8|       26|      7|       2|    1|
|   592|       1|       7|       20|      3|       2|    1|
|   593|      23|       7|       22|      7|       1|    1|
|   683|       4|       8|       30|      6|       2|    1|
|   685|       4|       9|       25|      1|       2|    1|
|   686|      16|       9|       23|      6|       1|    1|
|   687|       6|       7|       22|      8|       1|    1|
|   688|       1|       9|       24|      5|       2|    1|
|   689|       2|       6|       23|      4|       1|    1|
|   690|       6|       9|       27|      3|       2|    1|
|   691|       6|       6|       28|      3|       2|    1|
|   706|       3|       7|       24|      3|       2|    1|
|   707|       1|       7|       17|      4|       2|    1|
|   708|      15|       9|       22|     10|       2|    1|
|   709|       3|       6|       23|      4|       2|    1|
|   710|       7|       8|       16|      3|       2|    1|
|   711|       9|       6|       18|      2|       2|    1|
|   717|      18|       6|       21|      4|       2|    1|
|   724|      14|       9|       19|      3|       2|    1|
|   754|       3|       6|       17|      3|       2|    1|
|   755|       4|       8|       17|      3|       2|    1|
|   756|      15|      10|       19|      8|       2|    1|
|   757|       4|       8|       23|      2|       2|    1|
|   758|       3|       8|       18|      4|       2|    1|

</div><div class="kable-table">

| subno| timedrs| attdrug| atthouse| income| mstatus| race|
|-----:|-------:|-------:|--------:|------:|-------:|----:|
|    67|      12|       9|       24|     NA|       2|    1|
|    79|      23|       6|       29|     NA|       2|    1|
|    84|       7|       8|       22|     NA|       2|    1|
|    95|       4|       7|       25|     NA|       2|    1|
|   138|       7|       8|       28|     NA|       1|    1|
|   156|       1|       6|       21|     NA|       1|    1|
|   228|       2|       8|       26|     NA|       2|    1|
|   239|       8|       7|       20|     NA|       2|    1|
|   240|      12|       8|       30|     NA|       2|    1|
|   241|       2|       7|       17|     NA|       2|    1|
|   248|       3|       9|       25|     NA|       2|    1|
|   265|      14|       7|       29|     NA|       1|    2|
|   272|      24|      10|       25|     NA|       2|    1|
|   317|       7|       8|       23|     NA|       2|    1|
|   321|       1|       9|       21|     NA|       2|    1|
|   338|       2|       6|       NA|      5|       1|    2|
|   343|       3|       8|       25|     NA|       2|    2|
|   420|       6|       9|       27|     NA|       2|    1|
|   447|       6|       8|       28|     NA|       2|    1|
|   453|      13|       9|       22|     NA|       2|    1|
|   486|       2|       8|       25|     NA|       2|    1|
|   512|       4|       8|       19|     NA|       2|    1|
|   513|       3|       7|       27|     NA|       2|    1|
|   552|      27|       8|       23|     NA|       2|    1|
|   568|       3|       7|       22|     NA|       2|    1|
|   570|       4|       8|       22|     NA|       1|    1|
|   584|       0|       8|       11|     NA|       2|    2|

</div><div class="kable-table">

| subno| timedrs| attdrug| atthouse| income| mstatus| race|
|-----:|-------:|-------:|--------:|------:|-------:|----:|
|     1|       1|       8|       27|      5|       2|    1|
|     2|       3|       7|       20|      6|       2|    1|
|     3|       0|       8|       23|      3|       2|    1|
|     4|      13|       9|       28|      8|       2|    1|
|     5|      15|       7|       24|      1|       2|    1|
|     6|       3|       8|       25|      4|       2|    1|
|     7|       2|       7|       30|      6|       2|    1|
|     8|       0|       7|       24|      6|       2|    1|
|     9|       7|       7|       20|      2|       2|    1|
|    10|       4|       8|       30|      8|       1|    1|
|    11|      15|       9|       15|      7|       2|    1|
|    12|       0|       6|       22|      3|       2|    1|
|    13|       2|       6|       19|      5|       2|    1|
|    14|      13|       8|       25|      6|       2|    1|
|    15|       2|       5|       17|      1|       2|    1|
|    16|       2|       8|       19|      3|       2|    2|
|    21|       1|       8|       22|      1|       2|    1|
|    22|       2|       6|       21|      7|       1|    1|
|    23|       5|       8|       28|      2|       2|    1|
|    24|       5|      10|       25|      9|       2|    1|
|    25|       3|       6|       19|      4|       2|    1|
|    26|       4|       5|       31|      5|       2|    1|
|    27|       2|       8|       25|      2|       2|    1|
|    28|       0|       8|       26|      1|       2|    1|
|    29|      13|       9|       26|      2|       2|    1|
|    30|       7|       9|       33|      1|       2|    1|
|    31|       2|       8|       20|      5|       2|    1|
|    32|      12|       9|       26|      1|       2|    1|
|    33|       2|       6|       27|      3|       1|    1|
|    34|       5|       9|       30|      1|       2|    1|
|    35|       4|       7|       31|      4|       2|    1|
|    36|       6|       6|       25|      5|       2|    1|
|    37|       2|       9|       27|      5|       2|    1|
|    38|       3|       7|       23|      4|       2|    1|
|    39|      14|       9|       15|      4|       2|    1|
|    40|       7|       7|       32|      3|       2|    1|
|    45|       0|       8|       21|      9|       1|    2|
|    46|       1|       8|       28|      6|       2|    2|
|    47|       3|       8|       16|      1|       2|    1|
|    48|      60|       7|       24|      1|       2|    1|
|    49|       5|       8|       19|      1|       2|    1|
|    50|       3|       9|       23|      6|       2|    1|
|    51|       0|       7|       24|      8|       2|    1|
|    52|       3|       8|       31|      3|       2|    1|
|    53|       2|       6|       29|      1|       1|    1|
|    54|       1|       8|       23|      4|       2|    1|
|    55|       1|       9|       24|      4|       2|    1|
|    56|      13|       8|       25|      1|       2|    1|
|    57|       2|       7|       22|      2|       2|    1|
|    65|       5|       9|       26|      5|       1|    1|
|    66|      12|       6|       27|      5|       2|    1|
|    68|       1|       9|       27|      6|       2|    1|
|    69|      20|       9|       24|      6|       2|    1|
|    70|       0|       6|       23|      4|       2|    1|
|    71|       5|       7|       25|      4|       2|    1|
|    72|       0|       7|       22|      6|       2|    2|
|    73|       8|      10|       25|      9|       1|    2|
|    74|       9|       8|       15|      3|       2|    1|
|    75|      10|       8|       28|      6|       1|    1|
|    76|       1|      10|       25|      3|       2|    1|
|    77|       5|       8|       22|      7|       1|    1|
|    78|       5|       8|       21|      3|       2|    1|
|    80|       7|       6|       26|      1|       1|    2|
|    81|       1|       7|       23|      2|       2|    1|
|    82|      39|       7|       26|      2|       2|    1|
|    83|       2|       8|       21|      3|       1|    1|
|    85|       9|       7|       31|      5|       1|    1|
|    86|       0|       5|       35|      5|       2|    1|
|    87|       4|       8|       28|      5|       2|    1|
|    88|      16|      10|       28|      5|       2|    1|
|    89|       0|       7|       20|      5|       2|    1|
|    90|      16|       8|       24|      3|       2|    1|
|    91|      33|       9|       25|      1|       2|    1|
|    97|       2|       8|       16|      3|       2|    1|
|    98|      38|       8|       18|      2|       2|    1|
|    99|       8|       8|       23|      2|       2|    1|
|   101|       2|       8|       24|      9|       1|    1|
|   102|       2|       9|       29|     10|       1|    1|
|   103|       0|       9|       16|      1|       2|    2|
|   104|      15|       7|       22|      5|       2|    2|
|   105|       0|       7|       27|     10|       1|    1|
|   106|       1|       8|       23|      8|       1|    1|
|   107|       5|       7|       26|      1|       2|    1|
|   108|       7|       9|       30|      3|       2|    1|
|   109|      10|       6|       23|      3|       2|    1|
|   110|      22|       9|       26|      6|       1|    1|
|   111|      10|       7|       30|      3|       2|    1|
|   112|       0|       7|       25|      2|       2|    1|
|   113|      11|       8|       22|      1|       2|    1|
|   114|       9|       7|       21|      4|       2|    1|
|   115|       0|       8|       16|      4|       2|    1|
|   116|      34|       7|       23|      5|       2|    1|
|   117|       3|       9|       25|      4|       1|    1|
|   118|       1|       6|       20|      5|       1|    1|
|   119|       0|      10|       18|      1|       2|    2|
|   120|      10|       6|       23|      4|       2|    1|
|   121|       4|       8|       17|      4|       2|    2|
|   122|      27|       8|       23|      8|       1|    2|
|   123|       7|       9|       23|      5|       2|    2|
|   124|       1|       5|       22|      2|       2|    2|
|   125|       7|      10|       22|      6|       1|    1|
|   126|       3|       8|       28|      4|       2|    1|
|   127|       2|       6|       17|      2|       2|    1|
|   128|      11|       6|       27|      7|       2|    1|
|   129|       9|       6|       18|      4|       2|    1|
|   130|      11|       8|       26|      7|       2|    2|
|   131|      11|       8|       21|      7|       2|    1|
|   132|       8|       7|       21|      4|       2|    1|
|   133|       2|       9|       24|     10|       1|    1|
|   134|       4|       9|       26|      3|       2|    1|
|   135|       4|       8|       21|      3|       2|    1|
|   136|       6|       7|       18|      8|       2|    1|
|   137|      30|       5|       24|     10|       2|    2|
|   139|      15|       7|       26|      1|       2|    1|
|   140|       6|       7|       25|      8|       2|    2|
|   141|       7|       7|       23|      5|       2|    1|
|   142|       2|       9|       22|      3|       2|    1|
|   143|       1|       7|       19|      5|       2|    1|
|   144|       5|       7|       26|      3|       2|    1|
|   145|       1|       9|       29|      3|       2|    1|
|   146|      11|       6|       20|      4|       2|    1|
|   148|      16|      10|       20|      1|       2|    1|
|   149|       2|       7|       19|      5|       2|    1|
|   150|      14|       5|       24|      4|       2|    1|
|   151|       8|       7|       21|      3|       2|    1|
|   152|      17|       8|       32|      1|       2|    1|
|   153|       1|       8|       18|      3|       2|    1|
|   154|       0|       6|       22|      1|       2|    1|
|   155|       1|       9|       27|      8|       1|    2|
|   157|      10|       8|       19|      1|       1|    1|
|   158|       7|       7|       25|      1|       2|    1|
|   159|       2|       8|       17|      7|       1|    1|
|   160|       5|       6|       25|      3|       2|    1|
|   166|       6|       7|       31|      4|       1|    1|
|   167|       1|       8|       16|      7|       2|    1|
|   183|      16|      10|       17|      8|       2|    1|
|   184|       0|       5|       14|      8|       1|    1|
|   185|      17|       7|       30|      4|       2|    2|
|   187|       0|       8|       25|      6|       2|    1|
|   190|      10|       8|       28|      1|       2|    1|
|   192|       6|       7|       22|      1|       2|    1|
|   202|      20|       8|       24|      5|       2|    1|
|   203|       1|       8|       21|      2|       2|    1|
|   204|      25|       8|       25|      7|       1|    1|
|   205|       3|       9|       27|      7|       2|    1|
|   206|       6|       9|       20|      7|       1|    1|
|   208|       0|       7|       26|      1|       2|    1|
|   210|       8|       8|       25|      1|       2|    1|
|   212|       9|       9|       27|      3|       1|    1|
|   213|      19|      10|       23|      5|       2|    1|
|   214|       2|       8|       27|      3|       2|    1|
|   225|       2|       7|       21|      3|       2|    1|
|   226|       3|       7|       22|      5|       2|    1|
|   227|       7|       8|       19|      7|       2|    1|
|   229|       8|       6|       25|      4|       2|    1|
|   230|      49|       8|       34|      4|       2|    1|
|   231|       2|       8|       27|      1|       2|    2|
|   232|       1|       8|       22|      3|       2|    1|
|   233|       5|       6|       14|      3|       2|    1|
|   234|       5|       8|       29|      1|       2|    1|
|   235|      60|      10|       29|      4|       1|    1|
|   236|      10|       7|       26|      6|       2|    1|
|   237|      27|       8|       17|      7|       2|    1|
|   238|       7|       9|       22|      3|       2|    1|
|   242|       2|       7|       21|      1|       2|    1|
|   243|       4|       7|       22|      3|       2|    1|
|   244|       6|       5|       29|      3|       2|    1|
|   245|      27|       9|       22|      1|       2|    1|
|   246|       0|       8|       20|      9|       1|    1|
|   247|       9|      10|       30|      3|       2|    1|
|   249|       0|       8|       18|      5|       2|    1|
|   250|       2|       6|       19|      4|       2|    1|
|   251|       3|       8|       24|      3|       2|    1|
|   252|       5|       8|       22|      4|       2|    1|
|   253|       5|       9|       21|      8|       1|    2|
|   254|       1|       9|       29|      1|       2|    1|
|   255|       7|       9|       22|      1|       2|    1|
|   258|       4|       8|       26|      7|       2|    1|
|   259|       3|       7|       19|      6|       1|    1|
|   260|       7|       8|       23|      8|       1|    1|
|   261|       1|       7|       19|      9|       1|    1|
|   262|      52|       9|       31|      4|       2|    2|
|   263|       6|       7|       26|      2|       2|    1|
|   264|      18|       7|       23|      4|       2|    1|
|   266|       0|       7|       22|      4|       2|    1|
|   267|       8|       9|       29|      1|       2|    2|
|   268|      16|       8|       23|      7|       1|    1|
|   269|       2|       9|       17|      3|       2|    1|
|   270|      12|       8|       26|      1|       2|    1|
|   271|       3|       7|       25|      4|       2|    1|
|   273|       0|       9|       19|      4|       1|    1|
|   274|       0|       7|       16|      3|       2|    1|
|   276|      57|       9|       24|      2|       2|    1|
|   277|      11|       9|       29|      3|       2|    1|
|   278|       1|       8|       20|      6|       2|    1|
|   279|      18|       6|       25|      2|       2|    1|
|   280|       1|       5|       16|      6|       1|    1|
|   289|      11|       9|       35|      4|       2|    1|
|   290|       0|       8|       24|      7|       2|    1|
|   291|      52|       8|       19|      1|       2|    1|
|   292|      12|       7|       25|      4|       1|    1|
|   293|       6|       6|       29|      3|       2|    1|
|   294|       0|       7|       25|      3|       1|    1|
|   295|       2|       8|       29|      2|       2|    1|
|   299|       2|       8|       20|      6|       2|    1|
|   300|      13|      10|       25|      1|       1|    1|
|   301|       2|       7|       31|      4|       2|    1|
|   302|       3|       6|       33|      5|       2|    1|
|   303|       2|       9|       24|      2|       2|    1|
|   304|       2|       7|       26|      4|       2|    1|
|   305|       1|       7|       20|      7|       1|    1|
|   306|       2|       7|       17|      4|       2|    1|
|   307|       3|       8|       25|      7|       1|    1|
|   308|       1|       9|       24|      3|       2|    1|
|   309|       5|       5|       27|     10|       1|    1|
|   310|       6|       7|       25|     10|       1|    1|
|   311|       1|       7|       28|      7|       1|    1|
|   312|       2|       8|       29|      1|       2|    1|
|   313|       3|       6|       27|      4|       2|    1|
|   314|       7|      10|       24|      4|       2|    1|
|   315|       7|       6|       23|      7|       2|    1|
|   316|       0|       8|       19|      3|       2|    1|
|   318|       8|       7|       24|      3|       2|    1|
|   319|       9|       7|       21|      7|       2|    1|
|   320|       8|       6|       22|      3|       2|    1|
|   322|      14|       9|       30|      2|       2|    1|
|   323|       2|       9|       32|      1|       2|    1|
|   324|       4|       7|       21|      3|       2|    1|
|   325|       8|       8|       19|     10|       1|    1|
|   326|       3|       8|       24|      4|       2|    1|
|   327|      10|       8|       28|      1|       2|    1|
|   328|      21|      10|       28|     10|       1|    1|
|   329|       6|       7|       29|      7|       2|    1|
|   330|      58|       7|       29|      4|       2|    1|
|   335|      12|       8|       23|      4|       1|    1|
|   336|       5|       7|       26|      1|       2|    1|
|   337|       2|       9|       22|      1|       2|    1|
|   339|       4|       7|       25|      6|       2|    2|
|   340|       2|       7|       19|      5|       2|    2|
|   341|       5|       9|       22|      3|       2|    1|
|   342|       0|       9|       18|      5|       2|    1|
|   344|       7|       9|       23|      5|       2|    1|
|   345|       1|       9|       19|      8|       2|    2|
|   346|       2|       8|        2|      1|       1|    1|
|   347|       1|       6|       26|      5|       2|    1|
|   348|       4|       6|       24|      3|       2|    1|
|   349|       4|       7|       30|      4|       2|    1|
|   355|       4|       8|       30|      5|       2|    1|
|   357|       2|       8|       22|      3|       2|    1|
|   358|       0|       8|       26|      7|       2|    1|
|   359|      13|       6|       30|      4|       2|    1|
|   361|       3|       8|       25|      5|       2|    1|
|   362|       1|       8|       24|      5|       2|    1|
|   363|       4|       7|       23|      3|       2|    1|
|   365|       1|       8|       18|      4|       2|    1|
|   367|       3|       7|       23|      4|       2|    1|
|   369|       1|       6|       34|      4|       2|    1|
|   370|      57|       8|       23|      4|       2|    1|
|   372|       1|       8|       13|      1|       2|    2|
|   374|      17|       9|       27|      7|       2|    1|
|   378|      11|       7|       21|      3|       2|    1|
|   379|      43|       6|       28|      7|       1|    1|
|   380|       6|       9|       28|      7|       1|    1|
|   381|       6|       8|       20|      6|       1|    1|
|   382|       1|       9|       21|      1|       2|    1|
|   383|       0|       7|       25|      9|       1|    1|
|   384|      10|       6|       27|      9|       1|    1|
|   385|       3|       9|       23|     10|       2|    1|
|   386|      37|       9|       25|      6|       1|    1|
|   387|       6|       8|       22|      2|       2|    1|
|   392|      11|       7|       24|      9|       1|    1|
|   397|       4|       7|       15|      3|       2|    2|
|   398|      75|       9|       33|      9|       1|    1|
|   399|       7|       9|       24|      5|       2|    1|
|   400|       3|       6|       29|      7|       1|    2|
|   401|       3|       7|       21|      3|       2|    1|
|   402|       3|       7|       26|      2|       2|    1|
|   403|       4|       8|       29|      4|       1|    1|
|   404|       5|       9|       21|      1|       2|    1|
|   405|       2|      10|       21|      1|       2|    1|
|   406|       4|       7|       18|      5|       1|    1|
|   407|       2|       8|        2|      4|       1|    1|
|   413|      11|       9|       28|      2|       1|    1|
|   414|       0|       6|       20|      6|       2|    1|
|   417|       2|       7|       27|      4|       2|    1|
|   418|       4|       8|       23|      6|       2|    1|
|   421|       0|       8|       27|      5|       2|    2|
|   424|       2|       6|       25|      4|       2|    1|
|   425|       5|       6|       22|      4|       2|    1|
|   434|       2|       6|       29|      1|       2|    1|
|   435|      10|       9|       23|      7|       2|    1|
|   436|      29|       9|       14|      3|       2|    1|
|   437|       3|       8|       28|      8|       1|    1|
|   438|       0|       8|       29|      3|       2|    1|
|   439|      21|       8|       27|      7|       1|    1|
|   440|       0|       7|       23|      6|       1|    1|
|   441|       3|       8|       23|      5|       2|    1|
|   442|       1|       8|       20|      1|       2|    1|
|   443|       9|       9|       24|     10|       1|    1|
|   444|       3|       8|       26|      3|       2|    1|
|   445|       3|       7|       23|      1|       2|    1|
|   446|       5|       8|       25|      4|       1|    1|
|   448|       4|       9|       16|      4|       2|    1|
|   451|      16|       5|       29|      1|       1|    1|
|   452|       3|       9|       25|      9|       1|    1|
|   454|       3|       9|       29|      1|       2|    1|
|   455|       2|       6|       23|      3|       2|    1|
|   456|       2|       8|       26|      3|       2|    2|
|   457|       3|       7|       19|      5|       2|    2|
|   458|       7|       7|       27|      5|       1|    1|
|   459|       2|       8|       21|      4|       2|    2|
|   460|       2|       7|       18|      6|       2|    1|
|   461|       2|       7|       19|      4|       2|    1|
|   462|       2|       7|       23|      3|       2|    2|
|   463|       5|      10|       19|      7|       1|    2|
|   464|       3|       7|       21|      6|       2|    1|
|   465|       2|       6|       23|      5|       2|    1|
|   466|       4|       9|       28|      3|       2|    1|
|   467|       1|       7|       24|      6|       2|    1|
|   469|       3|       8|       27|      2|       2|    1|
|   472|       6|       9|       27|     10|       1|    1|
|   473|       4|       8|       25|      4|       1|    1|
|   474|      30|       8|       29|      4|       1|    1|
|   476|       0|       7|       19|      5|       2|    1|
|   479|      25|       7|       27|      3|       2|    1|
|   480|       0|       7|       31|      3|       1|    1|
|   481|       5|       8|       18|      2|       2|    1|
|   482|       3|       8|       23|      8|       1|    1|
|   483|       2|       9|       32|      4|       1|    1|
|   484|       2|       7|       20|      7|       1|    1|
|   485|       9|       8|       21|      3|       2|    1|
|   487|      13|       6|       21|      4|       2|    1|
|   488|       1|       8|       22|      3|       2|    1|
|   489|       4|       9|       29|      4|       2|    1|
|   490|       4|       7|       23|      1|       1|    1|
|   491|       3|       9|       20|      1|       2|    1|
|   492|       7|       9|       21|      1|       2|    1|
|   493|       7|       8|       26|      1|       2|    1|
|   494|      14|       6|       21|      2|       2|    1|
|   495|       4|       7|       24|      1|       2|    1|
|   496|      15|       6|       26|      6|       2|    1|
|   497|      37|       9|       30|      2|       2|    1|
|   498|       2|       7|       18|      2|       2|    1|
|   499|       4|       7|       22|      2|       2|    1|
|   500|       6|       9|       25|      4|       2|    1|
|   501|      10|       6|       21|      4|       2|    1|
|   502|      56|       8|       19|      3|       2|    1|
|   503|       3|      10|       23|      5|       2|    1|
|   504|       0|       9|       22|      1|       2|    1|
|   505|      18|       7|       21|      1|       2|    1|
|   506|       3|       9|       22|      3|       2|    1|
|   507|      18|       7|       24|      4|       2|    1|
|   508|       7|       8|       18|      4|       2|    1|
|   509|      29|       8|       18|      5|       2|    1|
|   510|       0|       5|       24|      4|       2|    1|
|   511|       5|       6|       25|      8|       1|    1|
|   514|       6|       7|       29|      2|       2|    1|
|   515|      21|       9|       22|      2|       2|    1|
|   516|       1|       8|       25|      1|       2|    1|
|   517|       3|       7|       27|      3|       2|    1|
|   518|       3|       8|       26|      6|       1|    2|
|   519|       0|       7|       28|      1|       2|    1|
|   520|      13|      10|       19|      2|       2|    1|
|   521|       5|       6|       23|      9|       1|    1|
|   522|       5|       8|       33|      4|       2|    1|
|   523|      37|       7|       24|     10|       1|    1|
|   524|       2|       8|       28|      4|       2|    1|
|   525|      11|       8|       28|      4|       2|    1|
|   526|      13|       7|       24|      9|       2|    1|
|   527|       2|       9|       21|      4|       2|    1|
|   528|       4|       9|       31|      3|       2|    1|
|   529|      21|       9|       23|      2|       2|    1|
|   530|       2|      10|       26|      4|       2|    1|
|   533|       4|       8|       26|      3|       2|    1|
|   534|       3|       8|       26|      3|       2|    2|
|   535|       3|       8|       24|      4|       2|    1|
|   536|      12|       8|       18|      3|       2|    1|
|   538|       1|       7|       21|      5|       1|    1|
|   539|       4|       7|       19|      2|       2|    1|
|   540|      13|       7|       17|      8|       1|    1|
|   547|       2|       7|       24|      1|       2|    1|
|   548|      81|       8|       24|      9|       1|    1|
|   549|      12|       7|       31|     10|       1|    1|
|   550|       2|       8|       19|      6|       2|    1|
|   551|      16|       7|       28|      4|       2|    1|
|   553|       2|       5|       26|      5|       2|    1|
|   554|       2|       7|       26|      2|       1|    1|
|   555|       8|       8|       18|      6|       2|    1|
|   556|       2|       8|       16|      4|       2|    1|
|   557|       4|       8|       29|      1|       2|    1|
|   558|       3|       8|       14|      4|       2|    1|
|   559|      19|       7|       23|      4|       2|    1|
|   560|       4|       9|       29|      1|       2|    1|
|   567|       1|       7|       19|      3|       2|    1|
|   569|      15|       6|       20|      1|       2|    1|
|   571|       4|       8|       21|      1|       2|    1|
|   572|      13|       8|       23|      6|       2|    1|
|   573|       6|       9|       26|      1|       2|    1|
|   574|       1|      10|       18|     10|       1|    1|
|   575|       3|       9|       18|      1|       2|    1|
|   576|       3|       9|       28|      4|       2|    1|
|   577|       0|       9|       31|      2|       2|    1|
|   578|      22|       7|       22|      2|       2|    1|
|   579|       4|       8|       28|      3|       2|    1|
|   580|      14|       7|       20|      2|       1|    1|
|   581|       6|       9|       20|      7|       1|    1|
|   582|      16|       7|       26|      7|       2|    2|
|   583|       6|       6|       27|      1|       2|    1|
|   585|       8|       6|       26|      7|       2|    1|
|   586|       0|       7|       27|      6|       2|    1|
|   587|       4|       7|       16|      4|       2|    1|
|   588|       5|       6|       12|      4|       2|    1|
|   589|       2|       8|       20|      3|       2|    1|
|   590|       6|       8|       19|      6|       1|    1|
|   591|      11|       8|       26|      7|       2|    1|
|   592|       1|       7|       20|      3|       2|    1|
|   593|      23|       7|       22|      7|       1|    1|
|   683|       4|       8|       30|      6|       2|    1|
|   685|       4|       9|       25|      1|       2|    1|
|   686|      16|       9|       23|      6|       1|    1|
|   687|       6|       7|       22|      8|       1|    1|
|   688|       1|       9|       24|      5|       2|    1|
|   689|       2|       6|       23|      4|       1|    1|
|   690|       6|       9|       27|      3|       2|    1|
|   691|       6|       6|       28|      3|       2|    1|
|   706|       3|       7|       24|      3|       2|    1|
|   707|       1|       7|       17|      4|       2|    1|
|   708|      15|       9|       22|     10|       2|    1|
|   709|       3|       6|       23|      4|       2|    1|
|   710|       7|       8|       16|      3|       2|    1|
|   711|       9|       6|       18|      2|       2|    1|
|   717|      18|       6|       21|      4|       2|    1|
|   724|      14|       9|       19|      3|       2|    1|
|   754|       3|       6|       17|      3|       2|    1|
|   755|       4|       8|       17|      3|       2|    1|
|   756|      15|      10|       19|      8|       2|    1|
|   757|       4|       8|       23|      2|       2|    1|
|   758|       3|       8|       18|      4|       2|    1|

</div>

### Çiftler bazında silme

**generalCorr paketi ile**


```r
library(generalCorr)
```

```
## Loading required package: np
```

```
## Warning in .recacheSubclasses(def@className, def, env): undefined subclass
## "ndiMatrix" of class "replValueSp"; definition not updated
```

```
## Nonparametric Kernel Methods for Mixed Datatypes (version 0.60-17)
## [vignette("np_faq",package="np") provides answers to frequently asked questions]
## [vignette("np",package="np") an overview]
## [vignette("entropy_np",package="np") an overview of entropy-based methods]
```

```
## Loading required package: xtable
```

```
## Loading required package: meboot
```

```
## Loading required package: dynlm
```

```
## Loading required package: zoo
```

```
## 
## Attaching package: 'zoo'
```

```
## The following objects are masked from 'package:base':
## 
##     as.Date, as.Date.numeric
```

```
## Loading required package: nlme
```

```
## 
## Attaching package: 'nlme'
```

```
## The following object is masked from 'package:dplyr':
## 
##     collapse
```

```
## Loading required package: tdigest
```

```
## Loading required package: hdrcde
```

```
## This is hdrcde 3.4
```

```
## Loading required package: lattice
```

```r
x=sample(1:10);y=sample(1:10);x[2]=NA; y[3]=NA
x
y
cbind(x,y)
 napair(x,y)

napair(screen$attdrug,screen$atthouse)


rowMeans(screen)
rowMeans(screen, na.rm=TRUE)
cov(screen, use="pairwise.complete.obs")

data <- airquality[, c("Ozone", "Solar.R", "Wind")]
mu <- colMeans(data, na.rm = TRUE)
cv <- cov(data, use = "pairwise")

library(lavaan)
```

```
## This is lavaan 0.6-17
## lavaan is FREE software! Please report any bugs.
```

```
## 
## Attaching package: 'lavaan'
```

```
## The following object is masked from 'package:psych':
## 
##     cor2cov
```

```r
fit <- lavaan("Ozone ~ 1 + Wind + Solar.R
              Ozone ~~ Ozone",
             sample.mean = mu, sample.cov = cv,
             sample.nobs = sum(complete.cases(data)))
     lm(Ozone ~ 1 + Wind + Solar.R,data=airquality)      
summary(fit)

## https://stefvanbuuren.name/fimd/sec-simplesolutions.html
```

```
##  [1] 10 NA  3  9  2  4  8  7  5  1
##  [1]  1  2 NA  9 10  7  8  4  3  6
##        x  y
##  [1,] 10  1
##  [2,] NA  2
##  [3,]  3 NA
##  [4,]  9  9
##  [5,]  2 10
##  [6,]  4  7
##  [7,]  8  8
##  [8,]  7  4
##  [9,]  5  3
## [10,]  1  6
## $newx
## [1] 10  9  2  4  8  7  5  1
## 
## $newy
## [1]  1  9 10  7  8  4  3  6
## 
## $newx
##   [1]  8  7  8  9  7  8  7  7  7  8  9  6  6  8  5  8  8  6  8 10  6  5  8  8  9
##  [26]  9  8  9  6  9  7  6  9  7  9  7  8  8  8  7  8  9  7  8  6  8  9  8  7  9
##  [51]  6  9  9  9  6  7  7 10  8  8 10  8  8  6  6  7  7  8  8  7  5  8 10  7  8
##  [76]  9  7  8  8  8  8  9  9  7  7  8  7  9  6  9  7  7  8  7  8  7  9  6 10  6
## [101]  8  8  9  5 10  8  6  6  6  8  8  7  9  9  8  7  5  8  7  7  7  9  7  7  9
## [126]  6 10  7  5  7  8  8  6  9  6  8  7  8  6  7  8 10  5  7  8  8  7  8  8  8
## [151]  9  9  7  8  9 10  8  7  7  8  8  6  8  8  8  6  8 10  7  8  9  7  8  7  7
## [176]  7  5  9  8 10  9  8  6  8  8  9  9  9  8  7  8  7  9  7  7  7  7  9  8  9
## [201]  8  7 10  9  7  9  9  8  6  5  9  8  8  7  6  7  8  8 10  7  6  9  7  7  7
## [226]  8  9  5  7  7  8  6 10  6  8  8  7  7  6  9  9  9  7  8  8  8 10  7  7  8
## [251]  7  9  7  7  9  9  8  9  9  8  6  6  7  8  8  8  6  8  8  7  8  7  6  8  8
## [276]  9  7  6  9  8  9  7  6  9  9  8  7  7  9  9  6  7  7  8  9 10  7  8  9  6
## [301]  7  8  9  8  6  6  6  9  9  8  8  8  7  8  8  9  8  7  8  8  9  5  9  9  9
## [326]  6  8  7  7  8  7  7  7 10  7  6  9  7  8  9  8  8  7  7  7  8  8  9  7  8
## [351]  8  6  8  9  7  9  9  8  6  7  6  9  7  7  9  6  8 10  9  7  9  7  8  8  5
## [376]  6  8  7  7  9  8  7  8  7 10  6  8  7  8  8  7  9  9  9 10  8  8  8  8  7
## [401]  7  7  7  8  7  8  7  8  5  7  8  8  8  8  7  9  7  7  6  8  8  8  9 10  9
## [426]  9  9  7  8  7  9  7  6  8  6  7  7  6  8  8  8  7  7  8  9  9  7  9  6  9
## [451]  6  7  7  9  6  8  6  6  9  6  8 10  8  8
## 
## $newy
##   [1] 27 20 23 28 24 25 30 24 20 30 15 22 19 25 17 19 22 21 28 25 19 31 25 26 26
##  [26] 33 20 26 27 30 31 25 27 23 15 32 21 28 16 24 19 23 24 31 29 23 24 25 22 26
##  [51] 27 24 27 24 23 25 22 25 15 28 25 22 21 29 26 23 26 21 22 31 35 28 28 20 24
##  [76] 25 25 16 18 23 24 29 16 22 27 23 26 30 23 26 30 25 22 21 16 23 25 20 18 23
## [101] 17 23 23 22 22 28 17 27 18 26 21 21 24 26 21 18 24 28 26 25 23 22 19 26 29
## [126] 20 20 19 24 21 32 18 22 27 21 19 25 17 25 31 16 17 14 30 25 28 22 24 21 25
## [151] 27 20 26 25 27 23 27 21 22 19 26 25 34 27 22 14 29 29 26 17 22 20 30 17 21
## [176] 22 29 22 20 30 25 18 19 24 22 21 29 22 26 19 23 19 31 26 23 29 22 29 23 17
## [201] 26 25 25 19 16 24 29 20 25 16 35 24 19 25 29 25 29 20 25 31 33 24 26 20 17
## [226] 25 24 27 25 28 29 27 24 23 19 23 24 21 22 21 30 32 21 19 24 28 28 29 29 23
## [251] 26 22 25 19 22 18 25 23 19  2 26 24 30 30 22 26 30 25 24 23 18 23 34 23 13
## [276] 27 21 28 28 20 21 25 27 23 25 22 24 15 33 24 29 21 26 29 21 21 18  2 28 20
## [301] 27 23 27 27 25 22 29 23 14 28 29 27 23 23 20 24 26 23 25 28 16 29 25 22 29
## [326] 23 26 19 27 21 18 19 23 19 21 23 28 24 27 27 25 29 19 27 31 18 23 32 20 21
## [351] 25 21 22 29 23 20 21 26 21 24 26 30 18 22 25 21 19 23 22 21 22 24 18 18 24
## [376] 25 19 27 29 22 25 27 26 28 19 23 33 24 28 28 24 21 31 23 26 26 26 24 18 21
## [401] 19 17 24 24 31 19 28 23 26 26 18 16 29 14 23 29 19 22 20 22 21 23 26 18 18
## [426] 28 31 22 28 20 20 26 27 11 26 27 16 12 20 19 26 20 22 30 25 23 22 24 23 27
## [451] 28 24 17 22 23 16 18 21 19 17 17 19 23 18
## 
##   [1]   6.428571   5.857143   5.714286   9.285714   7.857143   7.000000
##   [7]   7.857143   6.857143   6.857143   8.857143   8.571429   6.571429
##  [13]   6.857143   9.857143   6.142857   7.428571   8.000000   8.571429
##  [19]   9.857143  10.857143   8.571429  10.571429   9.571429   9.428571
##  [25]  11.714286  11.857143   9.857143  11.857143  10.428571  11.714286
##  [31]  12.000000  11.571429  11.857143  11.142857  12.000000  13.142857
##  [37]  12.285714  13.285714  11.142857  20.428571  12.142857  13.428571
##  [43]  13.285714  14.285714  13.285714  13.285714  13.714286  15.142857
##  [49]  13.285714  16.000000  17.000000         NA  16.285714  18.714286
##  [55]  15.142857  16.428571  15.857143  18.285714  16.000000  18.428571
##  [61]  16.857143  17.285714  16.857143         NA  17.571429  16.714286
##  [67]  22.714286  17.000000         NA  19.857143  19.142857  19.285714
##  [73]  21.428571  17.714286  20.571429  23.142857         NA  18.428571
##  [79]  23.857143  20.428571  20.857143  22.000000  19.000000  22.428571
##  [85]  21.571429  21.142857  21.285714  22.857143  22.000000  25.000000
##  [91]  23.428571  21.285714  22.571429  22.571429  20.857143  26.857143
##  [97]  22.857143  21.714286  21.714286  23.714286  22.571429  27.285714
## [103]  24.428571  22.571429  24.571429  24.571429  22.428571  26.000000
## [109]  24.142857  26.571429  25.857143  25.000000  25.714286  25.571429
## [115]  24.857143  25.428571  30.000000         NA  27.285714  27.142857
## [121]  26.571429  25.857143  25.428571  26.857143  27.142857  27.142857
## [127]  28.285714  26.428571  28.571429  27.571429  30.428571  26.571429
## [133]  26.571429  29.000000         NA  28.142857  28.714286  27.857143
## [139]  28.857143  30.857143  28.857143  33.857143  30.428571  35.285714
## [145]  32.714286  34.285714  33.000000  37.428571  34.000000  38.714286
## [151]  36.285714  35.714286  35.000000  36.428571  37.428571  39.000000
## [157]  36.714286  37.285714  38.000000  38.714286         NA  39.285714
## [163]  46.857143  39.000000  38.428571  37.714286  40.000000  48.571429
## [169]  41.142857  42.714286  40.285714         NA         NA         NA
## [175]  39.428571  40.285714  41.428571  43.857143  40.714286  43.142857
## [181]         NA  40.428571  40.571429  41.714286  42.000000  42.714286
## [187]  42.428571  42.428571  43.714286  42.285714  44.000000  42.714286
## [193]  51.714286  43.857143  45.571429         NA  43.142857  45.428571
## [199]  46.285714  43.285714  45.714286  44.714286         NA  43.857143
## [205]  43.285714  53.000000  47.428571  45.142857  47.571429  44.285714
## [211]  50.142857  47.428571  53.428571  48.857143  48.571429  47.285714
## [217]  48.428571  48.285714  50.142857  49.714286  50.285714  49.000000
## [223]  49.428571  48.857143  48.428571  50.285714  49.714286  51.142857
## [229]  51.428571  50.857143  50.714286  50.857143  51.714286  51.571429
## [235]  49.857143         NA  51.857143  52.285714  51.714286         NA
## [241]  54.285714  52.857143  51.714286  53.142857  52.571429  53.857143
## [247]  57.000000  54.428571  61.571429  54.857143  54.000000  53.428571
## [253]         NA  55.000000  53.857143  54.714286  53.857143         NA
## [259]  55.857143  55.142857  51.571429  55.428571  55.428571  56.714286
## [265]  57.857143  56.428571  57.428571  59.285714  57.857143  57.571429
## [271]  57.571429  57.000000  58.142857  59.571429  66.428571  57.000000
## [277]  62.428571  60.428571  66.428571  61.714286  60.428571  59.571429
## [283]  60.857143  62.571429  61.857143  66.428571  61.142857  63.571429
## [289]  61.428571  75.142857  63.857143  64.000000  62.571429  63.285714
## [295]  64.285714  63.285714  63.142857  63.142857  60.714286  66.428571
## [301]  64.142857  65.714286  66.000000         NA  66.428571  66.285714
## [307]  66.428571  67.857143  69.571429  70.571429  69.428571  68.714286
## [313]  72.000000  68.285714  69.000000  67.857143  71.000000  69.571429
## [319]  68.857143  70.000000         NA  69.142857  72.000000  71.428571
## [325]         NA  71.285714  70.285714  71.285714  70.714286  72.285714
## [331]  71.142857  70.857143  70.857143  71.571429  72.428571  72.000000
## [337]  72.000000  73.285714  72.571429  73.142857  75.142857  73.714286
## [343]  78.142857  72.857143  77.714286  74.714286  73.857143  75.142857
## [349]  76.000000  74.571429  75.571429         NA  76.285714  75.000000
## [355]  76.857143  75.285714  75.285714  76.142857  76.857143  77.142857
## [361]  76.285714  78.857143  82.571429  75.714286  76.714286  78.142857
## [367]  77.857143  84.428571  78.142857  77.000000  79.285714  78.000000
## [373]  80.428571  78.285714  81.714286  78.000000  79.571429         NA
## [379]         NA  80.142857  81.714286  79.142857  80.000000  80.571429
## [385]  79.714286  81.000000  80.857143  82.142857  86.142857  81.285714
## [391]  82.714286  83.142857  80.857143  82.571429  83.857143  82.142857
## [397]  82.428571  82.571429  82.428571  82.857143  82.000000  82.000000
## [403]  83.857143  83.428571  96.000000  87.285714  84.000000  87.000000
## [409]         NA  84.857143  84.714286  85.428571  84.142857  86.000000
## [415]  84.285714  87.857143  86.571429  85.714286         NA  87.714286
## [421]         NA  86.857143  89.285714  88.285714  87.857143  87.000000
## [427]  89.000000  88.857143  90.571429  89.285714  89.285714  89.285714
## [433]  91.714286  89.428571         NA  90.714286  89.857143  88.714286
## [439]  88.285714  89.285714  90.142857  92.285714  89.428571  93.428571
## [445] 104.857143 103.857143 106.000000 104.571429 104.285714 103.714286
## [451] 105.428571 105.285714 106.571429 105.571429 109.571429 106.857143
## [457] 106.714286 107.000000 109.857143 110.285714 112.285714 112.857143
## [463] 115.857143 113.857143 113.428571
##   [1]   6.428571   5.857143   5.714286   9.285714   7.857143   7.000000
##   [7]   7.857143   6.857143   6.857143   8.857143   8.571429   6.571429
##  [13]   6.857143   9.857143   6.142857   7.428571   8.000000   8.571429
##  [19]   9.857143  10.857143   8.571429  10.571429   9.571429   9.428571
##  [25]  11.714286  11.857143   9.857143  11.857143  10.428571  11.714286
##  [31]  12.000000  11.571429  11.857143  11.142857  12.000000  13.142857
##  [37]  12.285714  13.285714  11.142857  20.428571  12.142857  13.428571
##  [43]  13.285714  14.285714  13.285714  13.285714  13.714286  15.142857
##  [49]  13.285714  16.000000  17.000000  19.166667  16.285714  18.714286
##  [55]  15.142857  16.428571  15.857143  18.285714  16.000000  18.428571
##  [61]  16.857143  17.285714  16.857143  23.333333  17.571429  16.714286
##  [67]  22.714286  17.000000  20.666667  19.857143  19.142857  19.285714
##  [73]  21.428571  17.714286  20.571429  23.142857  22.333333  18.428571
##  [79]  23.857143  20.428571  20.857143  22.000000  19.000000  22.428571
##  [85]  21.571429  21.142857  21.285714  22.857143  22.000000  25.000000
##  [91]  23.428571  21.285714  22.571429  22.571429  20.857143  26.857143
##  [97]  22.857143  21.714286  21.714286  23.714286  22.571429  27.285714
## [103]  24.428571  22.571429  24.571429  24.571429  22.428571  26.000000
## [109]  24.142857  26.571429  25.857143  25.000000  25.714286  25.571429
## [115]  24.857143  25.428571  30.000000  30.500000  27.285714  27.142857
## [121]  26.571429  25.857143  25.428571  26.857143  27.142857  27.142857
## [127]  28.285714  26.428571  28.571429  27.571429  30.428571  26.571429
## [133]  26.571429  29.000000  31.000000  28.142857  28.714286  27.857143
## [139]  28.857143  30.857143  28.857143  33.857143  30.428571  35.285714
## [145]  32.714286  34.285714  33.000000  37.428571  34.000000  38.714286
## [151]  36.285714  35.714286  35.000000  36.428571  37.428571  39.000000
## [157]  36.714286  37.285714  38.000000  38.714286  44.500000  39.285714
## [163]  46.857143  39.000000  38.428571  37.714286  40.000000  48.571429
## [169]  41.142857  42.714286  40.285714  46.166667  48.833333  45.000000
## [175]  39.428571  40.285714  41.428571  43.857143  40.714286  43.142857
## [181]  48.000000  40.428571  40.571429  41.714286  42.000000  42.714286
## [187]  42.428571  42.428571  43.714286  42.285714  44.000000  42.714286
## [193]  51.714286  43.857143  45.571429  53.000000  43.142857  45.428571
## [199]  46.285714  43.285714  45.714286  44.714286  55.666667  43.857143
## [205]  43.285714  53.000000  47.428571  45.142857  47.571429  44.285714
## [211]  50.142857  47.428571  53.428571  48.857143  48.571429  47.285714
## [217]  48.428571  48.285714  50.142857  49.714286  50.285714  49.000000
## [223]  49.428571  48.857143  48.428571  50.285714  49.714286  51.142857
## [229]  51.428571  50.857143  50.714286  50.857143  51.714286  51.571429
## [235]  49.857143  59.666667  51.857143  52.285714  51.714286  59.166667
## [241]  54.285714  52.857143  51.714286  53.142857  52.571429  53.857143
## [247]  57.000000  54.428571  61.571429  54.857143  54.000000  53.428571
## [253]  59.000000  55.000000  53.857143  54.714286  53.857143  63.833333
## [259]  55.857143  55.142857  51.571429  55.428571  55.428571  56.714286
## [265]  57.857143  56.428571  57.428571  59.285714  57.857143  57.571429
## [271]  57.571429  57.000000  58.142857  59.571429  66.428571  57.000000
## [277]  62.428571  60.428571  66.428571  61.714286  60.428571  59.571429
## [283]  60.857143  62.571429  61.857143  66.428571  61.142857  63.571429
## [289]  61.428571  75.142857  63.857143  64.000000  62.571429  63.285714
## [295]  64.285714  63.285714  63.142857  63.142857  60.714286  66.428571
## [301]  64.142857  65.714286  66.000000  77.500000  66.428571  66.285714
## [307]  66.428571  67.857143  69.571429  70.571429  69.428571  68.714286
## [313]  72.000000  68.285714  69.000000  67.857143  71.000000  69.571429
## [319]  68.857143  70.000000  82.000000  69.142857  72.000000  71.428571
## [325]  83.333333  71.285714  70.285714  71.285714  70.714286  72.285714
## [331]  71.142857  70.857143  70.857143  71.571429  72.428571  72.000000
## [337]  72.000000  73.285714  72.571429  73.142857  75.142857  73.714286
## [343]  78.142857  72.857143  77.714286  74.714286  73.857143  75.142857
## [349]  76.000000  74.571429  75.571429  87.333333  76.285714  75.000000
## [355]  76.857143  75.285714  75.285714  76.142857  76.857143  77.142857
## [361]  76.285714  78.857143  82.571429  75.714286  76.714286  78.142857
## [367]  77.857143  84.428571  78.142857  77.000000  79.285714  78.000000
## [373]  80.428571  78.285714  81.714286  78.000000  79.571429  91.000000
## [379]  92.166667  80.142857  81.714286  79.142857  80.000000  80.571429
## [385]  79.714286  81.000000  80.857143  82.142857  86.142857  81.285714
## [391]  82.714286  83.142857  80.857143  82.571429  83.857143  82.142857
## [397]  82.428571  82.571429  82.428571  82.857143  82.000000  82.000000
## [403]  83.857143  83.428571  96.000000  87.285714  84.000000  87.000000
## [409] 102.166667  84.857143  84.714286  85.428571  84.142857  86.000000
## [415]  84.285714  87.857143  86.571429  85.714286 100.500000  87.714286
## [421] 101.000000  86.857143  89.285714  88.285714  87.857143  87.000000
## [427]  89.000000  88.857143  90.571429  89.285714  89.285714  89.285714
## [433]  91.714286  89.428571 101.166667  90.714286  89.857143  88.714286
## [439]  88.285714  89.285714  90.142857  92.285714  89.428571  93.428571
## [445] 104.857143 103.857143 106.000000 104.571429 104.285714 103.714286
## [451] 105.428571 105.285714 106.571429 105.571429 109.571429 106.857143
## [457] 106.714286 107.000000 109.857143 110.285714 112.285714 112.857143
## [463] 115.857143 113.857143 113.428571
##                 subno     timedrs       attdrug      atthouse        income
## subno    37699.637125  38.7834260  2.964600e+00 -79.663904819 -1.769448e+00
## timedrs     38.783426 119.8695032  1.320166e+00   6.312392940  1.425901e+00
## attdrug      2.964600   1.3201659  1.336550e+00   0.116407239 -7.280973e-05
## atthouse   -79.663905   6.3123929  1.164072e-01  20.101991323  3.641474e-03
## income      -1.769448   1.4259005 -7.280973e-05   0.003641474  5.850958e+00
## mstatus     -1.978003  -0.2978217 -2.887468e-03  -0.057933641 -4.731800e-01
## race        -4.374152  -0.1076381  6.192065e-03  -0.046734192  6.629326e-02
##               mstatus         race
## subno    -1.978003337 -4.374151835
## timedrs  -0.297821654 -0.107638116
## attdrug  -0.002887468  0.006192065
## atthouse -0.057933641 -0.046734192
## income   -0.473180017  0.066293257
## mstatus   0.172812384 -0.004134223
## race     -0.004134223  0.080571005
## 
## Call:
## lm(formula = Ozone ~ 1 + Wind + Solar.R, data = airquality)
## 
## Coefficients:
## (Intercept)         Wind      Solar.R  
##     77.2460      -5.4018       0.1004  
## 
## lavaan 0.6.17 ended normally after 1 iteration
## 
##   Estimator                                         ML
##   Optimization method                           NLMINB
##   Number of model parameters                         4
## 
##   Number of observations                           111
## 
## Model Test User Model:
##                                                       
##   Test statistic                                 0.000
##   Degrees of freedom                                 0
## 
## Parameter Estimates:
## 
##   Standard errors                             Standard
##   Information                                 Expected
##   Information saturated (h1) model          Structured
## 
## Regressions:
##                    Estimate  Std.Err  z-value  P(>|z|)
##   Ozone ~                                             
##     Wind             -5.545    0.644   -8.605    0.000
##     Solar.R           0.118    0.025    4.681    0.000
## 
## Intercepts:
##                    Estimate  Std.Err  z-value  P(>|z|)
##    .Ozone            75.402    8.463    8.910    0.000
## 
## Variances:
##                    Estimate  Std.Err  z-value  P(>|z|)
##    .Ozone           565.035   75.845    7.450    0.000
```

**regtools paketi**



### Ortalama atama

Tek bir değişkene ortalama atama


```r
df = data.frame(x = 1:20, y = c(1:10,rep(NA,10)))
df$y[is.na(df$y)] = mean(df$y, na.rm=TRUE)

screen3 <- screen
screen3$income[is.na(screen3$income)]<- mean(screen3$income, na.rm=TRUE)
summary(screen3$income)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    1.00    3.00    4.00    4.21    6.00   10.00
```

Bu işlem birden farklı şekilde yapılabilir. Her bir sütunda eksik veriyi ortalama ile tamamlama


```r
screen4 <- screen[,2:5]
for(i in 1:ncol(screen4)) {
screen4[ , i][is.na(screen4[ , i])] <- mean(screen4[ , i], na.rm = TRUE)
}
```

**if_else()** ile


```r
# df = transform(df, y = ifelse(is.na(y), mean(y, na.rm=TRUE), y))
screen5 <- screen

screen5 = transform(screen5, income = ifelse(is.na(income), mean(income, na.rm=TRUE), income))
summary(screen5)
```

```
##      subno          timedrs          attdrug          atthouse    
##  Min.   :  1.0   Min.   : 0.000   Min.   : 5.000   Min.   : 2.00  
##  1st Qu.:137.0   1st Qu.: 2.000   1st Qu.: 7.000   1st Qu.:21.00  
##  Median :314.0   Median : 4.000   Median : 8.000   Median :24.00  
##  Mean   :317.4   Mean   : 7.901   Mean   : 7.686   Mean   :23.54  
##  3rd Qu.:483.0   3rd Qu.:10.000   3rd Qu.: 9.000   3rd Qu.:27.00  
##  Max.   :758.0   Max.   :81.000   Max.   :10.000   Max.   :35.00  
##                                                    NA's   :1      
##      income         mstatus           race      
##  Min.   : 1.00   Min.   :1.000   Min.   :1.000  
##  1st Qu.: 3.00   1st Qu.:2.000   1st Qu.:1.000  
##  Median : 4.00   Median :2.000   Median :1.000  
##  Mean   : 4.21   Mean   :1.778   Mean   :1.088  
##  3rd Qu.: 6.00   3rd Qu.:2.000   3rd Qu.:1.000  
##  Max.   :10.00   Max.   :2.000   Max.   :2.000  
## 
```

**mutate() ile**


```r
screen %>%  
mutate(income = ifelse(is.na(income), mean(income, na.rm =TRUE), income))
```

<div class="kable-table">

| subno| timedrs| attdrug| atthouse|    income| mstatus| race|
|-----:|-------:|-------:|--------:|---------:|-------:|----:|
|     1|       1|       8|       27|  5.000000|       2|    1|
|     2|       3|       7|       20|  6.000000|       2|    1|
|     3|       0|       8|       23|  3.000000|       2|    1|
|     4|      13|       9|       28|  8.000000|       2|    1|
|     5|      15|       7|       24|  1.000000|       2|    1|
|     6|       3|       8|       25|  4.000000|       2|    1|
|     7|       2|       7|       30|  6.000000|       2|    1|
|     8|       0|       7|       24|  6.000000|       2|    1|
|     9|       7|       7|       20|  2.000000|       2|    1|
|    10|       4|       8|       30|  8.000000|       1|    1|
|    11|      15|       9|       15|  7.000000|       2|    1|
|    12|       0|       6|       22|  3.000000|       2|    1|
|    13|       2|       6|       19|  5.000000|       2|    1|
|    14|      13|       8|       25|  6.000000|       2|    1|
|    15|       2|       5|       17|  1.000000|       2|    1|
|    16|       2|       8|       19|  3.000000|       2|    2|
|    21|       1|       8|       22|  1.000000|       2|    1|
|    22|       2|       6|       21|  7.000000|       1|    1|
|    23|       5|       8|       28|  2.000000|       2|    1|
|    24|       5|      10|       25|  9.000000|       2|    1|
|    25|       3|       6|       19|  4.000000|       2|    1|
|    26|       4|       5|       31|  5.000000|       2|    1|
|    27|       2|       8|       25|  2.000000|       2|    1|
|    28|       0|       8|       26|  1.000000|       2|    1|
|    29|      13|       9|       26|  2.000000|       2|    1|
|    30|       7|       9|       33|  1.000000|       2|    1|
|    31|       2|       8|       20|  5.000000|       2|    1|
|    32|      12|       9|       26|  1.000000|       2|    1|
|    33|       2|       6|       27|  3.000000|       1|    1|
|    34|       5|       9|       30|  1.000000|       2|    1|
|    35|       4|       7|       31|  4.000000|       2|    1|
|    36|       6|       6|       25|  5.000000|       2|    1|
|    37|       2|       9|       27|  5.000000|       2|    1|
|    38|       3|       7|       23|  4.000000|       2|    1|
|    39|      14|       9|       15|  4.000000|       2|    1|
|    40|       7|       7|       32|  3.000000|       2|    1|
|    45|       0|       8|       21|  9.000000|       1|    2|
|    46|       1|       8|       28|  6.000000|       2|    2|
|    47|       3|       8|       16|  1.000000|       2|    1|
|    48|      60|       7|       24|  1.000000|       2|    1|
|    49|       5|       8|       19|  1.000000|       2|    1|
|    50|       3|       9|       23|  6.000000|       2|    1|
|    51|       0|       7|       24|  8.000000|       2|    1|
|    52|       3|       8|       31|  3.000000|       2|    1|
|    53|       2|       6|       29|  1.000000|       1|    1|
|    54|       1|       8|       23|  4.000000|       2|    1|
|    55|       1|       9|       24|  4.000000|       2|    1|
|    56|      13|       8|       25|  1.000000|       2|    1|
|    57|       2|       7|       22|  2.000000|       2|    1|
|    65|       5|       9|       26|  5.000000|       1|    1|
|    66|      12|       6|       27|  5.000000|       2|    1|
|    67|      12|       9|       24|  4.209567|       2|    1|
|    68|       1|       9|       27|  6.000000|       2|    1|
|    69|      20|       9|       24|  6.000000|       2|    1|
|    70|       0|       6|       23|  4.000000|       2|    1|
|    71|       5|       7|       25|  4.000000|       2|    1|
|    72|       0|       7|       22|  6.000000|       2|    2|
|    73|       8|      10|       25|  9.000000|       1|    2|
|    74|       9|       8|       15|  3.000000|       2|    1|
|    75|      10|       8|       28|  6.000000|       1|    1|
|    76|       1|      10|       25|  3.000000|       2|    1|
|    77|       5|       8|       22|  7.000000|       1|    1|
|    78|       5|       8|       21|  3.000000|       2|    1|
|    79|      23|       6|       29|  4.209567|       2|    1|
|    80|       7|       6|       26|  1.000000|       1|    2|
|    81|       1|       7|       23|  2.000000|       2|    1|
|    82|      39|       7|       26|  2.000000|       2|    1|
|    83|       2|       8|       21|  3.000000|       1|    1|
|    84|       7|       8|       22|  4.209567|       2|    1|
|    85|       9|       7|       31|  5.000000|       1|    1|
|    86|       0|       5|       35|  5.000000|       2|    1|
|    87|       4|       8|       28|  5.000000|       2|    1|
|    88|      16|      10|       28|  5.000000|       2|    1|
|    89|       0|       7|       20|  5.000000|       2|    1|
|    90|      16|       8|       24|  3.000000|       2|    1|
|    91|      33|       9|       25|  1.000000|       2|    1|
|    95|       4|       7|       25|  4.209567|       2|    1|
|    97|       2|       8|       16|  3.000000|       2|    1|
|    98|      38|       8|       18|  2.000000|       2|    1|
|    99|       8|       8|       23|  2.000000|       2|    1|
|   101|       2|       8|       24|  9.000000|       1|    1|
|   102|       2|       9|       29| 10.000000|       1|    1|
|   103|       0|       9|       16|  1.000000|       2|    2|
|   104|      15|       7|       22|  5.000000|       2|    2|
|   105|       0|       7|       27| 10.000000|       1|    1|
|   106|       1|       8|       23|  8.000000|       1|    1|
|   107|       5|       7|       26|  1.000000|       2|    1|
|   108|       7|       9|       30|  3.000000|       2|    1|
|   109|      10|       6|       23|  3.000000|       2|    1|
|   110|      22|       9|       26|  6.000000|       1|    1|
|   111|      10|       7|       30|  3.000000|       2|    1|
|   112|       0|       7|       25|  2.000000|       2|    1|
|   113|      11|       8|       22|  1.000000|       2|    1|
|   114|       9|       7|       21|  4.000000|       2|    1|
|   115|       0|       8|       16|  4.000000|       2|    1|
|   116|      34|       7|       23|  5.000000|       2|    1|
|   117|       3|       9|       25|  4.000000|       1|    1|
|   118|       1|       6|       20|  5.000000|       1|    1|
|   119|       0|      10|       18|  1.000000|       2|    2|
|   120|      10|       6|       23|  4.000000|       2|    1|
|   121|       4|       8|       17|  4.000000|       2|    2|
|   122|      27|       8|       23|  8.000000|       1|    2|
|   123|       7|       9|       23|  5.000000|       2|    2|
|   124|       1|       5|       22|  2.000000|       2|    2|
|   125|       7|      10|       22|  6.000000|       1|    1|
|   126|       3|       8|       28|  4.000000|       2|    1|
|   127|       2|       6|       17|  2.000000|       2|    1|
|   128|      11|       6|       27|  7.000000|       2|    1|
|   129|       9|       6|       18|  4.000000|       2|    1|
|   130|      11|       8|       26|  7.000000|       2|    2|
|   131|      11|       8|       21|  7.000000|       2|    1|
|   132|       8|       7|       21|  4.000000|       2|    1|
|   133|       2|       9|       24| 10.000000|       1|    1|
|   134|       4|       9|       26|  3.000000|       2|    1|
|   135|       4|       8|       21|  3.000000|       2|    1|
|   136|       6|       7|       18|  8.000000|       2|    1|
|   137|      30|       5|       24| 10.000000|       2|    2|
|   138|       7|       8|       28|  4.209567|       1|    1|
|   139|      15|       7|       26|  1.000000|       2|    1|
|   140|       6|       7|       25|  8.000000|       2|    2|
|   141|       7|       7|       23|  5.000000|       2|    1|
|   142|       2|       9|       22|  3.000000|       2|    1|
|   143|       1|       7|       19|  5.000000|       2|    1|
|   144|       5|       7|       26|  3.000000|       2|    1|
|   145|       1|       9|       29|  3.000000|       2|    1|
|   146|      11|       6|       20|  4.000000|       2|    1|
|   148|      16|      10|       20|  1.000000|       2|    1|
|   149|       2|       7|       19|  5.000000|       2|    1|
|   150|      14|       5|       24|  4.000000|       2|    1|
|   151|       8|       7|       21|  3.000000|       2|    1|
|   152|      17|       8|       32|  1.000000|       2|    1|
|   153|       1|       8|       18|  3.000000|       2|    1|
|   154|       0|       6|       22|  1.000000|       2|    1|
|   155|       1|       9|       27|  8.000000|       1|    2|
|   156|       1|       6|       21|  4.209567|       1|    1|
|   157|      10|       8|       19|  1.000000|       1|    1|
|   158|       7|       7|       25|  1.000000|       2|    1|
|   159|       2|       8|       17|  7.000000|       1|    1|
|   160|       5|       6|       25|  3.000000|       2|    1|
|   166|       6|       7|       31|  4.000000|       1|    1|
|   167|       1|       8|       16|  7.000000|       2|    1|
|   183|      16|      10|       17|  8.000000|       2|    1|
|   184|       0|       5|       14|  8.000000|       1|    1|
|   185|      17|       7|       30|  4.000000|       2|    2|
|   187|       0|       8|       25|  6.000000|       2|    1|
|   190|      10|       8|       28|  1.000000|       2|    1|
|   192|       6|       7|       22|  1.000000|       2|    1|
|   202|      20|       8|       24|  5.000000|       2|    1|
|   203|       1|       8|       21|  2.000000|       2|    1|
|   204|      25|       8|       25|  7.000000|       1|    1|
|   205|       3|       9|       27|  7.000000|       2|    1|
|   206|       6|       9|       20|  7.000000|       1|    1|
|   208|       0|       7|       26|  1.000000|       2|    1|
|   210|       8|       8|       25|  1.000000|       2|    1|
|   212|       9|       9|       27|  3.000000|       1|    1|
|   213|      19|      10|       23|  5.000000|       2|    1|
|   214|       2|       8|       27|  3.000000|       2|    1|
|   225|       2|       7|       21|  3.000000|       2|    1|
|   226|       3|       7|       22|  5.000000|       2|    1|
|   227|       7|       8|       19|  7.000000|       2|    1|
|   228|       2|       8|       26|  4.209567|       2|    1|
|   229|       8|       6|       25|  4.000000|       2|    1|
|   230|      49|       8|       34|  4.000000|       2|    1|
|   231|       2|       8|       27|  1.000000|       2|    2|
|   232|       1|       8|       22|  3.000000|       2|    1|
|   233|       5|       6|       14|  3.000000|       2|    1|
|   234|       5|       8|       29|  1.000000|       2|    1|
|   235|      60|      10|       29|  4.000000|       1|    1|
|   236|      10|       7|       26|  6.000000|       2|    1|
|   237|      27|       8|       17|  7.000000|       2|    1|
|   238|       7|       9|       22|  3.000000|       2|    1|
|   239|       8|       7|       20|  4.209567|       2|    1|
|   240|      12|       8|       30|  4.209567|       2|    1|
|   241|       2|       7|       17|  4.209567|       2|    1|
|   242|       2|       7|       21|  1.000000|       2|    1|
|   243|       4|       7|       22|  3.000000|       2|    1|
|   244|       6|       5|       29|  3.000000|       2|    1|
|   245|      27|       9|       22|  1.000000|       2|    1|
|   246|       0|       8|       20|  9.000000|       1|    1|
|   247|       9|      10|       30|  3.000000|       2|    1|
|   248|       3|       9|       25|  4.209567|       2|    1|
|   249|       0|       8|       18|  5.000000|       2|    1|
|   250|       2|       6|       19|  4.000000|       2|    1|
|   251|       3|       8|       24|  3.000000|       2|    1|
|   252|       5|       8|       22|  4.000000|       2|    1|
|   253|       5|       9|       21|  8.000000|       1|    2|
|   254|       1|       9|       29|  1.000000|       2|    1|
|   255|       7|       9|       22|  1.000000|       2|    1|
|   258|       4|       8|       26|  7.000000|       2|    1|
|   259|       3|       7|       19|  6.000000|       1|    1|
|   260|       7|       8|       23|  8.000000|       1|    1|
|   261|       1|       7|       19|  9.000000|       1|    1|
|   262|      52|       9|       31|  4.000000|       2|    2|
|   263|       6|       7|       26|  2.000000|       2|    1|
|   264|      18|       7|       23|  4.000000|       2|    1|
|   265|      14|       7|       29|  4.209567|       1|    2|
|   266|       0|       7|       22|  4.000000|       2|    1|
|   267|       8|       9|       29|  1.000000|       2|    2|
|   268|      16|       8|       23|  7.000000|       1|    1|
|   269|       2|       9|       17|  3.000000|       2|    1|
|   270|      12|       8|       26|  1.000000|       2|    1|
|   271|       3|       7|       25|  4.000000|       2|    1|
|   272|      24|      10|       25|  4.209567|       2|    1|
|   273|       0|       9|       19|  4.000000|       1|    1|
|   274|       0|       7|       16|  3.000000|       2|    1|
|   276|      57|       9|       24|  2.000000|       2|    1|
|   277|      11|       9|       29|  3.000000|       2|    1|
|   278|       1|       8|       20|  6.000000|       2|    1|
|   279|      18|       6|       25|  2.000000|       2|    1|
|   280|       1|       5|       16|  6.000000|       1|    1|
|   289|      11|       9|       35|  4.000000|       2|    1|
|   290|       0|       8|       24|  7.000000|       2|    1|
|   291|      52|       8|       19|  1.000000|       2|    1|
|   292|      12|       7|       25|  4.000000|       1|    1|
|   293|       6|       6|       29|  3.000000|       2|    1|
|   294|       0|       7|       25|  3.000000|       1|    1|
|   295|       2|       8|       29|  2.000000|       2|    1|
|   299|       2|       8|       20|  6.000000|       2|    1|
|   300|      13|      10|       25|  1.000000|       1|    1|
|   301|       2|       7|       31|  4.000000|       2|    1|
|   302|       3|       6|       33|  5.000000|       2|    1|
|   303|       2|       9|       24|  2.000000|       2|    1|
|   304|       2|       7|       26|  4.000000|       2|    1|
|   305|       1|       7|       20|  7.000000|       1|    1|
|   306|       2|       7|       17|  4.000000|       2|    1|
|   307|       3|       8|       25|  7.000000|       1|    1|
|   308|       1|       9|       24|  3.000000|       2|    1|
|   309|       5|       5|       27| 10.000000|       1|    1|
|   310|       6|       7|       25| 10.000000|       1|    1|
|   311|       1|       7|       28|  7.000000|       1|    1|
|   312|       2|       8|       29|  1.000000|       2|    1|
|   313|       3|       6|       27|  4.000000|       2|    1|
|   314|       7|      10|       24|  4.000000|       2|    1|
|   315|       7|       6|       23|  7.000000|       2|    1|
|   316|       0|       8|       19|  3.000000|       2|    1|
|   317|       7|       8|       23|  4.209567|       2|    1|
|   318|       8|       7|       24|  3.000000|       2|    1|
|   319|       9|       7|       21|  7.000000|       2|    1|
|   320|       8|       6|       22|  3.000000|       2|    1|
|   321|       1|       9|       21|  4.209567|       2|    1|
|   322|      14|       9|       30|  2.000000|       2|    1|
|   323|       2|       9|       32|  1.000000|       2|    1|
|   324|       4|       7|       21|  3.000000|       2|    1|
|   325|       8|       8|       19| 10.000000|       1|    1|
|   326|       3|       8|       24|  4.000000|       2|    1|
|   327|      10|       8|       28|  1.000000|       2|    1|
|   328|      21|      10|       28| 10.000000|       1|    1|
|   329|       6|       7|       29|  7.000000|       2|    1|
|   330|      58|       7|       29|  4.000000|       2|    1|
|   335|      12|       8|       23|  4.000000|       1|    1|
|   336|       5|       7|       26|  1.000000|       2|    1|
|   337|       2|       9|       22|  1.000000|       2|    1|
|   338|       2|       6|       NA|  5.000000|       1|    2|
|   339|       4|       7|       25|  6.000000|       2|    2|
|   340|       2|       7|       19|  5.000000|       2|    2|
|   341|       5|       9|       22|  3.000000|       2|    1|
|   342|       0|       9|       18|  5.000000|       2|    1|
|   343|       3|       8|       25|  4.209567|       2|    2|
|   344|       7|       9|       23|  5.000000|       2|    1|
|   345|       1|       9|       19|  8.000000|       2|    2|
|   346|       2|       8|        2|  1.000000|       1|    1|
|   347|       1|       6|       26|  5.000000|       2|    1|
|   348|       4|       6|       24|  3.000000|       2|    1|
|   349|       4|       7|       30|  4.000000|       2|    1|
|   355|       4|       8|       30|  5.000000|       2|    1|
|   357|       2|       8|       22|  3.000000|       2|    1|
|   358|       0|       8|       26|  7.000000|       2|    1|
|   359|      13|       6|       30|  4.000000|       2|    1|
|   361|       3|       8|       25|  5.000000|       2|    1|
|   362|       1|       8|       24|  5.000000|       2|    1|
|   363|       4|       7|       23|  3.000000|       2|    1|
|   365|       1|       8|       18|  4.000000|       2|    1|
|   367|       3|       7|       23|  4.000000|       2|    1|
|   369|       1|       6|       34|  4.000000|       2|    1|
|   370|      57|       8|       23|  4.000000|       2|    1|
|   372|       1|       8|       13|  1.000000|       2|    2|
|   374|      17|       9|       27|  7.000000|       2|    1|
|   378|      11|       7|       21|  3.000000|       2|    1|
|   379|      43|       6|       28|  7.000000|       1|    1|
|   380|       6|       9|       28|  7.000000|       1|    1|
|   381|       6|       8|       20|  6.000000|       1|    1|
|   382|       1|       9|       21|  1.000000|       2|    1|
|   383|       0|       7|       25|  9.000000|       1|    1|
|   384|      10|       6|       27|  9.000000|       1|    1|
|   385|       3|       9|       23| 10.000000|       2|    1|
|   386|      37|       9|       25|  6.000000|       1|    1|
|   387|       6|       8|       22|  2.000000|       2|    1|
|   392|      11|       7|       24|  9.000000|       1|    1|
|   397|       4|       7|       15|  3.000000|       2|    2|
|   398|      75|       9|       33|  9.000000|       1|    1|
|   399|       7|       9|       24|  5.000000|       2|    1|
|   400|       3|       6|       29|  7.000000|       1|    2|
|   401|       3|       7|       21|  3.000000|       2|    1|
|   402|       3|       7|       26|  2.000000|       2|    1|
|   403|       4|       8|       29|  4.000000|       1|    1|
|   404|       5|       9|       21|  1.000000|       2|    1|
|   405|       2|      10|       21|  1.000000|       2|    1|
|   406|       4|       7|       18|  5.000000|       1|    1|
|   407|       2|       8|        2|  4.000000|       1|    1|
|   413|      11|       9|       28|  2.000000|       1|    1|
|   414|       0|       6|       20|  6.000000|       2|    1|
|   417|       2|       7|       27|  4.000000|       2|    1|
|   418|       4|       8|       23|  6.000000|       2|    1|
|   420|       6|       9|       27|  4.209567|       2|    1|
|   421|       0|       8|       27|  5.000000|       2|    2|
|   424|       2|       6|       25|  4.000000|       2|    1|
|   425|       5|       6|       22|  4.000000|       2|    1|
|   434|       2|       6|       29|  1.000000|       2|    1|
|   435|      10|       9|       23|  7.000000|       2|    1|
|   436|      29|       9|       14|  3.000000|       2|    1|
|   437|       3|       8|       28|  8.000000|       1|    1|
|   438|       0|       8|       29|  3.000000|       2|    1|
|   439|      21|       8|       27|  7.000000|       1|    1|
|   440|       0|       7|       23|  6.000000|       1|    1|
|   441|       3|       8|       23|  5.000000|       2|    1|
|   442|       1|       8|       20|  1.000000|       2|    1|
|   443|       9|       9|       24| 10.000000|       1|    1|
|   444|       3|       8|       26|  3.000000|       2|    1|
|   445|       3|       7|       23|  1.000000|       2|    1|
|   446|       5|       8|       25|  4.000000|       1|    1|
|   447|       6|       8|       28|  4.209567|       2|    1|
|   448|       4|       9|       16|  4.000000|       2|    1|
|   451|      16|       5|       29|  1.000000|       1|    1|
|   452|       3|       9|       25|  9.000000|       1|    1|
|   453|      13|       9|       22|  4.209567|       2|    1|
|   454|       3|       9|       29|  1.000000|       2|    1|
|   455|       2|       6|       23|  3.000000|       2|    1|
|   456|       2|       8|       26|  3.000000|       2|    2|
|   457|       3|       7|       19|  5.000000|       2|    2|
|   458|       7|       7|       27|  5.000000|       1|    1|
|   459|       2|       8|       21|  4.000000|       2|    2|
|   460|       2|       7|       18|  6.000000|       2|    1|
|   461|       2|       7|       19|  4.000000|       2|    1|
|   462|       2|       7|       23|  3.000000|       2|    2|
|   463|       5|      10|       19|  7.000000|       1|    2|
|   464|       3|       7|       21|  6.000000|       2|    1|
|   465|       2|       6|       23|  5.000000|       2|    1|
|   466|       4|       9|       28|  3.000000|       2|    1|
|   467|       1|       7|       24|  6.000000|       2|    1|
|   469|       3|       8|       27|  2.000000|       2|    1|
|   472|       6|       9|       27| 10.000000|       1|    1|
|   473|       4|       8|       25|  4.000000|       1|    1|
|   474|      30|       8|       29|  4.000000|       1|    1|
|   476|       0|       7|       19|  5.000000|       2|    1|
|   479|      25|       7|       27|  3.000000|       2|    1|
|   480|       0|       7|       31|  3.000000|       1|    1|
|   481|       5|       8|       18|  2.000000|       2|    1|
|   482|       3|       8|       23|  8.000000|       1|    1|
|   483|       2|       9|       32|  4.000000|       1|    1|
|   484|       2|       7|       20|  7.000000|       1|    1|
|   485|       9|       8|       21|  3.000000|       2|    1|
|   486|       2|       8|       25|  4.209567|       2|    1|
|   487|      13|       6|       21|  4.000000|       2|    1|
|   488|       1|       8|       22|  3.000000|       2|    1|
|   489|       4|       9|       29|  4.000000|       2|    1|
|   490|       4|       7|       23|  1.000000|       1|    1|
|   491|       3|       9|       20|  1.000000|       2|    1|
|   492|       7|       9|       21|  1.000000|       2|    1|
|   493|       7|       8|       26|  1.000000|       2|    1|
|   494|      14|       6|       21|  2.000000|       2|    1|
|   495|       4|       7|       24|  1.000000|       2|    1|
|   496|      15|       6|       26|  6.000000|       2|    1|
|   497|      37|       9|       30|  2.000000|       2|    1|
|   498|       2|       7|       18|  2.000000|       2|    1|
|   499|       4|       7|       22|  2.000000|       2|    1|
|   500|       6|       9|       25|  4.000000|       2|    1|
|   501|      10|       6|       21|  4.000000|       2|    1|
|   502|      56|       8|       19|  3.000000|       2|    1|
|   503|       3|      10|       23|  5.000000|       2|    1|
|   504|       0|       9|       22|  1.000000|       2|    1|
|   505|      18|       7|       21|  1.000000|       2|    1|
|   506|       3|       9|       22|  3.000000|       2|    1|
|   507|      18|       7|       24|  4.000000|       2|    1|
|   508|       7|       8|       18|  4.000000|       2|    1|
|   509|      29|       8|       18|  5.000000|       2|    1|
|   510|       0|       5|       24|  4.000000|       2|    1|
|   511|       5|       6|       25|  8.000000|       1|    1|
|   512|       4|       8|       19|  4.209567|       2|    1|
|   513|       3|       7|       27|  4.209567|       2|    1|
|   514|       6|       7|       29|  2.000000|       2|    1|
|   515|      21|       9|       22|  2.000000|       2|    1|
|   516|       1|       8|       25|  1.000000|       2|    1|
|   517|       3|       7|       27|  3.000000|       2|    1|
|   518|       3|       8|       26|  6.000000|       1|    2|
|   519|       0|       7|       28|  1.000000|       2|    1|
|   520|      13|      10|       19|  2.000000|       2|    1|
|   521|       5|       6|       23|  9.000000|       1|    1|
|   522|       5|       8|       33|  4.000000|       2|    1|
|   523|      37|       7|       24| 10.000000|       1|    1|
|   524|       2|       8|       28|  4.000000|       2|    1|
|   525|      11|       8|       28|  4.000000|       2|    1|
|   526|      13|       7|       24|  9.000000|       2|    1|
|   527|       2|       9|       21|  4.000000|       2|    1|
|   528|       4|       9|       31|  3.000000|       2|    1|
|   529|      21|       9|       23|  2.000000|       2|    1|
|   530|       2|      10|       26|  4.000000|       2|    1|
|   533|       4|       8|       26|  3.000000|       2|    1|
|   534|       3|       8|       26|  3.000000|       2|    2|
|   535|       3|       8|       24|  4.000000|       2|    1|
|   536|      12|       8|       18|  3.000000|       2|    1|
|   538|       1|       7|       21|  5.000000|       1|    1|
|   539|       4|       7|       19|  2.000000|       2|    1|
|   540|      13|       7|       17|  8.000000|       1|    1|
|   547|       2|       7|       24|  1.000000|       2|    1|
|   548|      81|       8|       24|  9.000000|       1|    1|
|   549|      12|       7|       31| 10.000000|       1|    1|
|   550|       2|       8|       19|  6.000000|       2|    1|
|   551|      16|       7|       28|  4.000000|       2|    1|
|   552|      27|       8|       23|  4.209567|       2|    1|
|   553|       2|       5|       26|  5.000000|       2|    1|
|   554|       2|       7|       26|  2.000000|       1|    1|
|   555|       8|       8|       18|  6.000000|       2|    1|
|   556|       2|       8|       16|  4.000000|       2|    1|
|   557|       4|       8|       29|  1.000000|       2|    1|
|   558|       3|       8|       14|  4.000000|       2|    1|
|   559|      19|       7|       23|  4.000000|       2|    1|
|   560|       4|       9|       29|  1.000000|       2|    1|
|   567|       1|       7|       19|  3.000000|       2|    1|
|   568|       3|       7|       22|  4.209567|       2|    1|
|   569|      15|       6|       20|  1.000000|       2|    1|
|   570|       4|       8|       22|  4.209567|       1|    1|
|   571|       4|       8|       21|  1.000000|       2|    1|
|   572|      13|       8|       23|  6.000000|       2|    1|
|   573|       6|       9|       26|  1.000000|       2|    1|
|   574|       1|      10|       18| 10.000000|       1|    1|
|   575|       3|       9|       18|  1.000000|       2|    1|
|   576|       3|       9|       28|  4.000000|       2|    1|
|   577|       0|       9|       31|  2.000000|       2|    1|
|   578|      22|       7|       22|  2.000000|       2|    1|
|   579|       4|       8|       28|  3.000000|       2|    1|
|   580|      14|       7|       20|  2.000000|       1|    1|
|   581|       6|       9|       20|  7.000000|       1|    1|
|   582|      16|       7|       26|  7.000000|       2|    2|
|   583|       6|       6|       27|  1.000000|       2|    1|
|   584|       0|       8|       11|  4.209567|       2|    2|
|   585|       8|       6|       26|  7.000000|       2|    1|
|   586|       0|       7|       27|  6.000000|       2|    1|
|   587|       4|       7|       16|  4.000000|       2|    1|
|   588|       5|       6|       12|  4.000000|       2|    1|
|   589|       2|       8|       20|  3.000000|       2|    1|
|   590|       6|       8|       19|  6.000000|       1|    1|
|   591|      11|       8|       26|  7.000000|       2|    1|
|   592|       1|       7|       20|  3.000000|       2|    1|
|   593|      23|       7|       22|  7.000000|       1|    1|
|   683|       4|       8|       30|  6.000000|       2|    1|
|   685|       4|       9|       25|  1.000000|       2|    1|
|   686|      16|       9|       23|  6.000000|       1|    1|
|   687|       6|       7|       22|  8.000000|       1|    1|
|   688|       1|       9|       24|  5.000000|       2|    1|
|   689|       2|       6|       23|  4.000000|       1|    1|
|   690|       6|       9|       27|  3.000000|       2|    1|
|   691|       6|       6|       28|  3.000000|       2|    1|
|   706|       3|       7|       24|  3.000000|       2|    1|
|   707|       1|       7|       17|  4.000000|       2|    1|
|   708|      15|       9|       22| 10.000000|       2|    1|
|   709|       3|       6|       23|  4.000000|       2|    1|
|   710|       7|       8|       16|  3.000000|       2|    1|
|   711|       9|       6|       18|  2.000000|       2|    1|
|   717|      18|       6|       21|  4.000000|       2|    1|
|   724|      14|       9|       19|  3.000000|       2|    1|
|   754|       3|       6|       17|  3.000000|       2|    1|
|   755|       4|       8|       17|  3.000000|       2|    1|
|   756|      15|      10|       19|  8.000000|       2|    1|
|   757|       4|       8|       23|  2.000000|       2|    1|
|   758|       3|       8|       18|  4.000000|       2|    1|

</div>

### Beklenti Maksimizasyonu

**mvdalab** paketi ile önce eksik veri olusturulup sonra eksik veri BM yöntemi ile doldurulmuştur.


```r
library(mvdalab)
```

```
## 
## Attaching package: 'mvdalab'
```

```
## The following object is masked from 'package:psych':
## 
##     smc
```

```r
dat <- introNAs(iris, percent = 25)
imputeEM(dat)
```

<img src="01-Varsayımlar_files/figure-html/unnamed-chunk-30-1.png" width="100%" style="display: block; margin: auto;" />

```
##     Sepal.Length Sepal.Width Petal.Length Petal.Width Speciessetosa
## 1       4.928262    3.500000     1.419858   0.2000000   1.000000000
## 2       4.900000    3.000000     1.400000   0.2000000   1.000000000
## 3       4.700000    3.200000     1.300000   0.1858627   1.000000000
## 4       4.600000    3.137689     1.500000   0.1478723   0.821839916
## 5       5.000000    3.600000     1.400000   0.2000000   1.000000000
## 6       5.400000    3.417948     1.863693   0.3885471   1.000000000
## 7       4.600000    3.158750     1.400000   0.1210962   0.847617131
## 8       4.948701    3.400000     1.500000   0.2318823   1.000000000
## 9       4.400000    2.900000     1.400000   0.2000000   1.000000000
## 10      4.916695    3.100000     1.550600   0.1000000   1.000000000
## 11      5.400000    3.700000     1.500000   0.2000000   1.000000000
## 12      4.800000    3.400000     1.600000   0.2075398   1.000000000
## 13      4.800000    3.000000     1.480295   0.1000000   0.711575235
## 14      4.300000    3.457811     1.100000   0.1000000   1.000000000
## 15      5.191099    4.000000     1.200000   0.2000000   1.408701114
## 16      5.700000    3.427276     1.500000   0.4000000   1.000000000
## 17      5.400000    3.900000     1.300000   0.2139801   1.000000000
## 18      5.100000    3.500000     1.400000   0.3000000   1.000000000
## 19      5.700000    3.419937     1.976587   0.3000000   1.000000000
## 20      5.100000    3.800000     1.500000   0.2065859   1.247420351
## 21      5.400000    3.400000     1.700000   0.2000000   1.000000000
## 22      5.100000    3.700000     1.552804   0.4000000   1.000000000
## 23      4.906224    3.600000     1.000000   0.2000000   1.156024539
## 24      5.100000    3.300000     1.700000   0.3715124   0.844292729
## 25      4.800000    3.400000     1.900000   0.2000000   0.951157934
## 26      4.702428    3.000000     1.600000   0.2000000   0.692790133
## 27      5.012406    3.400000     1.600000   0.4000000   1.000000000
## 28      5.200000    3.500000     1.500000   0.2740000   1.000000000
## 29      4.925859    3.400000     1.400000   0.2000000   1.000000000
## 30      4.700000    3.200000     1.600000   0.2000000   1.000000000
## 31      4.800000    3.100000     1.550838   0.2000000   1.000000000
## 32      5.400000    3.400000     1.500000   0.4000000   0.905579926
## 33      5.200000    4.100000     1.500000   0.1000000   1.000000000
## 34      5.500000    3.419629     2.034134   0.2000000   0.882687943
## 35      4.736394    3.100000     1.500000   0.2000000   0.769995867
## 36      5.000000    3.200000     1.615579   0.2000000   1.000000000
## 37      5.500000    3.500000     1.751783   0.2000000   1.000000000
## 38      4.900000    3.600000     1.201091   0.1048367   1.156708487
## 39      4.400000    3.086889     1.300000   0.2000000   0.813030755
## 40      4.935932    3.400000     1.475626   0.2000000   1.000000000
## 41      5.000000    3.500000     1.300000   0.3000000   1.000000000
## 42      4.500000    2.300000     1.300000   0.3000000   1.000000000
## 43      4.400000    3.446035     1.300000   0.2000000   1.000000000
## 44      5.097252    3.500000     1.789174   0.6000000   1.000000000
## 45      5.100000    3.800000     1.900000   0.4000000   1.184669009
## 46      4.800000    3.000000     1.669974   0.3000000   1.000000000
## 47      5.100000    3.800000     1.600000   0.2000000   1.000000000
## 48      4.600000    3.200000     1.263409   0.2000000   0.869525328
## 49      5.300000    3.700000     1.642406   0.2000000   1.151468570
## 50      5.000000    3.300000     1.400000   0.2000000   1.000000000
## 51      7.000000    3.200000     4.700000   1.4000000   0.000000000
## 52      6.400000    3.200000     4.500000   1.5000000   0.278304275
## 53      6.900000    2.644445     4.900000   1.5000000   0.000000000
## 54      5.500000    2.300000     4.000000   1.3000000   0.000000000
## 55      6.500000    2.800000     4.600000   1.5000000   0.000000000
## 56      5.700000    2.668754     4.500000   1.3000000   0.000000000
## 57      6.300000    3.300000     4.514089   1.6000000   0.339543634
## 58      4.900000    2.400000     3.003229   1.0000000   0.005543352
## 59      5.746059    2.900000     3.940156   1.3000000   0.000000000
## 60      5.200000    2.700000     3.568355   1.4000000   0.090446245
## 61      5.000000    2.000000     3.364309   1.0000000  -0.317161129
## 62      5.900000    3.000000     4.200000   1.5000000   0.000000000
## 63      6.000000    2.200000     4.000000   1.0000000  -0.292231595
## 64      6.100000    2.900000     4.700000   1.4000000   0.000000000
## 65      5.600000    2.900000     3.855350   1.3000000   0.000000000
## 66      6.700000    3.100000     4.458798   1.4000000   0.000000000
## 67      5.600000    3.000000     3.939737   1.5000000   0.000000000
## 68      5.802890    2.700000     4.100000   1.3499836   0.015830266
## 69      5.691598    2.200000     4.500000   1.5000000  -0.391794494
## 70      5.600000    3.040725     3.403689   1.1000000   0.374805117
## 71      6.524736    3.200000     4.800000   1.8000000   0.196206287
## 72      6.100000    2.800000     4.000000   1.3000000   0.000000000
## 73      6.300000    2.500000     4.658539   1.5837140   0.000000000
## 74      6.100000    2.683283     4.189209   1.2000000   0.000000000
## 75      6.400000    2.900000     4.300000   1.3000000   0.000000000
## 76      6.600000    3.000000     4.400000   1.5204241   0.000000000
## 77      5.795976    2.800000     4.088253   1.4000000   0.000000000
## 78      5.907378    3.000000     4.253704   1.7000000   0.000000000
## 79      6.000000    2.900000     4.500000   1.5000000   0.000000000
## 80      5.700000    2.600000     3.500000   1.0000000   0.000000000
## 81      5.669053    2.400000     3.800000   1.1000000   0.000000000
## 82      5.625780    2.400000     3.700000   1.0000000   0.000000000
## 83      5.800000    2.700000     3.900000   1.3181564   0.000000000
## 84      6.000000    2.700000     5.100000   1.6000000   0.000000000
## 85      5.400000    3.000000     3.823701   1.5000000   0.000000000
## 86      6.000000    3.400000     4.500000   1.6000000   0.000000000
## 87      5.815213    3.100000     4.013279   1.5000000   0.000000000
## 88      6.300000    3.143253     4.400000   1.3000000   0.276402645
## 89      5.600000    3.000000     4.100000   1.3000000   0.000000000
## 90      5.500000    2.500000     4.000000   1.3000000   0.000000000
## 91      5.500000    2.828211     4.400000   1.2000000   0.151072593
## 92      6.100000    2.659563     4.600000   1.4000000   0.000000000
## 93      5.800000    2.600000     4.000000   1.3521831   0.000000000
## 94      5.000000    2.718528     3.397210   1.0000000   0.000000000
## 95      5.600000    2.700000     4.200000   1.3000000   0.000000000
## 96      5.700000    3.000000     4.200000   1.2000000   0.000000000
## 97      5.700000    2.677192     4.200000   1.3000000   0.000000000
## 98      6.200000    2.900000     4.203457   1.3000000   0.000000000
## 99      5.100000    2.723547     3.000000   1.1000000   0.000000000
## 100     5.700000    2.800000     4.100000   1.3000000   0.000000000
## 101     7.029881    3.300000     6.000000   2.5000000   0.000000000
## 102     6.827132    2.700000     5.816067   1.9000000   0.000000000
## 103     7.100000    3.000000     5.900000   2.1727395   0.000000000
## 104     6.300000    2.900000     5.600000   1.9541055   0.000000000
## 105     6.500000    2.775073     5.800000   2.2000000  -0.238444838
## 106     7.600000    3.000000     6.600000   2.1000000   0.000000000
## 107     4.900000    3.179298     4.500000   1.7000000   0.000000000
## 108     7.300000    2.900000     6.300000   1.8000000   0.000000000
## 109     6.877460    3.118214     5.800000   2.0787924   0.000000000
## 110     7.200000    3.083954     6.100000   2.5000000   0.000000000
## 111     6.500000    3.200000     5.100000   2.0000000   0.000000000
## 112     6.400000    3.143512     5.300000   1.9000000   0.000000000
## 113     6.800000    3.123834     5.679548   2.0426811   0.000000000
## 114     6.884719    2.500000     6.019931   2.0000000   0.000000000
## 115     5.800000    2.800000     5.100000   2.4000000  -0.164080362
## 116     6.400000    3.200000     5.300000   1.8771471   0.000000000
## 117     6.500000    3.000000     5.500000   1.9703222   0.000000000
## 118     7.700000    3.800000     5.961715   2.2000000   0.000000000
## 119     7.700000    3.087443     6.394772   2.3000000   0.000000000
## 120     6.000000    2.200000     5.000000   1.9489444   0.000000000
## 121     6.900000    3.200000     5.700000   2.3000000   0.000000000
## 122     5.600000    2.800000     3.587383   1.1225973   0.179365921
## 123     6.861709    2.800000     5.852628   2.0000000   0.000000000
## 124     6.300000    2.700000     5.114087   1.7848805  -0.169106856
## 125     6.700000    3.300000     5.570170   2.1000000   0.000000000
## 126     7.200000    3.200000     5.714282   1.8000000   0.079106981
## 127     6.200000    2.800000     4.800000   1.8467147   0.000000000
## 128     6.100000    3.000000     4.900000   1.8000000   0.074672986
## 129     6.400000    2.800000     5.600000   2.1000000   0.000000000
## 130     7.200000    3.000000     6.098181   2.2235368   0.000000000
## 131     7.400000    3.120255     6.100000   1.9000000   0.000000000
## 132     7.388650    3.800000     6.400000   2.0000000   0.411966625
## 133     6.400000    3.133241     5.600000   1.9298669   0.000000000
## 134     6.300000    2.800000     5.100000   1.5000000   0.000000000
## 135     6.100000    3.165678     5.600000   1.4000000   0.000000000
## 136     7.700000    3.000000     6.100000   2.3000000   0.000000000
## 137     6.946640    3.400000     5.600000   2.4000000   0.000000000
## 138     6.754020    3.100000     5.500000   1.8000000   0.000000000
## 139     6.000000    3.163953     4.800000   1.8000000   0.000000000
## 140     6.900000    3.036913     5.902477   2.1374215  -0.087181499
## 141     6.841887    3.100000     5.600000   2.0480109   0.000000000
## 142     6.900000    3.021019     5.100000   2.3000000  -0.072798553
## 143     5.800000    2.700000     5.100000   1.8060980   0.000000000
## 144     6.800000    3.200000     5.815166   2.3000000   0.000000000
## 145     6.700000    3.043502     5.493492   1.9594529  -0.007535164
## 146     6.700000    3.000000     5.200000   2.3000000  -0.074407827
## 147     6.300000    3.147527     5.251295   1.8567845   0.000000000
## 148     6.500000    3.000000     5.200000   2.0000000   0.000000000
## 149     6.200000    3.400000     5.400000   1.6346475   0.345542517
## 150     6.481639    3.000000     5.100000   1.8000000   0.032437091
##     Speciesversicolor Speciesvirginica
## 1         0.000000000       0.00000000
## 2         0.000000000       0.00000000
## 3         0.000000000       0.00000000
## 4         0.553010122      -0.37485004
## 5         0.000000000       0.00000000
## 6         0.000000000       0.00000000
## 7         0.524055774      -0.37167291
## 8         0.000000000       0.00000000
## 9         0.000000000       0.00000000
## 10        0.000000000       0.00000000
## 11        0.000000000       0.00000000
## 12        0.000000000       0.00000000
## 13        0.748633370      -0.46020860
## 14        0.000000000       0.00000000
## 15       -0.957966303       0.54926519
## 16        0.000000000       0.00000000
## 17        0.000000000       0.00000000
## 18        0.000000000       0.00000000
## 19        0.000000000       0.00000000
## 20       -0.618811931       0.37139158
## 21        0.000000000       0.00000000
## 22        0.000000000       0.00000000
## 23       -0.245841914       0.08981738
## 24        0.185648449      -0.02994118
## 25        0.094408253      -0.04556619
## 26        0.765672363      -0.45846250
## 27        0.000000000       0.00000000
## 28        0.000000000       0.00000000
## 29        0.000000000       0.00000000
## 30        0.000000000       0.00000000
## 31        0.000000000       0.00000000
## 32       -0.043211512       0.13763159
## 33        0.000000000       0.00000000
## 34       -0.039250545       0.15656260
## 35        0.597086651      -0.36708252
## 36        0.000000000       0.00000000
## 37        0.000000000       0.00000000
## 38       -0.245414673       0.08870619
## 39        0.661276225      -0.47430698
## 40        0.000000000       0.00000000
## 41        0.000000000       0.00000000
## 42        0.000000000       0.00000000
## 43        0.000000000       0.00000000
## 44        0.000000000       0.00000000
## 45       -0.627033674       0.44236467
## 46        0.000000000       0.00000000
## 47        0.000000000       0.00000000
## 48        0.468210770      -0.33773610
## 49       -0.502534834       0.35106626
## 50        0.000000000       0.00000000
## 51        1.000000000       0.00000000
## 52        0.003295692       0.71840003
## 53        1.000000000       0.00000000
## 54        1.000000000       0.00000000
## 55        1.000000000       0.00000000
## 56        1.000000000       0.00000000
## 57       -0.139220921       0.79967729
## 58        1.662448898      -0.66799225
## 59        1.000000000       0.00000000
## 60        1.093342895      -0.18378914
## 61        2.285719605      -0.96855848
## 62        1.000000000       0.00000000
## 63        1.731838033      -0.43960644
## 64        1.000000000       0.00000000
## 65        1.000000000       0.00000000
## 66        1.000000000       0.00000000
## 67        1.000000000       0.00000000
## 68        0.952879071       0.03129066
## 69        1.780146149      -0.38835166
## 70        0.449923377       0.17527151
## 71       -0.034180947       0.83797466
## 72        1.000000000       0.00000000
## 73        1.000000000       0.00000000
## 74        1.000000000       0.00000000
## 75        1.000000000       0.00000000
## 76        1.000000000       0.00000000
## 77        1.000000000       0.00000000
## 78        1.000000000       0.00000000
## 79        1.000000000       0.00000000
## 80        1.000000000       0.00000000
## 81        1.000000000       0.00000000
## 82        1.000000000       0.00000000
## 83        1.000000000       0.00000000
## 84        1.000000000       0.00000000
## 85        1.000000000       0.00000000
## 86        1.000000000       0.00000000
## 87        1.000000000       0.00000000
## 88        0.147771983       0.57582537
## 89        1.000000000       0.00000000
## 90        1.000000000       0.00000000
## 91        0.768291281       0.08063613
## 92        1.000000000       0.00000000
## 93        1.000000000       0.00000000
## 94        1.000000000       0.00000000
## 95        1.000000000       0.00000000
## 96        1.000000000       0.00000000
## 97        1.000000000       0.00000000
## 98        1.000000000       0.00000000
## 99        1.000000000       0.00000000
## 100       1.000000000       0.00000000
## 101       0.000000000       1.00000000
## 102       0.000000000       1.00000000
## 103       0.000000000       1.00000000
## 104       0.000000000       1.00000000
## 105       0.607739918       0.63070492
## 106       0.000000000       1.00000000
## 107       0.000000000       1.00000000
## 108       0.000000000       1.00000000
## 109       0.000000000       1.00000000
## 110       0.000000000       1.00000000
## 111       0.000000000       1.00000000
## 112       0.000000000       1.00000000
## 113       0.000000000       1.00000000
## 114       0.000000000       1.00000000
## 115       0.757080028       0.40700033
## 116       0.000000000       1.00000000
## 117       0.000000000       1.00000000
## 118       0.000000000       1.00000000
## 119       0.000000000       1.00000000
## 120       0.000000000       1.00000000
## 121       0.000000000       1.00000000
## 122       0.848865515      -0.02823144
## 123       0.000000000       1.00000000
## 124       0.824246083       0.34486077
## 125       0.000000000       1.00000000
## 126      -0.193944080       1.11483710
## 127       0.000000000       1.00000000
## 128       0.383646360       0.54168065
## 129       0.000000000       1.00000000
## 130       0.000000000       1.00000000
## 131       0.000000000       1.00000000
## 132      -1.215300842       1.80333422
## 133       0.000000000       1.00000000
## 134       0.000000000       1.00000000
## 135       0.000000000       1.00000000
## 136       0.000000000       1.00000000
## 137       0.000000000       1.00000000
## 138       0.000000000       1.00000000
## 139       0.000000000       1.00000000
## 140       0.125917040       0.96126446
## 141       0.000000000       1.00000000
## 142       0.172130815       0.90066774
## 143       0.000000000       1.00000000
## 144       0.000000000       1.00000000
## 145       0.168393162       0.83914200
## 146       0.232509988       0.84189784
## 147       0.000000000       1.00000000
## 148       0.000000000       1.00000000
## 149      -0.286327039       0.94078452
## 150       0.296693556       0.67086935
```

**mvdalab** paketi ile önce eksik veri olusturulup sonra eksik veri BM yöntemi ile doldurulmuştur.


```r
library(missMethods)
```

```
## 
## Attaching package: 'missMethods'
```

```
## The following objects are masked from 'package:naniar':
## 
##     impute_mean, impute_median
```

```r
dat2 <- delete_MCAR(iris[,2:4], p=0.2)
dat2<-impute_EM(dat2,stochastic = FALSE)
```

### Çoklu Atama

Çoklu atama için en sık kullanılan paketler **mice** ve **VIM** paketleridir.


```r
library(mice)
```

```
## 
## Attaching package: 'mice'
```

```
## The following object is masked from 'package:stats':
## 
##     filter
```

```
## The following objects are masked from 'package:base':
## 
##     cbind, rbind
```

```r
# https://datascienceplus.com/imputing-missing-data-with-r-mice-package/
```

Çoklu atama yapmadan önce **naniar** paketindekine benzer olarak **VIM** paketi ile de inceleyebilirsiniz.


```r
library(VIM)
```

```
## Loading required package: colorspace
```

```
## Loading required package: grid
```

```
## VIM is ready to use.
```

```
## Suggestions and bug-reports can be submitted at: https://github.com/statistikat/VIM/issues
```

```
## 
## Attaching package: 'VIM'
```

```
## The following object is masked from 'package:vtable':
## 
##     countNA
```

```
## The following object is masked from 'package:datasets':
## 
##     sleep
```

```r
mice_plot <- VIM::aggr(screen[,2:6], col=c('yellow','black'),
                       numbers=TRUE, sortVars=TRUE,
                       labels=names(screen[,2:6]), cex.axis=.8,
                       gap=1, ylab=c("Missing data","Pattern"))
```

<img src="01-Varsayımlar_files/figure-html/unnamed-chunk-33-1.png" width="100%" style="display: block; margin: auto;" />

```r
# logreg(Logistic Regression) – Used For Binary Variables polyreg(Bayesian polytomous regression) – Used For unordered levels>= 2

screen7 <- mice::mice(screen[2:5],m=1,maxit=20,
                    method = "pmm")
   mice::complete(screen7)
```

```
## 
##  Variables sorted by number of missings: 
##  Variable       Count
##    income 0.055913978
##  atthouse 0.002150538
##   timedrs 0.000000000
##   attdrug 0.000000000
##   mstatus 0.000000000
## 
##  iter imp variable
##   1   1  atthouse  income
##   2   1  atthouse  income
##   3   1  atthouse  income
##   4   1  atthouse  income
##   5   1  atthouse  income
##   6   1  atthouse  income
##   7   1  atthouse  income
##   8   1  atthouse  income
##   9   1  atthouse  income
##   10   1  atthouse  income
##   11   1  atthouse  income
##   12   1  atthouse  income
##   13   1  atthouse  income
##   14   1  atthouse  income
##   15   1  atthouse  income
##   16   1  atthouse  income
##   17   1  atthouse  income
##   18   1  atthouse  income
##   19   1  atthouse  income
##   20   1  atthouse  income
```

<div class="kable-table">

| timedrs| attdrug| atthouse| income|
|-------:|-------:|--------:|------:|
|       1|       8|       27|      5|
|       3|       7|       20|      6|
|       0|       8|       23|      3|
|      13|       9|       28|      8|
|      15|       7|       24|      1|
|       3|       8|       25|      4|
|       2|       7|       30|      6|
|       0|       7|       24|      6|
|       7|       7|       20|      2|
|       4|       8|       30|      8|
|      15|       9|       15|      7|
|       0|       6|       22|      3|
|       2|       6|       19|      5|
|      13|       8|       25|      6|
|       2|       5|       17|      1|
|       2|       8|       19|      3|
|       1|       8|       22|      1|
|       2|       6|       21|      7|
|       5|       8|       28|      2|
|       5|      10|       25|      9|
|       3|       6|       19|      4|
|       4|       5|       31|      5|
|       2|       8|       25|      2|
|       0|       8|       26|      1|
|      13|       9|       26|      2|
|       7|       9|       33|      1|
|       2|       8|       20|      5|
|      12|       9|       26|      1|
|       2|       6|       27|      3|
|       5|       9|       30|      1|
|       4|       7|       31|      4|
|       6|       6|       25|      5|
|       2|       9|       27|      5|
|       3|       7|       23|      4|
|      14|       9|       15|      4|
|       7|       7|       32|      3|
|       0|       8|       21|      9|
|       1|       8|       28|      6|
|       3|       8|       16|      1|
|      60|       7|       24|      1|
|       5|       8|       19|      1|
|       3|       9|       23|      6|
|       0|       7|       24|      8|
|       3|       8|       31|      3|
|       2|       6|       29|      1|
|       1|       8|       23|      4|
|       1|       9|       24|      4|
|      13|       8|       25|      1|
|       2|       7|       22|      2|
|       5|       9|       26|      5|
|      12|       6|       27|      5|
|      12|       9|       24|      6|
|       1|       9|       27|      6|
|      20|       9|       24|      6|
|       0|       6|       23|      4|
|       5|       7|       25|      4|
|       0|       7|       22|      6|
|       8|      10|       25|      9|
|       9|       8|       15|      3|
|      10|       8|       28|      6|
|       1|      10|       25|      3|
|       5|       8|       22|      7|
|       5|       8|       21|      3|
|      23|       6|       29|      1|
|       7|       6|       26|      1|
|       1|       7|       23|      2|
|      39|       7|       26|      2|
|       2|       8|       21|      3|
|       7|       8|       22|      3|
|       9|       7|       31|      5|
|       0|       5|       35|      5|
|       4|       8|       28|      5|
|      16|      10|       28|      5|
|       0|       7|       20|      5|
|      16|       8|       24|      3|
|      33|       9|       25|      1|
|       4|       7|       25|      3|
|       2|       8|       16|      3|
|      38|       8|       18|      2|
|       8|       8|       23|      2|
|       2|       8|       24|      9|
|       2|       9|       29|     10|
|       0|       9|       16|      1|
|      15|       7|       22|      5|
|       0|       7|       27|     10|
|       1|       8|       23|      8|
|       5|       7|       26|      1|
|       7|       9|       30|      3|
|      10|       6|       23|      3|
|      22|       9|       26|      6|
|      10|       7|       30|      3|
|       0|       7|       25|      2|
|      11|       8|       22|      1|
|       9|       7|       21|      4|
|       0|       8|       16|      4|
|      34|       7|       23|      5|
|       3|       9|       25|      4|
|       1|       6|       20|      5|
|       0|      10|       18|      1|
|      10|       6|       23|      4|
|       4|       8|       17|      4|
|      27|       8|       23|      8|
|       7|       9|       23|      5|
|       1|       5|       22|      2|
|       7|      10|       22|      6|
|       3|       8|       28|      4|
|       2|       6|       17|      2|
|      11|       6|       27|      7|
|       9|       6|       18|      4|
|      11|       8|       26|      7|
|      11|       8|       21|      7|
|       8|       7|       21|      4|
|       2|       9|       24|     10|
|       4|       9|       26|      3|
|       4|       8|       21|      3|
|       6|       7|       18|      8|
|      30|       5|       24|     10|
|       7|       8|       28|      3|
|      15|       7|       26|      1|
|       6|       7|       25|      8|
|       7|       7|       23|      5|
|       2|       9|       22|      3|
|       1|       7|       19|      5|
|       5|       7|       26|      3|
|       1|       9|       29|      3|
|      11|       6|       20|      4|
|      16|      10|       20|      1|
|       2|       7|       19|      5|
|      14|       5|       24|      4|
|       8|       7|       21|      3|
|      17|       8|       32|      1|
|       1|       8|       18|      3|
|       0|       6|       22|      1|
|       1|       9|       27|      8|
|       1|       6|       21|      5|
|      10|       8|       19|      1|
|       7|       7|       25|      1|
|       2|       8|       17|      7|
|       5|       6|       25|      3|
|       6|       7|       31|      4|
|       1|       8|       16|      7|
|      16|      10|       17|      8|
|       0|       5|       14|      8|
|      17|       7|       30|      4|
|       0|       8|       25|      6|
|      10|       8|       28|      1|
|       6|       7|       22|      1|
|      20|       8|       24|      5|
|       1|       8|       21|      2|
|      25|       8|       25|      7|
|       3|       9|       27|      7|
|       6|       9|       20|      7|
|       0|       7|       26|      1|
|       8|       8|       25|      1|
|       9|       9|       27|      3|
|      19|      10|       23|      5|
|       2|       8|       27|      3|
|       2|       7|       21|      3|
|       3|       7|       22|      5|
|       7|       8|       19|      7|
|       2|       8|       26|      3|
|       8|       6|       25|      4|
|      49|       8|       34|      4|
|       2|       8|       27|      1|
|       1|       8|       22|      3|
|       5|       6|       14|      3|
|       5|       8|       29|      1|
|      60|      10|       29|      4|
|      10|       7|       26|      6|
|      27|       8|       17|      7|
|       7|       9|       22|      3|
|       8|       7|       20|      1|
|      12|       8|       30|      1|
|       2|       7|       17|      6|
|       2|       7|       21|      1|
|       4|       7|       22|      3|
|       6|       5|       29|      3|
|      27|       9|       22|      1|
|       0|       8|       20|      9|
|       9|      10|       30|      3|
|       3|       9|       25|      5|
|       0|       8|       18|      5|
|       2|       6|       19|      4|
|       3|       8|       24|      3|
|       5|       8|       22|      4|
|       5|       9|       21|      8|
|       1|       9|       29|      1|
|       7|       9|       22|      1|
|       4|       8|       26|      7|
|       3|       7|       19|      6|
|       7|       8|       23|      8|
|       1|       7|       19|      9|
|      52|       9|       31|      4|
|       6|       7|       26|      2|
|      18|       7|       23|      4|
|      14|       7|       29|      3|
|       0|       7|       22|      4|
|       8|       9|       29|      1|
|      16|       8|       23|      7|
|       2|       9|       17|      3|
|      12|       8|       26|      1|
|       3|       7|       25|      4|
|      24|      10|       25|      1|
|       0|       9|       19|      4|
|       0|       7|       16|      3|
|      57|       9|       24|      2|
|      11|       9|       29|      3|
|       1|       8|       20|      6|
|      18|       6|       25|      2|
|       1|       5|       16|      6|
|      11|       9|       35|      4|
|       0|       8|       24|      7|
|      52|       8|       19|      1|
|      12|       7|       25|      4|
|       6|       6|       29|      3|
|       0|       7|       25|      3|
|       2|       8|       29|      2|
|       2|       8|       20|      6|
|      13|      10|       25|      1|
|       2|       7|       31|      4|
|       3|       6|       33|      5|
|       2|       9|       24|      2|
|       2|       7|       26|      4|
|       1|       7|       20|      7|
|       2|       7|       17|      4|
|       3|       8|       25|      7|
|       1|       9|       24|      3|
|       5|       5|       27|     10|
|       6|       7|       25|     10|
|       1|       7|       28|      7|
|       2|       8|       29|      1|
|       3|       6|       27|      4|
|       7|      10|       24|      4|
|       7|       6|       23|      7|
|       0|       8|       19|      3|
|       7|       8|       23|      6|
|       8|       7|       24|      3|
|       9|       7|       21|      7|
|       8|       6|       22|      3|
|       1|       9|       21|      7|
|      14|       9|       30|      2|
|       2|       9|       32|      1|
|       4|       7|       21|      3|
|       8|       8|       19|     10|
|       3|       8|       24|      4|
|      10|       8|       28|      1|
|      21|      10|       28|     10|
|       6|       7|       29|      7|
|      58|       7|       29|      4|
|      12|       8|       23|      4|
|       5|       7|       26|      1|
|       2|       9|       22|      1|
|       2|       6|       29|      5|
|       4|       7|       25|      6|
|       2|       7|       19|      5|
|       5|       9|       22|      3|
|       0|       9|       18|      5|
|       3|       8|       25|      3|
|       7|       9|       23|      5|
|       1|       9|       19|      8|
|       2|       8|        2|      1|
|       1|       6|       26|      5|
|       4|       6|       24|      3|
|       4|       7|       30|      4|
|       4|       8|       30|      5|
|       2|       8|       22|      3|
|       0|       8|       26|      7|
|      13|       6|       30|      4|
|       3|       8|       25|      5|
|       1|       8|       24|      5|
|       4|       7|       23|      3|
|       1|       8|       18|      4|
|       3|       7|       23|      4|
|       1|       6|       34|      4|
|      57|       8|       23|      4|
|       1|       8|       13|      1|
|      17|       9|       27|      7|
|      11|       7|       21|      3|
|      43|       6|       28|      7|
|       6|       9|       28|      7|
|       6|       8|       20|      6|
|       1|       9|       21|      1|
|       0|       7|       25|      9|
|      10|       6|       27|      9|
|       3|       9|       23|     10|
|      37|       9|       25|      6|
|       6|       8|       22|      2|
|      11|       7|       24|      9|
|       4|       7|       15|      3|
|      75|       9|       33|      9|
|       7|       9|       24|      5|
|       3|       6|       29|      7|
|       3|       7|       21|      3|
|       3|       7|       26|      2|
|       4|       8|       29|      4|
|       5|       9|       21|      1|
|       2|      10|       21|      1|
|       4|       7|       18|      5|
|       2|       8|        2|      4|
|      11|       9|       28|      2|
|       0|       6|       20|      6|
|       2|       7|       27|      4|
|       4|       8|       23|      6|
|       6|       9|       27|      3|
|       0|       8|       27|      5|
|       2|       6|       25|      4|
|       5|       6|       22|      4|
|       2|       6|       29|      1|
|      10|       9|       23|      7|
|      29|       9|       14|      3|
|       3|       8|       28|      8|
|       0|       8|       29|      3|
|      21|       8|       27|      7|
|       0|       7|       23|      6|
|       3|       8|       23|      5|
|       1|       8|       20|      1|
|       9|       9|       24|     10|
|       3|       8|       26|      3|
|       3|       7|       23|      1|
|       5|       8|       25|      4|
|       6|       8|       28|      1|
|       4|       9|       16|      4|
|      16|       5|       29|      1|
|       3|       9|       25|      9|
|      13|       9|       22|      1|
|       3|       9|       29|      1|
|       2|       6|       23|      3|
|       2|       8|       26|      3|
|       3|       7|       19|      5|
|       7|       7|       27|      5|
|       2|       8|       21|      4|
|       2|       7|       18|      6|
|       2|       7|       19|      4|
|       2|       7|       23|      3|
|       5|      10|       19|      7|
|       3|       7|       21|      6|
|       2|       6|       23|      5|
|       4|       9|       28|      3|
|       1|       7|       24|      6|
|       3|       8|       27|      2|
|       6|       9|       27|     10|
|       4|       8|       25|      4|
|      30|       8|       29|      4|
|       0|       7|       19|      5|
|      25|       7|       27|      3|
|       0|       7|       31|      3|
|       5|       8|       18|      2|
|       3|       8|       23|      8|
|       2|       9|       32|      4|
|       2|       7|       20|      7|
|       9|       8|       21|      3|
|       2|       8|       25|      2|
|      13|       6|       21|      4|
|       1|       8|       22|      3|
|       4|       9|       29|      4|
|       4|       7|       23|      1|
|       3|       9|       20|      1|
|       7|       9|       21|      1|
|       7|       8|       26|      1|
|      14|       6|       21|      2|
|       4|       7|       24|      1|
|      15|       6|       26|      6|
|      37|       9|       30|      2|
|       2|       7|       18|      2|
|       4|       7|       22|      2|
|       6|       9|       25|      4|
|      10|       6|       21|      4|
|      56|       8|       19|      3|
|       3|      10|       23|      5|
|       0|       9|       22|      1|
|      18|       7|       21|      1|
|       3|       9|       22|      3|
|      18|       7|       24|      4|
|       7|       8|       18|      4|
|      29|       8|       18|      5|
|       0|       5|       24|      4|
|       5|       6|       25|      8|
|       4|       8|       19|      6|
|       3|       7|       27|      2|
|       6|       7|       29|      2|
|      21|       9|       22|      2|
|       1|       8|       25|      1|
|       3|       7|       27|      3|
|       3|       8|       26|      6|
|       0|       7|       28|      1|
|      13|      10|       19|      2|
|       5|       6|       23|      9|
|       5|       8|       33|      4|
|      37|       7|       24|     10|
|       2|       8|       28|      4|
|      11|       8|       28|      4|
|      13|       7|       24|      9|
|       2|       9|       21|      4|
|       4|       9|       31|      3|
|      21|       9|       23|      2|
|       2|      10|       26|      4|
|       4|       8|       26|      3|
|       3|       8|       26|      3|
|       3|       8|       24|      4|
|      12|       8|       18|      3|
|       1|       7|       21|      5|
|       4|       7|       19|      2|
|      13|       7|       17|      8|
|       2|       7|       24|      1|
|      81|       8|       24|      9|
|      12|       7|       31|     10|
|       2|       8|       19|      6|
|      16|       7|       28|      4|
|      27|       8|       23|      9|
|       2|       5|       26|      5|
|       2|       7|       26|      2|
|       8|       8|       18|      6|
|       2|       8|       16|      4|
|       4|       8|       29|      1|
|       3|       8|       14|      4|
|      19|       7|       23|      4|
|       4|       9|       29|      1|
|       1|       7|       19|      3|
|       3|       7|       22|      4|
|      15|       6|       20|      1|
|       4|       8|       22|      2|
|       4|       8|       21|      1|
|      13|       8|       23|      6|
|       6|       9|       26|      1|
|       1|      10|       18|     10|
|       3|       9|       18|      1|
|       3|       9|       28|      4|
|       0|       9|       31|      2|
|      22|       7|       22|      2|
|       4|       8|       28|      3|
|      14|       7|       20|      2|
|       6|       9|       20|      7|
|      16|       7|       26|      7|
|       6|       6|       27|      1|
|       0|       8|       11|      6|
|       8|       6|       26|      7|
|       0|       7|       27|      6|
|       4|       7|       16|      4|
|       5|       6|       12|      4|
|       2|       8|       20|      3|
|       6|       8|       19|      6|
|      11|       8|       26|      7|
|       1|       7|       20|      3|
|      23|       7|       22|      7|
|       4|       8|       30|      6|
|       4|       9|       25|      1|
|      16|       9|       23|      6|
|       6|       7|       22|      8|
|       1|       9|       24|      5|
|       2|       6|       23|      4|
|       6|       9|       27|      3|
|       6|       6|       28|      3|
|       3|       7|       24|      3|
|       1|       7|       17|      4|
|      15|       9|       22|     10|
|       3|       6|       23|      4|
|       7|       8|       16|      3|
|       9|       6|       18|      2|
|      18|       6|       21|      4|
|      14|       9|       19|      3|
|       3|       6|       17|      3|
|       4|       8|       17|      3|
|      15|      10|       19|      8|
|       4|       8|       23|      2|
|       3|       8|       18|      4|

</div>

### Veri setindeki kayıp veriler


```r
screen <- screen %>% mutate(income = ifelse(is.na(income), mean(income, na.rm =TRUE), income)) %>% na.omit()
summary(screen)
```

```
##      subno          timedrs          attdrug         atthouse    
##  Min.   :  1.0   Min.   : 0.000   Min.   : 5.00   Min.   : 2.00  
##  1st Qu.:136.8   1st Qu.: 2.000   1st Qu.: 7.00   1st Qu.:21.00  
##  Median :313.5   Median : 4.000   Median : 8.00   Median :24.00  
##  Mean   :317.3   Mean   : 7.914   Mean   : 7.69   Mean   :23.54  
##  3rd Qu.:483.2   3rd Qu.:10.000   3rd Qu.: 9.00   3rd Qu.:27.00  
##  Max.   :758.0   Max.   :81.000   Max.   :10.00   Max.   :35.00  
##      income          mstatus          race      
##  Min.   : 1.000   Min.   :1.00   Min.   :1.000  
##  1st Qu.: 3.000   1st Qu.:2.00   1st Qu.:1.000  
##  Median : 4.000   Median :2.00   Median :1.000  
##  Mean   : 4.208   Mean   :1.78   Mean   :1.086  
##  3rd Qu.: 6.000   3rd Qu.:2.00   3rd Qu.:1.000  
##  Max.   :10.000   Max.   :2.00   Max.   :2.000
```

daha fazla bilgi için [\<https://bookdown.org/mwheymans/bookmi/missing-data-evaluation.html\>](https://bookdown.org/mwheymans/bookmi/missing-data-evaluation.html){.uri}

### Veri setindeki kayıp veriler

- **atthouse** değişkeninde bir kayıp değer bulunmaktadır 
ve liste bazında silme yöntemi ile veri setinden 
çıkarılmıştır.

- Veri setinde **income** değişkeni 26 kayıp değere sahiptir 
ve bu sayı örneklemin %5’inden fazladır. Eğer bu 
değişken araştırma açısından öneme sahip değilse, veri 
setinden çıkarılabilir, aksi halde kayıp verinin tahmin 
edilmesi yöntemlerinden biri kullanılabilir.

- income değişkenindeki kayıp değerler için kayıp verinin 
tahmin edilmesi yöntemlerinden ortalamanın 
yerleştirilmesi kullanılarak kayıp değer yerine değişkenin ortalama değeri (4.21 değeri) yerleştirilmiştir.


---
### Veri setindeki kayıp veriler


```r
screen <- screen %>% 
  mutate(income = ifelse(is.na(income), mean(income, na.rm =TRUE), income)) %>% na.omit()
summary(screen)
```

```
##      subno          timedrs          attdrug         atthouse    
##  Min.   :  1.0   Min.   : 0.000   Min.   : 5.00   Min.   : 2.00  
##  1st Qu.:136.8   1st Qu.: 2.000   1st Qu.: 7.00   1st Qu.:21.00  
##  Median :313.5   Median : 4.000   Median : 8.00   Median :24.00  
##  Mean   :317.3   Mean   : 7.914   Mean   : 7.69   Mean   :23.54  
##  3rd Qu.:483.2   3rd Qu.:10.000   3rd Qu.: 9.00   3rd Qu.:27.00  
##  Max.   :758.0   Max.   :81.000   Max.   :10.00   Max.   :35.00  
##      income          mstatus          race      
##  Min.   : 1.000   Min.   :1.00   Min.   :1.000  
##  1st Qu.: 3.000   1st Qu.:2.00   1st Qu.:1.000  
##  Median : 4.000   Median :2.00   Median :1.000  
##  Mean   : 4.208   Mean   :1.78   Mean   :1.086  
##  3rd Qu.: 6.000   3rd Qu.:2.00   3rd Qu.:1.000  
##  Max.   :10.000   Max.   :2.00   Max.   :2.000
```


Rubin, D. B. (1976). Inference with missing data. Biometrika , 63, 581 592.
