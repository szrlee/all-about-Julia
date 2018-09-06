# all-about-Julia
notes on learning [Julia](https://julialang.org/)

## Installation
Download suitable package in this [page](https://julialang.org/downloads/).
Follow the instructions to install Julia.

## [IJulia](https://github.com/JuliaLang/IJulia.jl)
IJulia is a Julia-language backend combined with the [Jupyter](https://jupyter.org) interactive environment (also used by [IPython](http://ipython.org/)). This combination allows you to interact with the Julia language using Jupyter/IPython's powerful [graphical notebook](http://ipython.org/notebook.html), which combines code, formatted text, math, and multimedia in a single document.

Before installing IJulia and Jupyter Notetbook, we need conda environment for Julia, which is managed by [Conda.jl](https://github.com/JuliaPy/Conda.jl).

### Using a pre-existing Conda installation

A simple way for conda and jupyter notebook configuration is to use Jupyter noterbook from [Anaconda](http://continuum.io/downloads).
distribution.

To use this Conda installation, ~~first create an environment for
`Conda.jl` and then~~ set the `CONDA_JL_HOME` environment variable to the full
path of the environment.
Then build `Conda.jl` and many of the packages that use it after this.
So as to install their dependancies to the specified enviroment.

```shell
conda create -n conda_jl python conda
```
The above command may finally lead to some error issue on windows and it is not neccessery.

```shell
export CONDA_JL_HOME="/path/to/miniconda/envs/conda_jl" # in Linux
SET CONDA_JL_HOME=\\path\\to\\miniconda\\envs\\conda_jl" # in Windows
julia -e 'Pkg.build("Conda")'
```

Check if the path to conda is expected with the following command in Julia cmd.
```julia
using Conda
Comnda.ROOTENV
```

Then you can directly add and build IJulia.
```julia
using Pkg
Pkg.add("IJulia")
Pkg.build("IJulia")
```

You can simply open the interactive Jupyter Notebook with the julia kernel.
```julia
using IJulia
notebook()
```

## Grammer

### Comparasion to MATLAB and Python
Acknowledge to this [cheetsheet](https://github.com/QuantEcon/QuantEcon.cheatsheet/blob/master/index.rst)

#### Creating Vectors

    +----------------------------+---------------------------+----------------------------------------+---------------------------+
    |         Operation          |           MATLAB          |                 Python                 |           Julia           |
    +============================+===========================+========================================+===========================+
    |                            | .. code-block:: matlab    | .. code-block:: python                 | .. code-block:: julia     |
    |                            |                           |                                        |                           |
    | Row vector: size (1, n)    |     A = [1 2 3]           |  A = np.array([1, 2, 3]).reshape(1, 3) |     A = [1 2 3]           |
    +----------------------------+---------------------------+----------------------------------------+---------------------------+
    |                            | .. code-block:: matlab    | .. code-block:: python                 | .. code-block:: julia     |
    |                            |                           |                                        |                           |
    | Column vector: size (n, 1) |     A = [1; 2; 3]         |  A = np.array([1, 2, 3]).reshape(3, 1) |     A = [1 2 3]'          |
    +----------------------------+---------------------------+----------------------------------------+---------------------------+
    |                            | Not possible              | .. code-block:: python                 | .. code-block:: julia     |
    |                            |                           |                                        |                           |
    | 1d array: size (n, )       |                           |  A = np.array([1, 2, 3])               |     A = [1; 2; 3]         |
    |                            |                           |                                        |                           |
    |                            |                           |                                        | or                        |
    |                            |                           |                                        |                           |
    |                            |                           |                                        | .. code-block:: julia     |
    |                            |                           |                                        |                           |
    |                            |                           |                                        |     A = [1, 2, 3]         |
    +----------------------------+---------------------------+----------------------------------------+---------------------------+
    |                            | .. code-block:: matlab    | .. code-block:: python                 | .. code-block:: julia     |
    |                            |                           |                                        |                           |
    | Integers from j to n with  |     A = j:k:n             |  A = np.arange(j, n+1, k)              |     A = j:k:n             |
    | step size k                |                           |                                        |                           |
    +----------------------------+---------------------------+----------------------------------------+---------------------------+
    |                            | .. code-block:: matlab    | .. code-block:: python                 | .. code-block:: julia     |
    |                            |                           |                                        |                           |
    | Linearly spaced vector     |     A = linspace(1, 5, k) |  A = np.linspace(1, 5, k)              |     A = linspace(1, 5, k) |
    | of k points                |                           |                                        |                           |
    +----------------------------+---------------------------+----------------------------------------+---------------------------+



#### Creating Matrices

    +--------------------------------+--------------------------+----------------------------------+--------------------------+
    | Operation                      |         MATLAB           | Python                           | Julia                    |
    +================================+==========================+==================================+==========================+
    |                                | .. code-block:: matlab   | .. code-block:: python           | .. code-block:: julia    |
    |                                |                          |                                  |                          |
    | Create a matrix                |     A = [1 2; 3 4]       |   A = np.array([[1, 2], [3, 4]]) |     A = [1 2; 3 4]       |
    +--------------------------------+--------------------------+----------------------------------+--------------------------+
    |                                | .. code-block:: matlab   | .. code-block:: python           | .. code-block:: julia    |
    |                                |                          |                                  |                          |
    | 2 x 2 matrix of zeros          |     A = zeros(2, 2)      |   A = np.zeros((2, 2))           |     A = zeros(2, 2)      |
    +--------------------------------+--------------------------+----------------------------------+--------------------------+
    |                                | .. code-block:: matlab   | .. code-block:: python           | .. code-block:: julia    |
    |                                |                          |                                  |                          |
    | 2 x 2 matrix of ones           |     A = ones(2, 2)       |   A = np.ones((2, 2))            |     A = ones(2, 2)       |
    +--------------------------------+--------------------------+----------------------------------+--------------------------+
    |                                | .. code-block:: matlab   | .. code-block:: python           | .. code-block:: julia    |
    |                                |                          |                                  |                          |
    | 2 x 2 identity matrix          |     A = eye(2, 2)        |   A = np.eye(2)                  |     A = eye(2, 2)        |
    +--------------------------------+--------------------------+----------------------------------+--------------------------+
    |                                | .. code-block:: matlab   | .. code-block:: python           | .. code-block:: julia    |
    |                                |                          |                                  |                          |
    | Diagonal matrix                |     A = diag([1 2 3])    |   A = np.diag([1, 2, 3])         |     A = diagm([1; 2; 3]) |
    +--------------------------------+--------------------------+----------------------------------+--------------------------+
    |                                | .. code-block:: matlab   | .. code-block:: python           | .. code-block:: julia    |
    |                                |                          |                                  |                          |
    | Uniform random numbers         |     A = rand(2, 2)       |   A = np.random.rand(2, 2)       |     A = rand(2, 2)       |
    +--------------------------------+--------------------------+----------------------------------+--------------------------+
    |                                | .. code-block:: matlab   | .. code-block:: python           | .. code-block:: julia    |
    |                                |                          |                                  |                          |
    | Normal random numbers          |     A = randn(2, 2)      |   A = np.random.randn(2, 2)      |     A = randn(2, 2)      |
    +--------------------------------+--------------------------+----------------------------------+--------------------------+



#### Manipulating Vectors and Matrices

    +--------------------------------+-------------------------------+---------------------------+---------------------------+
    | Operation                      |         MATLAB                | Python                    | Julia                     |
    +================================+===============================+===========================+===========================+
    |                                | .. code-block:: matlab        | .. code-block:: python    | .. code-block:: julia     |
    |                                |                               |                           |                           |
    | Transpose                      |     A.'                       |   A.T                     |     A.'                   |
    +--------------------------------+-------------------------------+---------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python    | .. code-block:: julia     |
    | Complex conjugate transpose    |                               |                           |                           |
    |                                |     A'                        |   A.conj()                |     A'                    |
    +--------------------------------+-------------------------------+---------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python    | .. code-block:: julia     |
    |                                |                               |                           |                           |
    | Concatenate horizontally       |     A = [[1 2] [1 2]]         |    B = np.array([1, 2])   |     A = [[1 2] [1 2]]     |
    |                                |                               |    A = np.hstack((B, B))  |                           |
    |                                | or                            |                           | or                        |
    |                                |                               |                           |                           |
    |                                | .. code-block:: matlab        |                           | .. code-block:: julia     |
    |                                |                               |                           |                           |
    |                                |     A = horzcat([1 2], [1 2]) |                           |    A = hcat([1 2], [1 2]) |
    +--------------------------------+-------------------------------+---------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python    | .. code-block:: julia     |
    |                                |                               |                           |                           |
    | Concatenate vertically         |     A = [[1 2]; [1 2]]        |    B = np.array([1, 2])   |     A = [[1 2]; [1 2]]    |
    |                                |                               |    A = np.vstack((B, B))  |                           |
    |                                | or                            |                           | or                        |
    |                                |                               |                           |                           |
    |                                | .. code-block:: matlab        |                           | .. code-block:: julia     |
    |                                |                               |                           |                           |
    |                                |     A = vertcat([1 2], [1 2]) |                           |    A = vcat([1 2], [1 2]) |
    +--------------------------------+-------------------------------+---------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python    | .. code-block:: julia     |
    |                                |                               |                           |                           |
    | Reshape (to 5 rows, 2 columns) |    A = reshape(1:10, 5, 2)    |    A = A.reshape(5, 2)    |    A = reshape(1:10, 5, 2)|
    +--------------------------------+-------------------------------+---------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python    | .. code-block:: julia     |
    |                                |                               |                           |                           |
    | Convert matrix to vector       |    A(:)                       |    A = A.flatten()        |    A[:]                   |
    +--------------------------------+-------------------------------+---------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python    | .. code-block:: julia     |
    |                                |                               |                           |                           |
    | Flip left/right                |    fliplr(A)                  |    np.fliplr(A)           |    flipdim(A, 2)          |
    +--------------------------------+-------------------------------+---------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python    | .. code-block:: julia     |
    |                                |                               |                           |                           |
    | Flip up/down                   |    flipud(A)                  |    np.flipud(A)           |    flipdim(A, 1)          |
    +--------------------------------+-------------------------------+---------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python    | .. code-block:: julia     |
    |                                |                               |                           |                           |
    | Repeat matrix (3 times in the  |    repmat(A, 3, 4)            |    np.tile(A, (4, 3))     |    repmat(A, 3, 4)        |
    | row dimension, 4 times in the  |                               |                           |                           |
    | column dimension)              |                               |                           |                           |
    +--------------------------------+-------------------------------+---------------------------+---------------------------+



#### Accessing Vector/Matrix Elements

    +--------------------------------+-------------------------------+-------------------------------+---------------------------+
    | Operation                      |         MATLAB                | Python                        | Julia                     |
    +================================+===============================+===============================+===========================+
    |                                | .. code-block:: matlab        | .. code-block:: python        | .. code-block:: julia     |
    |                                |                               |                               |                           |
    | Access one element             |     A(2, 2)                   |    A[1, 1]                    |     A[2, 2]               |
    +--------------------------------+-------------------------------+-------------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python        | .. code-block:: julia     |
    |                                |                               |                               |                           |
    | Access specific rows           |    A(1:4, :)                  |    A[0:4, :]                  |    A[1:4, :]              |
    +--------------------------------+-------------------------------+-------------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python        | .. code-block:: julia     |
    |                                |                               |                               |                           |
    | Access specific columns        |    A(:, 1:4)                  |    A[:, 0:4]                  |    A[:, 1:4]              |
    +--------------------------------+-------------------------------+-------------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python        | .. code-block:: julia     |
    |                                |                               |                               |                           |
    | Remove a row                   |    A([1 2 4], :)              |    A[[0, 1, 3], :]            |    A[[1, 2, 4], :]        |
    +--------------------------------+-------------------------------+-------------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python        | .. code-block:: julia     |
    |                                |                               |                               |                           |
    | Diagonals of matrix            |    diag(A)                    |    np.diag(A)                 |    diag(A)                |
    +--------------------------------+-------------------------------+-------------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python        | .. code-block:: julia     |
    |                                |                               |                               |                           |
    | Get dimensions of matrix       |    [nrow ncol] = size(A)      |    nrow, ncol = np.shape(A)   |    nrow, ncol = size(A)   |
    +--------------------------------+-------------------------------+-------------------------------+---------------------------+



#### Mathematical Operations

    +--------------------------------+-------------------------------+--------------------------------+---------------------------+
    | Operation                      |         MATLAB                | Python                         | Julia                     |
    +================================+===============================+================================+===========================+
    |                                | .. code-block:: matlab        | .. code-block:: python3        | .. code-block:: julia     |
    |                                |                               |                                |                           |
    | Dot product                    |     dot(A, B)                 |    np.dot(A, B) or A @ B       |     dot(A, B)             |
    +--------------------------------+-------------------------------+--------------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python3        | .. code-block:: julia     |
    |                                |                               |                                |                           |
    | Matrix multiplication          |     A * B                     |     A @ B                      |     A * B                 |
    +--------------------------------+-------------------------------+--------------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python         | .. code-block:: julia     |
    |                                |                               |                                |                           |
    | Element-wise multiplication    |     A .* B                    |    A * B                       |     A .* B                |
    +--------------------------------+-------------------------------+--------------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python         | .. code-block:: julia     |
    |                                |                               |                                |                           |
    | Matrix to a power              |     A^2                       |    np.linalg.matrix_power(A, 2)|     A^2                   |
    +--------------------------------+-------------------------------+--------------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python         | .. code-block:: julia     |
    |                                |                               |                                |                           |
    | Matrix to a power, elementwise |     A.^2                      |    A**2                        |     A.^2                  |
    +--------------------------------+-------------------------------+--------------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python         | .. code-block:: julia     |
    |                                |                               |                                |                           |
    | Inverse                        |     inv(A)                    |    np.linalg.inv(A)            |     inv(A)                |
    |                                |                               |                                |                           |
    |                                | or                            |                                | or                        |
    |                                |                               |                                |                           |
    |                                | .. code-block:: matlab        |                                | .. code-block:: julia     |
    |                                |                               |                                |                           |
    |                                |     A^(-1)                    |                                |    A^(-1)                 |
    +--------------------------------+-------------------------------+--------------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python         | .. code-block:: julia     |
    |                                |                               |                                |                           |
    | Determinant                    |     det(A)                    |    np.linalg.det(A)            |     det(A)                |
    +--------------------------------+-------------------------------+--------------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python         | .. code-block:: julia     |
    |                                |                               |                                |                           |
    | Eigenvalues and eigenvectors   |     [vec, val] = eig(A)       |    val, vec = np.linalg.eig(A) |     val, vec = eig(A)     |
    +--------------------------------+-------------------------------+--------------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python         | .. code-block:: julia     |
    |                                |                               |                                |                           |
    | Euclidean norm                 |     norm(A)                   |    np.linalg.norm(A)           |     norm(A)               |
    +--------------------------------+-------------------------------+--------------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python         | .. code-block:: julia     |
    |                                |                               |                                |                           |
    | Solve linear system            |     A\b                       |    np.linalg.solve(A, b)       |     A\b                   |
    | :math:`Ax=b` (when :math:`A`   |                               |                                |                           |
    | is square)                     |                               |                                |                           |
    +--------------------------------+-------------------------------+--------------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python         | .. code-block:: julia     |
    |                                |                               |                                |                           |
    | Solve least squares problem    |     A\b                       |    np.linalg.lstsq(A, b)       |     A\b                   |
    | :math:`Ax=b` (when :math:`A`   |                               |                                |                           |
    | is rectangular)                |                               |                                |                           |
    +--------------------------------+-------------------------------+--------------------------------+---------------------------+



#### Sum / max / min

    +--------------------------------+-------------------------------+---------------------------------+---------------------------+
    | Operation                      |         MATLAB                | Python                          | Julia                     |
    +================================+===============================+=================================+===========================+
    |                                | .. code-block:: matlab        | .. code-block:: python          | .. code-block:: julia     |
    |                                |                               |                                 |                           |
    | Sum / max / min of             |     sum(A, 1)                 |    sum(A, 0)                    |     sum(A, 1)             |
    | each column                    |     max(A, [], 1)             |    np.amax(A, 0)                |     maximum(A, 1)         |
    |                                |     min(A, [], 1)             |    np.amin(A, 0)                |     minimum(A, 1)         |
    +--------------------------------+-------------------------------+---------------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python          | .. code-block:: julia     |
    |                                |                               |                                 |                           |
    | Sum / max / min of each row    |     sum(A, 2)                 |    sum(A, 1)                    |     sum(A, 2)             |
    |                                |     max(A, [], 2)             |    np.amax(A, 1)                |     maximum(A, 2)         |
    |                                |     min(A, [], 2)             |    np.amin(A, 1)                |     minimum(A, 2)         |
    +--------------------------------+-------------------------------+---------------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python          | .. code-block:: julia     |
    |                                |                               |                                 |                           |
    | Sum / max / min of             |     sum(A(:))                 |    np.sum(A)                    |     sum(A)                |
    | entire matrix                  |     max(A(:))                 |    np.amax(A)                   |     maximum(A)            |
    |                                |     min(A(:))                 |    np.amin(A)                   |     minimum(A)            |
    +--------------------------------+-------------------------------+---------------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python          | .. code-block:: julia     |
    |                                |                               |                                 |                           |
    | Cumulative sum / max / min     |     cumsum(A, 1)              |    np.cumsum(A, 0)              |     cumsum(A, 1)          |
    | by row                         |     cummax(A, 1)              |    np.maximum.accumulate(A, 0)  |     cummax(A, 1)          |
    |                                |     cummin(A, 1)              |    np.minimum.accumulate(A, 0)  |     cummin(A, 1)          |
    +--------------------------------+-------------------------------+---------------------------------+---------------------------+
    |                                | .. code-block:: matlab        | .. code-block:: python          | .. code-block:: julia     |
    |                                |                               |                                 |                           |
    | Cumulative sum / max / min     |     cumsum(A, 2)              |    np.cumsum(A, 1)              |     cumsum(A, 2)          |
    | by column                      |     cummax(A, 2)              |    np.maximum.accumulate(A, 1)  |     cummax(A, 2)          |
    |                                |     cummin(A, 2)              |    np.minimum.accumulate(A, 1)  |     cummin(A, 2)          |
    +--------------------------------+-------------------------------+---------------------------------+---------------------------+



#### Programming

    +------------------------+----------------------------+----------------------------+-------------------------------+
    | Operation              |         MATLAB             | Python                     | Julia                         |
    +========================+============================+============================+===============================+
    |                        | .. code-block:: matlab     | .. code-block:: python     | .. code-block:: julia         |
    |                        |                            |                            |                               |
    | Comment one line       |     % This is a comment    |    # This is a comment     |     # This is a comment       |
    +------------------------+----------------------------+----------------------------+-------------------------------+
    |                        | .. code-block:: matlab     | .. code-block:: python     | .. code-block:: julia         |
    |                        |                            |                            |                               |
    | Comment block          |     %{                     |    # Block                 |     #=                        |
    |                        |     Comment block          |    # comment               |     Comment block             |
    |                        |     %}                     |    # following PEP8        |     =#                        |
    +------------------------+----------------------------+----------------------------+-------------------------------+
    |                        | .. code-block:: matlab     | .. code-block:: python     | .. code-block:: julia         |
    |                        |                            |                            |                               |
    | For loop               |     for i = 1:N            |    for i in range(n):      |     for i = 1:N               |
    |                        |        % do something      |        # do something      |        # do something         |
    |                        |     end                    |                            |     end                       |
    +------------------------+----------------------------+----------------------------+-------------------------------+
    |                        | .. code-block:: matlab     | .. code-block:: python     | .. code-block:: julia         |
    |                        |                            |                            |                               |
    | While loop             |     while i <= N           |    while i <= N:           |     while i <= N              |
    |                        |        % do something      |        # do something      |        # do something         |
    |                        |     end                    |                            |     end                       |
    +------------------------+----------------------------+----------------------------+-------------------------------+
    |                        | .. code-block:: matlab     | .. code-block:: python     | .. code-block:: julia         |
    |                        |                            |                            |                               |
    | If                     |     if i <= N              |    if i <= N:              |     if i <= N                 |
    |                        |        % do something      |       # do something       |        # do something         |
    |                        |     end                    |                            |     end                       |
    +------------------------+----------------------------+----------------------------+-------------------------------+
    |                        | .. code-block:: matlab     | .. code-block:: python     | .. code-block:: julia         |
    |                        |                            |                            |                               |
    | If / else              |     if i <= N              |   if i <= N:               |    if i <= N                  |
    |                        |        % do something      |       # do something       |       # do something          |
    |                        |     else                   |   else:                    |    else                       |
    |                        |        % do something else |       # so something else  |       # do something else     |
    |                        |     end                    |                            |    end                        |
    +------------------------+----------------------------+----------------------------+-------------------------------+
    |                        | .. code-block:: matlab     | .. code-block:: python     | .. code-block:: julia         |
    |                        |                            |                            |                               |
    | Print text and variable|     x = 10                 |   x = 10                   |    x = 10                     |
    |                        |     fprintf('x = %d \n', x)|   print(f'x = {x}')        |    println("x = $x")          |
    +------------------------+----------------------------+----------------------------+-------------------------------+
    |                        | .. code-block:: matlab     | .. code-block:: python     | .. code-block:: julia         |
    |                        |                            |                            |                               |
    | Function: one line/    |     f = @(x) x^2           |    f = lambda x: x**2      |     f(x) = x^2                |
    | anonymous              |                            |                            |                               |
    +------------------------+----------------------------+----------------------------+-------------------------------+
    |                        | .. code-block:: matlab     | .. code-block:: python     | .. code-block:: julia         |
    |                        |                            |                            |                               |
    | Function: multiple     |     function out  = f(x)   |    def f(x):               |     function f(x)             |
    | lines                  |        out = x^2           |        return x**2         |        return x^2             |
    |                        |     end                    |                            |     end                       |
    +------------------------+----------------------------+----------------------------+-------------------------------+


In the Python code we assume that you have already run `import numpy as np`
