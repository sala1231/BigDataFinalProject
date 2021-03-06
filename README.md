各年齡層與死因關聯
================

分析議題背景
------------

B0344240陳衍儒 B0344250歐宜宣 因生活習慣改變，許多印象中只有老年人才會得到的疾病，竟漸漸出現在年輕人身上。

分析動機
--------

希望藉由報告研究各年齡層與疾病死因的關聯性。

使用資料
--------

我們使用的是100年到104年之全國死因資料 資料內容包括年度、地區、死因、年齡、性別等。(X100\_104) 另外一個資料表則是地區代碼對應之地區(x1)

載入使用資料們

``` r
library(readxl)
```

    ## Warning: package 'readxl' was built under R version 3.3.3

``` r
X100_104 <- read_excel("C:/Users/Krystal/Desktop/100-104.xlsx")
 X1 <- read_excel("C:/Users/Krystal/Desktop/1.xlsx")
 X2 <- read_excel("C:/Users/Krystal/Desktop/2.xlsx")
```

資料處理與清洗
--------------

-   將年齡層的說明表格與X100\_104結合
-   把前五大死因抓出來存成另個資料框cause\_h5
-   再分成男、女兩個表做分析
-   用性別把cause\_h5分成h5\_m以及h5\_f
-   再分別將年份分成100-102和103.104兩個區間作圖

處理資料

``` r
library(dplyr)
```

    ## Warning: package 'dplyr' was built under R version 3.3.3

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
X100_104 <- inner_join(X100_104,X2,by="age_code")
X100_104 <- inner_join(X100_104,X1,by="100county")


#篩選前五大死因
cause_h5 <- subset(X100_104,X100_104$cause ==c("惡性腫瘤","腦血管疾病","肺炎","糖尿病"))
h5_m <- cause_h5[grepl("男",cause_h5$sex),]
h5_f <- cause_h5[grepl("女",cause_h5$sex),]
x103_104_m <- subset(h5_m,h5_m$year==c(103,104))
x103_104_f <- subset(h5_f,h5_f$year==c(103,104))
```

    ## Warning in h5_f$year == c(103, 104): 較長的物件長度並非較短物件長度的倍數

``` r
x100_102_m <- subset(h5_m,h5_m$year==c(100,101,102))
```

    ## Warning in h5_m$year == c(100, 101, 102): 較長的物件長度並非較短物件長度的
    ## 倍數

``` r
x100_102_f <- subset(h5_f,h5_f$year==c(100,101,102))
```

    ## Warning in h5_f$year == c(100, 101, 102): 較長的物件長度並非較短物件長度的
    ## 倍數

``` r
#算各地不同死因人數和
```

探索式資料分析
--------------

``` r
library(ggplot2)
```

    ## Warning: package 'ggplot2' was built under R version 3.3.3

``` r
library(plyr)
```

    ## Warning: package 'plyr' was built under R version 3.3.3

    ## -------------------------------------------------------------------------

    ## You have loaded plyr after dplyr - this is likely to cause problems.
    ## If you need functions from both plyr and dplyr, please load plyr first, then dplyr:
    ## library(plyr); library(dplyr)

    ## -------------------------------------------------------------------------

    ## 
    ## Attaching package: 'plyr'

    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     arrange, count, desc, failwith, id, mutate, rename, summarise,
    ##     summarize

``` r
df1 <- ddply(x103_104_m, c("年齡代碼", "N","cause"), summarise, year = factor(year))
ggplot(df1, aes(x=年齡代碼, y=N, fill=factor(year))) + 
  geom_bar(stat="identity", colour="black", position="dodge")+
  labs(title="103,104年男性四大死因")+
  facet_grid(cause~.)+
  theme(axis.title.y = element_text(angle = 0),
axis.text.x = element_text(angle = 60, hjust = 1),
plot.title = element_text(colour = "black", face = "bold", 
    size = 25, vjust = 2,hjust = 0.5), plot.margin = unit(c(0.2, 0.2, 0.2, 0.2), "inches"))
