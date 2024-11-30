# coe352_project1

# Spring-Mass System Simulation and SVD Decomposition

This project contains two problems that utilize Python to solve structural and mathematical problems using **Singular Value Decomposition (SVD)** and matrix manipulations. The focus is on analyzing spring-mass systems and performing custom SVD computations.

---

## Problem 1: Custom Singular Value Decomposition (SVD)

### Description
This problem involves implementing a custom SVD function to:
1. Decompose a given matrix into its singular components (U, Σ, V^T).
2. Calculate the pseudo-inverse of the matrix.
3. Compute the condition number (L2 norm).
4. Reconstruct the original matrix using the SVD components.


### Discrepancy between custom and NumPy SVD:
The negative sign difference between the custom SVD and Python’s SVD occurs because singular vectors can have their signs flipped without affecting the results. This is a normal part of SVD since the signs of the vectors are not unique. NumPy follows a consistent method for choosing signs, while a custom SVD might select different signs when calculating eigenvectors. This difference does not affect the correctness of the decomposition or the matrix reconstruction. If necessary, the signs can be adjusted to match NumPy’s output, or the difference can simply be noted as expected.

### Features
- **Custom Implementation**: Performs SVD from scratch using eigen decomposition.
- **Validation**: Compares the custom implementation with NumPy's built-in SVD.
- **Matrix Reconstruction**: Verifies correctness by reconstructing the matrix from its SVD components.

### Input
- A predefined 2D matrix to decompose.

### Output
- Left singular vectors (U)
- Singular values (Σ)
- Right singular vectors (V^T)
- Condition number
- Pseudo-inverse
- Matrix reconstruction comparison

### Example Output
```plaintext
Custom SVD U (Left Singular Vectors):
 [[ 0.17552297 -0.01988568 -0.98427448]
 [ 0.53923918  0.83841813  0.07922216]
 [ 0.82365818 -0.54466467  0.15788477]]
Custom SVD S (Singular Values Diagonal):
 [[13.5987949   0.          0.        ]
 [ 0.          5.36396002  0.        ]
 [ 0.          0.          0.54837044]]
Custom SVD V^T (Right Singular Vectors Transpose):
 [[ 0.59550034  0.68989106  0.41161835]
 [-0.08927469 -0.45237082  0.88735036]
 [ 0.79837922 -0.56516454 -0.20779717]]
Custom Condition Number: 24.798555729624834
Custom Pseudo-Inverse:
 [[-1.425  0.125  0.275]
 [ 1.025 -0.125 -0.075]
 [ 0.375  0.125 -0.125]]
Reconstructed Matrix (Custom SVD):
 [[1. 2. 1.]
 [4. 3. 7.]
 [7. 9. 2.]]
_________________________

NumPy SVD U:
 [[-0.17552297  0.01988568 -0.98427448]
 [-0.53923918 -0.83841813  0.07922216]
 [-0.82365818  0.54466467  0.15788477]]
NumPy SVD S (Singular Values Diagonal):
 [[13.5987949   0.          0.        ]
 [ 0.          5.36396002  0.        ]
 [ 0.          0.          0.54837044]]
NumPy SVD V^T:
 [[-0.59550034 -0.68989106 -0.41161835]
 [ 0.08927469  0.45237082 -0.88735036]
 [ 0.79837922 -0.56516454 -0.20779717]]
NumPy Pseudo-Inverse:
 [[-1.425  0.125  0.275]
 [ 1.025 -0.125 -0.075]
 [ 0.375  0.125 -0.125]]
NumPy Condition Number:
 24.798555729624677
Reconstructed Matrix (NumPy SVD):
 [[1. 2. 1.]
 [4. 3. 7.]
 [7. 9. 2.]]
```

---

## Problem 2: Spring-Mass System Analysis

### Description
This problem simulates a physical system of connected springs and masses subjected to gravitational forces. The analysis involves:
1. Calculating stiffness matrices.
2. Solving for displacements using SVD-based pseudo-inverse.
3. Computing elongations of springs and internal spring forces.

### Features
- **Interactive Input**: Accepts user-defined spring constants and masses.
- **Boundary Conditions**: Supports `Fixed-Free` and `Fixed-Fixed` configurations.
- **Matrix Operations**: Constructs matrices for connectivity, stiffness, and forces.
- **Custom SVD Integration**: Uses the custom SVD from Problem 1 to solve equations.

