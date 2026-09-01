# Step-3

**For equations to display correctly online, you may have to click the `Raw` button**.

**However, you need to exit `Raw` mode to display the Jupyter Notebook correctly.**

In [the Step-3 Jupyter Notebook](./Learning_FEM_1D_step3.ipynb), we continue using **classes**.
- we discuss additional boundary conditions (namely, Neumann BC and Robin BC in addition to the Dirichlet BC we have been using so far)
- we code an FEM class

## Boundary Conditions

### Boundary Conditions in 1D Finite Element Method (FEM) Codes: Dirichlet, Neumann, and Robin

Boundary conditions describe the behavior of the solution at the two ends of the domain, $x=0$ and $x=L$. To keep the signs consistent at both ends, we formulate flux conditions using the **outward current**

$$
J_n=-D\frac{du}{dn},
$$

where $n$ is the outward unit normal. Thus $n=-1$ at the left boundary and $n=+1$ at the right boundary. After integration by parts, the weak-form boundary contribution is $-J_n v$.

#### 1. **Dirichlet Boundary Conditions**

Dirichlet boundary conditions prescribe the solution value directly. For example,

$$
u(0)=u_0.
$$

where $u_0$ is a known constant.

**Example Application in 1D FEM:**
The nodal value at the boundary is known rather than solved for. The algebraic system must be modified so that the corresponding solution entry equals the prescribed value.

**Implementation Tip:**
To preserve symmetry, first move the entire boundary column contribution to the right-hand side. Then zero both the boundary row and column, set the diagonal entry to one, and set the boundary right-hand-side entry to $u_0$. This is symmetric Dirichlet elimination.

#### 2. **Neumann Boundary Conditions**

Neumann boundary conditions prescribe the outward current. At either boundary, write

$$
J_n=\bar J_n.
$$

The outward normal is already included in $J_n$, so the same formula and sign convention apply at $x=0$ and $x=L$.

**Example Application in 1D FEM:**
Because the weak-form boundary contribution is $-J_n v$, a prescribed outward current contributes

$$
-\bar J_n
$$

to the right-hand-side entry associated with the boundary node. The stiffness matrix is unchanged.

**Implementation Tip:**
For a boundary dictionary of type `neumann`, interpret `value` as $\bar J_n$ and subtract it from the corresponding boundary entry of the load vector.

#### 3. **Robin Boundary Conditions**

Robin conditions relate the outward current and the solution. We use the form

$$
J_n=\alpha u-\beta,
$$

where $\alpha$ and $\beta$ are prescribed data.

**Example Application in 1D FEM:**
Substitution in the weak-form boundary term gives

$$
-J_n v=-\alpha u v+\beta v.
$$

After moving the term involving the unknown to the left-hand side, the Robin condition adds $\alpha$ to the boundary diagonal and $\beta$ to the boundary entry of the right-hand side.

**Implementation Tip:**
For a boundary dictionary of type `robin`, store `alpha` and `beta` explicitly. Add `alpha` to the boundary diagonal and `beta` to the load vector.

#### 4. **Incoming-current and vacuum conditions**

A frequently used special Robin condition is

$$
J_n=\frac12u-2J^{\mathrm{in}}.
$$

It therefore adds $1/2$ to the boundary diagonal and $2J^{\mathrm{in}}$ to the right-hand side. Vacuum is obtained by setting $J^{\mathrm{in}}=0$.

**Implementation Tip:**
The code uses the boundary type `incoming_current`, with `value` equal to $J^{\mathrm{in}}$. Keeping this special name avoids confusing it with a completely general Robin condition.

### Summary

- **Dirichlet:** Prescribes $u$ and is imposed by symmetric row-and-column elimination.
- **Neumann:** Prescribes outward current $J_n=\bar J_n$ and contributes $-\bar J_n$ to the load vector.
- **Robin:** Uses $J_n=\alpha u-\beta$, adding $\alpha$ to the matrix and $\beta$ to the load vector.
- **Incoming current:** Uses $J_n=\tfrac12u-2J^{\mathrm{in}}$; vacuum is the case $J^{\mathrm{in}}=0$.

These definitions match the weak form and use the same signs at both endpoints because the outward normal is included in $J_n$.

## A class for the FEM solver

### Assignment: Implementing a 1D Finite Element Method (FEM) Solver

