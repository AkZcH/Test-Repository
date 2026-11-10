# PCA, Probabilistic PCA, Factor Analysis and Independent Component Analysis

## Exam-ready theoretical notes

> **Exam focus:** These notes prioritise concepts, definitions,
> intuition, differences, applications and limitations. Mathematical
> derivations are intentionally kept to a minimum. Learn the key model
> assumptions and the comparison table.

------------------------------------------------------------------------

## 1. Latent-variable models: the basic idea

-   A **latent variable** is a hidden variable that is not directly
    observed but helps explain the observed data.
-   A **latent-factor model** assumes that observed data can be
    represented using a smaller number of underlying factors.
-   This is useful when data has many features but much of the
    information is redundant or correlated.
-   Unlike a mixture model, which uses discrete hidden cluster
    assignments, these models use **continuous latent variables**.
-   A low-dimensional representation can capture the main structure of
    high-dimensional data.

**Why use latent factors?** - Reduce the number of dimensions. -
Discover hidden patterns or factors. - Represent correlated features
compactly. - Make data easier to visualise and use in later models.

------------------------------------------------------------------------

## 2. Principal Component Analysis (PCA)

### Definition

**Principal Component Analysis (PCA)** is a dimensionality-reduction
technique that transforms high-dimensional data into a smaller set of
components while retaining as much of the data's variation as possible.

PCA represents the original data using: - **Principal components:** new
directions or axes in the data. - **Scores / lower-dimensional
representation:** the coordinates of observations along those
directions.

Typically, the number of retained components is much smaller than the
original number of features.

### Main objectives

PCA has two equivalent interpretations:

1.  **Analysis view:** Find directions along which the projected data
    has maximum variance.
2.  **Synthesis view:** Find a lower-dimensional subspace that minimises
    the reconstruction error, or distance between the original data and
    its projection.

In the standard PCA formulation, the data is centred, meaning each
feature has its mean subtracted.

### Applications of PCA

-   **Dimensionality reduction:** Replace the original feature set with
    fewer components.
-   **Data visualisation:** Project high-dimensional data into two or
    three dimensions.
-   **Outlier detection:** An observation with a poor reconstruction may
    be an outlier.
-   **Feature extraction:** Use principal-component scores as input
    features for another model.
-   **Pattern discovery:** Reveal important underlying structure in
    data.

### Important characteristics

-   PCA is based on variance and captures directions of maximum
    variation.
-   It is commonly associated with orthogonal principal-component
    directions.
-   Components are ordered by the amount of variance they explain.
-   PCA is a linear dimensionality-reduction method.
-   Standard PCA is sensitive to feature scaling. Features measured on
    larger scales can dominate unless the data is appropriately scaled.

### Limitations

-   PCA captures linear structure.
-   Maximum variance does not necessarily mean maximum predictive
    usefulness.
-   Components can be difficult to interpret because they may combine
    many original features.
-   Results can be affected by outliers and feature scaling.

------------------------------------------------------------------------

## 3. Probabilistic PCA (PPCA)

### Definition

**Probabilistic PCA** is a probabilistic, continuous-latent-variable
generalisation of PCA. It models each observed data point as being
generated from lower-dimensional latent variables, together with
Gaussian noise.

### Core idea

-   Each observation is associated with a hidden, lower-dimensional
    latent vector.
-   The latent vector is assumed to follow a Gaussian distribution with
    zero mean and identity covariance in the standard model.
-   Given the latent vector, the observed data is modelled using a
    Gaussian distribution.
-   The noise has the same variance in every observed feature.

Conceptually:

**Observed data = contribution from latent factors + isotropic Gaussian
noise**

### Why is it called probabilistic?

Ordinary PCA gives a lower-dimensional representation and
reconstruction. PPCA instead specifies a probability model for how the
observed data is generated.

This allows the model to: - Describe uncertainty through a probability
distribution. - Treat latent variables as hidden variables and integrate
them out. - Use likelihood-based estimation. - Connect PCA to other
probabilistic latent-factor models.