### Workflow
1. Gather inputs for the number of springs, masses, spring constants, and masses.
2. Construct connectivity, stiffness, and force matrices.
3. Calculate:
   - Displacements of the masses.
   - Elongations of springs.
   - Internal spring forces.

### Input
- Number of springs and masses.
- Boundary condition: `Fixed-Free` or `Fixed-Fixed`.
- Spring constants and mass values.

### Output
- Connectivity matrix
- Spring constant matrix
- Force vector
- Stiffness matrix
- Displacement vector
- Spring elongations
- Internal spring forces

### Example Output
```plaintext
Enter the number of springs:
3
Enter the number of masses:
6
Enter boundary condition (1 for Fixed-Free, 2 for Fixed-Fixed):
2
Invalid configuration. Adjust your inputs.
Enter the number of springs:
2
Enter the number of masses:
2
Enter boundary condition (1 for Fixed-Free, 2 for Fixed-Fixed):
2
Invalid configuration. Adjust your inputs.
Enter the number of springs:
2
Enter the number of masses:
2
Enter boundary condition (1 for Fixed-Free, 2 for Fixed-Fixed):
1
Enter the spring constant for spring 1:
2
Enter the spring constant for spring 2:
2
Enter the mass for mass 1:
2
Enter the mass for mass 2:
2

Connectivity Matrix A:
[[ 1.  0.]
 [-1.  1.]]

Spring Constant Matrix C:
[[2. 0.]
 [0. 2.]]

Force Vector:
[-19.62 -19.62]

Stiffness Matrix K:
[[ 4. -2.]
 [-2.  2.]]

Displacement Vector:
[-19.62 -29.43]

Spring Elongations:
[-19.62  -9.81]

Internal Spring Forces:
[-39.24 -19.62]
```

### Description of Boundary Conditions:

Having two free boundary conditions in a spring-mass system is impractical. A system with both ends free to move implies that neither end is anchored, making it impossible to establish a force balance. Without fixed points for the springs to push or pull against, the system cannot generate tension or perform work. As a result, the masses can move freely, leading to an unstable equilibrium. If the masses are displaced, they won’t return to their original positions since there’s no restoring force to counteract the motion. Springs inherently rely on fixed structures to exert forces, and a system without anchors fails to function as intended.

Similarly, a fixed-free system does not work when the number of springs equals the number of masses. An additional spring is required to anchor the system to a rigid body. This ensures the system is stabilized and capable of operating under expected conditions.

The free-free boundary condition fails not only from a physical perspective but also from a mathematical standpoint due to rank deficiency and non-uniqueness of solutions.

In a spring-mass system, the stiffness matrix K represents the relationship between forces and displacements. For the system to have a unique and stable solution, K must be full rank and invertible. When both ends of the system are free, the stiffness matrix becomes rank-deficient because the system lacks sufficient constraints to anchor the forces. This results in linearly dependent rows or columns in K, meaning it cannot span the full solution space. As a consequence, K is singular and does not have a unique inverse.

Mathematically, this rank deficiency leads to non-unique solutions for the displacement vector. Any applied force would result in an infinite number of possible displacements because there are no fixed points to provide the necessary constraints. This corresponds to an unstable system in which the masses can move freely in arbitrary directions without being restored to equilibrium.

In summary, the free-free boundary condition leads to a rank-deficient stiffness matrix, resulting in non-uniqueness of solutions and the inability to compute meaningful displacements.

---

## How to Run

1. **Dependencies**: Ensure you have Python installed along with the required libraries (`numpy`).
2. **Running the Code**:
   - For Problem 1:
     ```bash
     python problem_1_svd.py
     ```
   - For Problem 2:
     ```bash
     python problem_2_spring_mass.py
     ```

---

## Applications
- **Problem 1**: Useful in mathematical computations involving matrix decompositions, especially in machine learning and image processing.
- **Problem 2**: Simulates physical systems, aiding in structural engineering and mechanics studies.

---

## Future Enhancements
- Extend support for 3D spring-mass systems.
- Add visualization tools for spring-mass configurations.
- Optimize custom SVD implementation for larger matrices.

---

## License
This project is open-source under the MIT License.
