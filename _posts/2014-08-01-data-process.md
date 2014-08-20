---
layout: post
title: data process
category : data process
tagline: data process
tags : [intro, data process]
---




Sheri data process
========================================================




## set working directory

```r
setwd("E:\\Dropbox\\book\\economics\\485\\projects\\sheri")
```


## load data from xls


```r
library("xlsx")
```

```
## Loading required package: rJava
## Loading required package: xlsxjars
```

```r
myData = read.csv("yielddata.csv", header = T, sep = ",",
                  skip =2, stringsAsFactors=FALSE, colClasses="character")
head(myData)
```

```
##   Crop.Year Municipality Ag.Product Dry.Irr Total.Acres.Insured
## 1      1978      Acadia      Barley     Dry                872 
## 2      1978   Athabasca      Barley     Dry             18,160 
## 3      1978    Barrhead      Barley     Dry             11,596 
## 4      1978      Beaver      Barley     Dry             31,257 
## 5      1978   Big Lakes      Barley     Dry              8,885 
## 6      1978 Birch Hills      Barley     Dry             12,476 
##   Average.Yield..kgs.acre. Number.of.Farms Average.Acres.per.Farm
## 1                   875.1              12                     73 
## 2                   776.0              50                    363 
## 3                 1,026.8              36                    322 
## 4                   470.1             121                    258 
## 5                   640.6              26                    342 
## 6                   799.5              46                    271
```

```r
#class(myData[,1])

# some numbers contain commas : 21,932 or 1,205.4
col2cvt <- 5:8
myData[,col2cvt] <- lapply(myData[,col2cvt],function(x){as.numeric(gsub(",", "", x))})
```





### exploratory data analysis




#### Only take six areas (Municipality):Forty Mile, Vulcan, Red Deer, Vermilion River, Westlock, Smoky River, myData1

```r
# set the date format
#myData[, 1] <- as.Date(myData[, 1], format = '%Y')

# Only take six areas (Municipality):Forty Mile, Vulcan, Red Deer, Vermilion River, Westlock, Smoky River,
# there is a space " " at the end of each municipality. Use gsub Get rid of the space " ". or use "string" package "str_trim"
myData[,2] = gsub(" ","", myData[,2] , fixed=TRUE)
#class(myData[,3])
# many regions we do not need
#table(myData[,2])
#str(myData[,2])
#  only pick up 6 regions
myData1 = subset(myData, Municipality %in% c("FortyMile" , "Vulcan", "RedDeer", "VermilionRiver","Westlock", "SmokyRiver"))
#head(myData1)
#str(myData1)
```


###Only 6 regions left to myData1

```r
# only 
table(myData1[,2])
```

```
## 
##      FortyMile        RedDeer     SmokyRiver VermilionRiver         Vulcan 
##            583            188            202            201            499 
##       Westlock 
##            176
```

```r
# print table for price
library(xtable)

print(xtable(summary(myData1[,2])), type = "html", include.rownames = F)
```

```
## Error: xtable.table is not implemented for tables of > 2 dimensions
```



#### Only take five products: Barley, Canola, Peas,Field, Wheat - Durum (only for Vulcan and Forty Mile),Wheat (group by year and Municipality) to myData2



```r
# there are many crops
table(myData1[,3])
```

```
## 
##                        Barley                    Beans, Dry 
##                           281                            20 
##     Beans, Dry - Black/Others   Beans, Dry - Great Northern 
##                             9                            13 
##             Beans, Dry - Pink            Beans, Dry - Pinto 
##                             6                            13 
##        Beans, Dry - Small Red                        Canola 
##                            13                           271 
##                  Corn (Grain)                          Flax 
##                             1                           128 
##                 Hybrid Canola                          Oats 
##                            14                           213 
##                   Peas, Field                      Potatoes 
##                           140                            15 
##                    Rye - Fall                  Rye - Spring 
##                            49                             1 
##                   Sugar Beets              Sugar Beets (60) 
##                            19                             1 
##            Triticale - Spring            Triticale - Winter 
##                            20                             1 
## Wheat - Canada Prairie Spring                 Wheat - Durum 
##                            65                           124 
##       Wheat - Hard Red Spring       Wheat - Hard Red Winter 
##                           268                            76 
##     Wheat - Soft White Spring 
##                            88
```

