# A grammar of modelling for R

I find statistical modelling in R frustrating. The code never feels intuitive and I spend too much time struggling with boilerplate. As I've spent more time modelling with R packages and teaching other how to use R, I have become increasingly convinced that:

1. We need to treat models (ex: KNN with k = 5) and model families (ex: KNN) as separate conceptual objects. Consequently, models and model families should be represented by their own objects with distinct classes, and distinct methods acting on those classes.

2. We have spent little time thinking about the general elements of a statistical model, little time standardizing language and methods used to fit and probe statistical models.

This repository contains my notes on how I think we should think about models (a grammar of modelling), and a proposal for a software interface in R based on this grammar.

The notes are compiled into a fairly short bookdown website [here](https://alexpghayes.github.io/modelling-in-r/). If you'd like to contribute,  you can use the edit button available at the top of each page.

Please let me know what you think in the [issues](https://github.com/alexpghayes/modelling-in-r/issues).

The issues contain my most recent thoughts since it takes a while for me to work everything into the main notes.
