---
title: "Feedback about Luigi Pipeline"
date: 2023-03-02T21:08:15+01:00
excerpt_separator: "<!--more-->"
categories:
  - blog
tags:
  - Luigi Pipeline
  - Feedback
---

# Introduction

In data science project, normally data scientists would use jupyter notebooks for developing Machine Learning
models. A good thing about jupyter notebooks is the interactiveness with end user. No matter which Machine Learning 
library you use, scikit-learn, tensorflow or pytorch, with jupyter notebook you can develop your model step by step
and check the intermediate outputs at every time. 

However, when the DS project is in production environment, the ML model training pipeline should be already stable 
and the interactiveness of jupyter notebook becomes less and less important. Should we then stop using jupyter notebooks
at this point? What's the alternative solution? I think many people would ask these two questions. And the answer is quite simple,
use the ML pipeline library to build your ML model training pipeline. 

# Machine Learning Pipeline
Machine Learning Pipeline is close to the idea of converting all the python code from jupyter notebooks to python scripts.
Instead of writing your own python scripts for training ML model, with ML pipeline library, such as 
[Luigi](https://github.com/spotify/luigi), [Apache Airflow](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjv18jvn779AhWAYPEDHZhUCDYQFnoECAsQAQ&url=https%3A%2F%2Fairflow.apache.org%2F&usg=AOvVaw388kHRAT6nV9AH5zbkuDlv)
,
[Kedro](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwi--JP7n779AhXtSfEDHTuzD-8QFnoECAkQAQ&url=https%3A%2F%2Fkedro.org%2F&usg=AOvVaw0JWG6cOFLtdy93D5ohUxle), you can quickly convert your python code in jupyter notebooks to a modern popular ML model training pipeline. Train 
a model becomes the most easiest thing to do, just through one terminal command, the training will be started. What you need
to do is just wait, wait until the pipeline finishes and get the outputs you need. 

# Why Luigi
I have used luigi in 2019. Back then, I just had 2 years of working experience and still counted as a newbie.
I continued developed a luigi pipeline. It's a luigi pipeline runs periodically. This time, I first thought about luigi.
Since there are other colleagues from work are also using luigi pipeline. I decide to use it again.

# Upside about Luigi
Luigi pipeline is very easy to learn and use. For each pipeline step what you need to do is to define the required inputs,
outs and a run function. If you have a very complicated logic about your training pipeline, such as chaining small pipeline
steps to a relative big pipeline steps or creating new pipeline steps at running, don't worry, it's very easy to implement 
it with luigi. 

# A tip about using Luigi
I have seen many people built a very big luigi pipeline, containing a lot of folders, sub-folders for source code. At the 
end, even with luigi, the whole pipeline structure is so bloated, such that developer gets lost in the code very easily.
And the maintenance of such huge pipeline is a nightmare. 

An easy way to keep your pipeline structure as simple as possible, is to build a python package containing all the core
functions, classes. What you have to do with your luigi pipeline is just import those functions or classes and use them
in the run function of each pipeline step. By doing so, your luigi pipeline structure will be just a folder containing each
pipeline step as a independent python file. The maintenance work for a python package and a simple luigi pipeline becomes 
much easier than for a huge complicated luigi pipeline. 

# Downside about luigi
Personally I like to write tests as many as possible to make sure the stability of my code. And tests makes debug easy
as well. But when I start to write tests for my luigi pipeline, I start to have headaches. There isn't many tutorials 
out there for write tests for luigi pipeline. And the library luigi itself doesn't provide any testing functions. 

I spent a lot of time on writing tests for luigi pipeline. Mock from unittest package, doesn't help that much. What I figured 
out at the end, is to prepare a testing data for my luigi pipeline. I run the pipeline with it to have the outputs from
all the pipeline steps. Then I zipped the outputs for each pipeline step and put them in the _resources_ folder of _tests_ folder.
To test each pipeline step, I just need to prepare the required inputs for it and execute the pipeline for the particular step.
Then check the outputs of the pipeline step. With this approach, I'm finally able to test my training pipeline. However,
I ended up with having too many binary files in the _resources_ folder. For now, I can accept it. 

# Conclusion
Luigi pipeline is definitely an awesome ML pipeline library, it's worth you to try at least. Just bear in mind that testing
your pipeline is not an easy job, but also not impossible. 

For the next step, I will try with - [Apache Airflow](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjv18jvn779AhWAYPEDHZhUCDYQFnoECAsQAQ&url=https%3A%2F%2Fairflow.apache.org%2F&usg=AOvVaw388kHRAT6nV9AH5zbkuDlv)
, and do a comparison with [Luigi](https://github.com/spotify/luigi).  

# References
- [Luigi](https://github.com/spotify/luigi)
- [Apache Airflow](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjv18jvn779AhWAYPEDHZhUCDYQFnoECAsQAQ&url=https%3A%2F%2Fairflow.apache.org%2F&usg=AOvVaw388kHRAT6nV9AH5zbkuDlv)
- [Kedro](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwi--JP7n779AhXtSfEDHTuzD-8QFnoECAkQAQ&url=https%3A%2F%2Fkedro.org%2F&usg=AOvVaw0JWG6cOFLtdy93D5ohUxle)