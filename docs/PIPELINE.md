# Pipeline

This project is best understood as one stage in a larger Fourier-Motzkin facet-discovery pipeline.

## End-to-end concept

```text
Global constraint
      ↓
IP / linear formulation generator
      ↓
Fourier-Motzkin projection
      ↓
Projected inequalities in original variable space
      ↓
Facet abstraction
      ↓
Facet classification and clustering   ← this repository
      ↓
Function discovery / reusable facet families
```

## Companion repository

The companion repository, [`jmedcoff/facet-discovery-tool`](https://github.com/jmedcoff/facet-discovery-tool), documents the upstream machinery:

- **Generator**: creates IP formulations for parameterized global constraints.
- **Projector**: applies Fourier-Motzkin elimination to remove auxiliary variables.
- **Abstractor**: extracts coefficient and constant patterns from the projected inequalities.
- **Discoverer**: learns functions mapping constraint parameters to facet coefficients.

This repository focuses on the ML-oriented middle layer: clustering, classifying, and recognizing like-facets.

## Input artifacts

The historical data files in this repository are projected inequality outputs, sorted/reduced Fourier-Motzkin outputs, and notebook-generated feature tables. Many filenames encode constraint families and parameter combinations. For example, files named like `sorted_at-least2535.fm` should be read as projected/sorted inequalities for a parameterized `at_least` constraint instance.

## Feature idea

The notebooks explored mappings from raw inequality coefficients into normalized feature vectors. The goal was to make inequalities comparable across different parameter settings.

A simplified representation looks like:

```text
raw inequality
    → normalize signs / coefficient pattern
    → extract structural counts and constants
    → map to feature tuple
    → classify / cluster facet type
```

## Output idea

The desired output is not merely a class label. The useful output is a discovered family:

```text
parameters → coefficient pattern → valid inequality family
```

For example, across multiple parameterized instances of a global constraint, the system may observe repeated inequality structures and infer that those structures are not isolated accidents; they may be reusable facets of the constraint polytope.

## Research significance

This is essentially a bridge between:

- high-level constraint modeling;
- polyhedral projection;
- machine learning over symbolic/numeric patterns;
- stronger linear optimization formulations.

The practical value is in helping optimization modelers discover formulation-strengthening inequalities automatically.