### Relationship between PCA and PPCA

-   PPCA is a probabilistic interpretation/generalisation of PCA.
-   Under the model assumptions, ordinary PCA is recovered as the
    observation-noise variance approaches zero.
-   PPCA models the observed data with a Gaussian distribution whose
    covariance has a restricted structure.

### Why use PPCA?

-   It gives PCA a statistical, generative interpretation.
-   It provides a likelihood-based framework.
-   It creates a bridge to Factor Analysis.
-   It can be extended to more complex probabilistic models, such as
    mixtures of PCA models.

------------------------------------------------------------------------

## 4. Factor Analysis (FA)

### Definition

**Factor Analysis (FA)** is a statistical method used to discover latent factors that explain the correlations among observed variables.

It assumes that observed variables are influenced by a smaller number of underlying factors, along with variable-specific noise.

### Core idea

-   A set of observed variables is explained by a smaller set of latent
    factors.
-   Each observed feature can have its own noise variance.
-   The latent factors are commonly assumed to follow a standard
    Gaussian distribution.
-   The model separates shared variation explained by latent factors
    from feature-specific noise.

Conceptually:

**Observed data = contribution from shared latent factors + feature-specific noise**

### Applications of Factor Analysis

-   Discovering underlying factors in psychological or personality
    measurements.
-   Identifying common patterns across related variables.
-   Reducing a large set of correlated variables to a smaller number of
    factors.
-   Applications across science and engineering where hidden factors may
    explain observed measurements.

### Important characteristics
-   FA focuses on explaining relationships and correlations among
    observed variables.
-   It allows a different noise variance for each observed feature.
-   It is not affected by simply scaling individual features in the same
    way PCA can be
-   Its factor solution is not inherently unique because rotations of
    the factor representation can produce an equivalent model.
-   Unlike PCA, FA is affected by rotations of the observed data.
-   There is no simple SVD-based solution comparable to PCA; different
    local optima may occur.

------------------------------------------------------------------------

## 5. PCA vs. Factor Analysis

Both methods represent observed data using a smaller number of latent
factors, but their assumptions and goals differ.

  -----------------------------------------------------------------------
  Basis                   PCA                     Factor Analysis
  ----------------------- ----------------------- -----------------------
  Main purpose            Reduce dimensions while Discover latent factors
                          retaining variance      explaining correlations

  Main perspective        Finds directions of     Models shared variation
                          maximum variance        among observed
                                                  variables

  Noise assumption        Same noise variance     Different noise
                          across all dimensions   variance for each
                          in PPCA                 observed dimension

  Noise structure         Isotropic Gaussian      Diagonal noise
                          noise in PPCA           covariance

  Feature scaling         Sensitive to feature    Not affected by scaling
                          scale                   individual features in
                                                  the same way

  Computation             Has a convenient        No equivalent simple
                          SVD-based solution      SVD solution; may have
                                                  local optima

  Interpretation          Components capture      Factors aim to explain
                          maximum variance        relationships among
                                                  variables
  -----------------------------------------------------------------------

### The key difference to remember

-   **PCA:** Focuses on representing the data with fewer dimensions
    while preserving as much variance as possible.
-   **FA:** Focuses on explaining correlations among observed variables
    through hidden common factors, while allowing each variable to have
    its own noise variance.

### Similarity

In practice, PCA and FA can produce fairly similar results, even though
their underlying assumptions and interpretations are different.

------------------------------------------------------------------------

## 6. Independent Component Analysis (ICA)

### Definition

**Independent Component Analysis (ICA)** is a matrix-factorisation
technique that attempts to recover hidden source signals by finding
components that are statistically independent and generally
non-Gaussian.

ICA is particularly useful when observed data consists of mixtures of
several unknown sources.

### Motivation

Factor Analysis has a major limitation: its latent-factor directions are
not uniquely identifiable because the factor representation can be
rotated without changing the model.

ICA addresses this issue by using non-Gaussian, statistically
independent latent components. Under suitable assumptions, this helps
identify the underlying factor directions.

