# Sesame Street - Children's knowledge of numbers
One of my statistics assignments during my master in History and Social Science. I analyzed the general hypothesis concerning a positive effect of watching the TV program Sesame Street on kids' knowledge on numbers. 

My goal with uploading this old exam assignment is to show the different methods within statistics I have worked with. Therefore, I will not have a theory section, but I will rather focus on the method and the overall result. 
For this assignment I have tested three hypothesis: 

* H1) Encouraging parents to let their children watch the TV program Sesame Street has a positive effect on children's knowledge about numbers.

* H2) Regular viewers of the TV show Sesame Street have a better knowledge of numbers than non-regular viewers.

I expect socioeconomic status to have a positive effect on both correlations (both encouragement and regular viewers) because Sesame Street originally targeted children
who lived in cities and had a low socioeconomic background (Lovelace, 1990). Based on this, I have developed the following hypothesis:

* H3) Low socioeconomic status positively moderates the effect of the TV program Sesame Str. on children's knowledge about numbers.


# Data and method 
The study is based on data from an experiment where the intervention is that parents have been encouraged to let their children watch Sesame Str. (encouragement). The study's dependent variable is interval-scaled in relation to children's knowledge about numbers after the experiment (postnumb). The other primary independent variable is whether or not the child watches Sesame Str. frequently (regular). The regular variable is included to check whether the respondents already watched the program often, which may have an impact on whether the intervention is effective. Then I have added a number of control variables, including gender, age (number of months) and children's knowledge of numbers before the intervention (prenumb). Gender is relevant to include because previous research has shown a difference between girls' and boys' results (Mares & Pan, 2013, 141). In addition, it must be expected that if you are older, you have a better understanding of numbers, therefore I also check for the children's previous knowledge of numbers (based on their age). Furthermore, I have added the variable setting which specifies whether you watch Sesame Str. at home or at school. The variable is included because you might expect there is more quite at school than at home or vice versa. Finally, the geographical and socio-economic location (site) will be included as an interaction to see if it moderates the correlation. It is categorically structured in which 1= 3 to 5 years old disadvantaged children from urban centres, 2= 4-year-olds better off children from the suburbs, 3= better off children from the countryside, 4= disadvantaged children from the countryside and 5= disadvantaged Spanish-speaking children. I do not have exact knowledge of the experiment, but it is to be expected that it is a field experiment, because the respondents have not been asked to see Sesame Str. in a laboratory. 

The field experiment has the advantage that the ecological validity is better, as the experiment takes place in real surroundings (Blom-Hansen & Serritzlew, 2014, 15). The assumption is generally in experiments that the control group and the treatment group are the same before the intervention. They must be the same because you want the control group to be an expression of the counterfactual situation (James et al., 2017, 66). This means that the control group would have shown the same effect if they were the group that was exposed to the intervention. Thus, the expectation is that the difference between the groups' results must be due to the intervention (encouragement). I further test the correlation by inserting the intervention variables into an OLS regression to test for any confunder variables. Finally, I will examine the OLS regressions again, but this time by clustering the standard errors for the variable's location. Checking for standard errors in relation to clusters is relevant when data is structured according to a group structure (Hussain Lauridsen, 2017, 41). The problem with data that have a group structure is that there is a high degree of multicollinearity within the groups, but between the groups a normal OLS does not show a dependent relationship. However, one should be aware that groupings must not be too small, as this can create misleading results (Hussain & Lauridsen, 2017, 42).


# Analysis 1

The experiments intervention is whether the parents have been encourged to let their children watch Sesame street. A ANOVA-Balance test shows (table 1) that there are a problem with the similarity between the control group and the treamment group. Both the setting and the regular variable are significant (<0.05) which indicates that there are a difference between the groups in relation to the intervention (encorugement to watch sesame street). The differences in the setting and regular variables is a problem for the study, because the intervention was not done in the same quantity across the groups in the setting and regular variables. 

The significance between the groups in relation to encouragement can be done with a ANOVA test in R (aov): 
```r
# Here with setting as example
bal.test3 <- aov(setting_new~view_enc, data=df)
```
<div align="center">

| Variable| ANOVA (p-value) | N              |
|---------|-----------------|----------------|
| Site    | 0.346           | 1=60<br>       |
|         |                 | 2=55<br>       |
|         |                 | 3=64<br>       |
|         |                 | 4=43<br>       |
|         |                 | 5=18           |
| Setting | < 0.01          | Home=143<br>   |
|         |                 | School=97      |
| Regular | < 0.01          | Not-regular=54 |
|         |                 | Regular=186    |
| Gender  | 0.756           | Men=115        |
|         |                 | Women=125      |

**Table 1**: ANOVA-test for encouragement
</div>

## H1
Figure 1 shows, that there are no significant difference between children that was encouraged to watch Sesame Str. and children that was not encoruged, in relation to their knowledge on numbers.
The differnece is insignificant becuase the confidence intervals overlaps in figure 1. Therefore, we can reject the H1, and on the bases of this study, conclude that there are no effect of encouraging parents to let their children watch Sesame Str. and their children's knowledge of numbers.  

Figure 1 is calculated and visualized with following code: 
```r
# Firstly a t-test for the p value between view_encouragement and post knowledge of numbers
t.test(df$postnumb~df$view_enc, var.equal=TRUE)

# The average treatment effect of view_enc 
plot.data.view_enc <- df %>%
  group_by(view_enc)%>%
  summarise(
    mean = mean(postnumb),
    lci = t.test(postnumb, conf.level = 0.95)$conf.int[1],
    uci = t.test(postnumb, conf.level = 0.95)$conf.int[2])
plot.data.view_enc

# Visualization 
ggplot(plot.data.view_enc, aes(view_enc, mean)) +        
  geom_point() +
  geom_errorbar(aes(ymin = lci, ymax = uci)) +
  labs(x= "Encourgement 0=no 1=yes", y = "Knowledge about numbers")
```

