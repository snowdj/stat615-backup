---
layout: post
title: simulatoin analysis
category : simulatoin
tagline: simulatoin analysis
tags : [data, simulatoin, analysis]
---


Sheri data simulatoin analysis
========================================================

## set working directory


```r
setwd("E:\\Dropbox\\book\\economics\\485\\projects\\sheri")
```

## Load data from yield 01 which is FortyMiles


```r
yield <- read.table("yield01.csv" , skip =0,  header = T, sep = ",", stringsAsFactors=FALSE)


subsetMunicipality = "FortyMile"

str(yield)
```

```
## 'data.frame':	36 obs. of  6 variables:
##  $ Crop.Year: int  1978 1979 1980 1981 1982 1983 1984 1985 1986 1987 ...
##  $ Barley   : num  50.8 32.8 46 49.6 42.5 ...
##  $ Canola   : num  NA NA NA NA NA ...
##  $ Durum    : num  NA NA 28.4 32.4 28.3 ...
##  $ Peas     : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ Wheat    : num  32.2 18.9 27.5 31.7 30.4 ...
```





## Detrand yield and get varibility (standard deviation) of yield for simulation
Method is from 

 - [Decompose the data into trend components](http://robjhyndman.com/hyndsight/tscharacteristics/)






## 



```r
library(forecast)

#source("decomp.r")
# str(yield)
# yield
# ts(yield$Barley)
#decompose(yield$Barley)
#fitDereand <- stl(yield$Barley, s.window= "periodic")



# remainder is the detrend data
#sdDtrend <- function(x) sd(decomp(x, FALSE)$remainder)

#sdDtrend1 <- function(x) sd(decompose(x, FALSE)$remainder)




## detrend yields data from full data set . delete the data with NA data.
yield1997 <- subset(yield, Crop.Year>1996)
detrendYield<-apply(yield1997[,-1],2, function(x) decomp(x, FALSE)$remainder)
```


## Calculate the covariance matrix



```r
covDetrendYield <-cov(detrendYield)
covDetrendYield
```

```
##           Barley    Canola    Durum     Peas     Wheat
## Barley 27.119420  9.359685 26.64500 30.23422 15.307117
## Canola  9.359685 16.966803 17.26439 14.60098  5.556236
## Durum  26.644999 17.264393 48.76749 57.24628 15.717698
## Peas   30.234220 14.600977 57.24628 77.74708 16.765585
## Wheat  15.307117  5.556236 15.71770 16.76559 10.012012
```

```r
#corDetrendYield<-cor(detrendYield)
#corDetrendYield
# with the NA in different cell, row , we need to deal with NA data.
sdDetreandYield <- apply(detrendYield,2, function(x) sdDtrend(x))
```

```
## Error: could not find function "sdDtrend"
```

```r
#sdyld <- NA


# print(xtable(t(as.data.frame(sdyld)),caption="Detrended Standard Deviation"),  type = "html", include.rownames = FALSE)
write.table(detrendYield, "detrendYield.csv")
write.table(sdDetreandYield, "sdDetreandYield.csv")
```

```
## Error: object 'sdDetreandYield' not found
```

```r
write.table(corDetrendYield, "corDetrendYield.csv")
```

```
## Error: object 'corDetrendYield' not found
```




## Load the mean of the yield

```r
meanYield <- read.csv("meanYield.csv", )

meanYield <- subset(meanYield, Municipality == subsetMunicipality)
```




## Simulation 1000 for the yields based on

[Simulating data following a given covariance structure](http://www.r-bloggers.com/simulating-data-following-a-given-covariance-structure/)


```r
library(lattice) # for splom
library(car)     # for vif

# number of observations to simulate
nobs = 1000
 


# Using a correlation matrix (let' assume that all variables
# have unit variance
M = corDetrendYield
```

```
## Error: object 'corDetrendYield' not found
```

```r
M = covDetrendYield
        
        
# Cholesky decomposition
L = chol(M)
nvars = dim(L)[1]
 
# R chol function produces an upper triangular version of L
# so we have to transpose it.
# Just to be sure we can have a look at t(L) and the
# product of the Cholesky decomposition by itself
 
t(L)
```

```
##          Barley     Canola    Durum       Peas  Wheat
## Barley 5.207631 0.00000000 0.000000  0.0000000 0.0000
## Canola 1.797302 3.70627959 0.000000  0.0000000 0.0000
## Durum  5.116530 2.17696589 4.224859  0.0000000 0.0000
## Peas   5.805753 1.12411525 5.939562  2.7382987 0.0000
## Wheat  2.939363 0.07374338 0.122566 -0.4055491 1.0896
```

```r
t(L) %*% L
```

```
##           Barley    Canola    Durum     Peas     Wheat
## Barley 27.119420  9.359685 26.64500 30.23422 15.307117
## Canola  9.359685 16.966803 17.26439 14.60098  5.556236
## Durum  26.644999 17.264393 48.76749 57.24628 15.717698
## Peas   30.234220 14.600977 57.24628 77.74708 16.765585
## Wheat  15.307117  5.556236 15.71770 16.76559 10.012012
```


rdata is ready for simulation with the mean and sd.


```r
set.seed(143) #Set the random number seed to allow us to repeat results (must be integer)


# Random variable from crops mean and 




#matrixYieldSimulated = matrix(c(rnorm(nvars*nobs), ), nrow=nvars, ncol=nobs)

# Random variables that follow an M correlation matrix, 
r = t(L) %*% matrix(rnorm(nvars*nobs), nrow=nvars, ncol=nobs)
r = t(r)
 

## Add mean to the matrix

# or matrix(... byrow= T)
r = r + t(matrix(rep(meanYield$meanYieldRegionOlympicConverted, nobs), ncol=nobs))


rdata = as.data.frame(r)
names(rdata)
```

```
## [1] "Barley" "Canola" "Durum"  "Peas"   "Wheat"
```

```r
# Plotting and basic stats
splom(rdata)
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-7.png) 

```r
cov(rdata)
```

```
##           Barley    Canola    Durum     Peas     Wheat
## Barley 24.655252  8.983635 24.69509 28.08011 13.896662
## Canola  8.983635 16.315507 15.97093 13.46394  5.186813
## Durum  24.695090 15.970933 44.77164 52.41808 14.401988
## Peas   28.080114 13.463935 52.41808 70.96628 15.512884
## Wheat  13.896662  5.186813 14.40199 15.51288  9.096277
```

```r
cor(rdata)
```

```
##           Barley    Canola     Durum      Peas     Wheat
## Barley 1.0000000 0.4479165 0.7432833 0.6713023 0.9279486
## Canola 0.4479165 1.0000000 0.5909198 0.3956815 0.4257635
## Durum  0.7432833 0.5909198 1.0000000 0.9299371 0.7136563
## Peas   0.6713023 0.3956815 0.9299371 1.0000000 0.6105689
## Wheat  0.9279486 0.4257635 0.7136563 0.6105689 1.0000000
```

```r
# We are not that far from what we want to simulate!
 
head(rdata)
```

```
##     Barley   Canola    Durum     Peas    Wheat
## 1 67.23468 44.88336 51.74598 38.57649 49.69005
## 2 58.10065 45.65925 55.91824 42.91867 45.44636
## 3 63.71500 45.51898 52.62714 44.11384 46.42476
## 4 52.96774 30.43134 42.26237 37.70667 40.65980
## 5 56.18855 33.13457 44.88393 35.75823 41.97801
## 6 70.41794 38.92241 61.06970 58.05929 52.60466
```

```r
#
write.table(rdata, "yldSimulation.csv", sep =",")
```





Old approach stuff


```r
# simnum=1000
# crops=5        
# simYld <- array(0, dim=c(simnum, crops))
# 
# 
# # yield random 
# for (i in 1:simnum) {
#         simYld[i,] <- rnorm(crops, meanyld, sdyld)
# }
# simYld<-as.data.frame(simYld)
# names(simYld)<-names(yield)[-1]
# write.table(simYld,"simYld.csv", col.names = TRUE,row.names = FALSE, sep=",", )
```




## read data

```r
#setwd("E:/Dropbox/book/economics/485/projects/sheri")
#getwd()
price<-read.csv("price08.csv",head=T, sep=",", stringsAsFactors=FALSE)
summary(price)
```

```
##      date               Wheat           Durum            Barley     
##  Length:75          Min.   :4.830   Min.   : 4.127   Min.   :2.768  
##  Class :character   1st Qu.:6.170   1st Qu.: 5.966   1st Qu.:3.272  
##  Mode  :character   Median :6.549   Median : 7.625   Median :4.027  
##                     Mean   :6.671   Mean   : 7.306   Mean   :4.041  
##                     3rd Qu.:7.468   3rd Qu.: 8.013   3rd Qu.:4.511  
##                     Max.   :8.867   Max.   :13.139   Max.   :6.616  
##      Canola            Peas      
##  Min.   : 8.757   Min.   :4.654  
##  1st Qu.:10.078   1st Qu.:6.221  
##  Median :11.462   Median :7.800  
##  Mean   :11.359   Mean   :7.420  
##  3rd Qu.:12.644   3rd Qu.:8.654  
##  Max.   :14.530   Max.   :9.525
```

```r
str(price)
```

```
## 'data.frame':	75 obs. of  6 variables:
##  $ date  : chr  "Jan-08" "Feb-08" "Mar-08" "Apr-08" ...
##  $ Wheat : num  8.87 8.77 8.51 8.51 8.44 ...
##  $ Durum : num  12.7 13.1 12.5 11.4 12.5 ...
##  $ Barley: num  4.11 4.19 4.25 4.41 4.61 ...
##  $ Canola: num  9.79 10.68 11.15 11.64 11.96 ...
##  $ Peas  : num  7.89 8.44 9.25 9.39 9.53 ...
```

```r
library(zoo)
# set the date format, "zoo" support the yearnon format, but as.Date not.
#price[, 1] <- as.yearmon(price[, 1], format = "%b-%y")
# add day  to the date to use the as.Date fn
price[, 1] <- as.Date(x=paste("1-",price[, 1], sep=""), format="%d-%b-%y")

head(price)
```

```
##         date Wheat  Durum Barley Canola  Peas
## 1 2008-01-01 8.867 12.662  4.112  9.794 7.892
## 2 2008-02-01 8.773 13.139  4.194 10.683 8.437
## 3 2008-03-01 8.513 12.547  4.247 11.153 9.253
## 4 2008-04-01 8.509 11.409  4.411 11.640 9.389
## 5 2008-05-01 8.436 12.542  4.608 11.957 9.525
## 6 2008-06-01 8.503 12.483  4.638 12.789 9.253
```


##  Fit ou model for price


## OU
a Brownian motion is usually formulated as 
[Kun Ren](http://renkun.me/blog/r/2014/04/05/fit-an-ou-process-with-realized-time-series-data.html)


$$dx_t = \mu\,dt+\sigma\,dW_t$$


which is the continuous case of a random walk. In some cases, it is quite convenient to use this formulation to describe the characteristic of asset prices due to its highly unpredictable behavior.

exhibit such stationary mean-reverting behavior
Ornstein-Uhlenbeck process. It is often used to characterize stationary mean-reverting data-generating process like 
$$dx_t = \theta (\mu-x_t)\,dt + \sigma\, dW_t$$

where xt denotes the the value of the portfolio, often called the spread, ?? and ?? are two parameters to capture the magnitude of the mean-reverting force, and ?? is a parameter to capture the diverting volatility.


In R, a package named {sde} provides functions to deal with a wide range of stochasic differential equations including the discrete version of Ornstein-Uhlenbeck process.

 {sde} package offers a much more general one. sde.sim is a function to simulate any stochastic differential equation in the form 

$$dx_t = \mu(x_t,t)\,dt + \sigma(x_t,t)\, dW_t$$


we simulate a spread process generated by an OU-process formulated by

$$dx_t = (0-0.5x)\,dt + 0.8\, dW_t$$




```r
# function for Calibration using Maximum Likelihood estimates
ouFit.ML = function(spread) {
    n = length(spread)
    delta = 1  # delta
    Sx = sum(spread[1:n - 1])
    Sy = sum(spread[2:n])
    Sxx = sum((spread[1:n - 1])^2)
    Syy = sum((spread[2:n])^2)
    Sxy = sum((spread[1:n - 1]) * (spread[2:n]))
    mu = (Sy * Sxx - Sx * Sxy)/((n - 1) * (Sxx - Sxy) - (Sx^2 - Sx * Sy))
    theta = -log((Sxy - mu * Sx - mu * Sy + (n - 1) * mu^2)/(Sxx - 2 * mu * Sx + (n - 1) * mu^2))/delta
    a = exp(-theta * delta)
    sigmah2 = (Syy - 2 * a * Sxy + a^2 * Sxx - 2 * mu * (1 - a) * (Sy - a * Sx) + (n - 1) * mu^2 * (1 - a)^2)/(n - 1)
    sigma = sqrt((sigmah2) * 2 * theta/(1 - a^2))
    theta = list(theta = theta, mu = mu, sigma = sigma, sigmah2 = sigmah2)
    return(theta)
}
```


### Generate all parameter for all crop price


```r
paraPrLi <- apply( price[,-1] ,2,function(x) ouFit.ML(x))
#str(paraMat)
#str(paraMat[1])
# unlist a list with lists as element and transfor to matrix
paraPrMat <- matrix(unlist(paraPrLi), ncol = 5, byrow = F )
#output

dfparaPrMat<-as.data.frame(paraPrMat)
names(dfparaPrMat)<-names(price)[-1]
row.names(dfparaPrMat)<-c("theta", "mu", "sigma", "sigmah2")
dfparaPrMat
```

```
##              Wheat      Durum     Barley      Canola       Peas
## theta   0.06570700 0.08732267 0.04176114  0.04122139 0.03129221
## mu      6.06791672 6.15687548 3.86530538 11.41694522 6.89601886
## sigma   0.28448286 0.68413805 0.25984705  0.44108602 0.35125333
## sigmah2 0.07583828 0.42945289 0.06477765  0.18675290 0.11959740
```

```r
write.table(dfparaPrMat,"paraPrMat.csv", col.names = TRUE,row.names = T, sep=",", )
```

## # actual price summary


```r
# mean
apply(price[,-1], 2, mean)
```

```
##     Wheat     Durum    Barley    Canola      Peas 
##  6.671053  7.306347  4.041493 11.359187  7.420200
```

```r
# sd 
apply(price[,-1], 2, sd)
```

```
##     Wheat     Durum    Barley    Canola      Peas 
## 1.0517899 2.2062752 0.8790107 1.5646912 1.3797000
```


### Ou simulatio function


```r
# simulation
# Brownian motion
# Example from Dixit and Pindyck, 1994, pp.74-76
# Simple mean-reverting process:% dx = nu (xbar - x) dt + sigma dz
#----------------------------------------------------------------------
# generate a function for simulation
OU.sim <- function(para = c(0.01, 200, 1), x0= 1, periods=100){
#  periods=100; #Number of periods
theta = para[1]; # speed of reversion
mu = para[2];
sigma = para[3]; # sigma in monthly terms
# theta = 1
sigma2 = sigma * sqrt((1-exp(-2*theta))/(2*theta));   # dt=1;
x= as.vector(rep(0, periods))  # all zero, 100 vector
epsilon=rnorm(periods)
x[1]= mu; # Starting value of first row of x
for (i in 2:periods){
  x[i] = x[i-1] + mu*(1-exp(-theta)) + x[i-1]*(exp(-theta)-1) + sigma2*epsilon[i-1];
}
return(x) 
}
```



### Generate simulation price matrix 1000*5


```r
priceSimulation <- matrix(nrow=1000,ncol=5)
for(i in 1:5){
     priceSimulation[,i]<-OU.sim(dfparaPrMat[1:3,i],x0 = dfparaPrMat[2,i],periods=1000)   
} 
priceSimulation<-data.frame(priceSimulation)
names(priceSimulation)<-names(price)[-1]
head(priceSimulation)
```

```
##      Wheat    Durum   Barley   Canola     Peas
## 1 6.067917 6.156875 3.865305 11.41695 6.896019
## 2 6.308254 5.919090 3.956971 10.95984 6.959166
## 3 6.244645 6.428478 3.815173 10.71225 6.999995
## 4 6.078139 5.728197 3.465643 10.75652 7.418278
## 5 6.417938 5.886690 3.324209 10.37819 8.244963
## 6 6.359347 5.472061 3.323214 10.12206 7.966186
```

```r
write.table(priceSimulation,"priceSimulation.csv", col.names = TRUE,row.names = FALSE, sep=",", )
```





## Simulate the revenue

Match the yield and price matrix by the crop names

yield: Barley   Canola    Durum     Peas    Wheat

price: Wheat  Durum Barley Canola  Peas


```r
yieldSimulation <- rdata[, c("Wheat",  "Durum", "Barley", "Canola",  "Peas" )]
head(yieldSimulation)
```

```
##      Wheat    Durum   Barley   Canola     Peas
## 1 49.69005 51.74598 67.23468 44.88336 38.57649
## 2 45.44636 55.91824 58.10065 45.65925 42.91867
## 3 46.42476 52.62714 63.71500 45.51898 44.11384
## 4 40.65980 42.26237 52.96774 30.43134 37.70667
## 5 41.97801 44.88393 56.18855 33.13457 35.75823
## 6 52.60466 61.06970 70.41794 38.92241 58.05929
```

```r
revSimulation<-yieldSimulation*priceSimulation
head(revSimulation)
```

```
##      Wheat    Durum   Barley   Canola     Peas
## 1 301.5151 318.5936 259.8826 512.4309 266.0242
## 2 286.6872 330.9851 229.9026 500.4182 298.6782
## 3 289.9061 338.3124 243.0837 487.6109 308.7967
## 4 247.1359 242.0872 183.5673 327.3354 279.7186
## 5 269.4123 264.2178 186.7825 343.8767 294.8253
## 6 334.5313 334.1771 234.0139 393.9750 462.5111
```




