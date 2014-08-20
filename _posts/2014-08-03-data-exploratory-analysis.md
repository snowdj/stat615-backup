---
layout: post
title: data exploratory analysis
category : exploratory analysis
tagline: data exploratory analysis
tags : [intro, data, exploratory analysis]
---



Sheri data exploratory analysis
========================================================

## set working directory

```r
setwd("E:\\Dropbox\\book\\economics\\485\\projects\\sheri")
```







### Get the yield data. Only try FortyMile



```r
# Read yield data, firs two row are  comment, , stringsAsFactors=FALSE
yield<-read.csv("myYield.csv", skip =0,  header = T, sep = ",")

yield<-subset(yield, Municipality== "FortyMile")
#str(yield)
#yield[yield$Crop.Year < 2004 & yield$Crop.Year > 1999,]

# Just take 154 obs. of  10 variables

# print table for price
library(xtable)
print(xtable(yield[1:10,]), type = "html", include.rownames = F)
```

<!-- html table generated in R 3.0.3 by xtable 1.7-3 package -->
<!-- Wed Aug 13 20:21:16 2014 -->
<TABLE border=1>
<TR> <TH> Crop.Year </TH> <TH> Municipality </TH> <TH> Product </TH> <TH> totalYieldRegion </TH> <TH> areaTotalRegion </TH> <TH> meanYieldRegion </TH> <TH> meanYieldRegionConverted </TH> <TH> meanYieldRegionOlympicConverted </TH> <TH> areaTotalRegionAllcrop </TH> <TH> areaProportionCrop </TH>  </TR>
  <TR> <TD align="right"> 1978 </TD> <TD> FortyMile </TD> <TD> Barley </TD> <TD align="right"> 19166500.80 </TD> <TD align="right"> 17328 </TD> <TD align="right"> 1106.10 </TD> <TD align="right"> 50.80 </TD> <TD align="right"> 58.99 </TD> <TD align="right"> 245453 </TD> <TD align="right"> 0.07 </TD> </TR>
  <TR> <TD align="right"> 1978 </TD> <TD> FortyMile </TD> <TD> Wheat </TD> <TD align="right"> 199857154.90 </TD> <TD align="right"> 228125 </TD> <TD align="right"> 876.09 </TD> <TD align="right"> 32.19 </TD> <TD align="right"> 39.64 </TD> <TD align="right"> 245453 </TD> <TD align="right"> 0.93 </TD> </TR>
  <TR> <TD align="right"> 1979 </TD> <TD> FortyMile </TD> <TD> Barley </TD> <TD align="right"> 8354679.60 </TD> <TD align="right"> 11716 </TD> <TD align="right"> 713.10 </TD> <TD align="right"> 32.75 </TD> <TD align="right"> 67.88 </TD> <TD align="right"> 242499 </TD> <TD align="right"> 0.05 </TD> </TR>
  <TR> <TD align="right"> 1979 </TD> <TD> FortyMile </TD> <TD> Wheat </TD> <TD align="right"> 118775432.50 </TD> <TD align="right"> 230783 </TD> <TD align="right"> 514.66 </TD> <TD align="right"> 18.91 </TD> <TD align="right"> 38.82 </TD> <TD align="right"> 242499 </TD> <TD align="right"> 0.95 </TD> </TR>
  <TR> <TD align="right"> 1980 </TD> <TD> FortyMile </TD> <TD> Barley </TD> <TD align="right"> 18703056.40 </TD> <TD align="right"> 18662 </TD> <TD align="right"> 1002.20 </TD> <TD align="right"> 46.03 </TD> <TD align="right"> 59.84 </TD> <TD align="right"> 284184 </TD> <TD align="right"> 0.07 </TD> </TR>
  <TR> <TD align="right"> 1980 </TD> <TD> FortyMile </TD> <TD> Durum </TD> <TD align="right"> 36044680.00 </TD> <TD align="right"> 46690 </TD> <TD align="right"> 772.00 </TD> <TD align="right"> 28.37 </TD> <TD align="right"> 62.03 </TD> <TD align="right"> 284184 </TD> <TD align="right"> 0.16 </TD> </TR>
  <TR> <TD align="right"> 1980 </TD> <TD> FortyMile </TD> <TD> Wheat </TD> <TD align="right"> 163849583.20 </TD> <TD align="right"> 218832 </TD> <TD align="right"> 748.75 </TD> <TD align="right"> 27.51 </TD> <TD align="right"> 34.13 </TD> <TD align="right"> 284184 </TD> <TD align="right"> 0.77 </TD> </TR>
  <TR> <TD align="right"> 1981 </TD> <TD> FortyMile </TD> <TD> Barley </TD> <TD align="right"> 13487634.10 </TD> <TD align="right"> 12487 </TD> <TD align="right"> 1080.13 </TD> <TD align="right"> 49.61 </TD> <TD align="right"> 39.64 </TD> <TD align="right"> 274693 </TD> <TD align="right"> 0.05 </TD> </TR>
  <TR> <TD align="right"> 1981 </TD> <TD> FortyMile </TD> <TD> Durum </TD> <TD align="right"> 49256755.20 </TD> <TD align="right"> 55872 </TD> <TD align="right"> 881.60 </TD> <TD align="right"> 32.39 </TD> <TD align="right"> 48.29 </TD> <TD align="right"> 274693 </TD> <TD align="right"> 0.20 </TD> </TR>
  <TR> <TD align="right"> 1981 </TD> <TD> FortyMile </TD> <TD> Wheat </TD> <TD align="right"> 177777244.80 </TD> <TD align="right"> 206334 </TD> <TD align="right"> 861.60 </TD> <TD align="right"> 31.66 </TD> <TD align="right"> 36.44 </TD> <TD align="right"> 274693 </TD> <TD align="right"> 0.75 </TD> </TR>
   </TABLE>










