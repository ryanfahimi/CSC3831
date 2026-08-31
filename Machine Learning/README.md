# Machine Learning

*Lecture slides in Drive under `Lectures/`
([CSC3831 Drive](https://drive.google.com/drive/folders/1ZFzIz2oyMbxYrSIvm4Wo_9jR9OrkdU-n))*

The classical `scikit-learn` toolkit — supervised methods first, then validation, then unsupervised.
Where [CS362](https://github.com/ryanfahimi/CS362) implements these algorithms by hand, this strand is
about using them properly.

## Practicals

**[1. Regression](Practicals/1_Regression.ipynb)** — linear regression, then multiple linear
regression, then a `statsmodels` section on **confidence intervals** and feature selection.

That last part is the reason to read this notebook rather than skim it. `scikit-learn` gives you a
coefficient; `statsmodels` gives you a coefficient *with a confidence interval and a p-value*. The
difference matters: a large coefficient with an interval spanning zero is not evidence of anything.
Feature selection follows directly — knowing which coefficients are distinguishable from noise is how
you decide what to keep.

**[2. Classification](Practicals/2_Classification.ipynb)** — deliberately ordered as **validation
first, classification second**.

*Part 1: validation approaches*, including cross-validation. A single train/test split gives a score
that depends on which rows happened to land where; k-fold cross-validation averages over k different
splits so the estimate is stable. *Part 2: classification*, with **hyper-parameter tuning**.

The order is the lesson: you need a trustworthy way to measure a model before tuning one, or you will
tune against noise. The related trap — using the test set to choose hyper-parameters and then
reporting its score — is why a separate validation set (or nested cross-validation) exists.

**[3. Unsupervised Learning](Practicals/3_Unsupervised_Learning.ipynb)** — the largest practical
(63 cells), in two halves.

*PCA.* Dimensionality reduction by projecting onto the directions of greatest variance. Two things
here are worth keeping: **biplots**, which overlay the original features onto the component axes so a
component becomes interpretable rather than an anonymous axis, and **eigenfaces**, the classic
demonstration where principal components of face images turn out to be face-like structures. PCA is
usually presented as compression; the eigenfaces example shows it also *finds structure*.

*Clustering*, with an explicit three-way comparison — **k-means vs. hierarchical vs. DBSCAN** — and,
notably, DBSCAN shown both untuned and tuned. The comparison is the content:

- **k-means** needs *k* up front, assumes roughly spherical clusters of similar size, and assigns
  every point to one.
- **Hierarchical** produces a whole dendrogram rather than one partition, so *k* is chosen after
  seeing the structure.
- **DBSCAN** finds arbitrarily shaped clusters and labels sparse points as noise instead of forcing
  them somewhere — but is very sensitive to its `eps` and `min_samples` parameters, which is exactly
  what the untuned-versus-tuned pair demonstrates.

## [Coursework Part 2](Coursework_Part_2.ipynb)

MNIST, in four steps: load and visualise the first 20 digits with labels → train a **logistic
regression** classifier and report findings → apply **PCA** to reduce dimensionality → generate a
**noisy copy** by adding random normal noise and re-evaluate.

**Why logistic regression on MNIST.** It is a linear model on raw pixels, so it establishes what is
achievable without any spatial reasoning — the baseline that the CNNs in
[Computer Vision](../Computer%20Vision/) are measured against. Reading the two coursework notebooks
together is the intended comparison.

**PCA on images.** Each digit is 784 pixels, most of them near-constant background. PCA compresses to
a fraction of that with little accuracy loss, which is a concrete demonstration that the *intrinsic*
dimensionality of the data is far below its representation.

**The noise step is the real experiment.** Adding random normal noise at a given scale and
re-measuring asks the question a clean test score cannot: how gracefully does this degrade? It also
sets up an interesting interaction with the previous step, since PCA discards low-variance directions
and noise is spread across all of them — so the PCA-reduced model may well be the *more* robust one.
That is the kind of finding "report on your findings" is fishing for.
