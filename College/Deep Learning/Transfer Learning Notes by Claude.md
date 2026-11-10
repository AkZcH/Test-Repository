# Transfer Learning and Domain Adaptation Methods

## 1. Introduction to Transfer Learning
### Pattern Classification
-   A model learns from features and uses them to classify data.
-   It learns a decision boundary to separate classes.

  

### IID Assumption

-   **Independent:** One sample does not depend on another.
-   **Identically distributed:** Training and test samples come from the
    same distribution.
-   In real applications, training and test data may come from different
    distributions.

  

### Distribution Shift

  

-   Happens when the training-data distribution differs from the

    test-data distribution.

-   It can reduce model performance.

  

### Transfer Learning

  

-   Transfer Learning uses knowledge from a related source domain or

    task to improve learning in a target domain or task.

-   **Source domain:** Where the existing knowledge comes from.

-   **Target domain:** Where the knowledge is applied.

  

### Why Transfer Learning?

  

-   Labeled data may be limited or expensive.

-   It can reduce training time and computation.

-   It allows useful knowledge from one task or domain to help another.

  

------------------------------------------------------------------------

  

## 2. Settings of Transfer Learning

  

### Inductive Transfer Learning

  

-   Source and target tasks are different.

-   Labeled target data is available.

-   Example: Use knowledge from cat/dog classification to help classify

    lions and tigers.

  

### Transductive Transfer Learning

  

-   Source and target tasks are the same.

-   Source and target domains are different.

-   Source data is labeled.

-   Target data is usually unlabeled.

-   Example: Train using DSLR images and adapt to webcam images.

  

### Unsupervised Transfer Learning

  

-   Labeled data is not available in either domain.

-   Often used for tasks such as clustering and dimensionality

    reduction.

  

### Quick Comparison

  

  Setting        Tasks                             Target labels

  -------------- --------------------------------- ---------------

  Inductive      Different                         Available

  Transductive   Same                              Not available

  Unsupervised   May not be specified as related   Not available

  

------------------------------------------------------------------------

  

## 3. Approaches to Transfer Learning

  

### Instance-Based Transfer

  

-   Select or reweight source samples.

-   Give higher weights to source samples that are more useful for the

    target domain.

  

### Feature-Representation Transfer

  

-   Learn a common feature representation for source and target data.

-   Reduce the difference between the domains.

  

### Model-Based Transfer

  

-   Reuse model parameters or prior knowledge from the source task.

  

### Relational-Knowledge Transfer

  

-   Transfer relationships between entities from one domain or task to

    another.

  

### Approach Comparison

  

  Approach                   Inductive    Transductive    Unsupervised

  ------------------------ ----------- --------------- ---------------

  Instance transfer                Yes             Yes   Not indicated

  Feature representation           Yes             Yes             Yes

  Model transfer                   Yes   Not indicated   Not indicated

  Relational transfer              Yes   Not indicated   Not indicated

  

------------------------------------------------------------------------

  

## 4. Transductive Transfer Learning and Covariate Shift

  

### Conditions

  

-   Source and target tasks are the same.

-   Source and target domains are different.

-   Source data is labeled.

-   Target data is unlabeled.

  

### Sample Selection Bias

  

-   Source samples do not properly represent target samples.

-   This can happen because the two domains use different sampling

    processes.

-   Example: Clear DSLR images and lower-quality webcam images.

  

### Covariate Shift

  

-   The input distributions are different.

-   The relationship between input and output remains the same.

  

\[ P_S(Y`\mid `{=tex}X)=P_T(Y`\mid `{=tex}X) \]

  

\[ P_S(X)`\ne `{=tex}P_T(X) \]

  

Where: - (S) = source domain - (T) = target domain - (X) = input - (Y) =

output or label

  

### Importance Weighting

  

-   Reweight source samples to make them more representative of the

    target domain.

-   More target-relevant source samples receive higher weights.

  

### Maximum Mean Discrepancy (MMD)

  

-   MMD measures the difference between two distributions.

-   It compares the mean representations of the distributions in a

    Reproducing Kernel Hilbert Space (RKHS).

  

### Kernel Mean Matching (KMM)

  

-   KMM finds weights for source samples.

-   Its goal is to make the weighted source distribution closer to the

    target distribution.

-   It uses RKHS and quadratic programming.

