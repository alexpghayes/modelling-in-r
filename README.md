# A grammar of modelling for R

I find statistical modelling in R frustrating. The code never feels intuitive and I spend too much time struggling with boilerplate.

As I've spent more time modelling with R packages and teaching other how to use R, I have become increasingly convinced that:

1. We need to treat models (ex: KNN with k = 5) and model families (ex: KNN) as separate conceptual objects. Consequently, models and model families should be represented by their own objects with distinct classes, and distinct methods acting on those classes.

2. We need to conceptually describing the general elements of a statistical model, and standardizing language and methods used to fit and probe statistical models. The result should be a grammar that is both intuitive to users and a target interface for researchers developing new methods.

This repo is a work in progress, describing how we should think about models, and how we might map those ideas to R objects and methods. The notes are [available here](https://alexpghayes.github.io/modelling-in-r/). 

**Feedback and comments encourage** in the [issues](https://github.com/alexpghayes/modelling-in-r/issues).
