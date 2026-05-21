 # Equipredict-B:1.0
This repository provides a compact code preview for the reaction-extent equilibrium calculation described in the associated manuscript.

The purpose of this repository is to document the numerical core of the method for review. 
## Repository Scope

This preview includes only:

- the concentration update from reaction extent,
- the logarithmic reaction quotient calculation,
- the analytic Jacobian used by nonlinear least-squares optimization,
- a residual function of the form `logQ - logK`,
- a tiny artificial least-squares solver example.

It intentionally excludes:

- experimental datasets,
- real compound names,
- project-specific coefficient matrices,
- equilibrium-constant tables,
- Excel input/output workflows,
- batch processing scripts,
- figure/table generation scripts,
- tuned solver settings used for real manuscript calculations.

## Mathematical Core

For initial concentration vector `C0`, coefficient matrix `V`, and reaction extent vector `xi`, concentrations are updated as:

```python
C = C0 + V @ xi
```

The logarithmic reaction quotient is calculated as:

```python
logQ = V.T @ np.log(C)
```

The analytic Jacobian with respect to `xi` is:

```python
J = V.T @ (V / C[:, None])
```

The least-squares residual is:

```python
residual = logQ(C0 + V @ xi) - logK
```

These expressions are implemented in [calculation_core.py](calculation_core.py).

## Least-Squares Demonstration

The file includes a small artificial example showing how the residual and Jacobian callbacks can be passed to SciPy:

```python
result = least_squares(
    residual_fn,
    x0=initial_xi,
    jac=jacobian_fn,
)
```

The demonstration uses generic arrays only. It is provided to verify the calculation structure, not to reproduce the manuscript dataset.

## Quick Check

Use Python 3.10 or later.

```bash
pip install -r requirements.txt
python calculation_core.py
```

Expected output includes a successful toy least-squares solve:

```text
solver success = True
solver xi = [0.72828584]
```

Small numerical differences may occur across Python, NumPy, or SciPy versions.

## Files

```text
calculation_core.py   # numerical core and artificial least-squares example
requirements.txt      # minimal Python dependencies
CODE_AVAILABILITY.md  # code/data availability statement for manuscript use
.gitignore            # excludes generated files and private data formats
```

## Citation

If this repository is referenced during review or after publication, please cite the associated manuscript. The bibliographic details can be added here after acceptance.

## License And Reuse

No open-source license is included in this preview repository. The code is provided for method inspection and academic review. For reuse beyond viewing the repository, please contact the authors.