## Plot of yield



```r
# boxplot
# boxplot(yield[,-1])
library(reshape)
library(ggplot2)
library(reshape2)
```

```
## 
## Attaching package: 'reshape2'
## 
## The following objects are masked from 'package:reshape':
## 
##     colsplit, melt, recast
```

```r
histyldb1<-ggplot(data=yield[,-1], aes(as.factor(Product), meanYieldRegionConverted, fill=factor(Product)))
histyldb1+ geom_boxplot() + guides(fill=guide_legend(title=NULL))+labs(title="Boxplot for History Yield", x= "Crop", y="yield")
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 




## Long to wide data frame



```r
# reshape data from wide to long

yield<-dcast(yield, Crop.Year ~ Product, value.var ="meanYieldRegionConverted" )

#head(yield,30)
#tail(yield,30)
print(xtable(head(yield)), type = "html", include.rownames = FALSE)
```

<!-- html table generated in R 3.0.3 by xtable 1.7-3 package -->
<!-- Wed Aug 13 20:21:19 2014 -->
<TABLE border=1>
<TR> <TH> Crop.Year </TH> <TH> Barley </TH> <TH> Canola </TH> <TH> Durum </TH> <TH> Peas </TH> <TH> Wheat </TH>  </TR>
  <TR> <TD align="right"> 1978 </TD> <TD align="right"> 50.80 </TD> <TD align="right">  </TD> <TD align="right">  </TD> <TD align="right">  </TD> <TD align="right"> 32.19 </TD> </TR>
  <TR> <TD align="right"> 1979 </TD> <TD align="right"> 32.75 </TD> <TD align="right">  </TD> <TD align="right">  </TD> <TD align="right">  </TD> <TD align="right"> 18.91 </TD> </TR>
  <TR> <TD align="right"> 1980 </TD> <TD align="right"> 46.03 </TD> <TD align="right">  </TD> <TD align="right"> 28.37 </TD> <TD align="right">  </TD> <TD align="right"> 27.51 </TD> </TR>
  <TR> <TD align="right"> 1981 </TD> <TD align="right"> 49.61 </TD> <TD align="right">  </TD> <TD align="right"> 32.39 </TD> <TD align="right">  </TD> <TD align="right"> 31.66 </TD> </TR>
  <TR> <TD align="right"> 1982 </TD> <TD align="right"> 42.55 </TD> <TD align="right">  </TD> <TD align="right"> 28.31 </TD> <TD align="right">  </TD> <TD align="right"> 30.45 </TD> </TR>
  <TR> <TD align="right"> 1983 </TD> <TD align="right"> 40.66 </TD> <TD align="right">  </TD> <TD align="right"> 28.11 </TD> <TD align="right">  </TD> <TD align="right"> 33.45 </TD> </TR>
   </TABLE>

```r
#summary(yield)
#head(yield)
#tail(yield,30)
```


## line to show trend


```r
#library(scales)


