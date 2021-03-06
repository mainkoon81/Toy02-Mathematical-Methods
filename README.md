# Mathematical-Methods

## 1. Differential Equation
> How do you **design**
 - a boat that doesn't tip over as it bobs in the water,
 - the suspension system of a car for a smooth ride,
 - circuits that tune to the correct frequencies in a cell phone?

> How do you **model**
 - the growth of antibiotic resistant bacteria,
 - gene expression,
 - online purchasing trends?

__[The answer]:__ Differential Equations. They are the language of the models we use to describe the world around us. We will develop the mathematical tools needed to solve linear differential equations. In the case of nonlinear differential equations, we will employ graphical methods and approximation to understand solutions.


-------------------------------------------------------------------------------------------------------------------
## 2. Time Series
Time series is a dataset **collected through time**. Now, since we are doing sampling with adjacent points in time, we naturally introduce a correlation into the system, which means the classical, statistical inference might not work in this setting.  
 - Available time series from 'astsa' package in R
   - jj (Johnson and Johnson Quarterly Earnings for 84 quarters)
   - flu (Pneumonia and influenza deaths in the U.S. per 10,000 people Monthly for 11 years)
   - globtemp (Land-ocean mean temperature deviations for the years 1880-2015)
   - globtempl (Land only mean temperature deviations for the years 1880-2015)
   - star (Variable Star: The magnitude of a star taken at midnight for 600 consecutive days )  
