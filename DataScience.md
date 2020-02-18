# Data Science 

- [Data Science](#data-science)
  - [Anaconda](#anaconda)
  - [R](#r)
  - [Machine Learning(ML)](#machine-learningml)
  - [Data Distributed](#data-distributed)
  - [Statistical Analysis](#statistical-analysis)
  - [Basic Python skills](#basic-python-skills)
  - [Create WordCloud By R](#create-wordcloud-by-r)
  - [DataScience on Python](#datascience-on-python)
  - [A gentle Introduction to Statistics](#a-gentle-introduction-to-statistics)
  - [Classical Statistical Data Analysis with analytic distributions](#classical-statistical-data-analysis-with-analytic-distributions)
    - [The Uniform distribution](#the-uniform-distribution)
  - [Statistical hypothesis testing](#statistical-hypothesis-testing)
  - [Random variates from arbitrary distribution](#random-variates-from-arbitrary-distribution)
    - [Inverse Transform Method](#inverse-transform-method)
    - [Acceptance-Rejection Method](#acceptance-rejection-method)
  - [Markov Chain (MC)](#markov-chain-mc)
    - [Reference of Hidden Markov Chain](#reference-of-hidden-markov-chain)
## Anaconda
  - create new conda environments  
    ![](https://markpersonal.oss-us-east-1.aliyuncs.com/pic/20200107200127.png)
## R
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
## Machine Learning(ML)
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
## Data Distributed
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
## Statistical Analysis
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
## Basic Python skills
- [part-1](./DataScience%20Reference/Introduction-to-Python-sp20-start.ipynb)
- [part-2](./DataScience%20Reference/Introduction-to-Python-sp20-part2-start.ipynb)
## Create WordCloud By R
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
## DataScience on Python
- [numpy](./DataScience%20Reference/introduction-to-numpy-fa20-start.ipynb)
- [pandas](./DataScience%20Reference/introduction-to-pandas-sp20-start.ipynb)
- Learn how to use Pyhton to predict the Stock
  - [WolfofWallStreet](./DataScience%20Reference/wolf-of-wall-street.ipynb)
- [fibonacci](./DataScience%20Reference/predicting-fibonacci-start.ipynb)
- [probability1](./DataScience%20Reference/probability1-sp20-start.ipynb)
- [birthdays](./DataScience%20Reference/probability1-birthdays.ipynb)
## A gentle Introduction to Statistics
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
## Classical Statistical Data Analysis with analytic distributions
- A **box (or whisker) plot** perfectly illustrates what we can do with basic statistical features of a random variable:

- The line in the middle is the median value of the data. Median is used over the mean
since it is more robust to outlier values. The first quartile is essentially the 25th
percentile; i.e 25% of the points in the data fall below that value. The third quartile is the 75th percentile; i.e 75% of the points in the data fall below that value. The min and max
values represent the upper and lower ends of our data range.

- When the box plot is short it implies that much of your data points are similar, since
there are many values in a small range

- When the box plot is tall it implies that much of your data points are quite different,
since the values are spread over a wide range

- If the median value is closer to the bottom then we know that most of the data has
lower values. If the median value is closer to the top then we know that most of the
data has higher values. Basically, if the median line is not in the middle of the box
then it is an indication of skewed data.

- Are the whiskers very long? That means your data has a high standard deviation
and variance i.e the values are spread out and highly varying. If you have long
whiskers on one side of the box but not the other, then your data may be highly
varying only in one direction.
The **histogram**, which we plotted here above below the whisker plot is super-important: It tells us *how* the data is distributed and gives us the opportunity to find the function that approximates our data: It needs to produce (fake) data that has the same histogram as the real data. The histogram is a **probability distribution**.

>**DEFINITION**: A **probability distribution** is a function which represents the probabilities of all possible values in an experiment

Notice how we went from *frequencies* (of the value of a random variable), which is what you get in a histogram, to *probabilities* of a random variable. Does this surprise you? Shouldn't, we used our `p` function to easily go from 2-child Danish B/G frequencies to probability distributions for BB, BG, GB, GG. Probabilities is nothing more than renormalized frequencies!

>**DEFINITION**: The **expected value** of a discrete random variable is the probability-weighted average of all its possible values. In other words, each possible value the random variable can assume is multiplied by its probability of occurring, and the resulting products are summed to produce the expected value. Intuitively, a random variable's expected value represents the mean of a large number of independent realizations of the random variable. The expected value is also known as the **expectation**, mathematical expectation, **mean**, or **first moment**.

Here are some classic well-known theoretical distributions whose histograms are very well-defined and which happen to model data that usually occurs on planet Earth (much like the Fibonacci numbers and the Golden ratio appear in nature):

- A [Bernoulli Distribution]() has only two possible outcomes and a single trial. A simple example can be a single toss of a biased/unbiased coin. In this example, the probability that the outcome might be heads can be considered equal to p and (1 - p) for tails (the probabilities of mutually exclusive events that encompass all possible outcomes needs to sum up to one).
<br>
- A [binomial Distribution]() can be thought as the sum of outcomes of an event following a Bernoulli distribution. The Binomial Distribution is therefore used in binary outcome events and the probability of success and failure is the same in all the successive trials. This distribution takes two parameters as inputs: the number of times on event takes place and the probability assigned to one of the two classes. The binomial distribution is frequently used to model the number of successes in a sample of size n drawn with replacement from a population of size N. If the sampling is carried out without replacement, the draws are not independent and so the resulting distribution is a hypergeometric distribution, not a binomial one. However, for N much larger than n, the binomial distribution remains a good approximatio. A simple example of a Binomial Distribution in action can be the toss of a biased/unbiased coin repeated a certain amount of times, or picking balls from an urn (with replacement) that contains balls of two different colors. The main characteristics of a Binomial Distribution are:
 - Given multiple trials, each of them is independent of each other (the outcome  of one trial doesn't affect another one).
 - Each trial can lead to just two possible results (eg. winning or losing), which have probabilities p and (1 - p).
<br>

- A [Uniform Distribution](https://en.wikipedia.org/wiki/Uniform_distribution_(continuous)) is the most basic of the seven we show here. It has a *single value* which only occurs in a certain range while anything outside that range is just 0. It’s very much an *on or off* distribution. We can also think of it as an indication of a
categorical variable with 2 categories: 0 or the value. Your categorical variable might
have multiple values other than 0 but we can still visualize it in the same was as a
piecewise function of multiple uniform distributions.
<br>

- A [Normal Distribution](https://en.wikipedia.org/wiki/Normal_distribution), commonly referred to as a **Gaussian Distribution**, is specifically defined by its **mean** and **standard deviation**. The mean value shifts the
distribution spatially (measure of *centrality*) and the standard deviation controls the *spread*. The import
distinction from other distributions (e.g Poisson) is that the standard deviation is the
*same in all directions*. Thus with a Gaussian distribution we know the average value
of our dataset as well as the spread of the data i.e is it spread over a wide range or is
it highly concentrated around a few values.
<br>

- A [Poisson Distribution](https://en.wikipedia.org/wiki/Poisson_distribution) is similar to the Normal but with an added factor of skewness. With a low value for the skewness a poisson distribution will have
relatively uniform spread in all directions just like the Normal. But when the
skewness value is high in magnitude then the spread of our data will be different in
one direction: It will be very *spread out* in one direction and in the other it will be
*highly concentrated*. Poisson Processes are used to model a series of discrete events in which we know the average time between the occurrence of different events but we don’t know exactly *when* each of these events might take place. For example, events that can be counted (e.g. emails or text messages). A process can be considered to belong to the class of Poisson Processes if it can meet the following criterias:
 - The events are independent of each other (if an event happens, this does not alter the probability that another event can take place).
 - Two events can’t take place simultaneously.
 - The average rate between events occurrence is constant.  
<br>

- A [Gamma Distribution](https://en.wikipedia.org/wiki/Gamma_distribution) is a distribution that arises naturally in processes for which the ***waiting times between events are relevant***. It can be thought of as a **waiting time** between **Poisson distributed events**, such as in when you wait for the "T" (i.e. queuing models), climatology, and financial services. Examples of events that may be modeled by a gamma distribution include but are not limited to:
 
   - Public transportation
   - Amount of [rainfall](http://journals.tubitak.gov.tr/engineering/issues/muh-00-24-6/muh-24-6-7-9909-13.pdf). accumulated in a reservoir
   - The size of loan defaults or aggregate [insurance claims](https://www.crcpress.com/Statistical-and-Probabilistic-Methods-in-Actuarial-Science/Boland/p/book/9781584886952)
   - The flow of items through manufacturing and distribution processes
   - Visitors to a website
   - Customers calling a help center 
   - Radioactive decay in atoms
   - Movements in a stock price


- A [Random walk](https://en.wikipedia.org/wiki/Random_walk) (or [Brownian motion](https://en.wikipedia.org/wiki/Brownian_motion)) can be any sequence of discrete steps (of always the same length) moving in **random directions**. Random Walks can take place in any type of dimensional space (eg. 1D, 2D, nD).
Specfically, a **Random Walk** is used to describe a *discrete-time process* while **Brownian Motion** can be used to describe a continuous-time random walk. Some examples of random walks applications are: tracing the path taken by molecules when moving through a gas during the diffusion process, sports events predictions etc…
<br>

There are more theoretical distributions we'll learn in class but these seven already give
us a lot of value. We can quickly see and interpret our categorical variables with a
Uniform Distribution. There are many algorithms that by default will perform well specifically 
with Gaussian so we should use Gaussian Distribution as much as possible. And the Poisson and the Gamma/Brown 
model a lot of natural processes, respectively for discrete and continuous events.

In our last notebook, when we examined a Celtics team, we saw how when we add a lot of 
distributions together we tend to get a nice gaussian distribution, and for good reason,
there is a great theorem behind this fact!
### The Uniform distribution
Simplest of all: same probability  𝑦  for all possible values of the random variable  𝑥 . Becomes apparent when we plot a lot of experiments:
## Statistical hypothesis testing
**Statistical hypothesis testing**, also called [confirmatory data analysis](https://en.wikipedia.org/wiki/Statistical_hypothesis_testing) is a framework for determining ***whether observed data deviates from what is expected***. 

A **hypothesis** is proposed for the statistical relationship between two data sets (or two data columns), and this is compared as an alternative to an idealized `null hypothesis` that proposes ***no relationship between them**. 

>**Hypothesis testing** is such a badly-taught subject in statistics that *few students clearly understand the theory behind it*, and just blindly call `SciPy` statistincal testing APIs. Bad preofessors! ***We*** have a secret weapon: we can *code* in python, run simulations, and *count*! Let's put our secret weapon to good use and learn *everything* about hypothesis testing.
## Random variates from arbitrary distribution
### Inverse Transform Method
- 首先生成一个均匀分布的随机数，再求指定分布的分布函数F(x)，然后求得F(x)的逆函数G(x)，将随机数带入逆函数得到的即为指定分布的随机数。
- [Reference from Wiki](https://en.wikipedia.org/wiki/Inverse_transform_sampling)
- ![](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/Inverse_transformation_method_for_exponential_distribution.jpg/800px-Inverse_transformation_method_for_exponential_distribution.jpg)
- Note that the image of the cumulative distribution function (CDF) 𝐹𝑋 is the interval [0,1] on the 𝑦 axis. (Purists will discuss whether the endpoints should be included or not.) Also note that the CDF is of course monotone.
- In inverse transform sampling, we sample uniformly from this image, i.e., 𝑈[0,1]. These are the dots on the 𝑦 axis. We then go right from these dots to the graph of 𝐹𝑋, then down to the 𝑥 axis. This is where the "inverse" comes in: because we start from the 𝑦 axis and end up on the 𝑥 axis.
- The result on the 𝑥 axis is distributed according to 𝐹𝑋.
  - Where 𝐹𝑋 is steep (i.e., the density 𝑓𝑋 is large), 𝑦 values that are close together yield 𝑥 values that are close together. We get a high density of 𝑥 values.
  - Where 𝐹𝑋 is flat (i.e., 𝑓𝑋 is small), 𝑦 values that are close together yield 𝑥 values that are farther apart. We get a low density of 𝑥 values.
### Acceptance-Rejection Method
- 首先生成一个均匀分布随机数a，设概率密度函数为f(x)，然后再生成一个均匀分布随机数b，若b<=f(a)，则成功得到指定分布的随机数，否则从头开始。
## Markov Chain (MC)
### Reference of Hidden Markov Chain
- [用骰子讲解隐马尔可夫链](https://www.cnblogs.com/fulcra/p/11065474.html)