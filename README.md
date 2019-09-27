# utl-forecast-the-next-four-months-using-a-moving-average-time-series
Forecast the next four months using a moving average time series

    SAS Forum: Forecast the next four months using a moving average time series                                                         
                                                                                                                                        
    Little out of my comfort zone.                                                                                                      
                                                                                                                                        
    I am using a MA model. I think that what the op wanted.                                                                             
                                                                                                                                        
    I was not sure if the op wants to futher do a second moving average                                                                 
    using the output from the forcasting model. This would require                                                                      
    iterative calls to R estimating one point at a time                                                                                 
                                                                                                                                        
    I am also using a centered moving average.                                                                                          
                                                                                                                                        
     graph                                                                                                                              
    https://tinyurl.com/y6yppq3f                                                                                                        
    https://github.com/rogerjdeangelis/utl-forecast-the-next-four-months-using-a-moving-average-time-series/blob/master/forecast.png    
                                                                                                                                        
    github                                                                                                                              
    https://tinyurl.com/y3g7gpfj                                                                                                        
    https://github.com/rogerjdeangelis/utl-forecast-the-next-four-months-using-a-moving-average-time-series                             
                                                                                                                                        
    SAS Fotum                                                                                                                           
    https://tinyurl.com/yxzzmszt                                                                                                        
    https://communities.sas.com/t5/SAS-Programming/How-to-create-new-rows-with-moving-average-calculation/m-p/591711                    
                                                                                                                                        
    Source for model                                                                                                                    
    https://www.r-bloggers.com/basic-forecasting/                                                                                       
                                                                                                                                        
    options validvarname=upcase;                                                                                                        
    libname sd1 "d:/sd1";                                                                                                               
    data sd1.have;                                                                                                                      
     retain month;                                                                                                                      
     informat date $11.;                                                                                                                
     input date  prop;                                                                                                                  
     month=input(scan(date,2,"/"),2.);                                                                                                  
    cards4;                                                                                                                             
    1/4/2019 0.1                                                                                                                        
    1/5/2019 0.2                                                                                                                        
    1/6/2019 0.3                                                                                                                        
    1/7/2019 0.25                                                                                                                       
    1/8/2019 0.5                                                                                                                        
    1/9/2019 .                                                                                                                          
    1/10/2019 .                                                                                                                         
    1/11/2019 .                                                                                                                         
    1/12/2019 .                                                                                                                         
    ;;;;                                                                                                                                
    run;quit;                                                                                                                           
                                                                                                                                        
                                                                                                                                        
    SD1.HAVE total obs=9                                                                                                                
                                                                                                                                        
     MONTH      DATE          PROP                                                                                                      
                                                                                                                                        
        4     1/4/2019        0.10                                                                                                      
        5     1/5/2019        0.20                                                                                                      
        6     1/6/2019        0.30                                                                                                      
        7     1/7/2019        0.25                                                                                                      
        8     1/8/2019        0.50                                                                                                      
        9     1/9/2019         .     Forecast                                                                                           
       10     1/10/2019        .                                                                                                        
       11     1/11/2019        .                                                                                                        
       12     1/12/2019        .                                                                                                        
                                                                                                                                        
    options ls=64 ps=32;                                                                                                                
    proc plot data=sd1.have(rename=prop=p12345678901234567890);;                                                                        
      plot p12345678901234567890*month="*" $ p12345678901234567890;                                                                     
    run;quit;                                                                                                                           
                                                                                                                                        
            4    5    6    7    8    9   10   11    12                                                                                  
          +-+----+----+----+----+----+----+----+----+                                                                                   
          |                                         |                                                                                   
     0.50 +                                         + 0.50                                                                              
          |                       * 0.5             |                                                                                   
     0.45 +                       /                 + 0.45                                                                              
          |                      /                  |                                                                                   
     0.40 +                     /                   + 0.40                                                                              
          |                    /                    |                                                                                   
     0.35 +                   /                     + 0.35                                                                              
          |                  /                      |                                                                                   
     0.30 +           * 0.3 /                       + 0.30                                                                              
          |          / \   /                        |                                                                                   
     0.25 +         /   \ /                         + 0.25                                                                              
          |        /     * 0.25                     |                                                                                   
     0.20 +       * 0.2                             + 0.20                                                                              
          |     /                                   |                                                                                   
     0.15 +    /                                    + 0.15                                                                              
          |   /                                     |                                                                                   
     0.10 +* 0.1                                    + 0.10                                                                              
          |                                         |                                                                                   
          -+----+----+----+----+----+----+----+-----+                                                                                   
           4    5    6    7    8    9   10   11     12                                                                                  
                                                                                                                                        
                             MONTH                                                                                                      
    *            _               _                                                                                                      
      ___  _   _| |_ _ __  _   _| |_                                                                                                    
     / _ \| | | | __| '_ \| | | | __|                                                                                                   
    | (_) | |_| | |_| |_) | |_| | |_                                                                                                    
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                                   
                    |_|                                                                                                                 
    ;                                                                                                                                   
                                                                                                                                        
    Up to 40 obs from WANT total obs=9                                                                                                  
                                                                                                                                        
      MONTH      PROP                                                                                                                   
                                                                                                                                        
         4      .                                                                                                                       
         5     0.20000   ** centered moving average                                                                                     
         6     0.25000                                                                                                                  
         7     0.35000                                                                                                                  
         8      .                                                                                                                       
         9     0.38232   ** predicted values                                                                                            
        10     0.43232                                                                                                                  
        11     0.48232                                                                                                                  
        12     0.53232                                                                                                                  
                                                                                                                                        
    options ls=72 ps=44;                                                                                                                
    proc plot data=want(rename=prop=p12345678901234567890);                                                                             
      format p12345678901234567890 5.2;                                                                                                 
      plot p12345678901234567890*month="*" $ p12345678901234567890/                                                                     
      href=8.5 box;                                                                                                                     
    run;quit;                                                                                                                           
                                                                                                                                        
                                                                                                                                        
                                                                                                                                        
            4     5     6     7     8     9    10    11    12                                                                           
     0.55 +-+-----+-----+-----+-----+-----+-----+-----+------+ 0.55                                                                     
          |                           |                      |                                                                          
          |                           |               0.53 * |                                                                          
          |                           |                 /    |                                                                          
          |                           |                /     |                                                                          
     0.50 +                           |               /      + 0.50                                                                     
          |                           |              /       |                                                                          
          | CENTERED MOVING AVERAGE   | PREDICTED   *  0.48  |                                                                          
          |                           |            /         |                                                                          
          |                           |           /          |                                                                          
     0.45 +                           |          /           + 0.45                                                                     
          |                           |         /            |                                                                          
        0 |                           |        *  0.43       |    0                                                                     
          |                           |       /              |                                                                          
          |                           |      /               |                                                                          
     0.40 +                           |     /                + 0.40                                                                     
          |                           |    *  0.38           |                                                                          
          |                           |                      |                                                                          
          |                           |                      |                                                                          
          |                           |                      |                                                                          
     0.35 +                   * 0.35  |                      + 0.35                                                                     
          |   Moving average /        |                      |                                                                          
          |   smooths                 |                      |                                                                          
          |   out elbow     /         |                      |                                                                          
          |   above                   |                      |                                                                          
     0.30 +               /           |                      + 0.30                                                                     
          |                           |                      |                                                                          
          |              /            |                      |                                                                          
          |                           |                      |                                                                          
          |            /              |                      |                                                                          
     0.25 +           *  0.25         |                      + 0.25                                                                     
          |         /                 |                      |                                                                          
          |        /                  |                      |                                                                          
          |       /                   |                      |                                                                          
          |      /                    |                      |                                                                          
     0.20 +      *  0.20              |                      + 0.20                                                                     
          -+-----+-----+-----+-----+-----+-----+-----+-------+                                                                          
           4     5     6     7     8     9    10    11    12                                                                            
                                                                                                                                        
                                 MONTH                                                                                                  
                                                                                                                                        
    *                                                                                                                                   
     _ __  _ __ ___   ___ ___  ___ ___                                                                                                  
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|                                                                                                 
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                                                 
    | .__/|_|  \___/ \___\___||___/___/                                                                                                 
    |_|                                                                                                                                 
    ;                                                                                                                                   
                                                                                                                                        
    %utlfkil(d:/xpt/want.xpt);                                                                                                          
                                                                                                                                        
    %utl_submit_r64('                                                                                                                   
    library(forecast);                                                                                                                  
    library(SASxport);                                                                                                                  
    library(haven);                                                                                                                     
    data<-unlist(read_sas("d:/sd1/have.sas7bdat")[1:5,3]);                                                                              
    mav<-ma(data[1:5], order=3);                                                                                                        
    moving_average = forecast(mav, h=4);                                                                                                
    png(file="d:/png/forecast.png");                                                                                                    
    plot(moving_average, ylim=c(.2,.6));                                                                                                
    want<-summary(moving_average);                                                                                                      
    want<-as.data.frame(want[,1]);                                                                                                      
    want<-as.data.frame(append(unlist(mav),unlist(want[,1])));                                                                          
    colnames(want)="PROP";                                                                                                              
    write.xport(want,file="d:/xpt/want.xpt");                                                                                           
    ');                                                                                                                                 
                                                                                                                                        
    libname xpt xport "d:/xpt/want.xpt";                                                                                                
                                                                                                                                        
    data want;                                                                                                                          
      retain month 3;                                                                                                                   
      set  xpt.want;                                                                                                                    
      month=month+1;                                                                                                                    
    run;quit;                                                                                                                           
                                                                                                                                        
    libname xpt clear;                                                                                                                  
                                                                                                                                        
    *_                                                                                                                                  
    | | ___   __ _                                                                                                                      
    | |/ _ \ / _` |                                                                                                                     
    | | (_) | (_| |                                                                                                                     
    |_|\___/ \__, |                                                                                                                     
             |___/                                                                                                                      
    ;                                                                                                                                   
                                                                                                                                        
    Forecast method: ETS(A,A,N)                                                                                                         
                                                                                                                                        
    Model Information:                                                                                                                  
    ETS(A,A,N)                                                                                                                          
                                                                                                                                        
    Call:                                                                                                                               
     ets(y = object, lambda = lambda, biasadj = biasadj,                                                                                
         allow.multiplicative.trend = allow.multiplicative.trend)                                                                       
                                                                                                                                        
      Smoothing parameters:                                                                                                             
        alpha = 0.2929                                                                                                                  
        beta  = 0                                                                                                                       
                                                                                                                                        
      Initial states:                                                                                                                   
        l = 0.2                                                                                                                         
        b = 0.05                                                                                                                        
                                                                                                                                        
      sigma:  0.0382                                                                                                                    
    Error measures:                                                                                                                     
                          ME       RMSE        MAE       MPE     MAPE      MASE                                                         
    Training set -0.02011843 0.03818813 0.03678512 -10.66642 15.42833 0.4904682                                                         
                        ACF1                                                                                                            
    Training set -0.07345189                                                                                                            
                                                                                                                                        
    Forecasts:                                                                                                                          
      Point Forecast     Lo 80     Hi 80     Lo 95     Hi 95                                                                            
    5      0.3823223 0.3333823 0.4312624 0.3074750 0.4571697                                                                            
    6      0.4323223 0.3813262 0.4833184 0.3543306 0.5103141                                                                            
    7      0.4823223 0.4293500 0.5352947 0.4013081 0.5633365                                                                            
    8      0.5323223 0.4774448 0.5871998 0.4483944 0.6162502                                                                            
    >                                                                                                                                   
                                                                                                                                        
                                                                                                                                        
    options ls=72 ps=44;                                                                                                                
    proc plot data=want(rename=prop=p12345678901234567890);                                                                             
      format p12345678901234567890 5.2;                                                                                                 
      plot p12345678901234567890*month="*" $ p12345678901234567890/                                                                     
      href=8.5;                                                                                                                         
    run;quit;                                                                                                                           
                                                                                                                                        
                                                                                                                                        
                                                                                                                                        
