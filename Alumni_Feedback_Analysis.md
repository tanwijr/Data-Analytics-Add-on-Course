## Basic Data Analysis

```{r ,message=FALSE, warning=FALSE}
# Cleaning of data
#library(tidyverse)
df=read.csv('CRS.csv',head=TRUE)
df[is.na(df)]=0
library(stringr)
library(psych)
library(kableExtra)
library(gmodels)
set.seed(42)
library(magrittr)
library(qwraps2)
# Load function
source("http://pcwww.liv.ac.uk/~william/R/crosstab.r")
```

## Exploratory Analysis of Alumni Feedback


```{r}
#require(RCurl)
#dataCSV <- getURL('https://docs.google.com/spreadsheets/d/e/2PACX-1vQ81NS0kq8tl0qKLhDIk7MT8_f0v3CWMMksAZLXOYly8r29hjDEwD2pKQtz_z0qjiD6PdhAXr7VRx8O/pub?gid=551768500&single=true&output=csv')
#tt = getForm("https://docs.google.com/spreadsheets/d/e/2PACX-1vQ81NS0kq8tl0qKLhDIk7MT8_f0v3CWMMksAZLXOYly8r29hjDEwD2pKQtz_z0qjiD6PdhAXr7VRx8O/pub?gid=551768500&single=true&output=csv", hl ="en_US", key = "0Aonsf4v9iDjGdHRaWWRFbXdQN1ZvbGx0LWVCeVd0T1E",  output = "csv",          .opts = list(followlocation = TRUE, verbose = TRUE, ssl.verifypeer = FALSE)) 

#holidays <- read.csv(textConnection(tt))
```
```{r}
#df=read.csv(na.omit(textConnection(tt)),header=TRUE,sep=',')
```
```{r}
#library(qdap)
#NAer(df)
df[df=="NA"] <- 0
```

## Factor re-leveling

```{r}
# convert the the likert scalled response to factor
fc=15:54
df[,fc] <- lapply(df[,fc] , function(X){ordered(X,
              levels = c("Poor","Fair","Good","Very Good", "Excellent"),labels=c("Poor","Fair", "Good","Very Good","Excellent"))})
df[,55]=factor(df[,55])
```



```{r}
df$Gender<- factor(df$Gender)
levels(df$Gender) <-c("Female","Male")
#df$Year.of.graduation.from.Saintgits<- factor(df$Year.of.graduation.from.Saintgits)
#levels(df$Gender) <-c("Female","Male")
```
```{r}
summary(df$Gender)
```

## Percentage analysis of Division with respect to Gender

```{r}
crosstab(df,col.vars =c("Division") ,row.vars =c("Gender"),type=c("t"),style='long')
```

```{r}
prop.table(table(df$Gender))*100
```


## Gender-wise working status


```{r}
t1=prop.table(table(df$Division))*100
xtable::xtable(t1)
```


```{r}
library(plotly)
df1=filter(df,Gender=="Male")
Temp=table(df1$Division)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```


```{r}
library(plotly)
df2=filter(df,Gender=="Female")
Temp=table(df2$Division)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```

## CGPA of Alumni

```{r}
library(plotly)
df2=filter(df,Division=="Alumni - Engineering")
df2$Overall.CGPA=as.factor(df2$Overall.CGPA)
Temp=table(df2$Overall.CGPA)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```



## Analysis of Question 1- Based on your expertise, how well does the present curriculum cover all the skills that are essential for undergraduate students to learn?


```{r}
#dfs=filter(df,Current.employment.stream=="Structural Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")
Temp=table(df[,15])
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
library(dplyr)
df2=df[,c(3,15:55)]
FSa=group_by(df2, Gender) %>%
  summarise(
    count = n(),
    mean = round(mean(as.integer(df2[,2])
, na.rm = TRUE),2),
    sd = round(sd(as.integer(df2[,2])
, na.rm = TRUE),2)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```


## Analysis of Question 2- How good is the current engineering curriculum in adequately preparing students for their future careers by exposing them to emerging technologies?



```{r}
#dfs=filter(df,Current.employment.stream=="Structural Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")
Temp=table(df[,16])
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
library(dplyr)
df2=df[,c(3,15:55)]
FSa=group_by(df2, Gender) %>%
  summarise(
    count = n(),
    mean = round(mean(as.integer(df2[,3])
, na.rm = TRUE),2),
    sd = round(sd(as.integer(df2[,3])
, na.rm = TRUE),2)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```

