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
$$\exists x\phi(x)\rightarrow \phi(t_1) \lor \phi(t_2) \lor \dots \lor \phi(t_n) $$ where $x$ is a variable symbol and $\phi(x)$ is a formula depending on $x$.

In this way we're able to reduce FOL formulas to propositional logic formulas by considering conjunctions and disjunctions.

Our implementation goes through the following steps: First we assume to have different symbols for each quantifier. Then we transform the formula in its Negated Normal Form by taking the negations in front of the quantifiers and dragging them in the internal formula and than transform the result in the Conjunctive Normal Form. Then we pass to the Prenex Normal Form by moving all the quantifiers at the beginning of the formula, taking care of not changing the meaning of the formula. 

Since this process is very delicate and complicated to be implemented, then the classes wich derive from the class Formula needed to be built in a sophisticated way. These classes are Logical Connectives, Predicates and Quantifiers, which all have a method for the graphic interface using elegant formulations in latex, a method for the negation, a method for the negated normal form and a method for the grounding. 

### Most General Unifier
Consider the following definitions:
- $\sigma$ is a unifier of terms $t$ and $u$ is $t\sigma=u\sigma$;
- $\sigma$ is more general than $\theta$ if $\theta=\sigma\cdot\phi$ for some substituion \phi;
- $\sigma$ is a most general unifier of terms $t$ and $u$ if it is a unifier and is more general than all the other unifiers.

The implementation can be described as follows:

<p align="center">
  <img src="https://github.com/silviapoletti/First-Order-Logic-Sympy-library-extension/blob/c4facda7e75df0cf4e7922d3d86010051f44ba2d/slides/most_general_unifier1.png" width="48%"/>
  <img src="https://github.com/silviapoletti/First-Order-Logic-Sympy-library-extension/blob/529be03295026fc7a61281d2a153ab8dfb35ee74/slides/most_general_unifier2.png" width="48%"/>
</p>

### Binary resolution rule

The binary resolution rule is defined as follows:

$$ \frac{ \[ l_1,\dots,l_n,P(t_1,\dots,t_n) \] \[ \neg P(u_1,\dots,u_n),l_{n+1},\dots,l_m \] }{ \[ l_1,\dots,l_m \] \sigma} $$ 

where $l_i$ is a literal and $\sigma$ is the most general unifier of $P(t_1,\dots,t_n)$ and $P(u_1,\dots,u_n)$.

We implement a specific class for the clause objects and we use the clauses to represent skolemized FOL formulas in a compact way. Therefore the universal quantifiers at the beginning of the formulas are implicit and each clause represent a disjunction of predicates, so that a set of clauses represent a conjunction of clauses.

Our implementation is based on the following diagram. 

<p align="center">
  <img src="https://github.com/silviapoletti/First-Order-Logic-Sympy-library-extension/blob/c4facda7e75df0cf4e7922d3d86010051f44ba2d/slides/most_general_unifier1.png" width="60%"/>
</p>

We give in input to the Binary Resolution Algorithm the two clauses and we consider every possible couple made of one literal from the first clause and the other from the second clause. If one literal is the negation of a predicate and the other literal is a predicate having the same name, then we proceed by the removal of the literals if the predicates are exactly the same, otherwise we try to unify them and then remove them.
