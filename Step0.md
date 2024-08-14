# Step-0

In [Step-0](./Learning_FEM_1D_step0.ipynb), I provide a basic introduction. 

We  solve the following diffusion + reaction equation, with a source term:

$$ -\frac{d}{dx}D\frac{du}{dx} + \sigma u = q \qquad \forall x \in [0,L]$$

We will assume all material properties $D, \sigma,q$ to be constant for now.

We use the following boundary conditions:
* at $x=0$, we impose a value $ u(0)=u_{\text{left}}$
* at $x=L$, we impose a value $ u(L)=u_{\text{right}}$

These conditions are known as Dirichlet boundary conditions.

## Meshing the domain
The interval $[0,L]$ is meshed using $N$ cells. In FEM, meshcells are known as '*elements*'. The vertices of the mesh are denoted by $x_i$ and there are $N+1$ vertices. Obviously, $x_0=0$ and $x_N=L$.

## Choice of solution space
We will approximate the solution to the above problem as a **piece-wise linear** function. Between two consecutive vertices, the function is linear, as shown in the plot below.


Many FEM concepts such as the assembly of the global matrix per element, the reference element, ... are not discussed at first. 

At the end of the [Step-0](./Learning_FEM_1D_step0.ipynb) Jupyter Notebook, I do the global assembly by looping over elements.

