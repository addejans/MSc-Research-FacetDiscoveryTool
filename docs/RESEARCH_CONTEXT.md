# Research Context

This repository should be read as an early research prototype in **machine-learning-assisted formulation discovery**.

The project was not simply about running scikit-learn on arbitrary mathematical data. The deeper question was whether a system could recognize structure in families of projected inequalities and use that structure to help discover stronger mathematical optimization formulations.

## Core research question

Can we learn recurring patterns in facet-inducing inequalities generated from families of constraint-programming models?

More concretely:

1. A global constraint is expressed in a high-level form.
2. A linear or integer-programming formulation is generated.
3. Auxiliary variables are projected out using Fourier-Motzkin elimination.
4. The projected system contains inequalities in the original decision-variable space.
5. The system tries to abstract, cluster, and classify similar inequalities.
6. Those clusters can suggest reusable facet families.

## Why this is interesting

Strong optimization formulations often depend on valid inequalities that are not obvious from the original business or constraint description. A weak formulation may solve slowly even if it is logically correct. A strong formulation can dramatically improve branch-and-bound, cut generation, relaxation strength, and practical solver performance.

This project explored the idea that some of that formulation-strengthening knowledge could be discovered from data.

## Modern framing

In modern language, this work sits near several active themes:

- formulation learning;
- ML-assisted cut discovery;
- automated valid-inequality recognition;
- polyhedral pattern recognition;
- CP-to-MILP translation;
- learning structure in optimization models;
- symbolic regression over projected coefficients;
- solver-aware model generation.

The companion Fourier-Motzkin repository provides the projection machinery. This repository studies the artifacts produced by that machinery.

## What this repository contributes

The historical notebooks explored feature extraction and classification of facets generated from global constraints such as `at_least` and `all_different`. The important contribution is the pipeline idea:

> Treat projected inequalities as data, represent them as features, and use ML-style clustering/classification to identify repeated facet families.

That is still a strong idea. The repository now documents it more clearly while preserving the original research artifacts.