```r
# only take some crop
myData2 = subset(myData1, Ag.Product %in% c("Barley", "Canola", "Peas, Field", "Wheat - Durum" ,"Wheat - Canada Prairie Spring","Wheat - Hard Red Spring","Wheat - Hard Red Winter","Wheat - Soft White Spring"))
table(myData2[,3])
```

```
## 
##                        Barley                        Canola 
##                           281                           271 
##                   Peas, Field Wheat - Canada Prairie Spring 
##                           140                            65 
##                 Wheat - Durum       Wheat - Hard Red Spring 
##                           124                           268 
##       Wheat - Hard Red Winter     Wheat - Soft White Spring 
##                            76                            88
```

```r
#  generate new variable by Ag.Product using "ifelse"

myData2$Product = ifelse( myData2$Ag.Product == "Barley" , "Barley", 
                        ifelse(myData2$Ag.Product =="Canola", "Canola",
                               ifelse(myData2$Ag.Product =="Peas, Field", "Peas",
                                      ifelse(myData2$Ag.Product =="Wheat - Durum", "Durum", "Wheat"))))                 
```


#### only 5 crops left to myData2


```r
# only 5 crops left
table(myData2$Product)
```

```
## 
## Barley Canola  Durum   Peas  Wheat 
##    281    271    124    140    497
```

#### Sample from myData2


```r
head(myData2)
```

```
##    Crop.Year   Municipality Ag.Product Dry.Irr Total.Acres.Insured
## 18      1978      FortyMile     Barley     Dry               17319
## 38      1978        RedDeer     Barley     Dry              197451
## 42      1978     SmokyRiver     Barley     Dry               30664
## 55      1978 VermilionRiver     Barley     Dry               59550
## 56      1978         Vulcan     Barley     Dry               61588
## 59      1978       Westlock     Barley     Dry               17322
##    Average.Yield..kgs.acre. Number.of.Farms Average.Acres.per.Farm Product
## 18                   1106.1              76                    228  Barley
## 38                   1318.0             314                    629  Barley
## 42                    857.6              93                    330  Barley
## 55                    898.7             126                    473  Barley
## 56                   1236.4             200                    308  Barley
## 59                   1006.4              49                    354  Barley
```

```r
tail(myData2)
```

```
##       Crop.Year Municipality                Ag.Product Dry.Irr
## 14520      2013      RedDeer Wheat - Soft White Spring     Dry
## 14523      2013      RedDeer               Peas, Field     Dry
## 14538      2013    FortyMile Wheat - Soft White Spring     Irr
## 14547      2013       Vulcan               Peas, Field     Irr
## 14596      2013    FortyMile               Peas, Field     Irr
## 14601      2013    FortyMile                    Barley     Irr
##       Total.Acres.Insured Average.Yield..kgs.acre. Number.of.Farms
## 14520                2023                   2210.9              11
## 14523                7604                   1193.4              42
## 14538                1159                   2745.7               7
## 14547                2503                   1500.3              16
## 14596                1112                   1543.1              11
## 14601                1872                   2279.5              20
##       Average.Acres.per.Farm Product
## 14520                    184   Wheat
## 14523                    181    Peas
## 14538                    166   Wheat
## 14547                    156    Peas
## 14596                    101    Peas
## 14601                     94  Barley
```

#### Get the  total output = yield * acres / average per year, but different from dry/irrigate



```r
myData2$totalYield = with(myData2, Average.Yield..kgs.acre. * Number.of.Farms * Average.Acres.per.Farm)
myData2$areaRegion =  with(myData2, Number.of.Farms * Average.Acres.per.Farm)
myData2$meanYield = with(myData2, totalYield/areaRegion)
head(myData2)
```

