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
