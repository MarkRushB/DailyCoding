# INFO6105-Data Science 

---
## Content <!-- omit in toc -->
- [INFO6105-Data Science](#info6105-data-science)
  - [Introduction](#introduction)
  - [1st Week](#1st-week)
    - [Anaconda](#anaconda)
    - [R](#r)
    - [Machine Learning(ML)](#machine-learningml)
    - [Data Distributed](#data-distributed)
    - [Statistical Analysis](#statistical-analysis)
  - [2nd Week](#2nd-week)
    - [Basic Python skills](#basic-python-skills)
    - [Create WordCloud By R](#create-wordcloud-by-r)
  - [3rd Week](#3rd-week)
    - [DataScience on Python](#datascience-on-python)
  - [4rd Week](#4rd-week)
    - [A gentle Introduction to Statistics](#a-gentle-introduction-to-statistics)
---
## Introduction
- **Professor**:Dino
- **Grading**: 
  - **30%** homework
  - **30%** mid-term
  - **30%** final projext
  - **10%** final exam
- **Tools**:  
    1. Python
    2. R
    3. Anaconda
---
## 1st Week
### Anaconda
  - create new conda environments  
    ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200107200127.png)
### R
  - What is R?
    - R is a free software environment for statistical computing, data mining, and graphics
      - Hopefully replace Matlab, the crack-cocaine of scientists- engineers-turned-programmers
    - R is one of the major languages for data science
      - It provides excellent visualization features, which is essential to explore the data before submitting it to any automated learning, as well as assessing the results of the learning algorithm
      - Many R packages for machine learning are available off the shelf and many modern methods in statistical learning are implemented in R as part of their development
    - We use statistical analysis for:
      - **inference - making conclusions based on data**
      - **prediction - what will happen when I observe new data?**
      - **we create models to do both of those things**
  - R **=** or **<-** ?
    - In R, both statements x = 3 and x <- 3 have the effect of assigning the value 3 to the variable x. So if they have the same effect, does it matter which you use?
    - When R (and S before it) was first created, **<- was the only choice for the assignment operator**. Old AT&T keyboards had arrow as a key!
    - But R uses = for yet another purpose: associating function arguments with values (as in pnorm(1, sd=2), **to set sd to 2**)
    - To make things easier for new users, R added the capability in 2001 to also allow = be used as an assignment operator, on the basis that the intent (assignment or association) is usually clear by context. So, **x=3** clearly means "**assign 3 to x**", whereas **f(x = 3**)clearly means "**call function f, setting the argument x to 3**".
    - There is one case where ambiguity might occur: if you wanted to assign a variable during a function call. The only way to do this in modern versions of R is: **f(x <- 3)** which means "assign 3 to x, and **call f with the first argument set to the value 3**
### Machine Learning(ML)
  - **Overview**
    -  In **supervised learning** (SL), the learning algorithm is presented with labelled example inputs, where the labels indicate the desired output
       -  SML itself is composed of **classification**, where the output is categorical, and **regression**, where the output is numerical
    -  In **unsupervised learning** (UL), no labels are provided, and the learning algorithm focuses solely on detecting structure in unlabeled input data
    -  Note that there are also **semi-supervised learning** approaches that use labelled data to inform unsupervised learning on the unlabeled data to identify and annotate new classes in the dataset (also called novelty detection)
    -  **Reinforcement learning** (RL), the learning algorithm performs a task using feedback from operating in a real or synthetic environment, with rewards over actions
 - **Supervised Learning**
   - Supervised Learning algorithm learns from a known data- set(Training Data) which has labels to make predictions
   - Classification:
     - Classification determines to which set of categories does a new observation belongs i.e. a classification algorithm learns all the features and labels of the training data and when new data is given to it, it has to assign labels to the new observations depending on what it has learned from the training data
   - Regression:
     - Regression is a supervised learning algorithm which helps in determining how does one variable influence another variable
 - **Unsupervised Learning**
   - Unsupervised learning algorithm draws inferences from data which does not have labels
   - ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200108101337.png)
   - In this example, the set of observations is divided into two clusters. Clustering is done on the basis of similarity between the observations. There is a high intra-cluster similarity and low inter-cluster similarity i.e. there is a very high similarity between all the buses but low similarity between the buses and cars.
 - **Reinforcement Learning**
   - Reinforcement Learning is a type of machine learning algorithm where the machine/agent in an environment learns ideal behavior in order to maximize its performance. Simple reward feedback is required for the agent to learn its behavior, this is known as the reinforcement signal
   - ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200108104837.png)
### Data Distributed
  - How data is distributed is the foundation of statistics
  - **Normal distribution (rnorm)**
    - ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200108105612.png)
    - ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200108105642.png)
  - **Central Limit Theorem**
    - In probability theory, the central limit theorem (CLT) states that, given certain conditions, the arithmetic mean of a sufficiently large number of iterates of independent random variables, each with a well-defined expected value and well- defined variance, will be approximately normally distributed, **regardless of the underlying distribution**
    - [梅花机/quincunx](https://www.mathsisfun.com/data/quincunx.html)
  - **Correlation**
    - In statistics, dependence is any statistical relationship between two random variables or two sets of data
      - Correlation refers to any of a broad class of statistical relationships involving dependence
      - Familiar examples of dependent phenomena include the correlation between the demand for a product and its price
      - Correlations are useful because they can indicate a predictive relationship that can be exploited
    - Statistical learning, or learning-by-experience, is all about correlation
      - When I see my mum, I get good food
      - If I climb high on a tree, I can fall and hurt myself
      - When the girl winks at me, it means she likes me (**你这怕是想的有点多，小孩名儿都想好了**)
  - **Linear relationships**
    - The Pearson correlation coefficient indicates the strength of a linear relationship between two variables
      - The Pearson correlation coefficient, which is sensitive only to a linear relationship between two variables
      - If we have a series of n measurements of X and Y written as xi and yi where i = 1, 2, ..., n
      - ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200108110836.png)
      - where x and y are the sample means of X and Y, and sx and sy are the sample standard deviations of X and Y
  - **Causation**
    - Correlation does not imply causation!
    - A correlation between age and height in children is fairly causally transparent (age → height), but a correlation between mood and health in people is less so
      - ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200108111042.png)
### Statistical Analysis
  - We use statistical analysis for:  
    - Inference - making conclusions based on data
    - Prediction - what will happen when I observe new data?
    - And we create models to do both of those things
  - **Variables**
    - The decision as to which variable in a data set is modeled as the **dependent variable** and which are modeled as the **independent variables** may be based on a presumption that the value of one of the variables is *caused by*, or *directly influenced by* the other variables
    - Independent variables are also called regressors, exogenous variables, explanatory variables, covariates, input variables, and predictor variables
  - **Linear Regression**
    - Regression is an approach for modeling the relationship between a scalar dependent variable y and one or more explanatory variable (independent variable) x
      - In linear regression, data are modeled using linear predictor functions
      - Linear regression models are often fitted using the least squares approach, but they may also be fitted in other ways..
      - ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200108133631.png)
  - **Logistic Regression**
    - ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200108133731.png)
    - ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200108133800.png)
  - **Regression Example**
    - A good grade in this class (the dependent variable) is directly influenced by how many hours per week (the predictor or independent variable) you review the slides and do the homework
      - For example:  
        - 1or less hours per week→F 
        - 2 – 3 hours per week→C
        - 4 – 5 hours per week→B
        - 8 – 9 hours per week→A
## 2nd Week
### Basic Python skills
- [part-1](./DataScience%20Reference/Introduction-to-Python-sp20-start.ipynb)
- [part-2](./DataScience%20Reference/Introduction-to-Python-sp20-part2-start.ipynb)
### Create WordCloud By R
- [Data processing](./DataScience%20Reference/4-textmining-cloud.R)
- [NLP-WordCloud](./DataScience%20Reference/5-nlp.R)
  - **Attention**
    1. At present, I think Python is better than R in dataScience area. So this class is just a introduce for us, if I wanna do some works like WordCloud, Pyhton is a good choice
    2. For Chinese WordCloud, Library `UDpipe` the Professor Dino Provided is bad at processing Chinese sentences, so there is another library named `JiebaR` (still aviliable on Python)
- JiebaR-WordCloud
  - - I used two R packages in this Chinese Word Cloud processing
  - `jiebaR`
    - this is a Chinese Text segmentation,keyword extraction and speech tagging for R
    - **Author**: Qin Wenfeng, Wu Yanyi
  - `wordcloud2`
  - Coding in R
      ```r
      ## At the first, We need install these packages as below
      install.packages("jiebaR")
      install.packages("wordcloud2")

      ## use jiebaR library
      library(jiebaR)

      ## read the .txt format document
      text<-readLines("/Users/sichenzhao/Mark/NortheasternUniversity/INFO6150-DataScience/Assignment/Assignment1/Part2-WordCloud/ChineseWordCloud/2019GovernmentReport.txt")

      ## devide paragraphs into phrases
      seg<-worker()
      segment(text,seg)

      ## make part of speech judgement(I think this step could be omitted)
      seg2<-worker("tag")
      segment(text,seg2)

      ## extract the key words
      seg3<-worker(type="keywords",topn=3)
      seg3<=text
      ## or
      for(i in text){a<-seg3<=i;print(a)}  

      ## stopWords
      text1<-gsub('[a-zA-Z]','',text)
      text2<-gsub("[的|和|了|来|与|到|由|等|从|以|一|为|在|上|各|去|对|侧|多|并|千|万|年|更|向|这是]","",text1)

      ## devide into phrases again
      seg<-worker()
      seg<=text2

      ## build the word frequency list
      freq<-freq(segment(text2,seg))

      ## sort the word frequency list(descending order)
      index <- order(-freq[,2])
      order2<-freq[index, ]

      ## use wordcloud2 library
      library(wordcloud2)

      ## make word cloud(set attributes according to your own preferences)
      wordcloud2(order2,size = 1,minRotation = -1, maxRotation = 1,rotateRatio = 0.8,fontFamily = "微软雅黑", color = "random-light",fontWeight = "bold",shape = "triangle")
      ```
## 3rd Week
### DataScience on Python
- [numpy](./DataScience%20Reference/introduction-to-numpy-fa20-start.ipynb)
- [pandas](./DataScience%20Reference/introduction-to-pandas-sp20-start.ipynb)
## 4rd Week
- [fibonacci](./DataScience%20Reference/predicting-fibonacci-start.ipynb)
- [probability1](./DataScience%20Reference/probability1-sp20-start.ipynb)
- [birthdays](./DataScience%20Reference/probability1-birthdays.ipynb)
### A gentle Introduction to Statistics
- mean
- median
- mode
- boxplot
- Covariance
  - Covariance is a measure that indicates how two variables are related from a linear perspective.
  - A positive covariance means the variables are positively linearly related, while a negative covariance means the variables are inversely linearly related.
  - If two variables are positively correlated, increasing one will increase also the other.I
  - f two variables or negatively correlated, decreasing the value of one will make the other increase in value.
  - With this measure, we can determine whether units increase or decrease together, but it is impossible to measure the degree with which the variables move together because covariance does not use one standard unit of measurement.
- Correlation
  - Correlation is a unit of measure that standardizes the measure of linear interdependence between two variables and, consequently, tells us how closely the two variables move.
  - The correlation measurement, called a correlation coefficient, will always take on a value between 1 and -1.
  - If the correlation coefficient is equal to 1, the two variables are in perfect positive linear correlation (if one increases, the other variable increases by the same amount).
  - If the correlation coefficient is equal to -1, the two variables are in perfect negative linear correlation (if one variable decreases, the other decreases by the same amount).
  - If the correlation coefficient is equal to 0, there is no linear correlation between the two variables (if one variable changes value, that doesn't give us any information about if the other variable is going to change as well or not).
  - The correlation value is the same regardeless of the unit system we are working with.
- Python中*args和**kwargs的区别
  - *args 用来将参数打包成tuple给函数体调用
  ```python
  def args_test(x, y, *args):
  print(x, y, args)
  args_test(1,2,3,4,5)
  1 2 (3, 4, 5)
  ```
  - **kwargs 打包关键字参数成dict给函数体调用
  ```python
  def kwargs_test(**kwargs):
  print(kwargs)
  kwargs_test(a=1, b=2, c=3)
  {'a': 1, 'c': 3, 'b': 2}
  ```
  - 参数arg、*args、**kwargs三个参数的位置必须是一定的。必须是(arg,*args,**kwargs)这个顺序，否则程序会报错
  ```python
  def param_test(arg, *args, **kwargs):
  print(arg, args, kwargs)
  param_test(1, 3, 5, a=6, b=9)
  1 (3, 5) {'b': 9, 'a': 6}
  ```