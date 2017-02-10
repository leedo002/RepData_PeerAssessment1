---
output:
  word_document: default
  pdf_document: default
  html_document: default
---
Reproducible Research Course Project 1
======================================
  
# Introduction
This assignment makes use of data from a personal activity monitoring device. This device collects data at 5 minute intervals through out the day. The data consists of two months of data from an anonymous individual collected during the months of October and November, 2012 and include the number of steps taken in 5 minute intervals each day.  
  
This document provides the step-by-step analysis with both the code and data.  The data is sourced from [Activity Monitoring Data][1].

[1]: https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip

## Analysis
  
The analysis is comprised of the following steps:  
- Loading and Preprocessing the Data  
- Calculating and Plotting the Mean Total Number of Steps  
- Plotting the Average Daily Activity Pattern  
- Imputing Missing Values  
- Plotting the Differences in Activity between Weekdays and Weekends  
  

### Loading and Preprocessing the Data

The data is loaded and preprocessed using the following R code:  
<div class="chunk" id="loaddata"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">library</span><span class="hl std">(dplyr)</span>
<span class="hl kwd">library</span><span class="hl std">(lattice)</span>
<span class="hl com">###############################################################################################################</span>
<span class="hl com"># Loading and preprocessing the data</span>
<span class="hl com">###############################################################################################################</span>
<span class="hl com">##setwd(&quot;C:/Users/leedo002/Documents/Education/Data Science by JHU/Course 5 - Reproducible Research/Week 2/Course Project&quot;)</span>
<span class="hl com">##activity&lt;-read.csv(&quot;activity.csv&quot;)  </span>
<span class="hl std">activity</span><span class="hl kwb">&lt;-</span><span class="hl kwd">read.csv</span><span class="hl std">(</span><span class="hl kwd">unz</span><span class="hl std">(</span><span class="hl str">&quot;activity.zip&quot;</span><span class="hl std">,</span> <span class="hl str">&quot;activity.csv&quot;</span><span class="hl std">))</span>
</pre></div>
</div></div>
  
### Calculating and Plotting the Mean Total Number of Steps
The first analytical step of this analysis is to answer the question "*What is mean total number of steps taken per day?*"

This step performs the following while ignoring missing values:  
1. Calculate the total number of steps taken per day  
2. Plot a histogram of the total number of steps taken each day  
3. Calculate and report the mean and median of the total number of steps taken per day  

The total number of steps taken per day was calculated using the following R code:  
<div class="chunk" id="CalcTotNumSteps"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">TotNumSteps</span><span class="hl kwb">&lt;-</span> <span class="hl kwd">sum</span><span class="hl std">(activity</span><span class="hl opt">$</span><span class="hl std">steps,</span><span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">)</span>
</pre></div>
</div></div>
  
The result of this calculation is ``<code class="knitr inline">570608</code>``.  
  
