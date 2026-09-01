# Learning_FEM_1D
Learning the Finite Element Method in 1D

We  solve a diffusion-reaction equation, with a source term, in 1D.

## Running the notebooks

Use Python 3.11 or newer. Create an environment, install `requirements.txt`, launch Jupyter from this directory, and run each notebook from a fresh kernel.


## Curriculum

In [Step-0](Step0.md), I provide a basic introduction. 
- Some FEM concepts, such as the assembly of the global matrix per element, the reference element, ... are not discussed at first. 
- At the end of the Jupyter Notebook, I finally do the global assembly by looping over elements.

In [Step-1](Step1.md), we introduce more advanced concepts:
- the reference element
- computing the local matrices using a numerical quadrature
- material properties will now be piece-wise constant, that is, the regions of the geometry (where material properties live) and the spatial mesh are two distinct concepts.

In [Step-2](Step2.md), we code a class for the MESH
- we handle material property layout
- mesh generation, including the use non-uniform meshes 


In [Step-3](Step3.md), we introduce other boundary conditions and code a class for the Finite-Element Method

In [Step-4](Step4.md), we verify the implementation:
- exact P1 reference matrices and load vector
- boundary-condition signs and symmetry
- an affine patch test
- convergence against an analytical solution

## Notation bridge to the lecture notes

Notebook `sigma` or `siga` is the reaction/absorption coefficient $\Sigma_a$ in the lecture notes; `q` or `src` is the source; `gn` is the element local-to-global node map; and `J=dx/2` is the 1D mapping Jacobian. Boundary data use the outward-current convention documented in Step-3.
