NFL Rankings/Playoff Predictions Project 
========================================================
author: Shivam Bhatt and Jordan Greenhall
date: 12/05/2018
width: 1200
height: 500

Overview
========================================================

- Primary goal was to use historical data to predict 2017 rankings
  - Top 6 in each conference made the playoffs
- Utilized historical team rankings based on 2014, 2015, and 2016
    - Separated through NFC and AFC Conferences


Key Variables Definitions/Glossary
========================================================

- PA: Total Points Against/Allowed
- PF: Total Points For/Scored
- Int: Total Interceptions
- X4DATT: 4th Down Attempts per game
- ANY.A: Adjusted Net Yards per Pass Attempt
- QB Rating: Average QB Rating (determined by NFL)
- SoS: Strength of Schedule 
- RZAtt: Red Zone Attempts (close to scoring)
- Rush Att: Total Running Attempts by team



Dependent Variable    
========================================================    
    
- Rankings were sourced from playoff seedings 
    - Reg season ranking was used for teams that didnot make the playoffs

- As a result, our dependent variable was created: 'OVR.RANK'

 

Correlation Studies
========================================================




```r
piecor2 <- corrplot(cormat2, method = "pie")
```

![plot of chunk unnamed-chunk-2](NFL_Project_Slides-figure/unnamed-chunk-2-1.png)





Correlation Studies (cont.)
========================================================

![plot of chunk unnamed-chunk-3](NFL_Project_Slides-figure/unnamed-chunk-3-1.png)


Relationships Between Predictors and OVR.RANK
========================================================

![plot of chunk unnamed-chunk-4](NFL_Project_Slides-figure/unnamed-chunk-4-1.png)![plot of chunk unnamed-chunk-4](NFL_Project_Slides-figure/unnamed-chunk-4-2.png)


Relationships Between Predictors and OVR.RANK
========================================================

![plot of chunk unnamed-chunk-5](NFL_Project_Slides-figure/unnamed-chunk-5-1.png)![plot of chunk unnamed-chunk-5](NFL_Project_Slides-figure/unnamed-chunk-5-2.png)

GGPairs/Correlation
========================================================

![plot of chunk unnamed-chunk-6](NFL_Project_Slides-figure/unnamed-chunk-6-1.png)

Linear Model
========================================================




```r
#model includes Points Allowed, tot Interceptions, tot Fumbles Lost, Stength of schedule, 4th down attempts, Points Scored, Av. Net Pass Yards per throw, QB Rating, Red Zone Attempts, and Rush Attempts
summary(lmod)
```

```

Call:
lm(formula = OVR.RANK ~ PA + Int + tot.FL + SoS + X4DAtt + PF + 
    ANY.A + QB.Rating + RZAtt + Rush.Att, data = nflnum)

Residuals:
    Min      1Q  Median      3Q     Max 
-6.3562 -1.7256 -0.1774  1.8551  4.8022 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
(Intercept)  4.507944   6.389917   0.705 0.482442    
PA           0.044648   0.005776   7.729 1.98e-11 ***
Int          0.025183   0.085432   0.295 0.768887    
tot.FL       0.117958   0.090679   1.301 0.196835    
SoS          0.095095   0.147146   0.646 0.519851    
X4DAtt       0.029648   0.067385   0.440 0.661062    
PF          -0.036075   0.009182  -3.929 0.000173 ***
ANY.A       -0.148823   0.914265  -0.163 0.871079    
QB.Rating   -0.027200   0.091537  -0.297 0.767083    
RZAtt        0.063288   0.050228   1.260 0.211113    
Rush.Att    -0.002016   0.006659  -0.303 0.762863    
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 2.403 on 85 degrees of freedom
Multiple R-squared:  0.7595,	Adjusted R-squared:  0.7312 
F-statistic: 26.84 on 10 and 85 DF,  p-value: < 2.2e-16
```




Linear Model MSE
========================================================




```r
#MSE
sqrt(mean((testDF$OVR.RANK - testDF$lmpreds)^2))
```

```
[1] 2.137696
```


Actual vs Predicted for Linear Model and Lm Resids
========================================================

```r
testDF[,c("Tm", "OVR.RANK", "lmpreds","lmresids")]
```

