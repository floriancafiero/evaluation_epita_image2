# Evaluation EPITA Image 2 — Visual Privacy Classification

This repository contains the material for the EPITA Image course evaluation project.

## Topic

The project focuses on **visual privacy prediction**.

The practical question is:

> Can we predict whether an image is public or private using precomputed visual features?

This is a realistic problem: before someone shares an image online, a system could warn them if the image may contain private or sensitive visual content.

Examples of potentially private visual cues include:

- people;
- children;
- bedrooms;
- bathrooms;
- family scenes;
- computer screens;
- documents;
- phones;
- indoor domestic spaces;
- private events.

The goal is not only to obtain a good classification score, but also to understand **what the model learns**.

---

## Scientific motivation

Visual privacy is difficult because an image is not private only because of one object.

For example:

- `person + street` may be public;
- `person + bedroom` may be private;
- `computer` alone is ambiguous;
- `computer + document + indoor scene` may be private;
- `family + home` may be private;
- `group + beach` may be public or private depending on context.

The central scientific question is therefore:

> Is visual privacy predictable from visual cues alone, or does it depend too much on social context?

This project asks you to investigate this question using classical machine learning methods.

---

## Dataset

The data comes from the artifact associated with the following paper:

> Learning Privacy from Visual Entities

For this project, we use only the **PrivacyAlert** part of the dataset.

Students do **not** work directly on the original images.  They work on already extracted visual features.

The relevant files are:

```text
data/
├── curated_imageprivacy_datasets.zip
├── objects_privacyalert.zip
└── scenes_privacyalert.zip
```

---

## What the files contain

### `curated_imageprivacy_datasets.zip`

This archive contains the labels and train/validation/test splits.

The most useful files are inside:

```text
curated_privacyalert/
├── annotations/
│   ├── labels.csv
│   └── labels_splits.csv
└── splits/
    ├── train_with_labels_2classes.csv
    ├── val_with_labels_2classes.csv
    └── test_with_labels_2classes.csv
```

The target variable is binary:

```text
public
private
```

### `scenes_privacyalert.zip`

This archive contains precomputed scene-related features.

These features describe the probable type of environment in the image, for example:

```text
bedroom
bathroom
office
street
beach
restaurant
classroom
kitchen
living room
```

### `objects_privacyalert.zip`

This archive contains precomputed object-related features.

These features describe objects detected in the image, for example:

```text
person
bed
laptop
cell phone
book
car
toilet
backpack
screen
chair
```

---

## Important restriction

For this project, students should **not**:

- download the original images;
- train a deep neural network;
- fine-tune a CNN, ViT, CLIP, DINO, or any foundation model;
- compute new embeddings;
- use graph neural networks;
- use the other datasets unless explicitly instructed.

The project is about **classical machine learning on precomputed visual features**.

---

## Allowed methods

Students may use methods seen in class, including:

- Logistic Regression;
- SVM;
- k-NN;
- Random Forest;
- XGBoost;
- PCA;
- t-SNE;
- UMAP;
- k-means;
- hierarchical clustering;
- Gaussian Mixture Models;
- DBSCAN / HDBSCAN.
- Any other method outside of DeepLearning / GenAI is accepted

---

## Main task

Given precomputed visual features, predict whether an image is:

```text
public
```

or:

```text
private
```

This is a binary classification task.

---

## Required metrics

Students should report at least:

- accuracy;
- macro-F1;
- precision for the `private` class;
- recall for the `private` class;
- confusion matrix.

The `private` class is especially important.

In a privacy-warning system, a false negative means:

> the model failed to warn the user about an image that was actually private.

Therefore, the **recall of the private class** is a central metric.

---

## Possible project topics

Each group should focus on at least two of the following questions.

### Topic 1 — Scene features vs object features

Question:

> Is visual privacy better predicted by the scene or by the objects?

Compare:

```text
scene features only
object features only
scene + object features
```

Expected discussion:

- Are indoor scenes more associated with privacy?
- Are objects such as beds, laptops, phones, or documents strong privacy cues?
- Does combining scenes and objects improve performance?

