# Step-2

**For equations to display correctly online, you may have to click the `Raw` button**.

**However, you need to exit `Raw` mode to display the Jupyter Notebook correctly.**

In [the Step-2 Jupyter Notebook](./Learning_FEM_1D_step2.ipynb),  we develop a class for the MESH
- we handle material property layout
- mesh generation, including the use non-uniform meshes 

### Assignment: Creating a 1D Mesh Class for Finite Element Method (FEM) Simulations

#### Objective:
In this assignment, you will create a Python class called `MESH` that is designed to generate a 1D mesh for use in a finite element method (FEM) simulation. The class will take inputs defining material regions and source distributions and will produce the mesh, assigning material and source IDs to each spatial cell. You will also implement logging to provide detailed output about the mesh generation process.

#### Requirements:

1. **Class Definition**:
   - Create a class named `MESH` in Python.

2. **Constructor (`__init__` method)**:
   - The constructor should take the following parameters:
     - `mat_layout`: A list of integers representing the material ID for each region.
     - `src_layout`: A list of integers representing the source ID for each region.
     - `width`: A list of floats representing the physical width of each region.
     - `n_ref`: A list of integers representing the number of spatial cells to divide each region into (refinement level).
     - `verbose`: A boolean flag (`True` or `False`) that controls the level of detail in the logging output. When `True`, the logger should output detailed debugging information.
   - **Input Validation**:
     - Ensure that `mat_layout`, `src_layout`, `width`, and `n_ref` are all lists of the same length. If they are not, raise a `ValueError` with an appropriate error message.
   - **Attributes**:
     - `self.n_zones`: The number of regions (derived from the length of `width`).
     - `self.n_cells`: The total number of spatial cells (sum of `n_ref`).
     - `self.dx`: A NumPy array containing the width of each spatial cell.
     - `self.J`: A NumPy array containing half of the width of each spatial cell.
     - `self.cell2mat`: A NumPy array mapping each cell ID to its corresponding material ID.
     - `self.cell2src`: A NumPy array mapping each cell ID to its corresponding source ID.
   - **Logging**:
     - Implement logging within the class using Python’s `logging` module.
     - Configure the logger to output messages at the `DEBUG` level if `verbose=True`, otherwise use the `INFO` level.
     - Log the range of cell IDs associated with each material region during mesh creation.

3. **Connectivity Method (`connectivity` method)**:
   - Implement a method named `connectivity` within the `MESH` class.
   - This method should create and assign a 2D NumPy array `self.gn` that maps each cell to its left and right neighbors.
   - Each row in `self.gn` should contain two integers: the current cell index and the next cell index.
   - Log the connectivity information for each cell using the logger.

4. **Additional Specifications**:
   - Ensure the class handles edge cases such as the first and last cells in the mesh correctly.
   - Use `numpy` for array operations.
   - Do not allow any cell indices to go out of bounds during connectivity generation.

5. **Testing the Class**:
   - After implementing the `MESH` class, create a test case with the following inputs:
     ```python
     mat_layout = [1, 2, 3]
     src_layout = [4, 5, 6]
     width = [10.0, 20.0, 15.0]
     n_ref = [5, 10, 7]
     ```
   - Instantiate the `MESH` class with `verbose=True`.
   - Ensure that the log outputs include details about the range of cells for each material and the connectivity information for each cell.

6. **Submission**:
   - Submit the Python file containing the `MESH` class definition.
   - Include a short report (1-2 pages) explaining your implementation, the purpose of each method, and how logging is handled.
   - Provide screenshots or log outputs demonstrating that the class behaves as expected with the provided test case.

#### Grading Criteria:
- **Correctness** (40%): Does the class correctly generate the mesh and connectivity based on the inputs?
- **Code Quality** (30%): Is the code well-organized, with clear naming conventions, comments, and appropriate use of logging?
- **Error Handling** (10%): Does the code appropriately handle invalid inputs and edge cases?
- **Documentation and Reporting** (20%): Is the class well-documented with meaningful docstrings? Does the report clearly explain the design and functionality?

---

### Tips:
- Make sure to test your class with different values of `mat_layout`, `src_layout`, `width`, and `n_ref` to ensure it handles various scenarios correctly.
- Think carefully about the structure of your connectivity matrix, especially how it will be used later in FEM simulations.
- Use the logging output to help debug and verify that each part of the mesh generation process is functioning as expected.