The histogram of the total number of steps taken each day was plotted using the following R code:
<div class="chunk" id="TotNumStepsHist"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">AggActNoNA</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">aggregate</span><span class="hl std">(steps</span> <span class="hl opt">~</span> <span class="hl std">date,</span> <span class="hl kwc">data</span><span class="hl std">=activity, mean,</span> <span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">)</span>
<span class="hl kwd">hist</span><span class="hl std">(AggActNoNA</span><span class="hl opt">$</span><span class="hl std">steps</span>
     <span class="hl std">,</span> <span class="hl kwc">col</span><span class="hl std">=</span><span class="hl str">&quot;red&quot;</span>
     <span class="hl std">,</span> <span class="hl kwc">main</span><span class="hl std">=</span><span class="hl str">&quot;Steps per Day Histogram&quot;</span>
     <span class="hl std">,</span> <span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;Number of Steps&quot;</span>
     <span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/TotNumStepsHist-1.png" title="plot of chunk TotNumStepsHist" alt="plot of chunk TotNumStepsHist" class="plot" /></div>
</div></div>
  
The mean and median of the total number of steps take per day were calculated using the following R code:  
<div class="chunk" id="TotNumStepsMeanMedian"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">StepsPerDayMeanNoNA</span><span class="hl kwb">&lt;-</span><span class="hl kwd">mean</span><span class="hl std">(AggActNoNA</span><span class="hl opt">$</span><span class="hl std">steps)</span>
<span class="hl std">StepsPerDayMedianNoNA</span><span class="hl kwb">&lt;-</span><span class="hl kwd">median</span><span class="hl std">(AggActNoNA</span><span class="hl opt">$</span><span class="hl std">steps)</span>
</pre></div>
</div></div>
  
The mean calculated was ``<code class="knitr inline">37.3825996</code>``   
and the median calculated was ``<code class="knitr inline">37.3784722</code>``.  
  
### Plotting the Average Daily Activity Pattern
The first analytical step of this analysis is to answer the question "*What is the average daily activity pattern?*"

This step performs the following:  
 1. Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)  
2.Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?  
  
The time series plot of the average number of steps taken each day by interval was plotted using the following R code:  
<div class="chunk" id="TimeSeriesPlot"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">AggStepsByIntervalNoNA</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">aggregate</span><span class="hl std">(steps</span> <span class="hl opt">~</span> <span class="hl std">interval,</span> <span class="hl kwc">data</span><span class="hl std">=activity, mean,</span> <span class="hl kwc">na.rm</span><span class="hl std">=</span><span class="hl num">TRUE</span><span class="hl std">)</span>
<span class="hl kwd">plot</span><span class="hl std">(  AggStepsByIntervalNoNA</span><span class="hl opt">$</span><span class="hl std">interval</span>
     <span class="hl std">, AggStepsByIntervalNoNA</span><span class="hl opt">$</span><span class="hl std">steps</span>
     <span class="hl std">,</span> <span class="hl kwc">type</span><span class="hl std">=</span><span class="hl str">&quot;l&quot;</span>
     <span class="hl std">,</span> <span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;Interval&quot;</span>
     <span class="hl std">,</span> <span class="hl kwc">ylab</span><span class="hl std">=</span><span class="hl str">&quot;Average Daily Steps&quot;</span>
     <span class="hl std">,</span> <span class="hl kwc">main</span><span class="hl std">=</span><span class="hl str">&quot;Average Daily Steps by Interval&quot;</span>
     <span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/TimeSeriesPlot-1.png" title="plot of chunk TimeSeriesPlot" alt="plot of chunk TimeSeriesPlot" class="plot" /></div>
</div></div>
  
The interval with the maximum of the total number of steps taken each day was calculated using the following R code:  
<div class="chunk" id="IntervalWithMostSteps"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">IntervalWithMostSteps</span><span class="hl kwb">&lt;-</span><span class="hl std">AggStepsByIntervalNoNA[AggStepsByIntervalNoNA</span><span class="hl opt">$</span><span class="hl std">steps</span><span class="hl opt">==</span><span class="hl kwd">max</span><span class="hl std">(AggStepsByIntervalNoNA</span><span class="hl opt">$</span><span class="hl std">steps),]</span><span class="hl opt">$</span><span class="hl std">interval</span>
</pre></div>
</div></div>

The result of the calculation was ``<code class="knitr inline">835</code>``.  

### Imputing Missing Values
The third analytical step of this analysis is to answer the question "*What is the impact on the total daily number of steps when imputing missing values?*"

This step performs the following: 
1.Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)  
2.Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.  
3.Create a new dataset that is equal to the original dataset but with the missing data filled in.  
4.Make a histogram of the total number of steps taken each day and Calculate and report the mean and  median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?   

The total number of missing values in the dataset was calculated using the following R code:  
<div class="chunk" id="TotMissingValues"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">TotMissingValues</span><span class="hl kwb">&lt;-</span><span class="hl kwd">sum</span><span class="hl std">(</span><span class="hl kwd">is.na</span><span class="hl std">(activity</span><span class="hl opt">$</span><span class="hl std">steps))</span>
</pre></div>
</div></div>
  
The result of the calculation was ``<code class="knitr inline">2304</code>``.  

All of the missing values were populated by using the mean of the corresponding interval across all days.  The following R code was used to impute the values and populate each missing value:  
<div class="chunk" id="ImputeMissingValues"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">activityImputed</span><span class="hl kwb">&lt;-</span><span class="hl std">activity</span>
<span class="hl std">naTrue</span><span class="hl kwb">&lt;-</span><span class="hl kwd">as.numeric</span><span class="hl std">(</span><span class="hl kwd">rownames</span><span class="hl std">(activityImputed[</span><span class="hl kwd">is.na</span><span class="hl std">(activityImputed</span><span class="hl opt">$</span><span class="hl std">steps),]))</span>
<span class="hl std">activityImputed[naTrue,]</span><span class="hl opt">$</span><span class="hl std">steps</span> <span class="hl kwb">&lt;-</span> <span class="hl std">AggStepsByIntervalNoNA[AggStepsByIntervalNoNA</span><span class="hl opt">$</span><span class="hl std">interval</span><span class="hl opt">==</span>
                                                           <span class="hl std">activityImputed[naTrue,]</span><span class="hl opt">$</span><span class="hl std">interval,]</span><span class="hl opt">$</span><span class="hl std">steps</span>
</pre></div>
</div></div>
  
A histogram was plotted for the total number of steps taken each day using the following R code:  
<div class="chunk" id="TotNumStepsImputedHistogram"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">AggActImputed</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">aggregate</span><span class="hl std">(steps</span> <span class="hl opt">~</span> <span class="hl std">date,</span> <span class="hl kwc">data</span><span class="hl std">=activityImputed, mean)</span>
<span class="hl kwd">par</span><span class="hl std">(</span><span class="hl kwc">mfrow</span><span class="hl std">=</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">1</span><span class="hl std">,</span><span class="hl num">2</span><span class="hl std">))</span>

<span class="hl com">#### last histogram</span>
<span class="hl kwd">hist</span><span class="hl std">(AggActNoNA</span><span class="hl opt">$</span><span class="hl std">steps</span>
     <span class="hl std">,</span> <span class="hl kwc">col</span><span class="hl std">=</span><span class="hl str">&quot;red&quot;</span>
     <span class="hl std">,</span> <span class="hl kwc">main</span><span class="hl std">=</span><span class="hl str">&quot;Steps per Day&quot;</span>
     <span class="hl std">,</span> <span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;Number of Steps&quot;</span>
     <span class="hl std">,</span> <span class="hl kwc">ylim</span><span class="hl std">=</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">0</span><span class="hl std">,</span><span class="hl num">20</span><span class="hl std">)</span>
<span class="hl std">)</span>

<span class="hl com">### this histogram</span>
<span class="hl kwd">hist</span><span class="hl std">(AggActImputed</span><span class="hl opt">$</span><span class="hl std">steps</span>
     <span class="hl std">,</span> <span class="hl kwc">col</span><span class="hl std">=</span><span class="hl str">&quot;red&quot;</span>
     <span class="hl std">,</span> <span class="hl kwc">main</span><span class="hl std">=</span><span class="hl str">&quot;Imputed Steps per Day&quot;</span>
     <span class="hl std">,</span> <span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;Number of Steps&quot;</span>
<span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/TotNumStepsImputedHistogram-1.png" title="plot of chunk TotNumStepsImputedHistogram" alt="plot of chunk TotNumStepsImputedHistogram" class="plot" /></div>
</div></div>
  
And finally the mean and median of the total number of steps taken per day were calculated using the following R code:  
<div class="chunk" id="TotNumStepsImputedMeanAndMedian"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">StepsPerDayMeanImputed</span><span class="hl kwb">&lt;-</span><span class="hl kwd">mean</span><span class="hl std">(AggActImputed</span><span class="hl opt">$</span><span class="hl std">steps)</span>
<span class="hl std">StepsPerDayMedianImputed</span><span class="hl kwb">&lt;-</span><span class="hl kwd">median</span><span class="hl std">(AggActImputed</span><span class="hl opt">$</span><span class="hl std">steps)</span>

<span class="hl std">MeanDiffPct</span> <span class="hl kwb">&lt;-</span> <span class="hl std">((StepsPerDayMeanImputed</span> <span class="hl opt">-</span> <span class="hl std">StepsPerDayMeanNoNA)</span><span class="hl opt">/</span><span class="hl std">StepsPerDayMeanNoNA)</span><span class="hl opt">*</span><span class="hl num">100</span>
<span class="hl std">MedianDiffPct</span> <span class="hl kwb">&lt;-</span> <span class="hl std">((StepsPerDayMedianImputed</span> <span class="hl opt">-</span> <span class="hl std">StepsPerDayMedianNoNA)</span><span class="hl opt">/</span><span class="hl std">StepsPerDayMedianNoNA)</span><span class="hl opt">*</span><span class="hl num">100</span>
</pre></div>
</div></div>
  
The value of the mean was  ``<code class="knitr inline">37.3825996</code>``  
  and the percent difference compared to the mean with missing values removed was ``<code class="knitr inline">0</code>`` .  
The value of the median was  ``<code class="knitr inline">37.3805359</code>``  
  and the percent difference compared to the mean with missing values removed was ``<code class="knitr inline">0.005521</code>`` .  
   

### Plotting the Differences in Activity between Weekdays and Weekends
The final analytical step of this analysis is to answer the question "*Are there differences in activity patterns between weekdays and weekends?*"

This step performs the following: 
1.Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.  
2.Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.  
  
The new factor variable for the type of day was created using the following R code:  
<div class="chunk" id="AddDayTypeFactor"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl std">actImpDay</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">transform</span><span class="hl std">(  activityImputed</span>
                       <span class="hl std">,</span> <span class="hl kwc">DayNum</span><span class="hl std">=</span><span class="hl kwd">as.numeric</span><span class="hl std">(</span><span class="hl kwd">strftime</span><span class="hl std">(activityImputed</span><span class="hl opt">$</span><span class="hl std">date,</span><span class="hl str">&quot;%u&quot;</span><span class="hl std">))</span>
                       <span class="hl std">,</span> <span class="hl kwc">DayType</span><span class="hl std">=</span><span class="hl kwd">as.character</span><span class="hl std">(</span><span class="hl str">&quot;&quot;</span><span class="hl std">)</span>
                       <span class="hl std">,</span> <span class="hl kwc">stringsAsFactors</span><span class="hl std">=</span><span class="hl num">FALSE</span>
                       <span class="hl std">)</span>

<span class="hl std">actImpDay[</span><span class="hl kwd">row.names</span><span class="hl std">(actImpDay[actImpDay</span><span class="hl opt">$</span><span class="hl std">DayNum</span><span class="hl opt">&lt;</span><span class="hl num">6</span><span class="hl std">,]),]</span><span class="hl opt">$</span><span class="hl std">DayType</span><span class="hl kwb">&lt;-</span><span class="hl str">&quot;Weekday&quot;</span>
<span class="hl std">actImpDay[</span><span class="hl kwd">row.names</span><span class="hl std">(actImpDay[actImpDay</span><span class="hl opt">$</span><span class="hl std">DayNum</span><span class="hl opt">&gt;</span><span class="hl num">5</span><span class="hl std">,]),]</span><span class="hl opt">$</span><span class="hl std">DayType</span><span class="hl kwb">&lt;-</span><span class="hl str">&quot;Weekend&quot;</span>

<span class="hl std">actImpDayTyp</span> <span class="hl kwb">&lt;-</span> <span class="hl kwd">transform</span><span class="hl std">(  actImpDay</span>
                          <span class="hl std">,</span> <span class="hl kwc">date</span><span class="hl std">=actImpDay</span><span class="hl opt">$</span><span class="hl std">date,</span> <span class="hl kwc">interval</span><span class="hl std">=actImpDay</span><span class="hl opt">$</span><span class="hl std">interval,</span> <span class="hl kwc">steps</span><span class="hl std">=actImpDay</span><span class="hl opt">$</span><span class="hl std">steps</span>
                          <span class="hl std">,</span> <span class="hl kwc">DayType</span><span class="hl std">=</span><span class="hl kwd">as.factor</span><span class="hl std">(actImpDay</span><span class="hl opt">$</span><span class="hl std">DayType)</span>
                         <span class="hl std">)</span>
</pre></div>
</div></div>
  
A panel time series plot of the average number of steps taken across weekday days vs. weekend days by 5-minute interval was plotted using the following R code:  

<div class="chunk" id="PanelTimeSeriesPlot"><div class="rcode"><div class="source"><pre class="knitr r"><span class="hl kwd">xyplot</span><span class="hl std">(steps</span><span class="hl opt">~</span><span class="hl std">interval</span> <span class="hl opt">|</span> <span class="hl std">DayType</span>
       <span class="hl std">, actImpDayTyp</span>
       <span class="hl std">,</span> <span class="hl kwc">type</span><span class="hl std">=</span><span class="hl str">&quot;l&quot;</span>
       <span class="hl std">,</span> <span class="hl kwc">layout</span><span class="hl std">=</span><span class="hl kwd">c</span><span class="hl std">(</span><span class="hl num">1</span><span class="hl std">,</span><span class="hl num">2</span><span class="hl std">)</span>
       <span class="hl std">,</span> <span class="hl kwc">xlab</span><span class="hl std">=</span><span class="hl str">&quot;Number of Steps&quot;</span>
       <span class="hl std">,</span> <span class="hl kwc">ylab</span><span class="hl std">=</span><span class="hl str">&quot;Interval&quot;</span>
       <span class="hl std">,</span> <span class="hl kwc">main</span><span class="hl std">=</span><span class="hl str">&quot;Steps per Interval - Weekend vs. Weekday&quot;</span>
       <span class="hl std">)</span>
</pre></div>
<div class="rimage default"><img src="figure/PanelTimeSeriesPlot-1.png" title="plot of chunk PanelTimeSeriesPlot" alt="plot of chunk PanelTimeSeriesPlot" class="plot" /></div>
</div></div>

# End of Document

