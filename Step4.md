# Step 4 — Verification

**For equations to display correctly online, you may have to click the `Raw` button.**

**However, you need to exit `Raw` mode to display the Jupyter Notebook correctly.**

[Open the Step 4 notebook](./Learning_FEM_1D_step4.ipynb).

This final step keeps verification separate from the short instructional notebooks. It checks:

- exact P1 reference matrices and load vector;
- outward-current Neumann and Robin signs;
- an affine patch and preservation of matrix symmetry;
- second-order $L^2$ convergence against an analytical solution.

Run it after Step 3. The first cell loads Step 3 in a captured cell, so the checks are self-contained from a fresh kernel.