-   It avoids explicitly estimating the source and target probability

    density functions.

  

**Relationship:** MMD measures distribution difference. KMM finds

weights to reduce that difference.

  

------------------------------------------------------------------------

  

## 5. Domain Adaptation and Structural Correspondence Learning (SCL)

  

### Domain Adaptation

  

-   A type of Transfer Learning.

-   It handles source and target domains with different data

    distributions.

-   The task is generally the same.

-   In the transductive setting, source labels are available and target

    labels are unavailable.

  

### Feature-Representation Transfer

  

-   Finds a common representation for source and target data.

-   The aim is to reduce the difference between their distributions.

-   It can use labeled source data and unlabeled target data.

  

### Structural Correspondence Learning (SCL)

  

**Pivot features** - Features that behave similarly across domains. -

They are useful for distinguishing classes.

  

**Non-pivot features** - Other features, often more domain-specific.

  

**Steps of SCL** 1. Select a set of pivot features. 2. Convert each

pivot feature into binary values: present or absent. 3. Create a

separate prediction task for each pivot. 4. Learn parameters for each

prediction task. 5. Combine the learned parameters into a matrix. 6.

Apply Eigen Decomposition to this matrix. 7. Learn a mapping and use it

to create new feature representations. 8. Train a classifier using the

new representations.

  

**Important:** Eigen Decomposition is applied to the matrix of learned

parameters, not directly to the binary pivot-feature values.

  

------------------------------------------------------------------------

  

## 6. Eigen Domain Transformation (EDT)

  

### Objective

  

-   Transform source-domain feature representations so they become more

    compatible with the target domain.

  

### Gaussian Distribution

  

-   A distribution in which data is concentrated around a central value.

-   Its shape and spread can be described using eigenvectors and

    eigenvalues.

  

### Eigenvectors and Eigenvalues

  

-   **Eigenvector:** Gives a direction of spread.

-   **Eigenvalue:** Gives the amount of variance along its corresponding

    eigenvector.

-   Large eigenvalue: data is widely spread in that direction.

-   Small eigenvalue: data is tightly concentrated in that direction.

  

### Why Gaussian Mixture Models (GMM)?

  

-   Real-world data rarely follows one single Gaussian distribution.

-   A GMM represents the data using multiple Gaussian distributions.

-   When target samples are few, centroid-based clustering can be used

    to form clusters that follow Gaussian distributions in both domains.

  

### Subspaces

  

-   A subspace is a smaller space within the original feature space.

-   It represents selected directions in which data varies.

-   A lower-dimensional subspace can represent data using fewer

    dimensions.

  

### Comparing Source and Target Subspaces

  

-   (U): Eigenvectors of the source domain.

-   (V): Eigenvectors of the target domain.

-   **Principal angles:** Angles between corresponding directions of two

    subspaces.

-   Small angles mean the subspaces are more aligned.

-   Large angles mean the subspaces are less aligned.

-   Subspace distance measures how different the two subspaces are.

-   EDT uses this comparison to guide the transformation.

  

### EDT Transformation

  

The PDF gives the following transformation:

  

\[ `\tilde{X}`{=tex} = XU\_{p\^\*}`\Lambda`{=tex}\_{p\^\*}\^{-1/2}

`\Gamma`{=tex}\_{p^*}^{1/2}V\_{p^*}^{T} \]

  

Where: - (X): Original source data. - (U\_{p\^\*}): Source-domain

eigenvectors. - (`\Lambda`{=tex}\_{p\^\*}): Source-domain eigenvalues. -

(`\Gamma`{=tex}\_{p\^*}): Target-domain eigenvalues. - (V\_{p\^*}):

Target-domain eigenvectors. - (p\^\*): Selected number of dimensions.

  

**Basic idea:** 1. Represent source data using source eigenvectors. 2.

Adjust the representation using source and target eigenvalues. 3. Map it

using target eigenvectors. 4. Obtain transformed source data that is

more compatible with the target domain.

  

-   EDT can be extended to RKHS for nonlinear transformation.

  

------------------------------------------------------------------------

  

## 7. Domain-Invariant Features

  

### Domain-Invariant Features

  

-   Features in which the differences between source and target domains

    are reduced.

-   The aim is to find a common subspace where the underlying

    distributions and manifolds of both domains are similar.

  

