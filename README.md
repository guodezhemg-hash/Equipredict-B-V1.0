 Calculation Core Preview

This repository is a compact preview of the numerical calculation used in a reaction-extent equilibrium model.

It is intended to show the central mathematical idea only. It is not a complete application, not a data-processing pipeline, and not a ready-to-use solver for a specific chemical system.

## Core Idea

For a reaction network with initial concentration vector `C0`, stoichiometric matrix `A`, and reaction extent vector `xi`, the concentration update is:

```python
C = C0 + A @ xi
```

The logarithmic reaction quotient is:

```python
logQ = A.T @ np.log(C)
```

The analytic Jacobian with respect to `xi` is:

```python
J = A.T @ (A / C[:, None])
```

These expressions are implemented in [calculation_core.py](calculation_core.py).

## Least-Squares Preview

The equilibrium problem can be expressed as a nonlinear least-squares objective:

```python
residual = logQ(C0 + A @ xi) - logK
```

The preview code exposes this structure through:

```python
residual_fn, jacobian_fn = make_least_squares_callbacks(C0, A, logK)
```

Conceptually, these callbacks are the quantities passed to a nonlinear least-squares optimizer:

```python
# Pseudocode only:
# result = least_squares(
#     residual_fn,
#     x0=initial_xi,
#     jac=jacobian_fn,
#     bounds=...,
# )
```

The actual optimizer configuration, bounds strategy, convergence handling, data validation, and project-specific preprocessing are intentionally omitted.

## What Is Included

- `concentration_from_extent()`
- `log_reaction_quotient()`
- `reaction_extent_jacobian()`
- `log_equilibrium_residual()`
- `make_least_squares_callbacks()`
- A tiny artificial demo using generic species-like arrays

## What Is Not Included

- No Excel reading or writing
- No batch-processing workflow
- No real experimental data
- No real substance names
- No project-specific reaction matrix
- No complete least-squares optimization pipeline or tuned solver configuration
- No production command-line interface

## Quick Check

```bash
pip install -r requirements.txt
python calculation_core.py
```

The demo uses artificial arrays only and is provided for checking the formulas.

## Notes

This preview is meant for academic or portfolio-style demonstration. If you need a complete implementation, you should build your own data interface, model validation, solver configuration, and result-export workflow around the mathematical core.

No license is provided in this preview folder. Unless a license is added later, reuse and redistribution are not granted beyond what the hosting platform requires for viewing the repository.