### A. How to Describe Time Series   
__a> Stationarity__
<img src="https://user-images.githubusercontent.com/31917400/78254900-7dcf5680-74ee-11ea-8e9b-076a354d77f1.jpg" /> 

 - In a stationary time series, there is no systematic change in **mean** (no trend), no systematic change in **variance**, and no **fluctuation**(periodic variations). Of course each random variable (![formula](https://render.githubusercontent.com/render/math?math=\X_1,\X_2,..)) has its own unique distribution, but the **`joint of such random variables`** theoretically does not change over time(or "index"). So..basically, picking up one of the curves, **`the properties of one section of a data are much like the properties of the other sections of the data`**, and **`the association b/w such consecutive data points should be very low`** (coz each point follows its own distribution independent each other). Therefore, the **`joint distribution of two random variables depends only on the lag spacing`** and not which curve you have on the random process. No matter where you look, to the left or the right on the distribution, the **`autocovariance only depends upon separation`**.  
 
   ![formula](https://render.githubusercontent.com/render/math?math=\mu(\t_n)=Constant)
   
   ![formula](https://render.githubusercontent.com/render/math?math=\gamma(\t_1,\t_2)=\gamma(\t_2-\t_1)=\gamma(k))
   
 - Since you only have one realization in front of you, how are you going to infer the properties of the process? Each one of the random variables along your time series is only giving you an individual point. "We assume we have a strictly stationary random process" (having a certain joint distribution of a set of random variables). Then property will be the same no matter where you look along the time series. It's fine to park yourself anywhere you'd like along the time series and look at the set of random variables. Preserve the spacing between them, but now look far to the left or far to the right along the stochastic process. You'll get the same joint distribution if your process is strictly stationary.
 - Usually "stationarity" is a property of a stochastic process of a model, not a time series. But we say `stationary time series` if you think that it can be modeled with **`stochastic process`**...If we have a non-stationary time series, which we usually have, we basically do some `transformations` to get the stationary time series. Once we have a stationary time series, we model it and then go back and model our non-stationary time series. So **we use the transformations as a middle step**.
 
__b> Acv (Auto-Cov-Function & Coefficient)__
 - `COV` measures the strength of linear association(so dependence) b/w two random variables...assuming both shares same unit.
 - `Stochastic process` put a lot of random variables ![formula](https://render.githubusercontent.com/render/math?math=\X_t)(each of which follows different distribution) together and give them a sequence. In deterministic processes (for example, solution of ordinary differential equation), You start with some point and the solution will tell you exact trajectory. The stochastic process is basically opposite of that. At every step `t`, you have some randomness (a single random sampling from ![formula](https://render.githubusercontent.com/render/math?math=\X_t)), i.e. there is a certain distribution that ![formula](https://render.githubusercontent.com/render/math?math=\X_t) follows at that time stamp `t`.
 - `Time Series` can be actually a series of realizations from the stochastic process going on the back one. If we characterize time series as a realization of a **stochastic process**, let's say the realization of ![formula](https://render.githubusercontent.com/render/math?math=\X_1) is my first datapoint in the time series, and the realization of ![formula](https://render.githubusercontent.com/render/math?math=\X_2) is my second datapoint in my time series. Thus, the stochastic process might come with ensemble of realizations(curves, TS, or sampling trajectories), but now I only have one time series(curve). 
 - `Auto-Cov-Function` takes **covariance** of different elements in our TS as our one realization(curve) of the stochastic process. If you take the value from ![formula](https://render.githubusercontent.com/render/math?math=\X_t) and ![formula](https://render.githubusercontent.com/render/math?math=\X_s) and `s` and `t` might be in different locations and we'll get the cavariance of them. <img src="https://user-images.githubusercontent.com/31917400/78037218-281a7300-7363-11ea-8162-b4ce0ee4fb1f.jpg" /> The Auto-Cov-Function above will only depend on the time difference between these random variables. It doesn't matter what `t` is. The **time difference `k`** actually decides the nature...coz we assume we are working with **`stationary times series`**: "the properties of the one part of the time series is same as the properties of the other parts of the time series".
   - cov(![formula](https://render.githubusercontent.com/render/math?math=\X_1) to ![formula](https://render.githubusercontent.com/render/math?math=\X_1plus_k)) = cov(![formula](https://render.githubusercontent.com/render/math?math=\X_10) to ![formula](https://render.githubusercontent.com/render/math?math=\X_10plus_k)) = ![formula](https://render.githubusercontent.com/render/math?math=\gamma_k)
   - The thing is, we usually do not have the stochastic process, but we only have a time series, just a single realization of the stochastic process. So we have to use the TS to approximate cov ~> ![formula](https://render.githubusercontent.com/render/math?math=\C_k)
 - `Auto-Cov-Coeff` at different lags `k` is ![formula](https://render.githubusercontent.com/render/math?math=\gamma_k) = 𝐶𝑜𝑣(![formula](https://render.githubusercontent.com/render/math?math=\X_t), ![formula](https://render.githubusercontent.com/render/math?math=\X_tplus_k)) ~> ![formula](https://render.githubusercontent.com/render/math?math=\C_k) <img src="https://user-images.githubusercontent.com/31917400/78255911-ce937f00-74ef-11ea-9514-3de8515577c3.jpg" />
   
   - : `acf(TS_data, type='covariance')` returns a bunch of Auto-Cov-Coefficient values ![formula](https://render.githubusercontent.com/render/math?math=\C_k). 

__c> ACF (Auto-Corr-Function & Coefficient)__
 - Assuming weak stationarity (to define our TS as a Random Process), the **`auto-correlation coefficient`** between two random variables - ![formula](https://render.githubusercontent.com/render/math?math=\X_t) and ![formula](https://render.githubusercontent.com/render/math?math=\X_tplus_k) - is: <img src="https://user-images.githubusercontent.com/31917400/78048697-05dc2180-7372-11ea-816a-5c71815aa778.jpg" />   
 
   - :  `acf(TS_data)` returns a bunch of Auto-Corr-Coefficient values ![formula](https://render.githubusercontent.com/render/math?math=\gamma_k). 
   - From the chart above, we do not have much correlation between all the different lags(all values are within the significance level). Just because we generated this data as a purely random process(same "rnorm" for every RV, which means we already implies the stationary distribution), we don't have to expect to see the correlation b/w different lags. TS values in each lag are all independent? 

__d> PACF (Patial Auto-Corr-Function & Coefficient)__
 - The ACF is one of the primary tools for characterizing AR(p) or MA(q)...Can you tell what the order of the process should be?
   - MA(`q`) has an **ACF** that cuts off after `q` lags...(so if you find ACF cuts off after 4 lags, you can be reasonably sure you have an MA(4) process). 
   - AR(`p`) has an **PACF** that cuts off after `p` lags...(so if you find PACF cuts off after 4 lags, you can be reasonably sure you have an AR(4) process). FIND the most likely order using R-command `ar(ts.data, order.max=?)` ???   

### B. How to Model Time Series   
__a> method 01. Random Walk__
 - It's not always stationary.
 - If assuming ![formula](https://render.githubusercontent.com/render/math?math=\X_t=) X_(t-1) + ![formula](https://render.githubusercontent.com/render/math?math=\epsilon_t) where ![formula](https://render.githubusercontent.com/render/math?math=\epsilon_t~\N(\mu,\sigma^2)), and if **![formula](https://render.githubusercontent.com/render/math?math=\X_0=0)**, then ![formula](https://render.githubusercontent.com/render/math?math=\X_1=\epsilon_1), thus: <img src="https://user-images.githubusercontent.com/31917400/78176394-51ff9280-7454-11ea-862a-6978765dfc0c.jpg" />
   ```
   X=NULL
   X[1] = 0
   
   for(i in 2:1000) {
      X[i] = X[i-1] + rnorm(1)
   }
   
   print(X)
   random_walk_test <- ts(X)
   plot(random_walk_test, main="Random Walk Example", xlab="Days", ylab=" ", lwd=2)
   ```
 - But, random walk above is not a stationary time series. It would not make sense to actually find acf of it because we define acf for stationary time series. If we plot `acf()`, it would show there is a high correlation in this data and there is no stationarity.
 - Can we remove this stupid trend in our random walk? How to turn back to the ransom process? `diff()` gives us the bunch of differences b/w each consecutive samples. if **![formula](https://render.githubusercontent.com/render/math?math=\X_0=0)**, then they are random noises that follow N(![formula](https://render.githubusercontent.com/render/math?math=\mu,\sigma^2)) that we set. 
   ```
   plot(diff(random_walk_test))
   plot(acf(diff(random_walk_test)))
   ```
   <img src="https://user-images.githubusercontent.com/31917400/78182015-46fd3000-745d-11ea-8ae7-146b02f3c641.jpg" />

__b> method 02. Moving Average Process__
 - It's always stationary. 
 - First, identify MA. What makes the **![formula](https://render.githubusercontent.com/render/math?math=\X_t)**? We can express **![formula](https://render.githubusercontent.com/render/math?math=\X_t)** as a linear combination of the **noises** that affects it.  
   <img src="https://user-images.githubusercontent.com/31917400/78192845-e8da4800-7470-11ea-8c6e-641972835c56.jpg" />
   
   - `q` refers to how far back to look along the noise sequence. 
   - MA(2)..? then.. starting from index `3`, taking average of the three components. 
   - If you take ACF of MA(2) process, the high correlation will cut off at `Lag_2`...we can see `2` spikes "above noise". 
   - Of course, the more neighbors the smoother..
   ```
   #Generate Noise
   noise = rnorm(10000)
   
   #Introduce variable
   MA_2 = NULL
   
   # Loop for generating MA(2) Process
   for(i in 3:10000) {
      MA_2[i] = noise[i] + 0.7*noise[i-1] + 0.2*noise[i-2]
   }
   
   # shift data to left by 2 units?
   MA_process <- MA_2[3:10000]
   MA_process = ts(MA_process)
   
   par(mfrow = c(2,1))
   plot(MA_process, main="MV(2)", ylab="standard density support", col="blue")
   acf(MA_process, main="Correlogram of MV(2)")
   ```
   <img src="https://user-images.githubusercontent.com/31917400/78264089-a8271100-74fa-11ea-965c-437cb270d31e.jpg" />
 
__c> method 03. Auto Regressive Process__
 - It's not always stationary.
 - Bring the previous terms! 
 - We can generalize Random Walk to **AR(p)**: an autoregressive process of order p.
 - `p` refers to the number of previous terms you bring:
   ### ![formula](https://render.githubusercontent.com/render/math?math=\X_t=\epsilon_t) + history 
     <img src="https://user-images.githubusercontent.com/31917400/78362075-5ab9ab00-75b1-11ea-80df-be06f489b62a.jpg" /> so...AR(p) is the generalization of Random Walk. 
     
   - The primary difference between an AR and MA model is based on the **correlation** between time series objects at different time points. The covariance between x(t) and x(t-n) is zero for MA models while the correlation of x(t) and x(t-n) gradually declines with n becoming larger in the AR model. This means that the MA model does not uses the past forecasts to predict the future values whereas it uses the errors from the past forecasts. While, the AR model uses the past forecasts to predict future values. 
   - The noise quickly vanishes with time in MA model while the AR model won't necessarily be stationary. In fact, we'll try to come up with some basic conditions that tell us when an auto-regressive process is stationary. 
   - When AR(2)? −1 < 𝜙2 < 1, 𝜙2 < 1+𝜙1, 𝜙2 < 1−𝜙1
   ```
   eps = rnorm(N, 0,1)
   
   X=NULL
   X[1] = eps[1]
   
   phi_coeff=0.4
   
   for(i in 2:1000) {
      X[i] = eps[i] + phi_coeff*X[i-1]
   }
   
   AR_process = ts(X)
   
   par(mfrow = c(2,1))
   plot(AR_process, main="AR(1)", ylab="standard density support", col="blue")
   acf(AR_process, main="Correlogram of AR(1)")
     
   ```
   <img src="https://user-images.githubusercontent.com/31917400/78380930-8a29e100-75cc-11ea-976a-6775173858fb.jpg" />
     
__d> method 04. Mixed Model__ (ARMA)




__e> method 05. Integrated Model__ (ARIMA)


__f> method 06. Integrated Model with Seasonality__ (SARIMA)

> ### How to measure the quality of the Time Series Model?
 : AIC (Akaike Information Criterion) tries to help you assess the relative quality of several competing models, just like an adjusted ![formula](https://render.githubusercontent.com/render/math?math=\R^2) in linear regression, **by giving `credit` for models** which reduce the SSE and at the same time **by building in a `penalty` for models** which bring in too many parameters. <img src="https://user-images.githubusercontent.com/31917400/78587672-3110b600-7835-11ea-9ef2-5e699cea01c6.jpg" /> **`We prefer a model with a lower AIC`**. 




### C. How to Forecast TS value Using Exponential Smoothing Technique


