### MMD in RKHS

  

1.  Take source data (X) and target data (Y).

2.  Map both using a feature mapping (`\Phi`{=tex}).

3.  Obtain (`\Phi`{=tex}(X)) and (`\Phi`{=tex}(Y)).

4.  Compare their mean representations.

  

The ideal mean-alignment condition is:

  

\[ `\operatorname{Mean}`{=tex}(`\Phi`{=tex}(X)) =

`\operatorname{Mean}`{=tex}(`\Phi`{=tex}(Y)) \]

  

-   RKHS provides a feature space in which distribution comparison using

    mean representations is useful.

-   Matching means does not automatically guarantee that the complete

    distributions are identical.

  

------------------------------------------------------------------------

  

## 8. Manifolds and Cost Functions

  

### Manifold

  

-   A space that locally looks Euclidean.

-   This means a small region of the space looks approximately flat.

-   A manifold can still be curved globally.

  

### Four Objectives / Cost Functions

  

**A. Reduce difference in means** - Reduce the difference between source

and target distributions. - Uses MMD.

  

**B. Preserve local spatial arrangement** - Nearby source data points

should remain nearby after transformation. - This prevents the

transformation from destroying useful local relationships.

  

**C. Estimate disparity in domain shapes** - Reduce differences in the

geometric shapes of the two domains.

  

**D. Increase inter-class distance** - Maximize the distance between

different classes in the source domain. - This helps a classifier

distinguish classes more easily.

  

### Manifold Distance

  

-   **Euclidean distance:** Straight-line distance between two points.

-   **Manifold distance:** Distance measured along the path on the

    manifold.

-   Manifold distance can be approximated using an adjacency graph.

  

### Adjacency Graph

  

-   Connects nearby data points.

-   Creates a network of paths through the data.

-   Path length in this graph can approximate manifold distance.

  

### Landmark Points

  

-   Representative points selected from a data manifold.

-   An intermediate landmark can be selected by maximizing:

  

\[ `\text{Manifold distance from adjacent landmarks}`{=tex} -

`\text{Euclidean distance from adjacent landmarks}`{=tex} \]

  

-   This helps identify points that capture the curvature of the

    manifold.

  

------------------------------------------------------------------------

  

## 9. Combined Optimization Framework

  

The four objectives are combined into one optimization framework:

  

1.  Reduce distribution differences.

2.  Preserve local neighbourhood relationships.

3.  Reduce differences in domain shapes.

4.  Increase inter-class distance in the source domain.

  

### Why Combine Them?

  

-   Optimizing only one objective can harm another.

-   Combining them helps align domains while preserving useful data

    structure and class information.

  

### Optimization

  

The PDF gives the combined cost function:

  

\[ f(W)=`\operatorname{trace}`{=tex}(W\^T PW) \]

  

For the RKHS version:

  

\[ `\boxed{

\begin{aligned}

\text{minimize}\quad & \operatorname{tr}(Z^T P_K Z)\\

\text{subject to}\quad & Z^T KZ=I

\end{aligned}

}`{=tex} \]

  

-   \(W\) and (Z) represent learned transformation parameters.

-   \(P\) and (P_K) combine the different objectives.

-   The optimization minimizes the combined cost.

-   The PDF states that the solution is formed using eigenvectors of

    (P_K\^{-1}K).

-   The learned representations are then used to transform source and

    target data.

  

------------------------------------------------------------------------

  

## 10. Experimental Evaluation

  

### Synthetic Data

  

-   The method is first tested on artificially generated data.

-   Both 2D and 3D examples are shown.

-   Results are compared before and after transformation.

-   This helps visually demonstrate domain alignment.

  

### Office+Caltech Dataset

  

-   Used for object categorization and domain adaptation.

-   Contains four domains:

    -   Amazon

    -   DSLR

    -   Webcam

    -   Caltech

-   The lecture describes 31 object classes across Amazon, DSLR and

    Webcam.

-   It also describes 10 object classes across all four domains.

  

### Experimental Results

  

-   The lecture compares multiple domain adaptation methods.

-   A 1-nearest-neighbour (KNN, (K=1)) classifier is used.

-   **NA** means no adaptation.

-   Performance is compared across source-to-target domain combinations.

-   Average accuracy is also reported.