# line
histyldl <- ggplot(yield, aes(yield[,1]))
yldBarley <- geom_line(aes(y = yield[,2], colour = "Barley"))
yldCanola <- geom_line(aes(y = yield[,3], colour = "Canola"))
yldDurum<- geom_line(aes(y = yield[,4], colour = "Durum"))
yldPeas<- geom_line(aes(y = yield[,5], colour = "Peas"))
yldWheat<-geom_line(aes(y = yield[,6], colour = "Wheat"))

histyldl + yldBarley + yldCanola + yldDurum + yldPeas+ yldWheat + labs(title="Trend of History yield", x= "Date", y="yield")+ theme(legend.title=element_blank())
```

```
## Warning: Removed 6 rows containing missing values (geom_path).
## Warning: Removed 2 rows containing missing values (geom_path).
## Warning: Removed 17 rows containing missing values (geom_path).
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 

##### 2001 data is too small, like outlier.

## Detrand yield and get varibility (standard deviation) of yield for simulation


```r
library(forecast)
source("decomp.r")
str(yield)
```

'data.frame':	36 obs. of  6 variables:
 $ Crop.Year: int  1978 1979 1980 1981 1982 1983 1984 1985 1986 1987 ...
 $ Barley   : num  50.8 32.8 46 49.6 42.5 ...
 $ Canola   : num  NA NA NA NA NA ...
 $ Durum    : num  NA NA 28.4 32.4 28.3 ...
 $ Peas     : num  NA NA NA NA NA NA NA NA NA NA ...
 $ Wheat    : num  32.2 18.9 27.5 31.7 30.4 ...

```r
sdDtrend <- function(x) sd(decomp(x, FALSE)$remainder)

#sd(decomp(yield$Wheat)$remainder)
#sd(decomp(yield[,3], FALSE)$remainder)
   
#sdyld0 <- apply(yield[,-1],2, function(x) sd(x))

## detrend yields data

detrendYield<-apply(yield[,-1],2, function(x) decomp(x, FALSE)$remainder)


# with the NA in different cell, row , we need to deal with NA data.
sdyld <- apply(yield[,-1],2, function(x) sdDtrend(x))

# recent mean of yield 5 year
# meanyld <- apply(yield[19:23,-1],2, mean)
#decomyld<-decomp(yield[,2], FALSE)
#plot(1991:2013,decomyld$trend, type="o")
#plot(1991:2013,decomyld$remainder, type="o" )

print(xtable(t(as.data.frame(sdyld)),caption="Detrended Standard Deviation"),  type = "html", include.rownames = FALSE)
```

<!-- html table generated in R 3.0.3 by xtable 1.7-3 package -->
<!-- Wed Aug 13 20:21:27 2014 -->
<TABLE border=1>
<CAPTION ALIGN="bottom"> Detrended Standard Deviation </CAPTION>
<TR> <TH> Barley </TH> <TH> Canola </TH> <TH> Durum </TH> <TH> Peas </TH> <TH> Wheat </TH>  </TR>
  <TR> <TD align="right"> 9.52 </TD> <TD align="right">  </TD> <TD align="right">  </TD> <TD align="right">  </TD> <TD align="right"> 5.89 </TD> </TR>
   </TABLE>

```r
write.table(yield, "yield01.csv")
write.table(meanyld, "meanyld.csv")
```

```
Error: object 'meanyld' not found
```

```r
write.table(sdyld, "sdyld.csv")
as.data.frame(meanyld)
```

```
Error: object 'meanyld' not found
```