### Blind Source Separation

**Blind Source Separation (BSS)** is the canonical application of ICA.

It involves: 1. Multiple unknown sources produce signals. 2. Multiple
microphones or sensors record different mixtures of those sources. 3.
Each recording contains a different combination of the original signals.
4. ICA attempts to recover the original independent source signals from
the mixtures.

**Example:** Two people speak simultaneously in a room. Two microphones
record different mixtures of their voices. ICA can be used to separate
the voices.

### Key assumptions and characteristics

-   The observed data is treated as a mixture of underlying source
    signals.
-   The latent components are assumed to be statistically independent.
-   The components are generally assumed to be non-Gaussian.
-   Data is usually centred and whitened before applying ICA.
-   Under suitable assumptions, ICA can identify the factors up to
    permutation, sign and scaling.
-   At most one latent factor can be Gaussian for the identifiability
    result described in the lecture.

### Why is non-Gaussianity important?

-   Gaussian distributions have rotational symmetry 
-   This means Gaussian latent factors do not provide enough information
    to distinguish rotated factor directions.
-   Non-Gaussian independent factors break this rotational symmetry.
-   As a result, ICA can identify factor directions under suitable
    assumptions.

### Common ICA approaches mentioned in the lecture

-   **Maximum-likelihood ICA:** Assumes a probability distribution for
    the latent components, often a heavy-tailed distribution.
-   **Kurtosis-based approach:** Maximises a measure of non-Gaussianity
    using kurtosis.
-   **Entropy-based approach:** Uses entropy as a measure related to
    non-Gaussianity.
-   **FastICA:** A popular Newton-like method that maximises kurtosis.

### Applications of ICA

The lecture lists applications including: - Blind source separation and
audio signal separation. - Optical imaging. - Neuronal spike sorting. -
Face recognition. - Modelling receptive fields of primary visual
neurons. - Predicting stock-market prices. - Mobile-phone
communications. - Detecting colour-based ripeness in tomatoes. -
Removing artefacts, such as eye blinks, from EEG data.

### ICA example from the lecture

The lecture presents retail cash-flow data from five stores over three
years. ICA identifies underlying patterns: - Components 1 and 2 reflect
the holiday season. - Components 3 and 4 reflect year-to-year
patterns. - Component 5 reflects a summer dip in sales.

This illustrates how ICA can uncover hidden, interpretable sources of
variation in mixed observations.

### Limitations / identifiability

Matrix factorisation can have three sources of non-uniqueness:

1.  **Permutation / label switching:** The order of the factors can be
    changed without changing the model.
2.  **Scaling:** A factor can be scaled if the corresponding latent
    representation is inversely scaled.
3.  **Rotation:** Rotating the factors can produce an equivalent
    representation in models such as PCA and FA.

ICA addresses the rotation ambiguity under its independence and
non-Gaussianity assumptions. It does not determine the order, sign or
scale of the recovered components.

------------------------------------------------------------------------

## 7. PCA vs. ICA

  -----------------------------------------------------------------------
  Basis                   PCA                     ICA
  ----------------------- ----------------------- -----------------------
  Main objective          Find directions of      Recover statistically
                          maximum variance        independent source
                                                  components

  Main criterion          Variance                Statistical
                                                  independence and
                                                  non-Gaussianity

  Components              Orthogonal directions   Independent components;
                          in the standard         independence is not the
                          formulation             same as orthogonality

  Typical use             Dimensionality          Blind source separation
                          reduction               

  Data preparation        Usually centre data     Usually centre and
                                                  whiten data

  What it identifies      Directions of maximum   Underlying independent
                          variance                factor directions under
                                                  suitable assumptions
  -----------------------------------------------------------------------

**Critical distinction:** PCA finds uncorrelated directions that
maximise variance. ICA aims to find statistically independent
components. Uncorrelatedness and statistical independence are not
generally the same thing.

------------------------------------------------------------------------