```
##    Crop.Year   Municipality Ag.Product Dry.Irr Total.Acres.Insured
## 18      1978      FortyMile     Barley     Dry               17319
## 38      1978        RedDeer     Barley     Dry              197451
## 42      1978     SmokyRiver     Barley     Dry               30664
## 55      1978 VermilionRiver     Barley     Dry               59550
## 56      1978         Vulcan     Barley     Dry               61588
## 59      1978       Westlock     Barley     Dry               17322
##    Average.Yield..kgs.acre. Number.of.Farms Average.Acres.per.Farm Product
## 18                   1106.1              76                    228  Barley
## 38                   1318.0             314                    629  Barley
## 42                    857.6              93                    330  Barley
## 55                    898.7             126                    473  Barley
## 56                   1236.4             200                    308  Barley
## 59                   1006.4              49                    354  Barley
##    totalYield areaRegion meanYield
## 18   19166501      17328    1106.1
## 38  260312908     197506    1318.0
## 42   26319744      30690     857.6
## 55   53560723      59598     898.7
## 56   76162240      61600    1236.4
## 59   17457014      17346    1006.4
```

#### Get the total per region output = yield * acres / region average per year, combining the dry/irrigate

```r
library(plyr)
tmyData2 = ddply(myData2, c("Crop.Year", "Municipality", "Product"), summarise,
               totalYieldRegion = sum(totalYield) ,
               areaTotalRegion = sum(areaRegion),
               meanYieldRegion = (totalYieldRegion/areaTotalRegion)
                )

head(tmyData2,30)
```

```
##    Crop.Year   Municipality Product totalYieldRegion areaTotalRegion
## 1       1978      FortyMile  Barley         19166501           17328
## 2       1978      FortyMile   Wheat        199857155          228125
## 3       1978        RedDeer  Barley        260312908          197506
## 4       1978        RedDeer  Canola         42465150           68250
## 5       1978        RedDeer   Wheat         11288862           10140
## 6       1978     SmokyRiver  Barley         26319744           30690
## 7       1978     SmokyRiver  Canola          5870600           15760
## 8       1978     SmokyRiver   Wheat          9964466           13144
## 9       1978 VermilionRiver  Barley         53560723           59598
## 10      1978 VermilionRiver  Canola         37042236           69720
## 11      1978 VermilionRiver   Wheat         44929292           54612
## 12      1978         Vulcan  Barley         76162240           61600
## 13      1978         Vulcan  Canola         21297936           33440
## 14      1978         Vulcan   Wheat        169021822          175513
## 15      1978       Westlock  Barley         17457014           17346
## 16      1978       Westlock  Canola          5159408           10075
## 17      1978       Westlock   Wheat          2145877            2189
## 18      1979      FortyMile  Barley          8354680           11716
## 19      1979      FortyMile   Wheat        118775433          230783
## 20      1979        RedDeer  Barley        191177016          141928
## 21      1979        RedDeer  Canola         39652828           65090
## 22      1979        RedDeer   Wheat         11587666           11448
## 23      1979     SmokyRiver  Barley         15669984           18744
## 24      1979     SmokyRiver  Canola          6216352           25280
## 25      1979     SmokyRiver   Wheat         12411306           12420
## 26      1979 VermilionRiver  Barley         46552205           41650
## 27      1979 VermilionRiver  Canola         34650461           75888
## 28      1979 VermilionRiver   Wheat         52387830           51150
## 29      1979         Vulcan  Barley         43430499           53152
## 30      1979         Vulcan  Canola         16333219           37539
##    meanYieldRegion
## 1        1106.1000
## 2         876.0862
## 3        1318.0000
## 4         622.2000
## 5        1113.3000
## 6         857.6000
## 7         372.5000
## 8         758.1000
## 9         898.7000
## 10        531.3000
## 11        822.7000
## 12       1236.4000
## 13        636.9000
## 14        963.0160
## 15       1006.4000
## 16        512.1000
## 17        980.3000
## 18        713.1000
## 19        514.6628
## 20       1347.0000
## 21        609.2000
## 22       1012.2000
## 23        836.0000
## 24        245.9000
## 25        999.3000
## 26       1117.7000
## 27        456.6000
## 28       1024.2000
## 29        817.1000
## 30        435.1000
```