## Analysis of Question 3- How well do you feel the current engineering curriculum aligns with the needs of industry and society?




```{r}
#dfs=filter(df,Current.employment.stream=="Structural Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")
Temp=table(df[,17])
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
library(dplyr)
df2=df[,c(3,15:55)]
FSa=group_by(df2, Gender) %>%
  summarise(
    count = n(),
    mean = round(mean(as.integer(df2[,4])
, na.rm = TRUE),2),
    sd = round(sd(as.integer(df2[,4])
, na.rm = TRUE),2)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```

## Analysis of Question 4- Do you think the current engineering curriculum provides enough opportunities for practical, hands-on learning?





```{r}
#dfs=filter(df,Current.employment.stream=="Structural Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")
Temp=table(df[,18])
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
library(dplyr)
df2=df[,c(3,15:55)]
FSa=group_by(df2, Gender) %>%
  summarise(
    count = n(),
    mean = round(mean(as.integer(df2[,5])
, na.rm = TRUE),2),
    sd = round(sd(as.integer(df2[,5])
, na.rm = TRUE),2)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```

## Analysis of Question 5- Is the balance between various course modes - Theory, Practical, Project, Theory-Practical (integrated), Theory-Project (integrated)- in the curriculum adequate?






```{r}
#dfs=filter(df,Current.employment.stream=="Structural Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")
Temp=table(df[,19])
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
library(dplyr)
df2=df[,c(3,15:55)]
FSa=group_by(df2, Gender) %>%
  summarise(
    count = n(),
    mean = round(mean(as.integer(df2[,7])
, na.rm = TRUE),2),
    sd = round(sd(as.integer(df2[,7])
, na.rm = TRUE),2)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```

## Analysis of Question 6- Does the curriculum enable the faculty members to innovate their teaching-learning pedagogies such as hybrid learning, blended learning, flipped classroom learning, experimental learning etc.?







```{r}
#dfs=filter(df,Current.employment.stream=="Structural Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")
Temp=table(df[,20])
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
library(dplyr)
df2=df[,c(3,15:55)]
FSa=group_by(df2, Gender) %>%
  summarise(
    count = n(),
    mean = round(mean(as.integer(df2[,7])
, na.rm = TRUE),2),
    sd = round(sd(as.integer(df2[,7])
, na.rm = TRUE),2)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```



## Analysis of Question 7- How relevant is the current engineering curriculum in preparing students for graduate studies in engineering and various competitive examinations like GATE, GMAT, etc.?








```{r}
#dfs=filter(df,Current.employment.stream=="Structural Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")
Temp=table(df[,21])
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
library(dplyr)
df2=df[,c(3,15:55)]
FSa=group_by(df2, Gender) %>%
  summarise(
    count = n(),
    mean = round(mean(as.integer(df2[,8])
, na.rm = TRUE),2),
    sd = round(sd(as.integer(df2[,8])
, na.rm = TRUE),2)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```




## Analysis of Question 6- Does the curriculum enable the faculty members to innovate their teaching-learning pedagogies such as hybrid learning, blended learning, flipped classroom learning, experimental learning etc.?







```{r}
#dfs=filter(df,Current.employment.stream=="Structural Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")
Temp=table(df[,20])
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
library(dplyr)
df2=df[,c(3,15:55)]
FSa=group_by(df2, Gender) %>%
  summarise(
    count = n(),
    mean = round(mean(as.integer(df2[,7])
, na.rm = TRUE),2),
    sd = round(sd(as.integer(df2[,7])
, na.rm = TRUE),2)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```



## Analysis of Question 8- How relevant is the current engineering curriculum in preparing students for graduate studies in engineering and various competitive examinations like GATE, GMAT, etc.?








```{r}
#dfs=filter(df,Current.employment.stream=="Structural Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")
Temp=table(df[,38])
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
library(dplyr)
df2=df[,c(3,15:55)]
FSa=group_by(df2, Gender) %>%
  summarise(
    count = n(),
    mean = round(mean(as.integer(df2[,23])
, na.rm = TRUE),2),
    sd = round(sd(as.integer(df2[,23])
, na.rm = TRUE),2)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```


























## Analysis of the question-My training in design helped me in my profession

