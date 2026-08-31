# Data Engineering

*Lecture slides in Drive under `Lectures/`
([CSC3831 Drive](https://drive.google.com/drive/folders/1ZFzIz2oyMbxYrSIvm4Wo_9jR9OrkdU-n))*

The strand that comes first, deliberately. Before modelling anything you have to know what the data
is — what is missing, what is duplicated, and what is wrong.

## Practicals

**[1. EDA](Practicals/1_EDA.ipynb)** — exploratory data analysis: loading, "taking a peek",
determining relationships, and narrowing down. The point of EDA is that it is *not* a checklist — you
look at distributions and correlations to form hypotheses about what the data can support, before
committing to a model that assumes something false.

**[2. Imputation](Practicals/2_Imputation.ipynb)** — the largest practical (65 cells) and the most
useful. It builds an argument step by step:

- *Analysing missingness first.* Uses the `missingno` library to visualise **where** nulls occur.
  This matters because the pattern determines what you are allowed to do — values missing at random
  can be imputed, values missing *because of* what they would have been cannot be, without bias.
- *Dropping rows fails.* Removing every record with at least one null leaves a tiny dataset. Shown,
  not asserted.
- *Dropping columns is better but limited.* `MashThickness` has ~30k nulls out of 74k records.
- *Mean imputation.* Replaces nulls with the column mean — and the notebook then points out that the
  column mean is unchanged afterwards, which is exactly the problem: you have preserved the centre
  while **destroying the variance** and any correlation the column had.
- *Multivariate regression imputation.* Train a model using the other columns as features to predict
  the missing one. Better because it uses the structure in the data rather than flattening it.

That progression from worst to best, with the failure of each step motivating the next, is the
strand's best piece of teaching.

**[2. Record Linkage](Practicals/2_Record_Linkage.ipynb)** — identifying records that refer to the
same real entity across sources without a shared key. Covers exact linkage and `fuzzymatcher` for
approximate string matching. The underlying problem is that "Jon Smith, 12 High St" and "John Smith,
12 High Street" are one person, and no join key will tell you so.

**[3. Outlier Detection](Practicals/3_Outlier_Detection.ipynb)** — **Local Outlier Factor** and
**Isolation Forest**, chosen to contrast. LOF is *local*: it compares a point's density to its
neighbours', so it catches a point that is unusual for its region even if unremarkable globally.
Isolation Forest is *global*: it randomly partitions the space and measures how few splits it takes
to isolate a point, on the reasoning that anomalies are easier to separate. Neither is correct in
general, which is why both are here.

## [Coursework Part 1](Coursework_Part_1.ipynb)

36 cells, structured as: **data understanding [7] → outlier identification [10] → imputation [10] →
conclusions [3]**, with citations.

The libraries used are the practicals' content applied together: `LocalOutlierFactor`,
`IsolationForest`, and `OneClassSVM` for outliers (three methods, compared rather than one applied);
`KNNImputer` and `IterativeImputer` for missing values; `missingno` for missingness patterns;
`LinearRegression` with `PolynomialFeatures` and `r2_score`/`root_mean_squared_error` for evaluating
the imputation.

**The evaluation trick worth remembering.** You cannot score an imputation against ground truth you
don't have. The standard method — and what the metrics here are for — is to take *complete* records,
delete values artificially, impute them, and compare against what you removed. That gives a real
error measurement on the method before you apply it to the values that were genuinely missing.

**Why the marks sit where they do.** Twenty of the thirty marks are on outliers and imputation, and
only seven on understanding the data. But the understanding section is what determines whether your
choices in the other two are defensible — which is the point the mark scheme is making by leaving
three marks for "conclusions and thoughts."