```r
str(tmyData2)
```

```
## 'data.frame':	823 obs. of  6 variables:
##  $ Crop.Year       : chr  "1978" "1978" "1978" "1978" ...
##  $ Municipality    : chr  "FortyMile" "FortyMile" "RedDeer" "RedDeer" ...
##  $ Product         : chr  "Barley" "Wheat" "Barley" "Canola" ...
##  $ totalYieldRegion: num  1.92e+07 2.00e+08 2.60e+08 4.25e+07 1.13e+07 ...
##  $ areaTotalRegion : num  17328 228125 197506 68250 10140 ...
##  $ meanYieldRegion : num  1106 876 1318 622 1113 ...
```






#### Keep recent five year for Olympic average

```r
# Only take years: 2009-2013
myData3 = subset(tmyData2, Crop.Year >= 2009 & Crop.Year <=2013)

head(myData3, 30)
```

```
##     Crop.Year   Municipality Product totalYieldRegion areaTotalRegion
## 694      2009      FortyMile  Barley         29090028           23038
## 695      2009      FortyMile  Canola         12750688           12676
## 696      2009      FortyMile   Durum        226672820          183700
## 697      2009      FortyMile    Peas         55880873           73062
## 698      2009      FortyMile   Wheat        209227395          177546
## 699      2009        RedDeer  Barley        159659018          110782
## 700      2009        RedDeer  Canola         81100166           98208
## 701      2009        RedDeer    Peas          7809360            6240
## 702      2009        RedDeer   Wheat        120398545           78033
## 703      2009     SmokyRiver  Barley          5938426            4431
## 704      2009     SmokyRiver  Canola        123721920          180880
## 705      2009     SmokyRiver    Peas          9052268            9018
## 706      2009     SmokyRiver   Wheat        149866370          143860
## 707      2009 VermilionRiver  Barley        105618492           83592
## 708      2009 VermilionRiver  Canola        119816176          188124
## 709      2009 VermilionRiver    Peas         32271037           31527
## 710      2009 VermilionRiver   Wheat        222912954          175585
## 711      2009         Vulcan  Barley         85630526           89728
## 712      2009         Vulcan  Canola         66558756          102236
## 713      2009         Vulcan   Durum         75874229           80996
## 714      2009         Vulcan    Peas         34073934           57757
## 715      2009         Vulcan   Wheat        148476943          173792
## 716      2009       Westlock  Barley         36755770           26352
## 717      2009       Westlock  Canola         68878726           79647
## 718      2009       Westlock    Peas          6373016            4660
## 719      2009       Westlock   Wheat         73093025           53640
## 720      2010      FortyMile  Barley         48594201           39675
## 721      2010      FortyMile  Canola         30405717           35058
## 722      2010      FortyMile   Durum         81617337           64261
## 723      2010      FortyMile    Peas         58530549           62202
##     meanYieldRegion
## 694       1262.6976
## 695       1005.8921
## 696       1233.9293
## 697        764.8418
## 698       1178.4405
## 699       1441.2000
## 700        825.8000
## 701       1251.5000
## 702       1542.9183
## 703       1340.2000
## 704        684.0000
## 705       1003.8000
## 706       1041.7515
## 707       1263.5000
## 708        636.9000
## 709       1023.6000
## 710       1269.5444
## 711        954.3345
## 712        651.0305
## 713        936.7651
## 714        589.9533
## 715        854.3370
## 716       1394.8000
## 717        864.8000
## 718       1367.6000
## 719       1362.6589
## 720       1224.8066
## 721        867.2975
## 722       1270.0913
## 723        940.9754
```

```r
str(myData3)
```

