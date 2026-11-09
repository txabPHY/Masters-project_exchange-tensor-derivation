# Automating Heisenberg Hamiltonian Exchange Tensor Derivation via Group Theory

MPhys project, University of Leeds, 2025.

A Python algorithm built to specify the allowed exchange interaction tensor components between any two atoms in a crystal. Given a magnetic crystal, it finds the space group and its symmetry operations, and applies these at the bond level. Each operation that leaves a bond invariant constrains the bond's exchange tensor, following Neumann's principle.

## Background

Exchange arises from the competition between Coulomb repulsion and kinetic energy, as a consequence of the Pauli exclusion principle. Between localised atomic spins, it gives rise to spontaneous magnetisation even in the absence of an applied magnetic field.

The interactions are characterised by the Hamiltonian for the Heisenberg model

$$
H = -\sum_{\langle i,j \rangle} \mathbf{S}_i \cdot \mathbf{J}_{ij} \cdot \mathbf{S}_j
$$

where $\mathbf{S}_i$ and $\mathbf{S}_j$ are the dimensionless spin vectors at sites $i$ and $j$, $\langle i,j \rangle$ indicates a sum over all pairs of neighbouring spins, and $\mathbf{J}_{ij}$ is the 3×3 exchange interaction tensor between spins $i$ and $j$ (in joules). $\mathbf{J}_{ij}$ can be split into an isotropic part, a symmetric anisotropic part, and an antisymmetric part (the Dzyaloshinskii-Moriya interaction, DMI).

Neumann's principle states that a physical property must respect the symmetries of the system it describes. Applied to a single bond, any symmetry operation that leaves the bond unchanged (or reverses it) reduces the number of independent components of $\mathbf{J}_{ij}$. A bond can have lower symmetry than the crystal as a whole, so DMI can be allowed on a bond even in a centrosymmetric crystal. Working these constraints out by hand is slow and prone to error, so this project automates it.

## What the code does

1. Reads a crystal structure from a `.cif` file (taken from the Materials Project) using ASE.
2. Finds the space group symmetry operations with spglib.
3. Builds a supercell and finds the neighbouring atoms within a given radius.
4. Finds the symmetry operations that leave each bond invariant, either directly or by reversing it.
5. Uses SymPy to solve for the constraints these operations place on J, giving the allowed form of the tensor.
6. Checks whether other bonds of the same length can be mapped onto the first by a space group operation. If they can, their tensors follow by rotation. If not, the bonds are symmetry distinct and are specified separately.
7. Plots the unit cell and bonds.

## Repository contents

| File | Description |
|------|-------------|
| `exchange_tensor_derivation.ipynb` | Main notebook containing all functions |
| `Final report.pdf` | Full project report with theory, method and results |
| `LICENSE` | MIT licence |

## Requirements

- Python 3.12
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
# Tensors for each bond within the unit cell
run_code_unit_cell("path/to/structure.cif")

# Tensors for bonds around each atom, using a supercell to include neighbours outside the unit cell
run_code_supercell("path/to/structure.cif", scale=3, skip_elements=[32])

# Tensors for all bonds within the first conventional cell, for comparison with figures and tables in the literature
run_code_all_conventional_cell_bonds("path/to/structure.cif")
```

Main options:

- `scale`: size of the supercell (default 3)
- `skip_elements`: atomic numbers of non-magnetic atoms to skip as bond endpoints, e.g. `[32]` for Ge in Mn5Ge3
- `pick_atom_index`: run for a single atom only, or `None` for all atoms
- `radius_mode`, `radius_factor`: how the neighbour cut-off radius is chosen
- `do_seperate_tensor_for_each_bond`, `do_first_tensor_for_bond_then_others_from_same_atom`, `do_seperate_tensor_for_bonds_checks_other_atoms`: choose how tensors are generated for the bonds in a shell
- `plotting`: turn plots on or off

Note: `run_code_unit_cell` has `skip_elements = [9]` (fluorine) hard-coded from the MnF2 calculations. Change this for other materials.

## Example: third-nearest-neighbour bonds in YIG (Y3Fe5O12)

Earlier models of YIG assumed all third-nearest-neighbour Fe–Fe bonds (5.44 Å) share the same exchange parameter. Using the space group of the full lattice, the algorithm finds that no symmetry operation maps the body-diagonal bonds onto the other corner-to-centre bonds, so they are symmetry distinct:

| Bond | Point group | Allowed components |
|------|-------------|--------------------|
| J3b (body diagonal) | 32 | 3, no DMI |
| J3a (other corner-to-centre bonds) | 2 | 6, DMI allowed |

```
[paste the output from your run here]
```

Note: two sentences in the report (just before Table II, and in the orbit–stabilizer discussion) swap the J3a and J3b labels. Table II and Eqs. 72–73 are correct.

## Results

- **Validation:** reproduces known results for a tetragonal test model with anisotropic couplings, and for the non-symmorphic diamond cubic structure.
- **MnF2:** finds symmetry-distinct bonds within a shell previously treated as equivalent.
- **YIG:** confirms the two distinct third-nearest-neighbour bonds shown above.
- **Mn5Ge3:** finds bonds that allow DMI despite the crystal being centrosymmetric, in agreement with experiment. See the limitations below.

Full details are in Section III of `Final report.pdf`.

## Limitations and future work

- spglib returns rotation matrices in fractional coordinates, and these are not yet converted to Cartesian before being applied to J. This doesn't affect orthogonal cells (cubic, tetragonal, orthorhombic), but the Mn5Ge3 results (hexagonal) should be rechecked once this is fixed.
- Some bond point groups could not be identified by spglib. This may be related to the issue above.
- Possible extensions: magnetic space groups and time-reversal symmetry, and using the orbit–stabilizer theorem to spot inequivalent bonds within a shell.


## Licence

MIT.
