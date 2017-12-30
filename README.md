# A grammar of modelling for R

These are some incomplete musings on what goes into a machine learning model and how to express machine learning concepts in code.

The notes are compiled into a fairly short bookdown website [here](https://alexpghayes.github.io/modelling-in-r/). If you'd like to contribute,  you can use the edit button available at the top of each page.

Please let me know what you think in the [issues](https://github.com/alexpghayes/modelling-in-r/issues).




# TODO

## OUTSIDE MiR

OPEN RSAMPLE ISSUE: request calibration specification object

FIGURE OUT HOW TO EXTRACT INDICES instead of tibbles from rset objects to make it more likely for researchers to use them in their own model implementations

recipes community standard for new packages. issue: need a model template/interface first

## INSIDE MIR

OPEN MiR ISSUE: think more carefully about why `mlr` is unsatisfying

OPEN MiR ISSUE: better language than calibration since calibration sounds like a single model vs selection across a family of models

OPEN MiR ISSUE: Language: "performance metric" vs "loss". how to specify that you should record multiple losses even if the hyperparameter selection only uses one of them

OPEN MiR ISSUE: what does it look like to work with models rather than model families? the whole point is models get fit from a helper basically and model families just provide wrapper code around fitting each model? think about penalized regressions and path fitting: the glmnet/cv.glmnet separation isn't clear: both are model families. discourage interacting with models through singleton model families, think about what the pros and cons are of this

OPEN MiR ISSUE: how to save performance results from hyperparameter search. should also be able to add this information easily for new hyperparameters and then compare if the calibration strategies are sufficiently compatible?

OPEN MiR ISSUE: should `add_design` just accept a recipe or also data? keep formula and matrix interfaces in mind

OPEN MiR ISSUE: better language for model and model_family instantiation

OPEN MiR ISSUE: how to handle ensembling

OPEN MiR ISSUE: what kinds of documentation to provide, what kinds of modelling recommendations

  - how to implement interface compliant models

OPEN MiR ISSUE: Pipelines

`Scikit-Learn` offers pipelines that allow you to do things like do PCA on data and then fit a GBM to that data, but with an arbitrary number of steps. I'm curious how this should work with the current direction of modelling in R, where `recipes` would presumably take care of the `PCA`. What if you want to train a model on the GBM predictions? Do you need a new `pipeline` object, or do you make a `step_gbm` for a recipe?

OPEN MiR ISSUE: Unsupervised transformations that are not maps

It's worth thinking about unsupervised techniques like `t-SNE` because you can `transform` data, but you can't ever `fit` a `t-SNE` object because (non-parametric) `t-SNE` doesn't define a mapping to a new space.

OPEN MiR ISSUE: DETAIL/IN THE WEEDS WISH LIST: I'd love to see a wrapper around PCA that easily lets the user specify a number of principal components to drop into `irlba` for high dimensional situations when computing the full PCA is overkill.

LONG TERM THING TO THINK ABOUT: making it easy to test the pipeline for models in production following the Google recommendations I recently came across on Twitter

TODO: OPEN MiR ISSUE: hyperparameter space and search ontology:

- random search variants: probability distributions
- grid search: fixed points
- GP/TPE: performance results at initial set of points in hp_space

sane conversion between grid, result_grid, and distribution:

dist to grid: specify how many points you want and then do latin hypersquares or quantiles throughout the probability distribution to get a representative seed for either grid search or GP/TPE methods

grid to dist: infer points and through on some standard log space distributions

grid has option result info

TODO: OPEN MiR ISSUE: `calibration` object should specify *what* you need to do the model comparison, not *how* to do it (that's where `tidyposterior` comes in). think about compatible types of calibration

- min RMSE, or min RMSE within 1-SE following Breiman

WISHLIST: easy parallelization for people who don't understand parallelization

ISSUE: document: new methods: be sure to support `some list` of metrics if you provide smart cross-validation. things that your model SHOULD NOT DO.

ISSUE: big change takes a lotta work / reference implementations / How to incentivize busy profs to rewrite packages they no longer have time to think about? / Professor-ware

## Community standards

I think it's particular important to establish community standards for how to implement a `recipes` interface when you come up with a new model. How do you deal with intercepts for example? I'm hoping to make a PR to the `recipes` package with a `detect_step` function to simplify this, but I'm not sure if there are any recommendations on how or why to use this type of functionality.

Related: a detailed tutorial for package developers describing how to conveniently deal with all three of the following data formats:

- `X` matrix, `y` vector/matrix response
- `formula` formula and `data` data frame
- `recipe` recipe and `data` data frame

Similarly, I'd like to see `rsample` export an object describing a resampling scheme similar to `fitControl` so that researchers can implement consistent resampling interfaces across packages. For example, I'm currently helping out with a package that includes a large number of options specifying cross-validation controls that could all be wrapper together and made consistent across packages. (TODO: add `lariat` package details?). **Question**: Is an `rset` object sufficient specification? Should `fit` methods accept `rset` objects rather than data frames? Or is a specification type deal better because of likely resistance to `rset` objects? That, whic of the following is going to be more accepted:

- `data` data frame, `fitControl` equivalent specification
- `rset` data only. Does this imply an additional line of code going from the data to the `rset`? How does this influence workflow?

a minimum set of metrics your model should support if you provide smart cross-validatio

Similarly, many modelling packages currently mix pre-processing and model fitting. Intercepts again come up as a sticky issue. If you want to be able to specify an intercept while providing both a `recipe`/`data` and `X`/`y` interface, you probably have to create a default `fit(model, X, y, intercept)` method and then a `fit(model, recipe, data)` that creates `X` and `y` and infers `intercept`. I find this unsatisfying and inelegant. The best solution seems to be making the `recipe` interface the standard, but I imagine a huge amount of push back on this (i.e. all my professors who say thinks like "I've never really gotten the point of data frames."). Similarly, many regression packages provide arguments that all users to specify:

- if data should be centered
- if data should be scaled
- Observation weights and offsets. My thought is that `recipes` should handle this.
- A subset of the data to work on. Again, `recipes` should handle this to separate modelling fitting and data preprocessing.
- if some sort of dimension reduction should be applied to data
- how to deal with missing data

I strongly believe the `recipes` package should handle all of the above. That way code can be more modular and consistent.

## Infrastructure to provide

- Skeleton package defining general inference and explaining where to fill in the details for a particular modelling method
- Providing a system to check for interface compliance
- Provide an interface compliant machine learning library to demonstrate how nice the interface is
- General principles for model developers/implementers: decouple calibration, preprocessing and hyperparameter selection as much as humanly possible

ISSUE: reference implementations. in what language?

## OVERLY DETAILED REQUEST: nice things to implement: smart PCA object, `yscale` and `2sescale` following WinVector and Gelman scalers (PCA should include smart ways to select number of PCs: elbow method, random matrix singular vector idea should also get implemented -- how to make this one work automatically: p-value of the true value within resample singular value pdf)

OVERLY DETAILED: automated elbow selection
OVERLY DETAILED: fit_predict and fit_score methods

QUESTION: utility of a score function?

OPEN ISSUE ## Supervised preprocessing

related: a formal definition of a feature set, ways to work with and compare them
(feature set/recipe combination object?) (optimizing over lots of feature sets -- instead of best subset selection, best set selection on a superset of well chosen sets)

step_univariatefilter to filter features based on predictive potential:

- PRESS statistic
- GLM deviance p-value (WinVector recommends upper bound of 1 / dimension of data)
- GAM deviance p-value
- variance

Question: what do in the presence of multiple outcomes? step_effectcode definitely needs clean data, what about step_univariatefilter? how does this affect the standard errors of the true coefficients, overall performance, etc?

step_effectcode following the footsteps of this

- Laplace smoothing
- partial pooling?

these depend on an outcome y, and regression and classification problems have different loss measurements

simple effect coding for categorical variables with continuous outcome: difference between conditional and grand means (new levels get zero)

what kind of statistical arise if you use data both for supervised preprocessing and for modelling? want a nice explanation of this, what are recommended data splits

step_outcomescale: rescale so that one unit of change in x results in one unit of change in y


## Thinking forward to AutoML

Use `recipes` as a set of low level building blocks that can also be programmatically manipulated to generate a whole bunch of new features

Alternatively, use recipes to write a smart data preprocessor with data quality guarantees as `vtreat` does

ways to investigate intermediate steps to make sure good maps have been learned (i.e. took right number of PCs in PCA) (plot and summary methods? summaries for the entire pipeline)

## Feature sets objects, what would that look like and what would that be useful for
- greedy interaction search, think about that kind of thing

KEY ADOPTION THOUGHTS:
  - make really nice ML and GLM interfaces as an introduction 
  - similarly for lasso and ridge / penalized regression, also parallelize for free

## ask for a tidy models package, and an internship to develop it

VAGUE / OPEN QUESTION: opinionation: what to make easy so that people actually do it. what to make difficult so that people don't do it. what kind of warnings for being an idiot. most important: what sort of resources to point people to about good practices: suppose you have a use who can blind fire predictive methods at a dataset and then figure out which one is best. how can you make their lives easier, better and more principled? what kind of references and best practices information should you be pointing them towards?

## tidy models parent package

Is there a package or organization like tidyverse or rlib where it's more appropriate to discuss the entire modelling workflow?

I'm currently writing formula and recipe interfaces to a new method developed by one of my professors and have been thinking about what a desirable interface looks like. I have some working notes here, mostly from the point of view of providing a template so that researchers can add new methods with consistent function signatures.

My understanding is that, to fit a particular family of models, you need to specify:

- a data design (dealt with by the recipes package)
- a hyperparameter space and search algorithm (is there a package for this? it seems like automated hyperparameter search is really taking off at the moment -- Python auto-sklearn and H2O automl for instance)
- a resampling strategy (rsample makes mapping across cv splits easy -- is there a plan to add a resampling strategy specification? is this the add_calibration() above?)

I've been thinking about this a bunch and would like to help out however I can. I'm particularly interested in convincing my professor to use whatever gets selected as an "official interface" for her new method, and in writing up some vignettes or blog posts describing how and why to implement that interface.