```
## 'data.frame':	130 obs. of  6 variables:
##  $ Crop.Year       : chr  "2009" "2009" "2009" "2009" ...
##  $ Municipality    : chr  "FortyMile" "FortyMile" "FortyMile" "FortyMile" ...
##  $ Product         : chr  "Barley" "Canola" "Durum" "Peas" ...
##  $ totalYieldRegion: num  2.91e+07 1.28e+07 2.27e+08 5.59e+07 2.09e+08 ...
##  $ areaTotalRegion : num  23038 12676 183700 73062 177546 ...
##  $ meanYieldRegion : num  1263 1006 1234 765 1178 ...
```

```r
names(myData3)
```

```
## [1] "Crop.Year"        "Municipality"     "Product"         
## [4] "totalYieldRegion" "areaTotalRegion"  "meanYieldRegion"
```

```r
# myData3$totalYield = with(myData3, Average.Yield..kgs.acre. * Number.of.Farms * Average.Acres.per.Farm)
# myData3$areaRegion =  with(myData3, Number.of.Farms * Average.Acres.per.Farm)
# myData3$meanYield = with(myData3, totalYield/areaRegion)
# #myData3$Crop.Year = factor(myData3$Crop.Year)
# #myData3$Municipality = factor(myData3$Municipality)
# #myData3$Ag.Product = factor(myData3$Ag.Product)
# #myData3$Average.Yield..kgs.acre = as.numeric(myData3$Average.Yield..kgs.acre)
# 
# #myData3$ Number.of.Farms = as.numeric(myData3$ Number.of.Farms)
```

#### Get the total region output = yield * acre or region average per year

```r
library(plyr)
# tmyData3 = ddply(myData3, c("Crop.Year", "Municipality", "Product"), summarise,
#                totalYieldRegion = sum(totalYield) ,
#                areaTotalRegion = sum(areaRegion),
#                meanYieldRegion = (totalYieldRegion/areaTotalRegion)
#                 )
# 
# 
# head(tmyData3,30)
# str(tmyData3)
```


#### Five year Olympic mean yield for "Municipality", "Product"


```r
# Five year Olympic mean yield
omyData3 = ddply(myData3, c("Municipality", "Product"), summarise,
               meanYieldRegionOlympic = mean(meanYieldRegion, trim = 0.2)
                 )
# head(omyData3,30)
# str(omyData3)
# test it is right
# cmyData3[c("Municipality" == "FortyMile", "Product" == "Barley"),]
# 
# subset(cmyData3, Municipality == "FortyMile" & Product == "Barley")
# 
# mean(subset(cmyData3, Municipality == "FortyMile" & Product == "Barley")$meanYieldRegion)
# mean(subset(cmyData3, Municipality == "FortyMile" & Product == "Barley")$meanYieldRegion, trim = 0.2)
# mean(c(1262.698, 1233.912, 1356.684) )
# 
# head(cmyData3,5)
# str(cmyData3)
```

####  Convert to right unit


```r
# ccmyData3$meanYieldRegionOlympic[ccmyData3$Product == "Barley"] = ccmyData3$meanYieldRegionOlympic/1000*45.93
#        
# if(ccmyData3$Product = "Barley"){
#         ccmyData3$meanYieldRegionOlympic = ccmyData3$meanYieldRegionOlympic/1000*45.93
#         }   
#  generate new variable by Ag.Product using "ifelse"

omyData3$meanYieldRegionOlympicConverted = ifelse(
        omyData3$Product == "Barley" , omyData3$meanYieldRegionOlympic/1000*45.93, 
                        ifelse(omyData3$Product =="Canola", omyData3$meanYieldRegionOlympic /1000*44.092,
                               ifelse(omyData3$Product =="Peas", omyData3$meanYieldRegionOlympic/1000*36.744,
                                      ifelse(omyData3$Product =="Durum", omyData3$meanYieldRegionOlympic/1000*36.744, 
                                             omyData3$meanYieldRegionOlympic/1000*36.744)
                                             )
                                      )
                               )

# this is a combined dataframe with all information, it add new data to cmyData3
# cmyData$meanYieldRegionOlympicConverted = ifelse(
#         omyData3$Product == "Barley" , omyData3$meanYieldRegionOlympic/1000*45.93, 
#                         ifelse(omyData3$Product =="Canola", omyData3$meanYieldRegionOlympic /1000*44.092,
#                                ifelse(omyData3$Product =="Peas", omyData3$meanYieldRegionOlympic/1000*36.744,
#                                       ifelse(omyData3$Product =="Durum", omyData3$meanYieldRegionOlympic/1000*36.744, 
#                                              omyData3$meanYieldRegionOlympic/1000*36.744)
#                                              )
#                                       )
#                                )

head(omyData3,30)
```

