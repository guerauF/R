When dealing with clinical data, ofen we can encounter NAs (not available data).

Ideally, missing data is completely at random (MCAR) but in some cases is not (MNAR).

As well, one possibility could be that although the trait was quantified, the value was out of scale. In 
such cases the best strategy is to inpute the highest value the technique could quantify reliably. In these 
cases we have some information about the real NA value.

If you have no information about which could be the real value there are several ways to deal with NAs.

1. If there are too many NAs in a specific sample or trait, remove it consequently. The rule of thumb is having more than 
5-10% of a trait not known or 30% of a sample, this should be removed if possible.

  To have an idea of how distributed NAs are in your data set you can use the function mice::md.pattern. 
  
  A more visual representation can be obtained with the VIM::aggr function:

  aggr_plot <- aggr(data, col=c('navyblue','red'), numbers=TRUE, sortVars=TRUE, 
labels=names(data), cex.axis=.7, gap=3, ylab=c("Histogram of missing data","Pattern"))

2. Imputation: with the mean (of the same sample subgroup), median or mode.

3. Prediction:

- kNN (DMwR::knnImputation). Is not useful for factor variable. When imputing do not include the response variable!
- rpart. For factos variables method=class while for numeric variables method=anova.
- mice (Multivariate Imputation by Chained Equations). 2 step imputation with mice and complete functions. Mice creates 
  several imputed datasets (default 5). If in the downstream analysis we need to fit a model we should pool the 
  different sets.
  
  modelFit1 <- with(tempData,lm(Temp~ Ozone+Solar.R+Wind))
  summary(pool(modelFit1))


