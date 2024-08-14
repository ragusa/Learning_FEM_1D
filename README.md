# Learning_FEM_1D
Learning the Finite Element Method in 1D

We  solve a diffusion-reaction equation, with a source term, in 1D.


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