```
##      Municipality Product meanYieldRegionOlympic
## 1       FortyMile  Barley              1284.4311
## 2       FortyMile  Canola               899.0997
## 3       FortyMile   Durum              1314.1949
## 4       FortyMile    Peas               991.7946
## 5       FortyMile   Wheat              1225.2318
## 6         RedDeer  Barley              1511.4000
## 7         RedDeer  Canola               865.1333
## 8         RedDeer    Peas              1191.4333
## 9         RedDeer   Wheat              1628.6087
## 10     SmokyRiver  Barley              1350.5333
## 11     SmokyRiver  Canola               774.1000
## 12     SmokyRiver    Peas              1042.4333
## 13     SmokyRiver   Wheat              1288.0603
## 14 VermilionRiver  Barley              1306.5000
## 15 VermilionRiver  Canola               803.1667
## 16 VermilionRiver    Peas              1000.1667
## 17 VermilionRiver   Wheat              1280.6669
## 18         Vulcan  Barley              1477.9196
## 19         Vulcan  Canola               880.4510
## 20         Vulcan   Durum              1310.8507
## 21         Vulcan    Peas              1347.7995
## 22         Vulcan   Wheat              1267.0766
## 23       Westlock  Barley              1611.1000
## 24       Westlock  Canola              1005.6667
## 25       Westlock    Peas              1405.5000
## 26       Westlock   Wheat              1774.5902
##    meanYieldRegionOlympicConverted
## 1                         58.99392
## 2                         39.64310
## 3                         48.28878
## 4                         36.44250
## 5                         45.01992
## 6                         69.41860
## 7                         38.14546
## 8                         43.77803
## 9                         59.84160
## 10                        62.03000
## 11                        34.13162
## 12                        38.30317
## 13                        47.32849
## 14                        60.00755
## 15                        35.41322
## 16                        36.75012
## 17                        47.05682
## 18                        67.88085
## 19                        38.82085
## 20                        48.16590
## 21                        49.52354
## 22                        46.55746
## 23                        73.99782
## 24                        44.34185
## 25                        51.64369
## 26                        65.20554
```

```r
str(omyData3)
```

```
## 'data.frame':	26 obs. of  4 variables:
##  $ Municipality                   : chr  "FortyMile" "FortyMile" "FortyMile" "FortyMile" ...
##  $ Product                        : chr  "Barley" "Canola" "Durum" "Peas" ...
##  $ meanYieldRegionOlympic         : num  1284 899 1314 992 1225 ...
##  $ meanYieldRegionOlympicConverted: num  59 39.6 48.3 36.4 45 ...
```

#### Combine all data together to omni dataframe




```r
# First get data from base datafram myData2
myYield = tmyData2


# Add the 5 year Olympic mean
myYield$meanYieldRegionOlympicConverted = ifelse(
        myYield$Product == "Barley" , omyData3$meanYieldRegionOlympicConverted, 
                        ifelse(myYield$Product =="Canola", omyData3$meanYieldRegionOlympicConverted,
                               ifelse(myYield$Product =="Peas", omyData3$meanYieldRegionOlympicConverted,
                                      ifelse(myYield$Product =="Durum", omyData3$meanYieldRegionOlympicConverted, 
                                             omyData3$meanYieldRegionOlympicConverted
                                             )
                                      )
                               )
        )

str(myYield)
```

