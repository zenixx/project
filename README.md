# **Различен анализ на данни на учениците от 2 училища и провеждането на няколко различни теста за даден вид информация**
**Проект на Стенли Джалев, спец. Статистика по дисциплината "Статистическа Лаборатория"**

**Фми 2019г.**

## Задача
- Анализ на данните
- Проверка на състоянията но родителите
- Вероятното приемане на ученика - уни и спец.
- Проверка, чрез различни видове тестове и тяхната зависимост
  - chisq.test
  - shapiro.test
  - kruskal.test
- И по - сложният анализ simple/multiple linear regression

# Начални данни
Данните са във файл с разширение .csv и представляват 396 реда и 33 колони 
## Зареждане и проверка
Зареждаме файла по обичайния начин с **read.csv**


```
MC<-read.csv("student-mat.csv")
```
Преглеждаме го:

```
>summary(MC)
school   sex          age       address famsize   Pstatus      Medu            Fedu      
 GP:349   F:208   Min.   :15.0   R: 88   GT3:281   A: 41   Min.   :0.000   Min.   :0.000  
 MS: 46   M:187   1st Qu.:16.0   U:307   LE3:114   T:354   1st Qu.:2.000   1st Qu.:2.000  
                  Median :17.0                             Median :3.000   Median :2.000  
                  Mean   :16.7                             Mean   :2.749   Mean   :2.522  
                  3rd Qu.:18.0                             3rd Qu.:4.000   3rd Qu.:3.000  
                  Max.   :22.0                             Max.   :4.000   Max.   :4.000  
       Mjob           Fjob            reason      guardian     traveltime      studytime    
 at_home : 59   at_home : 20   course    :145   father: 90   Min.   :1.000   Min.   :1.000  
 health  : 34   health  : 18   home      :109   mother:273   1st Qu.:1.000   1st Qu.:1.000  
 other   :141   other   :217   other     : 36   other : 32   Median :1.000   Median :2.000  
 services:103   services:111   reputation:105                Mean   :1.448   Mean   :2.035  
 teacher : 58   teacher : 29                                 3rd Qu.:2.000   3rd Qu.:2.000  
                                                             Max.   :4.000   Max.   :4.000  
    failures      schoolsup famsup     paid     activities nursery   higher    internet  romantic 
 Min.   :0.0000   no :344   no :153   no :214   no :194    no : 81   no : 20   no : 66   no :263  
 1st Qu.:0.0000   yes: 51   yes:242   yes:181   yes:201    yes:314   yes:375   yes:329   yes:132  
 Median :0.0000                                                                                   
 Mean   :0.3342                                                                                   
 3rd Qu.:0.0000                                                                                   
 Max.   :3.0000                                                                                   
     famrel         freetime         goout            Dalc            Walc           health     
 Min.   :1.000   Min.   :1.000   Min.   :1.000   Min.   :1.000   Min.   :1.000   Min.   :1.000  
 1st Qu.:4.000   1st Qu.:3.000   1st Qu.:2.000   1st Qu.:1.000   1st Qu.:1.000   1st Qu.:3.000  
 Median :4.000   Median :3.000   Median :3.000   Median :1.000   Median :2.000   Median :4.000  
 Mean   :3.944   Mean   :3.235   Mean   :3.109   Mean   :1.481   Mean   :2.291   Mean   :3.554  
 3rd Qu.:5.000   3rd Qu.:4.000   3rd Qu.:4.000   3rd Qu.:2.000   3rd Qu.:3.000   3rd Qu.:5.000  
 Max.   :5.000   Max.   :5.000   Max.   :5.000   Max.   :5.000   Max.   :5.000   Max.   :5.000  
    absences            G1              G2              G3       
 Min.   : 0.000   Min.   : 3.00   Min.   : 0.00   Min.   : 0.00  
 1st Qu.: 0.000   1st Qu.: 8.00   1st Qu.: 9.00   1st Qu.: 8.00  
 Median : 4.000   Median :11.00   Median :11.00   Median :11.00  
 Mean   : 5.709   Mean   :10.91   Mean   :10.71   Mean   :10.42  
 3rd Qu.: 8.000   3rd Qu.:13.00   3rd Qu.:13.00   3rd Qu.:14.00  
 Max.   :75.000   Max.   :19.00   Max.   :19.00   Max.   :20.00
```
Започваме програмата с проверката на средната възраст на учениците и дали има някой outliner-и, ученици който може би са повтаряли

Правим проверката и построяване boxplot 