<div align="center">
  <img src="https://github.com/peeje17/Sesame-Street---Childrens-learning/blob/main/ate.png" alt="Figure 1" width="WIDTH" height="HEIGHT">
  **Figure 1**: Encouragement: Average treatment effect on children's knowledge of numbers
</div>

## H2 

The second hypothesis H2, whether regular viewers of sesame street have an effect of the same children's knowledge of numbers, shows more promising results with the average treatment effect.
I have used the same code as above, but now for the variable regular. 



<div align="center">
  <img src="https://github.com/peeje17/Sesame-Street---Childrens-learning/blob/main/regular_ate.png" alt="Figure 1" width="WIDTH" height="HEIGHT">
  Figure 2: Regular: Average treatment effect on children's knowledge of numbers
</div>


I will now explore whether the other variables in the dataset alters the treatment effect as shown in figure 2 using a OLS regression. 

```r
reg_regular2 = lm(postnumb~regular*site+age+sex_new+prenumb+site+setting_new, data=df) 
summary(reg_regular2)

# for visualization of the confidence intervals
plot_summs(reg_regular2)
```

<div align="center">
  <img src="https://github.com/peeje17/Sesame-Street---Childrens-learning/blob/main/regular_ols.png" alt="Figure 1" width="WIDTH" height="HEIGHT">
  **Figure 3**: OLS regression on children's knowledge about numbers 
</div>

The regular variable continues to be significant, which then indicates, on basis of this data, that watching Sesame Street regularly increases children's knowledge about numbers. However, the coefficient on 7.69 is only a small effect on the postnumb variable which goes from 0 to 54.     


## H3
The Sesame Street program originally targeted children who lived in cities and had a low socioeconomic background (Lovelace, 1990). Therefore, I expect being a regular viewer of the program and coming from a disadvantaged home increases those children's knowledge about numbers (H3). The site variable can help me answer that question, which are grouped as following. 

1= 3 to 5 years old disadvantaged children from urban centres 
2= 4-year-old better off children from the suburbs 
3= better off children from the countryside 
4= disadvantaged children from the countryside 
5= disadvantaged Spanish-speaking children

The next OLS regression I have clustered the standard errors according to location. This means that the multicollinearity that occurs within the groups is taken into account for the location of the variable, so that the model can assume a dependent relationship between the groups. However, it is worth mentioning that clustered standard errors is more reliable when having a large dataset, and the dataset I am using only has 240 observations. 

For the regular variable I find no meaningful results in relation sites interaction on regular viewer's knowledge about numbers (not shown). The regular variable is still significant. More interesting is the interaction between the encouragement variable and the site variable which now shows a significant result.

<div align="center">
  <img src="https://github.com/peeje17/Sesame-Street---Childrens-learning/blob/main/view_enc_cluster.png" alt="Figure 1" width="WIDTH" height="HEIGHT">
  **Figure 4**: OLS regression (clustered) on children's knowledge about numbers 
</div>

When beeing encouraged to watch Sesame Str. and moving from the group 1 (disadvantaged inner city) to group 2 (advantaged suburban) and 4 (disadvantaged rural) shows a significant positive result, with the largest effect of going from group 1 to 2, which has a effect of 4.25 on scale from 0 to 54. Based on these results I have to reject H3, because the results shows a positive relationship going from disadvantaged inner city children to advantaged suburban children. 


# Conclusion and discussion
The overall conclusion and interesting result based on this dataset, is that regular viewers of the TV show Sesame Street have a better knowledge of numbers than non-regular viewers (H2). The internal validity is high because of the many control variables which are included in the study. However, the external validity is more modest because the dataset only contains 240 observations. The first hypothesis concerning the encouragement from parents to let their children watch the TV program Sesame Str. showed no significant effect. The last hypothesis analyzed whether low socioeconomic status positively moderates the effect of the TV program Sesame Str. on children's knowledge about numbers with the use of clustered standard errors. Here the results are more muddy. There was no significant results of different groups of regular viewers, but with encouragement and especially with group 1 to 2 showed a positive significant result. The positive relation of being encouraged to watch Sesame Str. and going from group 1 (disadvantaged children from urban centers) and to group 2 (better of children from the suburbs), contradicts the expectation from H3, which anticipated the TV program would be more effectful for disadvantaged children. This results must also be seen in the light of the very poor site variable, where serval dimensions are included and therefore has a change for muddy results. There are for example differences between the groups concerning age, race and geography, which then makes it difficult to distinguish the variables from each other. 

# Litterature

* Blom-Hansen, J., & Serritzlew, S. (2014). Endogenitet og eksperimenter—Forskningsdesignet som løsning. Politica, Journal Article. https://go.exlibris.link/rrqLKPkV
* Hussain, M. A., & Lauridsen, J. T. (2017). Videregående kvantitative metoder (Bd. 5). Samfundslitteratur. https://go.exlibris.link/fj6tJrfG
* James, O., Jilke, S., & Van Ryzin, G. G. (2017). Causal Inference and the Design and Analysis of Experiments. I Experiments in Public Management Research (s. 59–88). Cambridge University Press. https://doi.org/10.1017/9781316676912.005
* Lovelace, V. (1990). “Sesame Street” as a Continuing Experiment. Educational Technology Research and Development, 38(4), 17–24.
* Mares, M.-L., & Pan, Z. (2013). Effects of Sesame Street: A meta-analysis of children’s learning in 15 countries. Journal of Applied Developmental Psychology, 34(3), 140–151. https://doi.org/10.1016/j.appdev.2013.01.001