```{r}
Temp=table(dfs$My.training.in.design.helped.me.in.my.profession)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfs, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(as.integer(My.training.in.design.helped.me.in.my.profession)
, na.rm = TRUE),
    sd = sd(My.training.in.design.helped.me.in.my.profession
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```
## Analysis of the Question-My training in Saintgits helped me in using engineering tools (like AutoCAD, STAAD etc.)

```{r}
dfs$My.training.in.Saintgits.helped.me.in.using.engineering.tools..like.AutoCAD..STAAD.etc..[is.na(dfs$My.training.in.Saintgits.helped.me.in.using.engineering.tools..like.AutoCAD..STAAD.etc..)]=0
dfs$My.training.in.Saintgits.helped.me.in.using.engineering.tools..like.AutoCAD..STAAD.etc...1[is.na(dfs$My.training.in.Saintgits.helped.me.in.using.engineering.tools..like.AutoCAD..STAAD.etc...1)]=0
dfs$My.training.in.Saintgits.helped.me.in.using.engineering.tools..like.AutoCAD..STAAD.etc...2[is.na(dfs$My.training.in.Saintgits.helped.me.in.using.engineering.tools..like.AutoCAD..STAAD.etc...2)]=0
dfs$My.training.in.Saintgits.helped.me.in.using.engineering.tools..like.AutoCAD..STAAD.etc...3[is.na(dfs$My.training.in.Saintgits.helped.me.in.using.engineering.tools..like.AutoCAD..STAAD.etc...3)]=0
dfs$My.training.in.Saintgits.helped.me.in.using.engineering.tools..like.AutoCAD..STAAD.etc...5[is.na(dfs$My.training.in.Saintgits.helped.me.in.using.engineering.tools..like.AutoCAD..STAAD.etc...5)]=0
tools_cumulative=c(dfs$My.training.in.Saintgits.helped.me.in.using.engineering.tools..like.AutoCAD..STAAD.etc..,dfs$My.training.in.Saintgits.helped.me.in.using.engineering.tools..like.AutoCAD..STAAD.etc...1,dfs$My.training.in.Saintgits.helped.me.in.using.engineering.tools..like.AutoCAD..STAAD.etc...2,dfs$My.training.in.Saintgits.helped.me.in.using.engineering.tools..like.AutoCAD..STAAD.etc...3,dfs$My.training.in.Saintgits.helped.me.in.using.engineering.tools..like.AutoCAD..STAAD.etc...5)
Temp=table(tools_cumulative)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
## Additional software or tool for your current career

```{r}
#df2=filter(df,Gender=="Female")
tb1=table(dfs$Did.you.learn.any.additional.software.or.tool.for.your.current.career.)
fig <- plot_ly(type='pie', labels=names(tb1), values=tb1,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=T)
fig
```

## Additional software Tools learned by the Students in Structural Engineering Stream After Graduation

```{r}
kableExtra::kable(dfs$If.yes..provide.details.)
```
## Analysis of Geotechnical Stream

## Analysis of the question- Training in Saintgits help you in conducting investigation and finding soil properties?


```{r}
dfg=filter(df,Current.employment.stream=="Geotechnical Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfg$Training.in.Saintgits..help.you.in.conducting.investigation.and.finding.soil.properties.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfg, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits..help.you.in.conducting.investigation.and.finding.soil.properties.
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits..help.you.in.conducting.investigation.and.finding.soil.properties.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```

## Training in Saintgits  help you to identify geotechnical problem and solve
```{r}
dfg=filter(df,Current.employment.stream=="Geotechnical Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfg$Training.in.Saintgits..help.you.to.identify.geotechnical.problem.and.solve)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfg, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits..help.you.to.identify.geotechnical.problem.and.solve
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits..help.you.to.identify.geotechnical.problem.and.solve
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```

##Q3- TTraining in Saintgits help you to plan and execute geotechnical construction activities
```{r}
dfg=filter(df4,Current.employment.stream=="Geotechnical Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfg$Training.in.Saintgits..help.you.to.plan.and.execute.geotechnical.construction.activities.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfg, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits..help.you.to.plan.and.execute.geotechnical.construction.activities.
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits..help.you.to.plan.and.execute.geotechnical.construction.activities.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```

## Analysis of Environmental Stream

##Q1- Training in Saintgits help to design water treatment systems


```{r}
dfe=filter(df1,Current.employment.stream=="Environmental Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfe$Training.in.Saintgits.help.to.construct.water.treatment.systems)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfe, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.help.to.construct.water.treatment.systems
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.help.to.construct.water.treatment.systems
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```

## Q3-Training in Saintgits help to design sewage treatment systems
```{r}
dfe=filter(df1,Current.employment.stream=="Environmental Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfe$Training.in.Saintgits.help.to.design.sewage.treatment.systems)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfe, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.help.to.design.sewage.treatment.systems
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.help.to.design.sewage.treatment.systems
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```
## Q3-Training in Saintgits help to design sewage treatment systems
```{r}
dfe=filter(df1,Current.employment.stream=="Environmental Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfe$Training.in.Saintgits.help.to.design.sewage.treatment.systems)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfe, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.help.to.design.sewage.treatment.systems
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.help.to.design.sewage.treatment.systems
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```
## Q4-Training in Saintgits help to constructsewage treatment systems
```{r}
dfe=filter(df1,Current.employment.stream=="Environmental Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfe$Training.in.Saintgits.help.to.construct.sewage.treatment.systems)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfe, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.help.to.construct.sewage.treatment.systems
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.help.to.construct.sewage.treatment.systems
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```
## Q5-Training in Saintgits identify the pollution issues and solve
```{r}
dfe=filter(df1,Current.employment.stream=="Environmental Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfe$Training.in.Saintgits.identify.the.pollution.issues.and.solve.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfe, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.identify.the.pollution.issues.and.solve.
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.identify.the.pollution.issues.and.solve.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```

## Analysis of Transportation Stream

##Q1- Training in Saintgits helps to conduct traffic survey and design signal


```{r}
dft=filter(df4,Current.employment.stream=="Transportation Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dft$Training.in.Saintgits.help.you.design.and.construct.pavements.for.roads.and.airports)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dft, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.help.you.design.and.construct.pavements.for.roads.and.airports
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.help.you.design.and.construct.pavements.for.roads.and.airports
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```
##Q2- Training in Saintgits help you to select materials for road construction


```{r}
dft=filter(df4,Current.employment.stream=="Transportation Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dft$Training.in.Saintgits.help.you.design.and.construct.pavements.for.roads.and.airports)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dft, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.help.you.design.and.construct.pavements.for.roads.and.airports
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.help.you.design.and.construct.pavements.for.roads.and.airports
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```
##Q3- Training in Saintgits help you design and construct pavements for roads and airports


```{r}
dft=filter(df4,Current.employment.stream=="Transportation Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dft$Training.in.Saintgits.help.you.design.and.construct.pavements.for.roads.and.airports)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dft, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.help.you.design.and.construct.pavements.for.roads.and.airports
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.help.you.design.and.construct.pavements.for.roads.and.airports
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```
##Q4- Training in Saintgits help you to fix alignment for roads and railways


```{r}
dft=filter(df4,Current.employment.stream=="Transportation Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dft$Training.in.Saintgits.help.you.to.fix.alignment.for.roads.and.railways.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dft, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.help.you.to.fix.alignment.for.roads.and.railways.
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.help.you.to.fix.alignment.for.roads.and.railways.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```
## Analysis of Construction Stream

##Q1- Training in Saintgits helps you to set-out and plan the construction of structures


```{r}
dfc=filter(df4,Current.employment.stream=="Construction Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfc$Training.in.Saintgits.helps.you.to.set.out.and.plan.the.construction.of.structures)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfc, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.helps.you.to.set.out.and.plan.the.construction.of.structures
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.helps.you.to.set.out.and.plan.the.construction.of.structures
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```
##Q2- Training in Saintgits enable to select suitable equipments and construction methods


```{r}
dfc=filter(df4,Current.employment.stream.1=="Construction Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfc$Training.in.Saintgits.enable.to.select.suitable.equipments.and.construction.methods)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfc, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.enable.to.select.suitable.equipments.and.construction.methods
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.enable.to.select.suitable.equipments.and.construction.methods
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```
##Q3- Training in Saintgits helps you to prepare a schedule and monitor construction work

```{r}
dfc=filter(df4,Current.employment.stream=="Construction Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfc$Training.in.Saintgits.helps.you.to.prepare.a.schedule.and.monitor.construction.work.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfc, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.helps.you.to.prepare.a.schedule.and.monitor.construction.work.
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.helps.you.to.prepare.a.schedule.and.monitor.construction.work.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```
##Q4- Training in Saintgits helps you to select materials for construction ensure quality control in construction

```{r}
dfc=filter(df,Current.employment.stream=="Construction Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfc$Training.in.Saintgits.helps.you.to.select.materials.for.construction.ensure.quality.control.in.construction.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfc, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.helps.you.to.select.materials.for.construction.ensure.quality.control.in.construction.
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.helps.you.to.select.materials.for.construction.ensure.quality.control.in.construction.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```

##Q5- Training in Saintgits made you capable of executing the construction as per the architectural ad structural drawings
```{r}
dfc=filter(df4,Current.employment.stream=="Construction Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfc$Training.in.Saintgits.made.you.capable.of.executing.the.construction.as.per.the.architectural.ad.structural.drawings.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfc, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.made.you.capable.of.executing.the.construction.as.per.the.architectural.ad.structural.drawings.
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.made.you.capable.of.executing.the.construction.as.per.the.architectural.ad.structural.drawings.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```

## Analysis of responses in survey and quantity estimation
##Q1- Training in Saintgits help me to conduct survey
```{r}
dfsur=filter(df,Current.employment.stream=="Survey and Estimation")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfsur$Training.in.Saintgits.help.me.to.conduct.survey.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfsur, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.help.me.to.conduct.survey.
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.help.me.to.conduct.survey.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```

#Q2-Rate your confidence in carrying out survey in conventional and digital methods
```{r}
dfsur=filter(df4,Current.employment.stream=="Survey and Estimation")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfsur$Rate.your.confidence.in.carrying.out.survey.in.conventional.and.digital.methods.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfsur, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Rate.your.confidence.in.carrying.out.survey.in.conventional.and.digital.methods.
, na.rm = TRUE),
    sd = sd(Rate.your.confidence.in.carrying.out.survey.in.conventional.and.digital.methods.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```

#Q3-Rate your confidence in setting out and fixing alignments of roads
```{r}
dfsur=filter(df4,Current.employment.stream=="Survey and Estimation")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfsur$Rate.your.confidence.in.setting.out.and.fixing.alignments.of.roads.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfsur, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Rate.your.confidence.in.setting.out.and.fixing.alignments.of.roads.
, na.rm = TRUE),
    sd = sd(Rate.your.confidence.in.setting.out.and.fixing.alignments.of.roads.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```

#Q4-Training in Saintgits help me to prepare estimate for all types of civil construction
```{r}
dfsur=filter(df1,Current.employment.stream=="Survey and Estimation")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfsur$Training.in.Saintgits.help.me.to.prepare.estimate.for.all.types.of.civil.construction.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfsur, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.help.me.to.prepare.estimate.for.all.types.of.civil.construction.
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.help.me.to.prepare.estimate.for.all.types.of.civil.construction.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable
```

## Analysis of Water resource engineering
#Q1-Training in saintgits help you to conduct hydrological studies
```{r}
dfw=filter(df,Current.employment.stream=="Water Resources")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfw$Training.in.saintgits.help.you.to.conduct.hydrological.studies)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfw, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.saintgits.help.you.to.conduct.hydrological.studies
, na.rm = TRUE),
    sd = sd(Training.in.saintgits.help.you.to.conduct.hydrological.studies
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable
```

#Q2-Training in saintgits help you to design and construct water retaining strutcures
```{r}
dfw=filter(df4,Current.employment.stream=="Water Resources")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfw$Training.in.saintgits.help.you.to.design.and.construct.water.retaining.strutcures)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfw, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.saintgits.help.you.to.design.and.construct.water.retaining.strutcures
, na.rm = TRUE),
    sd = sd(Training.in.saintgits.help.you.to.design.and.construct.water.retaining.strutcures
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable
```

#Q3-Training in saintgits help you to design and construct irrigation structures
```{r}
dfw=filter(df4,Current.employment.stream=="Water Resources")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfw$Training.in.saintgits.help.you.to.design.and.construct.irrigation.structures)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfw, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.saintgits.help.you.to.design.and.construct.irrigation.structures
, na.rm = TRUE),
    sd = sd(Training.in.saintgits.help.you.to.design.and.construct.irrigation.structures
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable
```

#Q4-Training in Saintgits help you to analyze fluid flow and design corresponding systems
```{r}
dfw=filter(df4,Current.employment.stream=="Water Resources")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfw$Training.in.Saintgits.help.you.to.analyze.fluid.flow.and.design.corresponding.systems)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfw, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.help.you.to.analyze.fluid.flow.and.design.corresponding.systems
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.help.you.to.analyze.fluid.flow.and.design.corresponding.systems
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable
```

## Analysis of Other engineering stream

#Q1-Rate your analytical skill acquired from training in Saintgits
```{r}
dfo=filter(df,Current.employment.stream.1=="Other engineering field")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfo$Rate.your.analytical.skill.acquired.from.training.in.Saintgits)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfo, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Rate.your.analytical.skill.acquired.from.training.in.Saintgits
, na.rm = TRUE),
    sd = sd(Rate.your.analytical.skill.acquired.from.training.in.Saintgits
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable
```
#Q2-Rate your design skill acquired from training in Saintgits
```{r}
dfo=filter(df4,Current.employment.stream=="Other engineering field")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfo$Rate.your.design.skill.acquired.from.training.in.Saintgits)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfo, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Rate.your.design.skill.acquired.from.training.in.Saintgits
, na.rm = TRUE),
    sd = sd(Rate.your.design.skill.acquired.from.training.in.Saintgits
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable
```

#Q3-Rate your construction skill acquired from training in Saintgits
```{r}
dfo=filter(df4,Current.employment.stream=="Other engineering field")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfo$Rate.your.construction.skill.acquired.from.training.in.Saintgits)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfo, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Rate.your.construction.skill.acquired.from.training.in.Saintgits
, na.rm = TRUE),
    sd = sd(Rate.your.construction.skill.acquired.from.training.in.Saintgits
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable
```
## Analysis of non-engineering/business

#Q1-Training in Saintgits contributed to your analytical skills
```{r}
dfn=filter(df4,Current.employment.stream=="Non engineering business")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfn$Training.in.Saintgits.contributed.to.your.analytical.skills)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfn, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.contributed.to.your.analytical.skills
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.contributed.to.your.analytical.skills
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable
```

## q3- nUMERICAL AND PROBLEM SOLVING SKILLS
```{r}
dfn=filter(df,Current.employment.stream=="Non engineering business")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfn$Training.in.Saintgits.contributed.to.your.numerical.and.problem.solving.skills)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfn, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.contributed.to.your.numerical.and.problem.solving.skills
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.contributed.to.your.numerical.and.problem.solving.skills
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable
```
## Q4- Team work skill

## q4- NUMERICAL AND PROBLEM SOLVING SKILLS
```{r}
dfn=filter(df,Current.employment.stream=="Non engineering business")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(dfn$Training.in.Saintgits.contributed.to.your.management.and.team.work.skills)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(dfn, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.contributed.to.your.management.and.team.work.skills
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.contributed.to.your.management.and.team.work.skills
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable
```

## Analysis of Teaching and Research

## q1- Training in Saintgits help me to attain clarity in teaching engineering subjects effectively
```{r}
#dftr=filter(df,Current.employment.stream=="Non engineering business")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(df$Training.in.Saintgits.help.me.to.attain.clarity.in.teaching.engineering.subjects.effectively.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(df, Current.Teaching.Stream) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.help.me.to.attain.clarity.in.teaching.engineering.subjects.effectively.
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.help.me.to.attain.clarity.in.teaching.engineering.subjects.effectively.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable
```
## q2- Training at Saintgits has enabled me to analyze and solve engineering problems better
```{r}
#dftr=filter(df,Current.employment.stream=="Non engineering business")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(df$Training.at.Saintgits.has.enabled.me.to.analyze.and.solve..engineering.problems.better.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(df, Current.Teaching.Stream) %>%
  summarise(
    count = n(),
    mean = mean(Training.at.Saintgits.has.enabled.me.to.analyze.and.solve..engineering.problems.better.
, na.rm = TRUE),
    sd = sd(Training.at.Saintgits.has.enabled.me.to.analyze.and.solve..engineering.problems.better.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable
```
## q3- Training in Saintgits helps to pursue independent and lifelong learning
```{r}
#dftr=filter(df,Current.employment.stream=="Non engineering business")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(df$Training.in.Saintgits.helps.to.pursue.independent.and.lifelong.learning)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(df, Current.Teaching.Stream) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.helps.to.pursue.independent.and.lifelong.learning
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.helps.to.pursue.independent.and.lifelong.learning
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(FSa)
```
## q4- Training in Saintgits helps to adapt in diverse environments and to instils team dynamics
```{r}
#dftr=filter(df,Current.employment.stream=="Non engineering business")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(df$Training.in.Saintgits.helps.to.adapt.in.diverse.environments.and.to.instills.team.dynamics)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(df, Current.Teaching.Stream) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.helps.to.adapt.in.diverse.environments.and.to.instills.team.dynamics
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.helps.to.adapt.in.diverse.environments.and.to.instills.team.dynamics
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(FSa)
```

## q5- Training in Saintgits helps to follow ethical practice and accept responsibilities
```{r}
#dftr=filter(df,Current.employment.stream=="Non engineering business")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(df$Training.in.Saintgits.helps.to.follow.ethical.practice.and.accept.responsibilities)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(df, Current.Teaching.Stream) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.helps.to.follow.ethical.practice.and.accept.responsibilities
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.helps.to.follow.ethical.practice.and.accept.responsibilities
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(FSa)
```

## q6- Training in Saintgits helps assess the impact of the engineering solutions in societal andenvironmental contexts for sustainable development
```{r}
#dftr=filter(df,Current.employment.stream=="Non engineering business")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(df$Training.in.Saintgits.helps.assess.the.impact.of.the.engineering.solutions.in.societal.and.environmental.contexts.for.sustainable.development.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(df, Current.Teaching.Stream) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.helps.assess.the.impact.of.the.engineering.solutions.in.societal.and.environmental.contexts.for.sustainable.development.
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.helps.assess.the.impact.of.the.engineering.solutions.in.societal.and.environmental.contexts.for.sustainable.development.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(FSa)
```

## q7- Training in Saintgits helps me to communicate effectively on complex Civil Engineeringactivities with the Engineering community and the society
```{r}
#dftr=filter(df,Current.employment.stream=="Non engineering business")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(df$Training.in.Saintgits.helps.me.to.communicate.effectively.on.complex.Civil.Engineering.activities.with.the.Engineering.community.and.the.society.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(df, Current.Teaching.Stream) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.helps.me.to.communicate.effectively.on.complex.Civil.Engineering.activities.with.the.Engineering.community.and.the.society.
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.helps.me.to.communicate.effectively.on.complex.Civil.Engineering.activities.with.the.Engineering.community.and.the.society.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(FSa)
```
## q8- Training in Saintgits was adequate for me to use and adapt to modern tools and technology
```{r}
#dftr=filter(df,Current.employment.stream=="Non engineering business")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(df$Training.in.Saintgits.was.adequate.for.me.to.use.and.adapt.to.modern.tools.and.technology)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(df, Current.Teaching.Stream) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.was.adequate.for.me.to.use.and.adapt.to.modern.tools.and.technology
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.was.adequate.for.me.to.use.and.adapt.to.modern.tools.and.technology
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(FSa)
```

## q9- TrainTraining in Saintgits equipped me to conduct independent research in my area of interest
```{r}
#dftr=filter(df,Current.employment.stream=="Non engineering business")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(df$Training.in.Saintgits.equipped.me.to.conduct.independent.research.in.my.area.of.interest.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(df, Current.Teaching.Stream) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.equipped.me.to.conduct.independent.research.in.my.area.of.interest.
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.equipped.me.to.conduct.independent.research.in.my.area.of.interest.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(FSa)
```
## Affective domain details

##Q1- Do you clearly recall courses that have been helpful to you in your career?

```{r}
#dftr=filter(df,Current.employment.stream=="Non engineering business")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(df$Do.you.clearly.recall.courses.that.have.not.been.helpful.to.you.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(df, Current.Teaching.Stream) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.equipped.me.to.conduct.independent.research.in.my.area.of.interest.
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.equipped.me.to.conduct.independent.research.in.my.area.of.interest.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(FSa)
```

## Analysis of Common Questions

#Q1-Training in Saintgits helped me effectively communicate with technical community andsociety.
```{r}
#dfn=filter(df4,Current.employment.stream=="Non engineering business")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(df$Training.in.Saintgits.helped.me.effectively.communicate.with.technical.community.and..society.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(df, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.helped.me.effectively.communicate.with.technical.community.and..society.
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.helped.me.effectively.communicate.with.technical.community.and..society.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable
```

## Q2- Effectiveness of Mathematics

```{r}
#dfn=filter(df4,Current.employment.stream=="Non engineering business")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(df$Training.at.Saintgits.in.different.branches..Mathematics...has.given.awareness.about.various.methods.for.mathematical.solutions.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(df, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.at.Saintgits.in.different.branches..Mathematics...has.given.awareness.about.various.methods.for.mathematical.solutions.
, na.rm = TRUE),
    sd = sd(Training.at.Saintgits.in.different.branches..Mathematics...has.given.awareness.about.various.methods.for.mathematical.solutions.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable
```

##Q3- Mathematics and analytical skills

```{r}
#dfn=filter(df4,Current.employment.stream=="Non engineering business")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(df$Training.in.Mathematics.at.Saintgits.has..improved.my.analytical.skills)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(df, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Mathematics.at.Saintgits.has..improved.my.analytical.skills
, na.rm = TRUE),
    sd = sd(Training.in.Mathematics.at.Saintgits.has..improved.my.analytical.skills
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable
```

##Q3- Effectiveness of Physics and Chemistry

```{r}
#dfn=filter(df4,Current.employment.stream=="Non engineering business")
library(plotly)
#df2=filter(df,Gender=="Female")

Temp=table(df$Training.in.Physics.and.Chemistry.at.Saintgits.supplement.the.basic.understanding..of.the..Engineering.concepts)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r}
FSa=group_by(df, After.graduating.from.Saintgits..have.you.ever.been.employed.in.a.vocation.related.to.your.core.course.) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Physics.and.Chemistry.at.Saintgits.supplement.the.basic.understanding..of.the..Engineering.concepts
, na.rm = TRUE),
    sd = sd(Training.in.Physics.and.Chemistry.at.Saintgits.supplement.the.basic.understanding..of.the..Engineering.concepts
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable
```
## Analysis of basic qualities obtained through training at saintgits- Construction Engineering


1. Training in Saintgits helps you to set-out and plan the construction of structures

```{r}
dfc=filter(df,Current.employment.stream=="Construction Engineering")
library(plotly)
#df2=filter(df,Gender=="Female")
dfc$Training.in.Saintgits.helps.you.to.set.out.and.plan.the.construction.of.structures[is.na(dfc$Training.in.Saintgits.helps.you.to.set.out.and.plan.the.construction.of.structures)]=0
Temp=table(dfc$Training.in.Saintgits.helps.you.to.set.out.and.plan.the.construction.of.structures)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```

```{r,echo=FALSE}
library(dplyr)
FSa=group_by(dfc, Gender) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.helps.you.to.set.out.and.plan.the.construction.of.structures
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.helps.you.to.set.out.and.plan.the.construction.of.structures
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```
2. Training in saintgits helps you to prepare a schedule and monitor construction work- Construction Engineering

```{r}
library(plotly)
dfc$Training.in.Saintgits.helps.you.to.prepare.a.schedule.and.monitor.construction.work.[is.na(dfc$Training.in.Saintgits.helps.you.to.prepare.a.schedule.and.monitor.construction.work.)]=0
Temp=table(dfc$Training.in.Saintgits.helps.you.to.prepare.a.schedule.and.monitor.construction.work.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=F)
fig
```
```{r,echo=FALSE}
library(dplyr)
FSa=group_by(dfc, Gender) %>%
  summarise(
    count = n(),
    mean = mean(Training.in.Saintgits.helps.you.to.prepare.a.schedule.and.monitor.construction.work.
, na.rm = TRUE),
    sd = sd(Training.in.Saintgits.helps.you.to.prepare.a.schedule.and.monitor.construction.work.
, na.rm = TRUE)
  )
#Fsa=cbind.data.frame(FSa)
kable(FSa) %>%
  kable_styling(bootstrap_options = c("striped", "hover"),full_width = F,position = "center")
#xtable::xtable(Fsa)
```


3. Training in Saintgits helps you to select materials for construction ensure quality control in construction.


```{r}
library(plotly)
df2=filter(dfc,Gender=="Female")
Temp=table(dfc$Training.in.Saintgits.helps.you.to.select.materials.for.construction.ensure.quality.control.in.construction.)
fig <- plot_ly(type='pie', labels=names(Temp), values=Temp,  textinfo='label+percent',
              
               insidetextorientation='radial',showlegend=T)
fig
```
