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


####Project Closed
**Reason:** The program uses a lot of levels of abstractions, making it impossible to fix bugs as I extend it. 
**Conclusions**
- Building abstractions is one of the most fundametal things in computer programming. This project was meant to demonstrate how building abstractions is useful in creating large programs.
- The program uses higher order functions to a great degree. I tried to extract as many common patterns as I could see and generalized them as higher order functions.
- Even though abstractions are important, building too many abstractions (especially in the same file) is not very benificial. A balance must be struck between abstraction and the clarity of code.
- In the end, the program was only ever meant to be a proof of concept and nothing more. It was not built with neither flexibilty nor extendility which became very obvious when I tried to extend it.
- Armed with all the knowledge of common patterns, higher order functions and symbolic expressions, I am sure I can build a more robust differentiator in the future. I might do so one day, but it is not a priority. 
