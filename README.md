# Facet Discovery Tool

**Machine learning for recognizing structure in facet-inducing inequalities and projected constraint polytopes.**

This repository is a historical MSc research artifact from **September 2017 to April 2018**. The original project explored whether machine learning could classify and cluster families of facets that arise from projected constraint-programming formulations.

The short version: the companion Fourier-Motzkin project generates and projects linear formulations. This repository studies the resulting inequalities and tries to recognize repeated facet patterns.

## Research idea

Constraint programming is expressive, but mathematical optimization solvers are often fastest when the model is represented as a strong linear or mixed-integer formulation. The broader research program was about connecting those worlds:

1. Generate linear formulations of global constraints.
2. Use Fourier-Motzkin elimination to project away auxiliary variables.
3. Extract the resulting inequalities in the original decision-variable space.
4. Abstract those inequalities into coefficient/right-hand-side patterns.
5. Cluster or classify like-facets so reusable facet families can be discovered.

This repository focuses on step 5: **classification and clustering of like-facets**.

## Concrete example

For a step-by-step walkthrough using an `at_least(1,3,2,4)`-style constraint, see:

[`docs/CONCRETE_EXAMPLE.md`](docs/CONCRETE_EXAMPLE.md)

The example shows how a global constraint becomes a linear formulation, how Fourier-Motzkin projection removes auxiliary variables, how projected inequalities are abstracted into coefficient patterns, and how those patterns can be clustered into reusable facet families.

## Relationship to the Fourier-Motzkin repo

This project is directly related to the companion Fourier-Motzkin / facet-discovery repository:

- Companion repo: [`jmedcoff/facet-discovery-tool`](https://github.com/jmedcoff/facet-discovery-tool)
- Original link used in this repo: [`jorts1114/facet-discovery-tool`](https://github.com/jorts1114/facet-discovery-tool)

The companion repo describes a pipeline with four stages:

| Stage | Purpose |
| --- | --- |
| Generator | Generate IP formulations for global constraints. |
| Projector | Use Fourier-Motzkin elimination to project onto the desired variable subspace. |
| Abstractor | Convert projected inequalities into coefficient and constant patterns. |
| Discoverer | Learn functions that map input parameters to facet coefficients. |

This repository sits between the **Abstractor** and **Discoverer** stages. It experiments with feature engineering and ML-style grouping of facet patterns.

## Why this mattered

The research direction is essentially **formulation learning** before that term became common. Instead of only asking a solver to optimize, the question was:

> Can we learn the structure of strong formulations themselves?

That is useful because strong valid inequalities and facet-defining inequalities can dramatically improve optimization performance. If a system can discover, classify, and reuse those patterns, then it can help turn expressive constraint models into solver-efficient linear models.

## Repository status

This is a preserved research prototype, not a polished Python package. The notebooks and data files reflect exploratory work done during the MSc project.

The repo has been reorganized so the historical material is easier to understand:

```text
notebooks/experiments/       Exploratory notebooks from the original research timeline
artifacts/presentations/     MSc presentation slides
artifacts/archives/          Original uploaded zip artifacts
data/raw/                    Loose raw output and helper files
bounds/                      Historical Fourier-Motzkin output files
k0/                          Historical Fourier-Motzkin output files
sorted20180211/              Historical sorted Fourier-Motzkin output files
sorted20171105_Reduced/      Historical reduced Fourier-Motzkin output files
sorted_reduced/              Historical reduced copy of sorted output files
docs/                        Modern research context and pipeline documentation
```

## Key documentation

- [`docs/CONCRETE_EXAMPLE.md`](docs/CONCRETE_EXAMPLE.md) — walks through a concrete `at_least` example from constraint to projected inequalities to facet patterns.
- [`docs/RESEARCH_CONTEXT.md`](docs/RESEARCH_CONTEXT.md) — explains the research problem in modern optimization language.
- [`docs/PIPELINE.md`](docs/PIPELINE.md) — maps the Fourier-Motzkin pipeline to this repo's ML/facet-classification role.
- [`docs/REPOSITORY_MAP.md`](docs/REPOSITORY_MAP.md) — explains where the historical artifacts live after cleanup.

## Historical note

The original README said the code was "very sloppy" and not intended for reuse. That was true in the narrow software-engineering sense, but it undersold the research. The code should be read as a notebook-based research prototype for exploring facet clustering and polyhedral pattern recognition.

## Conference reference

An abstract for a talk including this project appeared at the **49th Southeastern International Conference on Combinatorics, Graph Theory & Computing**, March 5–9, 2018.

Original abstract link: http://www.math.fau.edu/combinatorics/kruk49.pdf