## 8. Quick comparison: PCA, PPCA, FA and ICA

  -----------------------------------------------------------------------
  Method                  What it does            Key assumption / idea
  ----------------------- ----------------------- -----------------------
  PCA                     Reduces dimensions      Finds directions of
                          while preserving        maximum variance
                          variance                

  PPCA                    Gives PCA a             Gaussian latent
                          probabilistic           variables and equal
                          latent-variable model   observation-noise
                                                  variance

  FA                      Discovers hidden        Gaussian latent factors
                          factors explaining      and feature-specific
                          correlations            noise variances

  ICA                     Recovers independent    Independent, generally
                          hidden sources          non-Gaussian latent
                                                  components
  -----------------------------------------------------------------------

### One-line memory aid

-   **PCA:** Preserve variance.
-   **PPCA:** PCA with a probability model.
-   **FA:** Explain correlations using hidden factors.
-   **ICA:** Separate independent sources.

------------------------------------------------------------------------

## 9. Important definitions to memorise

**Latent variable:** A hidden variable that is not directly observed but
helps explain observed data.

**Latent factor:** An underlying hidden variable that accounts for
shared patterns or relationships among observed variables.

**Dimensionality reduction:** Reducing the number of features or
dimensions while retaining useful information.

**Principal component:** A direction in feature space along which
projected data has high variance.

**Factor loading:** A model parameter describing how latent factors
contribute to observed variables.

**Isotropic noise:** Noise with the same variance in every dimension.

**Blind Source Separation:** Recovering original source signals from
observed mixtures without knowing the original sources or mixing process
in advance.

**Whitening:** A preprocessing transformation that makes the transformed
data have identity covariance.

**Non-Gaussianity:** Deviation of a distribution from a Gaussian
distribution.

**Statistical independence:** A condition in which knowing the value of
one variable provides no information about another variable's value.

------------------------------------------------------------------------

## 10. Likely theoretical exam questions

Prepare short, structured answers for these.

1.  Define PCA. Explain its objectives and applications.
2.  Explain the two classical interpretations of PCA: analysis view and
    synthesis view.
3.  What are latent variables? Why are they useful in dimensionality
    reduction?
4.  What is Probabilistic PCA? How does it differ from ordinary PCA?
5.  Explain the relationship between PCA and Probabilistic PCA.
6.  Define Factor Analysis. Explain its basic idea and applications.
7.  Differentiate between PCA and Factor Analysis.
8.  Why does Factor Analysis allow different noise variances for
    different observed variables?
9.  Why are PCA and Factor Analysis solutions not always unique?
10. What is Independent Component Analysis?
11. Explain Blind Source Separation with an example.
12. Why does ICA use non-Gaussian independent components?
13. How does ICA address the rotation ambiguity found in Factor
    Analysis?
14. Differentiate between PCA and ICA.
15. What are the main sources of non-uniqueness in matrix factorisation?
16. What is whitening, and why is data usually whitened before ICA?
17. Mention important applications of ICA.
18. What is FastICA?

------------------------------------------------------------------------

## 11. Last-minute revision

### PCA

-   Dimensionality reduction.
-   Retains maximum variance.
-   Analysis view: maximise projected variance.
-   Synthesis view: minimise reconstruction error.
-   Used for visualisation, outlier detection and feature extraction.

### PPCA

-   Probabilistic version of PCA.
-   Uses continuous latent variables.
-   Assumes Gaussian latent variables and equal noise variance across
    dimensions.
-   Ordinary PCA is recovered as noise variance approaches zero.

### Factor Analysis

-   Finds hidden factors explaining correlations.
-   Allows a different noise variance for each observed feature.
-   Factor directions are not unique because of rotation.
-   No simple SVD solution like PCA.

### ICA

-   Finds statistically independent, generally non-Gaussian components.
-   Canonical use: blind source separation.
-   Usually uses centred and whitened data.
-   Can identify factor directions under suitable assumptions, up to
    permutation, sign and scaling.
-   FastICA is a popular method.
