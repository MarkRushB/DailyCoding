# Data Science
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
    - There is one case where ambiguity might occur: if you wanted to assign a variable during a function call. The only way to do this in modern versions of R is: **f(x <- 3)** which means "assign 3 to x, and call f with the first argument set to the value 3


