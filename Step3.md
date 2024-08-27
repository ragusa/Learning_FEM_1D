# Step-3

**For equations to display correctly online, you may have to click the `Raw` button**.

**However, you need to exit `Raw` mode to display the Jupyter Notebook correctly.**

In [the Step-3 Jupyter Notebook](./Learning_FEM_1D_step3.ipynb), we continue using **classes**.
- we discuss additional boundary conditions (namely, Neumann BC and Robin BC in addition to the Dirichlet BC we have been using so far)
- we code an FEM class

## Boundary Conditions

### Boundary Conditions in 1D Finite Element Method (FEM) Codes: Dirichlet, Neumann, and Robin

Boundary conditions are essential in solving differential equations, as they define the behavior of the solution at the boundaries of the domain. In the context of the 1D Finite Element Method (FEM), boundary conditions are used to specify how the solution behaves at the edges of the domain (e.g., at \(x = 0\) and \(x = L\), where \(L\) is the length of the domain). The three most common types of boundary conditions are Dirichlet, Neumann, and Robin. Below is a detailed explanation of each type.

#### 1. **Dirichlet Boundary Conditions**

Dirichlet boundary conditions specify the value of the solution directly at the boundary. In mathematical terms, if \( u(x) \) is the solution to the differential equation, a Dirichlet boundary condition at \( x = 0 \) would be of the form:

\[ u(0) = u_0 \]

where \( u_0 \) is a known constant.

**Example Application in 1D FEM:**
In a 1D FEM code, applying a Dirichlet boundary condition means directly setting the value of the solution vector at the boundary node to the specified value. For instance, if your domain is divided into finite elements with nodes at \( x = 0 \) and \( x = L \), you would enforce the Dirichlet condition by modifying the global stiffness matrix and load vector to ensure that the solution at the boundary node equals \( u_0 \).

**Implementation Tip:**
In practice, this might involve setting the corresponding row in the stiffness matrix to zero (except for the diagonal element) and setting the load vector entry to the value \( u_0 \). The stiffness matrix entry at the boundary node's diagonal would typically be set to 1 to maintain matrix stability.

#### 2. **Neumann Boundary Conditions**

Neumann boundary conditions specify the value of the derivative (flux) of the solution at the boundary. For example, a Neumann boundary condition at \( x = 0 \) might be expressed as:

\[ \frac{du}{dx}\bigg|_{x=0} = q_0 \]

where \( q_0 \) is a known constant flux.

**Example Application in 1D FEM:**
In 1D FEM, Neumann boundary conditions are applied by adjusting the load vector. The specified flux \( q_0 \) is added directly to the load vector entry corresponding to the boundary node. This reflects the influence of the boundary condition on the overall solution.

**Implementation Tip:**
To implement a Neumann boundary condition, simply modify the load vector entry at the boundary node to include the contribution from the boundary flux. The stiffness matrix remains unchanged.

#### 3. **Robin Boundary Conditions**

Robin boundary conditions are a combination of Dirichlet and Neumann boundary conditions. They specify a linear combination of the solution and its derivative at the boundary. The general form of a Robin boundary condition at \( x = 0 \) is:

\[ \alpha u(0) + \beta \frac{du}{dx}\bigg|_{x=0} = \gamma \]

where \( \alpha \), \( \beta \), and \( \gamma \) are known constants.

**Example Application in 1D FEM:**
In 1D FEM, Robin boundary conditions require modifying both the stiffness matrix and the load vector. The boundary condition introduces additional terms that couple the solution and its derivative, and these must be incorporated into the global system of equations.

**Implementation Tip:**
To implement a Robin boundary condition:
- Adjust the stiffness matrix at the boundary node by adding a term proportional to \( \alpha \).
- Modify the load vector entry by adding a term proportional to \( \gamma \) and possibly \( \beta \) depending on the discretization scheme.

### Summary

In summary, each type of boundary condition in 1D FEM—Dirichlet, Neumann, and Robin—has a specific role in defining the behavior of the solution at the domain boundaries:

- **Dirichlet:** Directly sets the solution value at the boundary.
- **Neumann:** Specifies the derivative (flux) of the solution at the boundary.
- **Robin:** Combines both the solution value and its derivative at the boundary.

Correctly implementing these boundary conditions is crucial for ensuring the accuracy and stability of the FEM solution. Understanding how to apply them in a 1D FEM code lays the groundwork for extending these concepts to more complex problems and higher dimensions.

## A class for the FEM solver

### Assignment: Implementing a 1D Finite Element Method (FEM) Solver

#### Objective:
In this assignment, you will develop a Python class called `FEM_solver` that implements the Finite Element Method (FEM) for solving 1D partial differential equations (PDEs). The class will be capable of generating elemental matrices, assembling the global system, applying various boundary conditions, and solving the system of equations. You will also implement methods to ensure the class handles different types of boundary conditions, such as Dirichlet, Neumann, and Robin.

#### Requirements:

1. **Class Definition**:
   - Create a Python class named `FEM_solver`.

2. **Constructor (`__init__` method)**:
   - The constructor should take the following parameters:
     - `degree`: An integer specifying the degree of the quadrature for numerical integration. Default is 3.
     - `verbose`: A boolean flag (`True` or `False`) that controls the verbosity of the output. When `True`, additional information about the matrices and vectors should be printed.
   - Inside the constructor:
     - Initialize the basis functions and their derivatives using the `basis()` method.
     - Compute the elemental matrices by calling the `compute_elemental_matrices(degree)` method.

3. **Basis Functions (`basis` method)**:
   - Implement a method named `basis` that defines:
     - `self.b`: A list of lambda functions representing the basis functions over the reference element \([-1, 1]\).
     - `self.dbdx`: A list of lambda functions representing the derivatives of the basis functions with respect to the spatial coordinate.

4. **Elemental Matrices (`compute_elemental_matrices` method)**:
   - Implement a method named `compute_elemental_matrices(degree)` that:
     - Uses Gauss-Legendre quadrature of the specified `degree` to integrate the basis functions and their derivatives.
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
     - Applies Dirichlet, Neumann, and Robin boundary conditions as specified in the `bc` dictionary.
     - Modifies `A` and `rhs` in place according to the boundary conditions.
     - Returns the modified `A` and `rhs`.

7. **System Solver (`solve_system` method)**:
   - Implement a method named `solve_system(A, b)` that:
     - Solves the linear system \(Ax = b\) using a sparse solver and returns the solution vector.

8. **Testing the Class**:
   - After implementing the `FEM_solver` class, create a test case with a simple mesh and material properties.
   - Define boundary conditions (Dirichlet, Neumann, and Robin) and verify that the class handles them correctly.
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
- Pay close attention to the application of boundary conditions, as this is crucial for the correct behavior of the FEM solver.
- Test your class thoroughly with different configurations to ensure it handles various scenarios as expected.
- Use the `verbose` option to debug and verify the internal calculations of the class.
