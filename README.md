# Automating Heisenberg Hamiltonian Exchange Tensor Derivation via Group Theory

MPhys project, University of Leeds, 2025.
Supervisor: Joseph Barker

An algorithm built with Python that is able to specify the allowed exchange interaction tensor components between any two atoms in a crystal. Given a magnetic crystal, it finds the space group and subsequent symmetry operations, and applies these at the bond level. If the bond is left invariant, it can specify the exchange tensor for the bond following Neumann's principle.

## Background

Exchange is a result of the competition between the Coulomb repulsion and kinetic energies due to the Pauli exclusion principle among localised atomic spins. This gives rise to spontaneous magnetisation even in the absence of an applied magnetic field.

The interactions are characterised by the Hamiltonian for the Heisenberg model
where Si and Sj represent the dimensionless spin vectors at
sites i and j, ⟨i,j⟩indicates a summation over all pairs of
neighbouring spins, and Jij is the exchange interaction tensor
between spins iand j(in joules).

The interactions are characterised by the Hamiltonian for the Heisenberg model

$$
H = \sum_{\langle ij \rangle} \mathbf{S}_i^T \, \mathbf{J}_{ij} \, \mathbf{S}_j
$$

where Si and Sj represent the dimensionless spin vectors at
sites i and j, ⟨i,j⟩indicates a summation over all pairs of
neighbouring spins, and Jij is the exchange interaction tensor
between spins i and j (in joules).


## What the code does

1. Reads a crystal structure from [file type, e.g. a CIF file] using ASE.
2. Finds the space group symmetry operations with spglib.
3. [Builds a supercell and finds neighbouring atoms within a given radius.]
4. [Finds the symmetry operations that leave a bond unchanged, or map it onto an equivalent bond.]
5. Uses SymPy to solve for the constraints these operations place on J, giving the allowed form of the tensor.
6. [Generates the tensors for symmetry-equivalent bonds from the first one.]
7. [Plots the unit cell and bonds.]

## Repository contents

| File | Description |
|------|-------------|
| `exchange_tensor_derivation.ipynb` | Main notebook containing all functions |
| `Final report.pdf` | Full project report with theory, method and results |
| `LICENSE` | MIT licence |

## Requirements

- Python [version]
- numpy
- sympy
- spglib
- ase
- matplotlib
- Jupyter

Install with:

```bash
pip install numpy sympy spglib ase matplotlib jupyter
```

## Usage

Open the notebook and run all cells to load the functions. Then call one of:

```python
# [what this does, e.g. tensors for bonds within the unit cell]
run_code_unit_cell("[path/to/structure.cif]")

# [what this does]
run_code_supercell([arguments])

# [what this does]
run_code_all_conventional_cell_bonds([arguments])
```

[Describe the key inputs, such as the neighbour radius or which atoms to consider.]

## Example

[Pick one material you tested. Show the input, then the output tensor, ideally checked against a known result from the literature.]

```
[example output]
```

## Results

[A few lines on what you found or validated. Point to the relevant section of the report.]

## Limitations and future work

- [e.g. only tested on certain structure types]
- [e.g. tolerance choices]
- [e.g. ideas for extending it]

## Acknowledgements

The symmetry operation class was written by Dr Joseph Barker (private communication) and is used with permission.
[Thank your supervisor or anyone else here.]

## Licence

MIT. See `LICENSE`.