```

![](README_files/figure-markdown_github/unnamed-chunk-3-1.png)

``` r
df2 <- ddply(x103_104_f,c("年齡代碼", "N","cause"), summarise, year = factor(year))
ggplot(df2, aes(x=年齡代碼, y=N, fill=factor(year))) + 
  geom_bar(stat="identity", colour="black", position="dodge")+
  labs(title="103,104年女性四大死因")+
  facet_grid(cause~.)+
  theme(axis.title.y = element_text(angle = 0),
axis.text.x = element_text(angle = 60, hjust = 1),
plot.title = element_text(colour = "black", face = "bold", 
    size = 25, vjust = 2,hjust = 0.5), plot.margin = unit(c(0.2, 0.2, 0.2, 0.2), "inches"))
```

![](README_files/figure-markdown_github/unnamed-chunk-3-2.png)

``` r
df3 <- ddply(x100_102_m,c("年齡代碼", "N","cause"), summarise, year = factor(year))
ggplot(df3, aes(x=年齡代碼, y=N, fill=factor(year))) + 
  geom_bar(stat="identity", colour="black", position="dodge")+
  labs(title="100~102年男性四大死因")+
  facet_grid(cause~.)+
  theme(axis.title.y = element_text(angle = 0),
axis.text.x = element_text(angle = 60, hjust = 1),
plot.title = element_text(colour = "black", face = "bold", 
    size = 25, vjust = 2,hjust = 0.5), plot.margin = unit(c(0.2, 0.2, 0.2, 0.2), "inches"))
```

![](README_files/figure-markdown_github/unnamed-chunk-3-3.png)

``` r
df4 <- ddply(x100_102_f,c("年齡代碼", "N","cause"), summarise, year = factor(year))
ggplot(df4, aes(x=年齡代碼, y=N, fill=factor(year))) + 
  geom_bar(stat="identity", colour="black", position= "dodge")+
  labs(title="100~102年女性四大死因")+
  facet_grid(cause~.)+
  theme(axis.title.y = element_text(angle = 0),
axis.text.x = element_text(angle = 60, hjust = 1),
plot.title = element_text(colour = "black", face = "bold", 
    size = 25, vjust = 2,hjust = 0.5), plot.margin = unit(c(0.2, 0.2, 0.2, 0.2), "inches"))
```

![](README_files/figure-markdown_github/unnamed-chunk-3-4.png)

``` r
#100~104年男性四大死因:全台各年齡層人數
df_m <- ddply(h5_m, c("年齡代碼", "N","cause"), summarise, year = factor(year))
ggplot(df_m, aes(x=年齡代碼, y=N, fill=factor(year))) + 
  geom_bar(stat="identity", colour="black", position="dodge")+
  labs(title="100~104年男性四大死因")+
  facet_grid(cause~.)+
  theme(axis.title.y = element_text(angle = 0),
axis.text.x = element_text(angle = 60, hjust = 1),
plot.title = element_text(colour = "black", face = "bold", 
    size = 25, vjust = 2,hjust = 0.5), plot.margin = unit(c(0.2, 0.2, 0.2, 0.2), "inches"))
```

![](README_files/figure-markdown_github/unnamed-chunk-3-5.png)

``` r
#100~104年女性四大死因:全台各年齡層人數
df_f <- ddply(h5_f, c("年齡代碼", "N","cause"), summarise, year = factor(year))
ggplot(df_f, aes(x=年齡代碼, y=N, fill=factor(year))) + 
  geom_bar(stat="identity", colour="black", position="dodge")+
  labs(title="100~104年女性四大死因")+
  facet_grid(cause~.)+
  theme(axis.title.y = element_text(angle = 0),
axis.text.x = element_text(angle = 60, hjust = 1),
plot.title = element_text(colour = "black", face = "bold", 
    size = 25, vjust = 2,hjust = 0.5), plot.margin = unit(c(0.2, 0.2, 0.2, 0.2), "inches"))
```

![](README_files/figure-markdown_github/unnamed-chunk-3-6.png)

結論
----

惡性腫瘤長年居死因之冠，分析近五年，發現男性因惡性腫瘤死亡年齡層上升，女性則為下降，雖未細分惡性腫瘤之種類，仍能看出惡性腫瘤對人民的危害十分嚴重，其他像是糖尿病、肺炎等疾病，也使的人民的性命受到威脅，雖醫療品質不斷進步，仍有許多人不敵病魔，我們能做的就是多注意自己的生活習慣、飲食健康，並避免接觸汙染源或是容易致病的因素。
