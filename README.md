# First Order Logic - Sympy library extension

Introduction of classes (variables, quantifiers, predicates and clauses) and methods for FOL by extending the Propositional Logic semantics and methods that are built-in in the Sympy library. In particular, the project concerns the implementation of a method for getting the grounding of a FOL formula, a method for finding the most general unifier between two terms and a method for applying the Binary Resolution Rule on two clauses.
The whole implementation is contained in the Python notebook. It also includes a user-friendly interface (that uses LaTeX) for the visualization of the formulas. 

This work is an extention of [Filip Šamánek - "Implementation of FOL algorithms for the SymPy library" (2019)](https://dspace.cvut.cz/handle/10467/83057?show=full).

In the Sympy library we can already find a module for Propositional Logic. The aim of the project is to expand the library to support FOL: see the diagram below.

<p align="center">
  <img src="https://github.com/silviapoletti/First-Order-Logic-Sympy-library-extension/blob/7f141a691c3d8cc51c54692ac8896aeeb1364778/extension_diagram.png" width="70%"/>
</p>

### Grounding
A formula is ground when it does not contain any variable. For grounding we only consider closed formulas in which there are no free occurrences of variables. The following shows how grounding is performed in case of a universal quantifier and an existential quantifier:
$$\forall x\phi(x)\rightarrow \phi(t_1) \land \phi(t_2) \land \dots \land \phi(t_n) $$
$$\exist x\phi(x)\rightarrow \phi(t_1) \lor \phi(t_2) \lor \dots \lor \phi(t_n) $$
In this way we're able to reduce FOL formulas to propositional logic formulas by considering conjunctions and disjunctions.