```
## 'data.frame':	823 obs. of  7 variables:
##  $ Crop.Year                      : chr  "1978" "1978" "1978" "1978" ...
##  $ Municipality                   : chr  "FortyMile" "FortyMile" "RedDeer" "RedDeer" ...
##  $ Product                        : chr  "Barley" "Wheat" "Barley" "Canola" ...
##  $ totalYieldRegion               : num  1.92e+07 2.00e+08 2.60e+08 4.25e+07 1.13e+07 ...
##  $ areaTotalRegion                : num  17328 228125 197506 68250 10140 ...
##  $ meanYieldRegion                : num  1106 876 1318 622 1113 ...
##  $ meanYieldRegionOlympicConverted: num  59 39.6 48.3 36.4 45 ...
```

```r
head(myYield)
```

```
##   Crop.Year Municipality Product totalYieldRegion areaTotalRegion
## 1      1978    FortyMile  Barley         19166501           17328
## 2      1978    FortyMile   Wheat        199857155          228125
## 3      1978      RedDeer  Barley        260312908          197506
## 4      1978      RedDeer  Canola         42465150           68250
## 5      1978      RedDeer   Wheat         11288862           10140
## 6      1978   SmokyRiver  Barley         26319744           30690
##   meanYieldRegion meanYieldRegionOlympicConverted
## 1       1106.1000                        58.99392
## 2        876.0862                        39.64310
## 3       1318.0000                        48.28878
## 4        622.2000                        36.44250
## 5       1113.3000                        45.01992
## 6        857.6000                        69.41860
```





#### Add the area proportion for each crop


```r
### ddply and join approach: works
# myYield2 = join(myYield,
#      ddply(myYield,c("Crop.Year", "Municipality"),
#            summarise, areaTotalRegionAllcrop = sum(areaTotalRegion)),
#      by=(c ("Crop.Year","Municipality")),type="left")
# str(myYield2)
# head(myYield2)

### ave{} approach

myYield$areaTotalRegionAllcrop <- with( myYield, ave(areaTotalRegion,Crop.Year, Municipality , FUN = sum) )
myYield$areaProportionCrop = with(myYield, areaTotalRegion/areaTotalRegionAllcrop)


head(myYield)
```

```
##   Crop.Year Municipality Product totalYieldRegion areaTotalRegion
## 1      1978    FortyMile  Barley         19166501           17328
## 2      1978    FortyMile   Wheat        199857155          228125
## 3      1978      RedDeer  Barley        260312908          197506
## 4      1978      RedDeer  Canola         42465150           68250
## 5      1978      RedDeer   Wheat         11288862           10140
## 6      1978   SmokyRiver  Barley         26319744           30690
##   meanYieldRegion meanYieldRegionOlympicConverted areaTotalRegionAllcrop
## 1       1106.1000                        58.99392                 245453
## 2        876.0862                        39.64310                 245453
## 3       1318.0000                        48.28878                 275896
## 4        622.2000                        36.44250                 275896
## 5       1113.3000                        45.01992                 275896
## 6        857.6000                        69.41860                  59594
##   areaProportionCrop
## 1         0.07059600
## 2         0.92940400
## 3         0.71587120
## 4         0.24737582
## 5         0.03675298
## 6         0.51498473
```

```r
# 
# # try reshape
# library(reshape)
# 
# 
# # try library(sqldf)
# library(sqldf)
# 
# myYield1 = sqldf("SELECT
# *
# , sum(areaTotalRegion) as areaTotalRegionAllcrop
# FROM myYield
# GROUP BY
# Crop_Year
# , Municipality;")
# 
# head(myYield1)
# str(myYield1)
# sqldf("select * from myYield;")
      
      
# add the total area for all crop in one region back to myYield
```


#### Export myYield to myYield.csv at "E:\\Dropbox\\book\\economics\\485\\projects\\sheri"

```r
write.table(myYield, "myYield.csv", sep=",")
```
