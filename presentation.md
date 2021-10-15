---
theme: gaia
_class: lead

paginate: true
backgroundColor: #fff
marp: true
backgroundImage: url('https://marp.app/assets/hero-background.jpg')
---

# **Coding and Climate Research**

**An introduction to coding and how it is used at InfluenceMap**

---

## Introduction

Wikipedia describes coding as:

"The process of designing and building an executable computer program to accomplish a specific computing result or to perform a specific task."

However, the best way I have heard coding described is as "The language your computer speaks" and hence the language you have to speak if you want to best communicate with a computer. 

---
## What makes code so powerful 


There are some features inherent to coding that make it very powerful:

* Code is reusable 
* Code is easily replicable
* Tasks written in code can be easily automated
* Code is incredibly flexible 
* As we rely more and more on the digital world, code gets more and more useful

---

## Python and server-side languages

* Python is one of the worlds most popular coding languages, it runs on all the major operating systems, has a relatively intuitive syntax for a coding language and has a huge amount of online support. 

* This is the language we will be working through later.

* Python is server-side language, meaning you download it and run it on your computer.

* Due to it's ease of use, Python is often used to control other programming tools written in other languages. In this sense Python is sometimes described as a glue that holds more complex applications together.

---
## Python Uses

#### 1) Data Analysis

* At InfluenceMap we use Python for all of our data analysis.

* Python's syntax makes it easier to understand and check code.

* There are a lot of Python packages that can be easily installed and used for data analysis, such as Pandas and Numpy.

* Python makes it easier to work with external databases.

---
## Python Uses

#### 2) Data Viz

* At InfluenceMap we use Python to create most our data visualizations.

* There are a ton of packages in Python for making interactive graphs.

* We like to use [Plotly](https://plotly.com/python/), which is a free package that allows you to easily make interactive graphs packed full of features.

* We have also developed our [own modules](https://git.influencemap.org/JakeCarbone/IM-Plotly-Functions) that contain code for styling and preparing Plotly graphs.

---
## Python Uses

#### 3) Web scrapping

* Python is a great language for scraping. There are a whole host of Python packages out there to make scraping easier. Scrappers written in Python give InfluenceMap access to data we would overwise be unable to access.  

* For example, we use Python to scrape government regulatory disclosure sites. The data from these scrappers can then be searched to find comments from companies and trade associations on policies. 

---

## Python Uses

#### 4) Websites

* At a high level, a website is run by some code on a server that will get requests from users, pull the relevant data from a database and send back a response.  

* Python's gluey nature makes it a great choice for making websites.

* There are a number of Python modules that provide frameworks for building websites, like [Django](https://www.djangoproject.com/), [Flask](https://flask.palletsprojects.com/en/2.0.x/) and [Pelican](https://blog.getpelican.com/). InfluenceMap's website is built in Python.

---
## Important information when learning to code

* The logic behind coding can seem a little alien at first but don't let that deter you. It will come with time and once you are comfortable with one language it will really help you learn other languages.

* Coding is like 5% writing code and 95% trying to figure out why your code isn't working. Again don't let that deter you.

* Getting set up and installing Python can be a surprisingly difficult task. So don't feel put off if that takes you a while.


---
## Before we start the tutorials

# **Any Questions?**

--- 

## Tutorials

Now lets start the tutorials:
1. [Intro_to_Python_tutorial](Intro_to_Python_tutorial.ipynb) - This is the first tutorial and goes over the basics of using Python.

2. [Python_in_use](Python_in_use.ipynb) - This is the second tutorial and walks you through using Python to scrape and analyse information from the web. 


---

## Other resources

* [This Free interactive Python Scrimba course](https://scrimba.com/learn/python) - This is a free introductory Python course from Scrimba.

* [First Python Notebook course](https://www.firstpythonnotebook.org/) - This is a free tutorial developed for journalists and makes an excellent next step following the completion of our tutorial.

* Google/alternative search engine - When coding it is impossible to remember or know every function. Hence, it is common for coders to frequently be searching for the code functions they want to use. Modules will often come with detailed tutorials and documentation to help you understand how to use their code.
