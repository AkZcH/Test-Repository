#### Assumption Till Now
The Training and the future test data come from the same task and same domain.
Represented in the same feature and label spaces
Follow the same distribution

# Transfer Learning
It is the ability of a system or model to apply its knowledge and skills learnt previously to new tasks in new domains which share some similarity with the domain and tasks it was trained on.
Transfer learning uses small no. of labelled or unlabeled
E.g- 

---
# Domain Adaptation and Structural Correspondence Learning

### Domain Adaptation
A model is trained in one domain but needs to perform well in another domain with a different data distribution

$$
\text{P}_{S}(X) \ne \text{P}_\text{T})X
$$
This is to adapt the model so it perform well on target domain

This is needed as-
input differences such as blurry, noisy, low resolution, low contrast, quality, etc.
The difference can cause a model trained on source data perform poorly on target data
The principle is to find common feature representation that reduces the difference b/w source and target domains
We try to make the learnt feature representations more compatible

### Structural Correspondence Learning
A feature transfer technique used in domain adaptation
Uses pivot features. These are features which behave the same across domains and hence can be used to discriminate b/w classes.
Other features are non-pivot features
Idea is to fin relationship b/w common pivot and non-pivot features

#### Process
1. Select the pivot features
2. For each feature, apply the binary encoding on it
3. Create a prediction problem for each feature, as each feature is treated as an independent binary prediction task
4. Learn parameters for each prediction problem
5. perform eigen decomposition on the matrix of learnt parameters
6. Learn a linear mapping and construct new features

Eigen decomposition is performed on the matrix of learnt parameters