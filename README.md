# **Анализ на данни на учениците от 2 училища и провеждането на няколко различни теста за даден вид информация**
**Проект на Стенли Джалев, спец. Статистика по дисциплината "Статистическа Лаборатория"**

**Фми 2019г.**

## Задача
- Анализ на данните
- Проверка, чрез различни видове тестове и тяхната зависимост
  - chisq.test
  - shapiro.test
  - kruskal.test
  - и др.
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
## Започваме програмата с проверката на средната възраст на учениците и дали има някой outliner-и - ученици който може би са повтаряли

Построяваме хистограма за възрастта, проверяваме колко е средната и я закръгляме

```
[1] 16.6962
> round(mean(age),digits = 0)
[1] 17
```

Правим проверката за outliner-ите като построим boxplot

![alt text](https://github.com/zenixx/project/blob/master/Rplot.png)

## Правим анализ на пропуснатите часове

Първо ще пуснем Q-нормално разпределение за дадения вектор
![alt text](https://github.com/zenixx/project/blob/master/Rplot01.png)

И също така shapiro.test
```
> 	Shapiro-Wilk normality test

data:  MC[, "absences"]
W = 0.66683, p-value < 2.2e-16
```
Ако W = 1 то горната и долната част твърдят едно и също нещо
ако е <1 както е в нашият случай то може да наблюдаваме голяма разлика от нормалното разпределение и също така
p-value е доста по - малко от 0.05 затова ще приемем нулевата хипотеза.

## Ще направим cor(сравнение) на трите оценки за първия и втория срок и фаналната оценка. Като така ще видим дали има някаква промяна при учениците. Също по - късно ще проверим няколко величини които биха влияли на финалната оценка, но първо .....

```
           G2
[1,] 0.8521181

            G3
[1,] 0.8014679

           G3
[1,] 0.904868
```
## Следващият тест е chisq.test между това дали има разлика времето което учиш и броя невзети изпити:
Първо проверявам дали има някаква зависимост защото ако е отрицателно това значи, че всичко е точно и учениците които учат повече, имат по - високи оценки
```
      failures
[1,] -0.173563
```
Правим табличка за дадената извадка от данни за да я изследваме
```
      0   1   2   3
  1  74  16   6   9
  2 158  26   7   7
  3  54   7   4   0
  4  26   1   0   0
```
Проверяваме chisq.test
```
	Pearson's Chi-squared test

data:  (table(MC$studytime, MC$failures))
X-squared = 16.212, df = 9, p-value = 0.06258
```
Виждаме, че p-value е близо до 0.05 което би трябвало да следва, че ще приемем нулевата хипотеза и това ще значи, че двете променливи не са зависими от което ще приемем, че наистина учениците, които учат повече ще имат по - малко не взети изпити от тези, които учат по - малко

## Ще видим дали има някаква връзка между времето прекарано в интернет и дали си в "romantic" връзка
Започваме като проверяваме дали данните са ни в numeric value
```
[1] FALSE
```

Сменяме стойностите от binary на numeric и проверяваме пак дали са numeric
```
> as.numeric(MC$internet)
is.numeric(MC$internet)
[1] FALSE
```
Виждаме, че не не се променят стойностите на дадения ред, затова ще направим следното:
```
> MC$internet=as.numeric(MC$internet)
```
Проверяваме пак
```
is.numeric(MC$internet)
[1] TRUE
```
И правим същото за romantic .....

**След това продължаваме с проверката с кой използва интернет и е в романтична връзка
Първо въвеждаме променлива x1 за всички, които не използват интернет и проверяваме дали са във връзка или не**
![alt text](https://github.com/zenixx/project/blob/master/Rplot02.png)

И същото за променливата x2

![alt text](https://github.com/zenixx/project/blob/master/Rplot03.png)

Дали са нормално разпределени

![alt text](https://github.com/zenixx/project/blob/master/Rplot04.png)

## И ще завършим с 2 регресии за това дали годините дали влиаят на отсъствията, времето прекарано навън и свободното време и втората за финалната оценка какви фактори са най - вероятни да влиаят на нея.

Започваме с първата и правим summary на данните от нея.
```
Call:
lm(formula = MC[, "G3"] ~ MC[, "absences"] + MC[, "goout"] + 
    MC[, "freetime"])

Residuals:
     Min       1Q   Median       3Q      Max 
-11.7804  -1.9287   0.2432   3.1915   9.2616 

Coefficients:
                 Estimate Std. Error t value Pr(>|t|)    
(Intercept)      11.35762    0.91361  12.432  < 2e-16 ***
MC[, "absences"]  0.02533    0.02873   0.881  0.37864    
MC[, "goout"]    -0.62129    0.21514  -2.888  0.00409 ** 
MC[, "freetime"]  0.26101    0.23995   1.088  0.27736    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 4.548 on 391 degrees of freedom
Multiple R-squared:  0.02221,	Adjusted R-squared:  0.0147 
F-statistic:  2.96 on 3 and 391 DF,  p-value: 0.03219
```
Голямо t-value индикира това, че е по - малко вероятно коефициента да не е равно на 0

След това даваме някакво име на summary-то и взимаме residuals му правим няколко изследвания
```
> model <- summary(lm(MC[,"age"]~MC[,"absences"] + MC[,"goout"] + MC[,"freetime"]))
> r <- residuals(model)
```
Правим нормално разпределение

![alt text](https://github.com/zenixx/project/blob/master/Rplot05.png)

И shapiro.test

```
> 	Shapiro-Wilk normality test

data:  r
W = 0.96333, p-value = 2.232e-08
```
