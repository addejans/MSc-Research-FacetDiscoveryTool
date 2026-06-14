# Concrete Example: From `at_least` Constraint to Facet Pattern

This example shows how the facet-discovery research pipeline should be read in concrete terms.

The companion Fourier-Motzkin project uses global constraints such as `at_least` and projects their linear formulations back into the original decision-variable space. This repository then studies the resulting inequalities and tries to recognize repeated facet patterns.

## 1. Start with a global constraint

Suppose we have a global constraint of the form:

```text
at_least(m = 1, n = 3, k = 2, l = 4)
```

Read this informally as a parameterized counting/global constraint over three decision variables. The exact semantics are less important here than the workflow: the constraint has parameters, those parameters generate a formulation, and the projected inequalities may reveal reusable structure.

## 2. Generate an IP formulation

The companion Fourier-Motzkin repo describes this stage as the **Generator**. The generator creates a linear/integer programming formulation of the global constraint.

Some variables are the original decision variables:

```text
x1, x2, x3
```

Other variables are auxiliary variables introduced by the formulation:

```text
y(i, j)
```

A generated formulation contains equations linking each `x_i` to auxiliary `y` variables, plus additional inequalities describing the global constraint.

## 3. Project away auxiliary variables

The **Projector** stage uses Fourier-Motzkin elimination to remove auxiliary variables and recover inequalities only in the original `x`-space.

For example, after projection, the companion repo shows inequalities like:

```text
-1 x1 +1 x2 +1 x3 <= 6
+1 x1 -1 x2 +1 x3 <= 6
+1 x1 +1 x2 -1 x3 <= 6

-1 x1 +1 x2 -1 x3 <= 2
-1 x1 -1 x2 +1 x3 <= 2
+1 x1 -1 x2 -1 x3 <= 2

-1 x1 -1 x2 -1 x3 <= -2
+1 x1 +1 x2 +1 x3 <= 10

+1 x1 <= 4
-1 x1 <= 0
+1 x2 <= 4
-1 x2 <= 0
+1 x3 <= 4
-1 x3 <= 0
```

These inequalities are no longer written in terms of the formulation's auxiliary variables. They describe the projected polytope in the original decision-variable space.

## 4. Abstract the inequality pattern

The next step is to stop treating each inequality as a one-off row. Instead, the inequality is converted into a structured representation.

For example:

```text
-1 x1 +1 x2 +1 x3 <= 6
```

can be represented as:

```text
coefficients = [-1, 1, 1]
rhs          = 6
sign_pattern = [-, +, +]
support_size = 3
family_hint  = mixed-sign cardinality / bound structure
```

Another inequality:

```text
+1 x1 +1 x2 -1 x3 <= 6
```

has the same support size and the same general coefficient pattern, but with the negative coefficient on a different variable. A human would recognize these as belonging to the same family up to permutation symmetry.

The research question is whether the system can recognize that too.

## 5. Turn projected inequalities into ML features

This repository experimented with converting inequalities into feature tuples. A simplified version of the idea is:

```text
raw inequality
    -> normalize sign and scale
    -> canonicalize coefficient pattern
    -> extract coefficient counts
    -> extract RHS behavior
    -> attach source parameters
    -> classify or cluster
```

A possible feature representation might look like:

```text
constraint_family = at_least
parameters        = (m=1, n=3, k=2, l=4)
coeff_pattern     = [-1, +1, +1]
negative_count    = 1
positive_count    = 2
rhs               = 6
support_size      = 3
```

Across many parameter settings, the same kind of inequality may appear repeatedly with different constants. That is where the discovery problem becomes interesting.

## 6. Cluster like-facets

Once inequalities are represented as comparable feature vectors, the project can cluster or classify them.

For example, these inequalities likely belong to one family:

```text
-1 x1 +1 x2 +1 x3 <= 6
+1 x1 -1 x2 +1 x3 <= 6
+1 x1 +1 x2 -1 x3 <= 6
```

These belong to another:

```text
+1 x1 <= 4
+1 x2 <= 4
+1 x3 <= 4
```

And these belong to another:

```text
-1 x1 <= 0
-1 x2 <= 0
-1 x3 <= 0
```

The first family is structurally more interesting than simple variable bounds. It may correspond to a nontrivial projected inequality family induced by the original global constraint.

## 7. Discover reusable formulas

The long-term goal is not just to say "these inequalities look alike." The goal is to infer reusable formulas.

A discovered pattern might look like:

```text
For a parameterized at_least(m, n, k, l) constraint,
a family of projected inequalities has coefficient pattern:

one negative coefficient, n-1 positive coefficients, and RHS f(m, n, k, l)
```

Then the next research step is to learn or derive the function:

```text
rhs = f(m, n, k, l)
```

That is the bridge from data mining to formulation discovery. If the pattern is valid and general, it can become a reusable valid inequality or facet family for future optimization models.

## Why this matters

A weak formulation can be logically correct but computationally poor. A stronger formulation can make the LP relaxation tighter and help a MILP solver prove bounds faster.

This research asks whether some of that formulation-strengthening knowledge can be learned automatically from projected examples.

In modern terms, this is an early version of:

```text
constraint model -> projected polytope -> facet data -> learned formulation structure
```

That is why this repo matters beyond the old notebooks. It is not just classifying rows in a dataset; it is trying to learn the structure of strong optimization formulations.