#### Objective:
In this assignment, you will develop a Python class called `FEM_solver` that implements the Finite Element Method (FEM) for solving 1D partial differential equations (PDEs). The class will be capable of generating elemental matrices, assembling the global system, applying various boundary conditions, and solving the system of equations. You will also implement methods to ensure the class handles different types of boundary conditions, such as Dirichlet, Neumann, Robin, and incoming-current conditions.

#### Requirements:

1. **Class Definition**:
   - Create a Python class named `FEM_solver`.

2. **Constructor (`__init__` method)**:
   - The constructor should take the following parameters:
     - `n_q`: An integer specifying the number of Gauss points used for numerical integration. Default is 3.
     - `verbose`: A boolean flag (`True` or `False`) that controls the verbosity of the output. When `True`, additional information about the matrices and vectors should be printed.
   - Inside the constructor:
     - Initialize the basis functions and their derivatives using the `basis()` method.
     - Compute the elemental matrices by calling the `compute_elemental_matrices(n_q)` method.

3. **Basis Functions (`basis` method)**:
   - Implement a method named `basis` that defines:
     - `self.b`: A list of lambda functions representing the basis functions over the reference element \([-1, 1]\).
     - `self.dbdx`: A list of lambda functions representing the derivatives of the basis functions with respect to the reference coordinate.

4. **Elemental Matrices (`compute_elemental_matrices` method)**:
   - Implement a method named `compute_elemental_matrices(n_q)` that:
     - Uses Gauss-Legendre quadrature with `n_q` points to integrate the basis functions and their derivatives.
     - Computes the elemental stiffness matrix `self.Kxx`, the mass matrix `self.M`, and the load vector `self.Q`.
     - If `verbose` is `True`, prints the matrices and vector for debugging purposes.

5. **System Assembly (`assemble_system` method)**:
   - Implement a method named `assemble_system(mesh, prop, bc)` that:
     - Assembles the global stiffness matrix `A` and the global load vector `rhs` based on the properties of the mesh and materials.
     - Applies material properties and source terms from the `prop` dictionary.
     - Calls the `apply_boundary_conditions(A, rhs, bc, n_nodes)` method to apply boundary conditions.
     - Returns the assembled global stiffness matrix `A` and load vector `rhs`.

6. **Boundary Conditions (`apply_boundary_conditions` method)**:
   - Implement a method named `apply_boundary_conditions(A, rhs, bc, n_nodes)` that:
     - Applies Dirichlet, outward-current Neumann, general Robin, and incoming-current boundary conditions as specified in the `bc` dictionary.
     - Uses symmetric elimination for Dirichlet data and the sign convention defined above for natural boundary terms.
     - Modifies `A` and `rhs` in place according to the boundary conditions.
     - Returns the modified `A` and `rhs`.

7. **System Solver (`solve_system` method)**:
   - Implement a method named `solve_system(A, b)` that:
     - Solves the linear system \(Ax = b\) using a sparse solver and returns the solution vector.

8. **Testing the Class**:
   - After implementing the `FEM_solver` class, create a test case with a simple mesh and material properties.
   - Define boundary conditions (Dirichlet, Neumann, Robin, and incoming current) and verify that the class handles them correctly.
   - Use the `verbose=True` flag to check the output of the elemental matrices and the assembled system.

9. **Submission**:
   - Submit the Python file containing the `FEM_solver` class definition.
   - Include a short report (1-2 pages) explaining your implementation, the purpose of each method, and how boundary conditions are handled.
   - Provide examples of the test cases you used and their results.

#### Grading Criteria:
- **Correctness** (40%): Does the class correctly implement the FEM and handle the various boundary conditions?
- **Code Quality** (30%): Is the code well-organized, with clear naming conventions, comments, and appropriate use of functions?
- **Error Handling** (10%): Does the code appropriately handle invalid inputs and edge cases?
- **Documentation and Reporting** (20%): Is the class well-documented with meaningful docstrings? Does the report clearly explain the design and functionality?

---

### Tips:
- Pay close attention to the outward-current convention and the application of boundary conditions, as these are crucial for the correct behavior of the FEM solver.
- Test your class thoroughly with different configurations to ensure it handles various scenarios as expected.
- Use the `verbose` option to debug and verify the internal calculations of the class.