```
                     Tm OVR.RANK   lmpreds    lmresids
1     Arizona Cardinals       10 11.169853 -1.16985289
2       Atlanta Falcons        6  6.409490 -0.40949042
3      Baltimore Ravens        7  4.267437  2.73256255
4         Buffalo Bills        6  9.794775 -3.79477496
5     Carolina Panthers        5  7.008049 -2.00804886
6         Chicago Bears       15  9.853343  5.14665730
7    Cincinnati Bengals        9 10.249156 -1.24915588
8      Cleveland Browns       16 16.780958 -0.78095804
9        Dallas Cowboys        9  7.659741  1.34025874
10       Denver Broncos       12 13.090993 -1.09099346
11        Detroit Lions        7  6.879561  0.12043932
12    Green Bay Packers       11 11.370582 -0.37058151
13       Houston Texans       15 12.488925  2.51107509
14   Indianapolis Colts       14 13.068457  0.93154315
15 Jacksonville Jaguars        3  1.940301  1.05969889
16   Kansas City Chiefs        4  3.905779  0.09422107
17 Los Angeles Chargers        8  3.205069  4.79493058
18     Los Angeles Rams        3  3.779665 -0.77966488
19       Miami Dolphins       11 12.992521 -1.99252119
20    Minnesota Vikings        2  2.050812 -0.05081211
21 New England Patriots        1  1.751485 -0.75148516
22   New Orleans Saints        4  3.483963  0.51603651
23      New York Giants       16 13.743920  2.25608044
24        New York Jets       13 11.027072  1.97292834
25      Oakland Raiders       10 11.207414 -1.20741396
26  Philadelphia Eagles        1  2.282115 -1.28211521
27  Pittsburgh Steelers        2  4.218301 -2.21830063
28  San Francisco 49ers       13 11.003883  1.99611669
29     Seattle Seahawks        8  5.785792  2.21420837
30 Tampa Bay Buccaneers       14 10.884879  3.11512148
31     Tennessee Titans        5  8.604656 -3.60465611
32  Washington Redskins       12 10.695510  1.30448990
```


Lasso Model
========================================================





![plot of chunk unnamed-chunk-13](NFL_Project_Slides-figure/unnamed-chunk-13-1.png)


```r
#Lasso <- cv.glmnet(x = Xvar, y = Yvar, family = "multinomial", alpha = 1)
#Variables Selected: tot Fumbles Lost, Sack Pctg, SRS, All purpose yds, tot plays
```



Lasso MSE
========================================================


```r
#MSE
sqrt(mean((testDF$OVR.RANK - testDF$Lassopreds)^2))
```

```
[1] 2.391868
```


Actual vs Predicted for Lasso and Lasso Residuals
========================================================




```
                     Tm OVR.RANK Lassopreds Lassoresids
1     Arizona Cardinals       10  10.801851  -0.8018510
2       Atlanta Falcons        6   4.974133   1.0258667
3      Baltimore Ravens        7   5.208623   1.7913768
4         Buffalo Bills        6  11.043498  -5.0434977
5     Carolina Panthers        5   5.492590  -0.4925903
6         Chicago Bears       15   9.503536   5.4964638
7    Cincinnati Bengals        9  11.667183  -2.6671825
8      Cleveland Browns       16  15.887873   0.1121270
9        Dallas Cowboys        9   7.339884   1.6601160
10       Denver Broncos       12  13.458356  -1.4583562
11        Detroit Lions        7   7.432291  -0.4322908
12    Green Bay Packers       11   9.761486   1.2385142
13       Houston Texans       15  13.286427   1.7135731
14   Indianapolis Colts       14  14.954214  -0.9542137
15 Jacksonville Jaguars        3   4.233084  -1.2330838
16   Kansas City Chiefs        4   5.822988  -1.8229877
17 Los Angeles Chargers        8   4.749259   3.2507414
18     Los Angeles Rams        3   3.401234  -0.4012341
19       Miami Dolphins       11  11.642895  -0.6428954
20    Minnesota Vikings        2   2.289364  -0.2893636
21 New England Patriots        1   2.483334  -1.4833342
22   New Orleans Saints        4   2.463745   1.5362552
23      New York Giants       16  12.418299   3.5817010
24        New York Jets       13  12.165929   0.8340709
25      Oakland Raiders       10  11.088812  -1.0888119
26  Philadelphia Eagles        1   3.034987  -2.0349870
27  Pittsburgh Steelers        2   4.366767  -2.3667671
28  San Francisco 49ers       13  10.189331   2.8106687
29     Seattle Seahawks        8   7.059937   0.9400628
30 Tampa Bay Buccaneers       14   9.714469   4.2855315
31     Tennessee Titans        5  10.325844  -5.3258441
32  Washington Redskins       12   9.973885   2.0261146
```



Sample Tree from CForest
========================================================

![plot of chunk unnamed-chunk-18](NFL_Project_Slides-figure/unnamed-chunk-18-1.png)



Another C Tree
========================================================

![plot of chunk unnamed-chunk-19](NFL_Project_Slides-figure/unnamed-chunk-19-1.png)


Questions?
========================================================

