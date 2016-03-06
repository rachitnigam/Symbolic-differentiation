## Symbolic-differentiation
A racket based based symbolic differentiator which can give the nth partial derivative of symbolic function. The function skeleton is inspired from specifications given in [SICP](https://en.wikipedia.org/wiki/Structure_and_Interpretation_of_Computer_Programs) sections 2.56 - 2.58.

####What is this?
This is a program which takes in a symbolic representation of a mathematical function and produces the corresponding derivative with respect to a given variable. Specific details about this implementation are given below. 

The differentiation function belongs to a special type of function which interact with [S-expressions](https://en.wikipedia.org/wiki/S-expression). A S-expression is a special type of expression with can either be composed of atoms (smallest expression in the language) or S-expressions themselves. S-expressions were first introduced in [John McCarthy's paper](http://www-formal.stanford.edu/jmc/recursive.ps) which explained the formal specifications of the interpreter for LISP.

####Current status
Here is a list of the functions and operations currently supported in the program.

#####**Binary Operations**
* + (Addition) : Supported 
* * (Multiplication) : Supported
* ^ (Exponentiation): Supported
* / (Division): Not supported
* - (Substration): Not supported

#####**Mathematical Functions**
* sin : Trigonometric sine
* cos : Trigonometric cosine
* tan : Trigonometric tangent
* cot : Trigonometric cotangent
* sec : Trigonometric secant
* csc : Trigonometric cosecant
* log : Logarithm to the specified base
* ln : Natural logarithm (base e)


####To do list
- Create a proper setup and tutorial
- Break up the program into smaller modules
- optimize the code, remove redundant calls to functions
- Write a parser for better interaction
- Set up an interactive environment
- Write an algebraic simplifier to make results more readable