---

### Topic 2 — Comparison of classical models

Question:

> Which classical machine learning model works best?

Compare several models, for example:

```text
Logistic Regression
Linear SVM
RBF SVM
Random Forest
XGBoost
k-NN
```

Expected discussion:

- Is a linear model already strong?
- Does XGBoost improve performance?
- Is the best model also the most interpretable?
- Is there a trade-off between performance and simplicity?

---

### Topic 3 — Interpretability of visual features

Question:

> Which features are most associated with private or public images?

Use interpretable models such as:

```text
Logistic Regression
Linear SVM
Random Forest
XGBoost
```

Expected outputs:

- top features associated with `private`;
- top features associated with `public`;
- discussion of ambiguous features.

Example:

```text
bedroom  → probably private
street   → probably public
person   → ambiguous
laptop   → context-dependent
```

Critical question:

> Does the model really learn privacy, or does it learn shortcuts such as “indoor = private”?

---

### Topic 4 — PCA, UMAP, and dimensionality reduction

Question:

> Can we reduce the feature space without losing predictive performance?

Possible experiments:

```text
raw features
PCA 5
PCA 10
PCA 25
PCA 50
PCA 100
```

Expected outputs:

- performance as a function of the number of PCA components;
- 2D visualization using PCA, UMAP, or t-SNE;
- discussion of whether `public` and `private` images are visually separable.

Critical question:

> Is privacy captured by a few global dimensions, or by many small and specific visual cues?

---

### Topic 5 — Decision threshold and social cost

Question:

> In a privacy-warning system, should we prefer more warnings or fewer missed private images?

Many classifiers produce probabilities or scores.  
The default threshold is often:

```text
0.5
```

But you can compare:

```text
0.2
0.3
0.5
0.7
0.8
```

Expected discussion:

- What happens to precision when the threshold changes?
- What happens to recall for the private class?
- Which threshold would be appropriate for an assistant that warns users before publication?

Critical question:

> Is it worse to produce too many false alerts, or to miss genuinely private images?

---

### Topic 6 — Error analysis and ambiguous cases

Question:

> What do the model errors reveal about visual privacy?

Analyze:

```text
false positives
false negatives
```

A false positive is:

```text
label = public
prediction = private
```

A false negative is:

```text
label = private
prediction = public
```

Expected analysis:

- Which features appear often in false positives?
- Which features appear often in false negatives?
- Are some errors understandable?
- Are some labels themselves ambiguous?

Critical question:

> Are the model’s errors only technical mistakes, or do they reveal that privacy is socially contextual?

---

## Expected output

Each group should submit:

1. a reproducible notebook;
2. a table of scores;
3. a confusion matrix;
4. at least one visualization;
5. an error analysis;
6. a short critical discussion.

The final answer should not only say:

> Our model gets the best score.

It should also answer:

> What does the model learn, and where does it fail?

---

## Suggested evaluation grid

| Criterion | Points |
|---|---:|
| Reproducible pipeline | 4 |
| Correct use of metrics | 4 |
| Methodological comparison | 4 |
| Visualization | 3 |
| Error analysis | 3 |
| Critical discussion | 2 |
| **Total** | **20** |

---

## Minimal workflow

A typical workflow may look like this:

```text
load labels and splits
load scene and/or object features
merge features with labels
train model on train split
tune on validation split
evaluate on test split
compute metrics
analyze errors
interpret results
```

---

## Practical interpretation

This project simulates a simple privacy assistant.

Such a system could be used to warn users before sharing images online:

> This image may contain private visual content. Please check before posting.

However, the project also asks whether such a system is reliable.

The key limitation is that privacy is not only visual.  
It also depends on:

- who is sharing the image;
- who appears in the image;
- whether people gave consent;
- the publication context;
- the intended audience;
- cultural and social norms.

---

## Reference

Xompero, A., & Cavallaro, A. (2025). *Learning privacy from visual entities*. arxiv.
