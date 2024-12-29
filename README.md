# Sesame Street - Childrens learning
My statistics assignments during my master in History and Social Science. I analyzed the general hypothesis concerning a postive effect of watching the TV program Sesame Street on kids' knowledge on numbers. 

My goal with uploading this old exam assignment is to show the different methods within statistics I have worked with. Therefore, I will not have a theory section, but I will rather focus on the methodolgy issues and the overall result. 
For this assignment I have tested four hypothesis: 

* H1) Encouraging parents to let their children watch the TV program Sesame Street has a positive effect on children's knowledge about numbers.

* H2) Regular viewers of the TV show Sesame Street have a better knowledge of numbers than non-regular viewers.

I expect socioeconomic status and geography to have a positive effect on the correlation because Sesame Street originally targeted children
who lived in cities and had a low socioeconomic background (Lovelace, 1990). Based on this, I have I developed the following two hypotheses:

* H3) Low socioeconomic status positively moderates the effect of the TV program Seseame Str. on children's knowledge about numbers.

* H4) Children from rural areas negatively moderate the effect of the TV program Seseame Str. on children's knowledge about numbers.


# Data and method 
The study is based on data from an experiment where the intervention is that parents have been encouraged to let their children watch Sesame Str. (encouragement). The study's dependent variable is interval-scaled in relation to children's knowledge of numbers after the experiment (postfigures). The other primary independent variable is whether or not you watch Sesame Str. frequently (regularity). The regularity variable is included to check whether the respondents already watch the program often, which may have an impact on whether the call is effective. Then I have added a number of control variables, including gender, age (number of months) and children's knowledge of numbers before the intervention (prenumbers). Gender is relevant to include because previous research has shown a difference between girls' and boys' results (Mares & Pan, 2013, 141). In addition, it must be expected that if you are older, you have a better understanding of numbers, therefore I also check for the children's previous knowledge of numbers. Furthermore, I have added the variable setting which specifies whether you watch Sesame Str. at home or at school. The variable is included because you might expect there is more quite at school than at home or vice versa. Finally, the geographical and socio-economic location (site) will be included as an interaction link to see if it moderates the correlation. It is categorically structured in which 1= 3 to 5 years old disadvantaged children from urban centres, 2= 4-year-olds better off children from the suburbs, 3= better off children from the countryside, 4= disadvantaged children from the countryside and 5= disadvantaged Spanish-speaking children. I do not have exact knowledge of the experiment, but it is to be expected that it is a field experiment, because the respondents have not been asked to see Sesame Str. in a laboratory. 

The field experiment has the advantage that the ecological validity is better, as the experiment takes place in real surroundings (Blom-Hansen & Serritzlew, 2014, 15). The assumption is generally in experiments that the control group and the treatment group are the same before the intervention. They must be the same because you want the control group to be an expression of the counterfactual situation (James et al., 2017, 66). This means that the control group would have shown the same effect if they were the group that was exposed to the intervention. Thus, the expectation is that the difference between the groups' results must be due to the intervention (encouragement). I then further test the correlation by inserting the intervention variables into an OLS regression to test for any confunder variables. Finally, I will examine the OLS regressions again, but this time by clustering the standard errors for the variable's location. Checking for standard errors in relation to clusters is relevant when data is structured according to a group structure (Hussain Lauridsen, 2017, 41). The problem in data that have a group structure is that there is a high degree of multicollinearity within the groups, but between the groups, a normal OLS does not show a dependent relationship. However, one should be aware that groupings must not be too small, as this can create misleading results (Hussain & Lauridsen, 2017, 42). Thus, the study is based on an experiment, but tests the causality of an OLS regression to finally examine the meaning of groupings in the dataset.

Table 1
See the full codebook here: https://search.r-project.org/CRAN/refmans/bain/html/sesamesim.html. 


# Analysis 1

The experiments intervention is whether the parents have been encourged to let their children watch Sesame street. A ANOVA-Balance test shows (table 1) that there are a problem with the similarity between the control group and the treamment group. Both the setting and the regular variable are significant (<0.05) which indicates that there are a difference between the groups in relation to the intervention (encorugement to watch sesame street). The differences in the setting and regular variables is a problem for the study, because the intervention was not done in the same quantity across the groups in the setting and regular varibles. 

The significance bewtween the groups in relation to encourgement can be done with a anova test in R (aov): 
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

**Table 1**: Anova-test for encourgement
</div>

## H1
Figure 1 shows, that there are no significant difference between children that was encourged to watch sesame streen and children that was not encoruged, in relation to their knowledge on numbers.
The differnece is insignificant becuase the confidence intervals overlaps in figure 1. Therefore, we can reject the H1, and on the bases of this study, conclude that there are no effect of encourgring parents to let their children watch sesame street and their childrens knowlege of numbers.  

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

  **Figure 1**: Encorugement: Average treatment effect on childrens knowlegde of numbers
</div>

## H2 

The second hypothiss H2, whether regular viewers of sesame street have an effect of the same childrens knowledge of numbers shows more promising results with the average treatment effect.
I have used the same code as above, but now the variable regular. 



<div align="center">
  <img src="https://github.com/peeje17/Sesame-Street---Childrens-learning/blob/main/regular_ate.png" alt="Figure 1" width="WIDTH" height="HEIGHT">

  **Figure 2**: Regular: Average treatment effect on childrens knowlegde of numbers
</div>


For the next I will explorer wheter the other variables in the dataset alters the treatment effect as shown in figure 2. 

```r
reg_regular2 = lm(postnumb~regular*site+age+sex_new+prenumb+site+setting_new, data=df) 
summary(reg_regular2)

# for visualization of the confidence intervals
plot_summs(reg_regular2)
```

<div align="center">
  <img src="https://github.com/peeje17/Sesame-Street---Childrens-learning/blob/main/regular_ols.png" alt="Figure 1" width="WIDTH" height="HEIGHT">

  **Figure 3**: OLS regression on childrens knowledge about numbers 

</div>

1 Expaing the regression and the interaction 
2 consider during one regression with cluster 
3 conclusion



