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