=========================================
Introduction to the Theory of Computation
=========================================

Chapter One of Linz's text introduces vocabulary and proof techniques. Here I am adding a couple sections on Propositional Logic and Set Theory. It is a good idea to understand sets and logic when studying formal languages and their accepting machine counter-parts. We'll learn a little logic first and then some set theory.

Propositional Logic
=======================

There are problems in life and in theoretical computer science or mathematics where we state that certain facts are true or false. Sometimes a fact's truth-value implies the truth or falsity of another fact.
To deal with true and false facts and their implications, a philosopher named Aristotle developed a formal logic system called propositional logic. In propositional logic we can have the following.

	* Things that may be true or false are called propositional variables and are usually represented by a capital letter, like A, B, or C.
	* A propositional expression consists of propositional variables or other propositional expressions joined by one of the propositional operators which are

		* :math:`P1 \wedge P2` which means  :math:`P1` AND :math:`P2` and is called *conjunction*.
		* :math:`P1 \vee P2` which means  :math:`P1` OR :math:`P2` and is called *disjunction*.
		* :math:`P'` which means NOT :math:`P` and is called *negation*.
		* :math:`P1 \rightarrow P2` which means :math:`P1` IMPLIES :math:`P2` and is called *implication*. :math:`P1` is called the *antecedent* and :math:`P2` is called the *conclusion*.
		* :math:`P1 \leftrightarrow P2` which means :math:`P1` IMPLIES :math:`P2` and :math:`P2` IMPLIES :math:`P1` and is called *if and only if* which is usually abbreviated as iff.

Parentheses and braces may be used to override the precedence of operations. Otherwise, negation has the highest precedence followed by conjunction, then disjunction, and finally implication and iff.

Propositional Logic Operators
--------------------------------

Now that we have seen how the logical operators are written in propositional logic, we need to understand precisely how they can be used to operate on the logical values true and false. Each logical operator operates on one or two propositional expressions and itself denotes a propositional expression. The table below provides the truth value that results from the truth or falsity of its one or two subexpressions. Consider :math:`A` and :math:`B`, two propositional expressions with the truth values provided below. This truth-table shows the result of combining these two propostional expressions by using the given operator and with the given truth values.

+-----------+-----------+------------+-----------------+--------------------+-------------------------+-----------------------------+
|           |           |            |                 |                    |                         |                             |
| :math:`A` | :math:`B` | :math:`A'` | :math:`A\vee B` | :math:`A \wedge B` | :math:`A \rightarrow B` | :math:`A \leftrightarrow B` |
|           |           |            |                 |                    |                         |                             |
+===========+===========+============+=================+====================+=========================+=============================+
| F         | F         | T          | F               | F                  | T                       | T                           |
+-----------+-----------+------------+-----------------+--------------------+-------------------------+-----------------------------+
| F         | T         | T          | T               | F                  | T                       | F                           |
+-----------+-----------+------------+-----------------+--------------------+-------------------------+-----------------------------+
| T         | F         | F          | T               | F                  | F                       | F                           |
+-----------+-----------+------------+-----------------+--------------------+-------------------------+-----------------------------+
| T         | T         | F          | T               | T                  | T                       | T                           |
+-----------+-----------+------------+-----------------+--------------------+-------------------------+-----------------------------+



Using propositional logic we may be asked to prove something is true or false. This can be done in one of two different ways. Either an exhaustive proof may be constructed using a truth-table or you may use logical simplification rules to reduce or simplify a propositional statement to true or false. First we'll look at truth-tables, then we'll consider an example where the propositional expression is reduced via these simplification rules.

Proof by Truth-Tables
------------------------

If we are asked to prove a propositional logic expression, often called a sentence, is true or false, we can construct a proof table to show exhaustively that it is true in all circumstances.

For instance, consider this expression.

Prove :math:`(A \rightarrow B) \rightarrow (A' \vee B)`.

+-----------+-----------+------------+------------------------+-------------------+--------------------------------------------------+
|           |           |            |                        |                   |                                                  |
| :math:`A` | :math:`B` | :math:`A'` | :math:`A\rightarrow B` | :math:`A' \vee B` | :math:`(A\rightarrow B) \rightarrow (A' \vee B)` |
+===========+===========+============+========================+===================+==================================================+
| F         | F         | T          | T                      | T                 | T                                                |
+-----------+-----------+------------+------------------------+-------------------+--------------------------------------------------+
| F         | T         | T          | T                      | T                 | T                                                |
+-----------+-----------+------------+------------------------+-------------------+--------------------------------------------------+
| T         | F         | F          | F                      | F                 | T                                                |
+-----------+-----------+------------+------------------------+-------------------+--------------------------------------------------+
| T         | T         | F          | T                      | T                 | T                                                |
+-----------+-----------+------------+------------------------+-------------------+--------------------------------------------------+

The truth-table above proves that :math:`(A \rightarrow B) \rightarrow (A' \vee B)` is a true statement. Another name for a statement which is always true is a *tautology*. In this case, it does not matter what truth values are assigned to :math:`A` and :math:`B`.

Take another look at this truth-table. Notice that each column of the truth-table adds just one more logical operator. Also notice that all possible values of true and false are covered for :math:`A` and :math:`B` by starting with two falses and then two trues for :math:`A` and alternating true and false for :math:`B`. If there were three variables in the statement, then there would be four falses followed by four trues for the first variable, then two falses and two trues alternating for the second variable, and finally true/false alternating for the third variable. By using this systematic approach you can be sure to capture all combinations of true and false for a statment. If there are three variables in your statement, then eight rows would be needed in the truth-table. :math:`2^{numberOfVariables}` is the number of rows needed in a truth-table to prove that a statement is true or not under all interpretations.

Practice
---------

	* Use a truth table to prove that :math:`A \rightarrow B` is not the same as :math:`B \rightarrow A`. :math:`B \rightarrow A` is called the *converse* of :math:`A \rightarrow B` and they do not mean the same thing.
	* Use a truth table to prove that :math:`A \rightarrow B \leftrightarrow B' \rightarrow A'`. :math:`B' \rightarrow A'` is called the *contrapositive* of :math:`A \rightarrow B` and the two statements are equivalent.

Equivalence and Inference Rules
-------------------------------------------

Another way to prove a statement is true or false is through the use of equivalences and inference rules for propositional logic. The proof in the previous section actually hints at one of these rules. It turns out that not only does :math:`(A\rightarrow B) \rightarrow (A' \vee B)` but also :math:`(A' \vee B) \rightarrow (A\rightarrow B)`. In other words :math:`(A\rightarrow B) \iff (A' \vee B)`. This double arrow is pronounced, "equivalent" and indicates that the two expressions have the same truth values in all interpretations. Another way to state this is :math:`(A\rightarrow B) \leftrightarrow (A' \vee B)`. So, one can replace the other. Here are some of the important equivalences in propositional logic.

	1. :math:`A \vee B \iff B \vee A`, *Commutativity*
	2. :math:`A \wedge B \iff B \wedge A`, *Commutativity*
	3. :math:`(A \vee B) \vee C \iff A \vee (B \vee C)`, *Associativity*
	4. :math:`(A \wedge B) \wedge C \iff A \wedge (B \wedge C)`, *Associativity*
	5. :math:`A \wedge (B \vee C) \iff (A \wedge B) \vee (A \wedge C)`, *Distribution*
	6. :math:`A \vee (B \wedge C) \iff (A \vee B) \wedge (A \vee C)`, *Distribution*
	7. :math:`A\rightarrow B \iff A' \vee B`, *Implication*
	8. :math:`(A \vee B) \Longrightarrow B' \iff A`, *Simplification*
	9. :math:`A \wedge A' \iff False`, *Complementation*
	10. :math:`A \vee A' \iff True`, *Complementation*
	11. :math:`A \wedge A \iff A`, *Idempotent*
	12. :math:`A \vee A \iff A`, *Idempotent*
	13. :math:`A \vee True \iff True`, *Identity*
	14. :math:`A \vee False \iff A`, *Identity*
	15. :math:`A \wedge False \iff False`, *Identity*
	16. :math:`A \wedge True \iff A`, *Identity*
	17. :math:`A'' \iff A`, *Double Negation*
	18. :math:`A \leftrightarrow B \iff (A \rightarrow B) \wedge (B \rightarrow A)`, *iff* for if and only if.
	19. :math:`(A \vee B)' \iff A' \wedge B'`, *DeMorgan's Law*
	20. :math:`(A \wedge B)' \iff A' \vee B'`. *DeMorgan's Law*
	21. :math:`A \Longrightarrow A \vee B`, *Addition*
	22. :math:`A \wedge (A \vee B) \iff A`, *Absorption*
	23. :math:`A \vee (A \wedge B) \iff A`, *Absorption*



In proofs we may also use some rules of inference when trying to infer whether a statement is true or false. Here are the rules of inference.

	24. :math:`A \wedge (A \rightarrow B) \Longrightarrow B`, *modus ponens*
	25. :math:`(A \rightarrow B) \wedge B' \Longrightarrow A'` *modus tolens*
	26. :math:`(A \vee B) \wedge (A' \vee C) \Longrightarrow B \vee C`, *resolution*


Proof by Derivation
------------------------
We can now prove statements to be true in propositional logic using these equivalence and inference rules. To do this, we start with the original statement and using the rules in the previous section we arrive at the value of true. Using these rules we prove that statements are *valid* meaning that they are true regardless of the truth values assigned to the individual variables. We call the assignment of variables to truth values an *interpretation* or a *model*. We want to prove that a statement is *valid* which means that it is true in all *interpretations*.
Let's consider an example to see how a proof can be constructed from these equivalences. Consider proving

	:math:`[(A \vee B) \rightarrow C] \wedge (C \rightarrow D) \wedge A \rightarrow D`

Here is the proof. Each line refers to one of the equivalences above which was used in simplifying it from the previous step.

	* We start with what we want to prove,
	* :math:`[(A \vee B) \rightarrow C] \wedge (C \rightarrow D) \wedge A \rightarrow D` :math:`\iff`
	* :math:`[(A \vee B)' \vee C] \wedge (C \rightarrow D) \wedge A \rightarrow D`, by (7) :math:`\iff`
	* :math:`[(A' \wedge B') \vee C] \wedge (C \rightarrow D) \wedge A \rightarrow D`, by (19) :math:`\iff`
	* :math:`[(A' \wedge B') \vee C] \wedge (C' \vee D) \wedge A \rightarrow D`, by (7) :math:`\iff`
	* :math:`[(A' \wedge B') \vee D] \wedge A \rightarrow D`, by (26) and (1) :math:`\iff`
	* :math:`[(A' \vee D) \wedge (B' \vee D)] \wedge A \rightarrow D`, by (6) and (1) :math:`\iff`
	* :math:`(A' \vee D) \wedge (B' \vee D) \wedge A \rightarrow D`, by a generalization of (4) :math:`\iff`
	* :math:`D \wedge (B' \vee D) \rightarrow D`, by (8), (1), and (2) :math:`\iff`
	* :math:`D \rightarrow D`, by (22) and (1) :math:`\iff`
	* :math:`D' \vee D`, by (7) :math:`\iff`
	* :math:`True`, by (10)
	* :math:`\Box`

The proof above deserves some comments to describe how it was done. There are a large number of provided equivalences and inference rules, so just how did we decide which ones to use? First, you must remember the rules of precedence to correctly understand what is being asked.

	* Implication has the lowest precedence.
	* This is followed by disjunction, or the *or* operator.
	* Next, is the conjunction or *and* operator.
	* Finally, the negation operator has the highest precedence.

In the statement above the last implication operator is the top-level logical operator. So we are trying to prove that everything on the left implies :math:`D`.

Next, we went about removing implication so that we ended up with a conjunction of disjunctions with negation on individual variables in the antecedent. A conjunction of disjunctions with negation on individual variables is called *conjunctive normal form*. Every propositional logic expression can be reduced to conjunctive normal form. That is what the term *normal form* means. Every expression can be reduced to a normal form if a normal form exists for a language. In conjunctive normal form we end up with a series of disjunctions all *anded* together. In this proof we only reduced the antecedent to conjunctive normal form. We didn't bother reducing the entire statement to conjunctive normal form right away.

Finally, resolution (i.e. rule 26) was used to eliminate some variables while simplification and absorption eliminated others. DeMorgan's law was useful in moving the negation down to the individual propositional variables.

When we reach the final value of true we have proved that the propositional statement is true no matter what truth-values are applied to the individual variables. In other words, the statement is valid or a *tautology*. It is true independent of any assignment of true and false to the propositional variables. The final box at the end states that the proof is complete.

Proof by Contradiction
------------------------
Another means of proving something is called a proof by contradiction. To prove by contradiction we start by assuming that the statement is false. Then we try to prove that we arrive at a contradiction where a propositional variable must be both true and false at the same time in some interpretation of the propositional variables.

Consider the example that we just proved was true. Let's prove it by contradiction instead. So, we'll assume that the statement is false. Then we'll show that we arrive at a contradiction.

	* We start with the negated statement,
	* :math:`([(A \vee B) \rightarrow C] \wedge (C \rightarrow D) \wedge A \rightarrow D)'` :math:`\iff`
	* :math:`([(A \vee B) \rightarrow C] \wedge (C \rightarrow D) \wedge A)' \vee D)'` , by (7) :math:`\iff`
	* :math:`([(A \vee B) \rightarrow C] \wedge (C \rightarrow D) \wedge A) \wedge D'`, by (19) :math:`\iff`
	* :math:`[(A \vee B) \rightarrow C] \wedge (C \rightarrow D) \wedge A \wedge D'`, generalization of (4) :math:`\iff`
	* :math:`[(A \vee B) \rightarrow C] \wedge (C \rightarrow D) \wedge (A \vee B) \wedge D'`, by (21) :math:`\iff`
	* :math:`C \wedge (C \rightarrow D) \wedge D'`, by (2) and (24) :math:`\iff`
	* :math:`D  \wedge D'`, by (24)
	* :math:`False`, by (9)
	* :math:`\Box`

Since we arrived at false, that says that or original statement was false and therefore the negation of it, the thing we assumed was false in our proof by contradiction, was actually true.

In general, when proving something by contradiction you make an assumption that you believe to be false, then you go about showing that it is indeed false. Then the opposite of what you first assumed is actually true. This type of proof can be especially useful since you only have to find one counter-example to be able to conclude that your assumption could not be true and conclude that what you originally wanted to prove is true. In the next section we'll do another proof by contradiction.

Other Proof techniques
-----------------------

Read pages 10-13 for more about proofs including induction and another example of proof by contradiction.

Exercises
-----------

These exercises help you practice using the concepts that were covered in this section.

	Construct a truth-table to prove the following tautologies are valid.

	1. :math:`(A \vee B)' \iff A' \wedge B'`
	2. :math:`(A \wedge B)' \iff A' \vee B'`
	3. :math:`A \leftrightarrow B \iff (A \rightarrow B) \wedge (B \rightarrow A)`
	4. :math:`A \vee (A \wedge B) \iff A`
	5. :math:`A \wedge (A \vee B) \iff A`
	6. :math:`(A\rightarrow B) \iff (A' \vee B)`

	Prove the following are true in any interpretation using the equivalence and inference rules in this section. Be sure to use one and only one rule per line in your proof (with the exception of rules 1-4) and include the rule number of each rule you used in your proof with the line where it was used as shown in the examples in the text.

	7. :math:`A \wedge (B \rightarrow C) \rightarrow (B \rightarrow (A \wedge C))`
	8. :math:`[A \rightarrow (B \vee C)] \wedge B' \wedge C' \rightarrow A'`
	9. :math:`A' \wedge (B \rightarrow A) \rightarrow B'`
	10. :math:`(A \wedge B) \rightarrow (A \rightarrow B')'`
	11. :math:`[A \rightarrow (B \rightarrow C)] \rightarrow [B \rightarrow (A \rightarrow C)]`
	12. :math:`(A' \rightarrow B') \wedge B \wedge (A \rightarrow C) \rightarrow C`


Sets
========

.. container:: figboxright

	.. _setfig1:

	.. figure:: subset.png

		A Subset and a Superset

This section supplements material found in the short section 1.1 of Linz's text. You should read through that material on pages 3-6 if the fifth edition. Then supplement it with the material written here.

A *set* is a collection of items. There are very few restrictions on sets. They do not need to contain the same type of items. The order that the items are written in a set does not matter. So the set {1,2,3,4} is the same as the set {2,1,4,3}. In fact, a set does not need to contain any items. In this case it would be the *empty set*. A set may contain a finite number of items, or elements, or an infinite number of elements. The set of cars you have owned in your life is a finite set. It may even be the *empty set* right now. The set of integers is an infinite set.

A *subset* is also a *set* but describes a relationship between two sets. The set *A* is a subset of *B* if every element in *A* is also in *B*. We can write this mathematically as follows.

	:math:`A \subseteq B \iff \forall a \in A, a \in B`

The :math:`\subseteq` symbol denotes subset containment. The potentially smaller set, :math:`A`, is written on the left while the potentially bigger set, :math:`B`, is written on the right. The line below the horseshoe indicates that the two sets may be equal in size, in which case they would denote the same set. This is much the same as writing the :math:`\leq` symbol to denote a number that may be less than or equal to another in an ordering of the numbers. A subset that is not equal to its *superset* is called a *proper subset* which is written as follows.

	:math:`A \subset B \iff \forall a \in A, a \in B ~and~ \exists b \in B \ni b \notin A`

This is read as :math:`A` is a proper subset of :math:`B` if and only if for all :math:`a` in :math:`A`, :math:`a` is also in :math:`B` and there exists at least one :math:`b` that is in :math:`B` such that :math:`b` is not in :math:`A`. Figure 1 depicts :math:`A` as a proper subset of :math:`B`.


.. container:: figboxright

	.. _setfig2:

	.. figure:: sets.png

		A Set Diagram

The picture of figure 1 is that of a *Venn Diagram*. Venn diagrams were first introduced by John Venn around 1880 to describe mathematical sets. The *universe* is typically not labelled but denotes the entire world of items that potentially might be a part of some set. Typically there are items that are not part of any set that we might be interested in describing. The area inside a circle denotes the items of that set. Since sets may overlap they may be drawn on top of each other or overlapping some part of the two sets.


Figure 2 denotes two sets that overlap a little bit but not completely. The (American) football shaped area (i.e. region 4) that the two sets share is the *intersection* of the two sets. The *intersection* of two sets are those items that are present in both sets. The *union* of two sets are the items that are a part of either set. Regions 2, 3, and 4 of the Venn diagram in figure 2 are the union of sets A and B. The *complement* of a particular set are those items that are not inside the set. Regions 1 and 3 make up the complement of set A in figure 2. Complement, union, and intersection each have their own set operators as shown in this table. In addition, some common set operations are detailed there as well.

	+----------------------+--------------------------------------------+-------------------------------------------------------------------+
	|    Operator Name     |          Symbolic Representation           |                            Description                            |
	+======================+============================================+===================================================================+
	| Subset               | :math:`A \subseteq B`                      | True if :math:`A` is inside :math:`B` (see figure 1)              |
	+----------------------+--------------------------------------------+-------------------------------------------------------------------+
	| Proper Subset        | :math:`A \subset B`                        | True if :math:`A` is a subset of :math:`B` and :math:`A \neq B`   |
	+----------------------+--------------------------------------------+-------------------------------------------------------------------+
	| Superset             | :math:`B \supseteq A`                      | True if :math:`B` is a superset of :math:`A`.                     |
	+----------------------+--------------------------------------------+-------------------------------------------------------------------+
	| Proper Superset      | :math:`B \supset A`                        | True if :math:`B` is a superset of :math:`A` and :math:`A \neq B` |
	+----------------------+--------------------------------------------+-------------------------------------------------------------------+
	| Union                | :math:`A \cup B`                           | Regions 2,3,4 of figure 2                                         |
	+----------------------+--------------------------------------------+-------------------------------------------------------------------+
	| Intersection         | :math:`A \cap B`                           | Region 4 of figure 2                                              |
	+----------------------+--------------------------------------------+-------------------------------------------------------------------+
	| Complement           | :math:`\bar{A}`                            | Regions 1,3 of figure 2                                           |
	+----------------------+--------------------------------------------+-------------------------------------------------------------------+
	| Set Difference       | :math:`A - B`                              | Region 2 of figure 2                                              |
	+----------------------+--------------------------------------------+-------------------------------------------------------------------+
	| Set Addition         | :math:`A + B`                              | Regions 2,3,4  (same as union)                                    |
	+----------------------+--------------------------------------------+-------------------------------------------------------------------+
	| Set Cardinality      | denoted by :math:`\arrowvert A \arrowvert` | Number of items in :math:`A`.                                     |
	+----------------------+--------------------------------------------+-------------------------------------------------------------------+
	| Set Membership       | :math:`a \in A`                            | True if :math:`a` is in :math:`A`                                 |
	+----------------------+--------------------------------------------+-------------------------------------------------------------------+
	| Symmetric Difference | :math:`A \ominus B`                        | Same as :math:`(A-B) \cup (B-A)`                                  |
	+----------------------+--------------------------------------------+-------------------------------------------------------------------+
	| Cross Product        | :math:`A \times B`                         | Constructs a new set. Described below.                            |
	+----------------------+--------------------------------------------+-------------------------------------------------------------------+
	| Power Set            | :math:`2^A`                                | Constructs a new set. Described below.                            |
	+----------------------+--------------------------------------------+-------------------------------------------------------------------+
	| Empty Set            | :math:`\emptyset`                          | The set with no items. Sometimes denoted {}.                      |
	+----------------------+--------------------------------------------+-------------------------------------------------------------------+


The cross product of two sets is a new set that consists of all the pairs of items from the two constituent sets. Consider set A = {1,2,3,4} and B = {"hi", "there"}. Then A X B = {(1,"hi"), (2,"hi"), (3,"hi"), (4,"hi"), (1,"there"), (2,"there"), (3,"there"), (4,"there")}. From this description it should be clear that the cardinality of any finite cross product is :math:`\arrowvert A \times B \arrowvert = \arrowvert A \arrowvert * \arrowvert B \arrowvert`.

The power set of a set is the set of all subsets. So :math:`2^A` is the set of all subsets of A. If A={1,2,3,4}, then

	:math:`2^A` = { :math:`\emptyset`, {1}, {2}, {3}, {4}, {1,2}, {1,3}, {1,4}, {2,3}, {2,4}, {3,4}, {1,2,3}, {1,2,4}, {1,3,4}, {2,3,4}, {1,2,3,4}}

For a finite set, the cardinality of its powerset can be computed as :math:`\arrowvert 2^A \arrowvert = 2 ^ {\arrowvert A \arrowvert}`. Notice that the empty set is always a member of any power set.

Sets come into play in many algorithms in Computer Science. Because of their importance, sets have been implemented in pretty much all programming languages. For instance, `Python has an implementation of the set datatype <https://docs.python.org/3.4/library/stdtypes.html#set-types-set-frozenset>`_. In fact, there are two implementations of sets within Python, the *frozenset* and the *set* datatypes. The *frozenset* implementation provides an immutable set type. Immutable datatypes are values that once created cannot be changed. Mutable datatypes can be altered (i.e. mutated) after they are created. The *set* datatype is mutable. You can read about the methods supported by these datatypes on the help webpage linked above. The operations supported by these datatype mirror the operations outlined in the table in this section.

Both the *set* and *frozenset* datatypes of Python implement the set membership test in O(1) time complexity meaning they can look to see if an item is in a set in a constant amount of time, independent of the set's size. Other set operations are dependent on the size of the set or sets involved in the operation. Sets are commonly implemented using a hash table which means that the items of a set must be hashable. Most data items in Python are hashable. If you wish to implement your own hashing function for a class you implement the \_\_hash\_\_ method. It must return a evenly distributed integer value as its result.

Exercises
-----------

Complete the following exercises involving sets.

	13. Provide the region numbers of the Venn diagram from fig. 2 that are included in the set :math:`\overline{A \ominus B}`.
	14. Provide the region numbers of the Venn diagram from fig. 2 that are included in the set :math:`(A - B) \cup A'`.
	15. If :math:`A=\{1,2,3\}` and :math:`B=\{2,3,4\}`, then what are the items of the set :math:`A \times B`?
	16. If :math:`A=\{a,b,c\}` and :math:`B=\{x,y,z\}` and the universe is the set of all lowercase letters, then what are the items of the set :math:`(A - B) \cup A'`?
	17. Assume :math:`A=\{a,b,c\}`. What is :math:`\arrowvert 2^A \arrowvert`? Provide the contents of the set.
	18. Consider the set :math:`A = \{1,2,3,4\}`. Let :math:`C(S) = \bigcup_{i=1}^{\arrowvert S \arrowvert} \{e_i\} \in S`. In this case :math:`C(A)` would be :math:`\{\{1\},\{2\},\{3\},\{4\}\}`. What is :math:`2^A - C(A)`? Provide a formula for computing :math:`\arrowvert 2^A - C(A) \arrowvert` for any finite set.

.. 13: 1,4 or if the value was not clear on the website it is possible they wrote 2,3

.. 15: {(1,2),(1,3),(1,4),(2,2),(2,3),(2,4),(3,2),(3,3),(3,4)}

.. 17: {emtpySet,{1,2,3,4},{1,2,3},{1,2,4},{1,3,4},{2,3,4},{1,2},{1,3},{1,4},{2,3},{2,4},{3,4}} . The formula would be |2^A-C(A)| = 2^|A| - |A|.

Infinite Sets
-------------

The content provided here may be supplemented by this excellent `crash course provided by Peter Suber <http://legacy.earlham.edu/~peters/writing/infapp.htm>`_.

Some sets are infinite in size. For example, the set of integers, denoted by :math:`Z`, is an infinite set. We can count the items in that set, but we would never finish of course, so trying to determine its cardinality by counting would be futile. However, we can say that the set is countably infinite if we can find a method, called an *enumeration*, of counting all the integers. But, the integers contain both negative and positive numbers. How would we provide a method of counting them?

It turns out we can provide that enumeration if we start at 0. Here is the enumeration that proves that the integers are countably infinite in size.

	0, -1, 1, -2, 2, -3, 3, -4, 4, ...

By counting in this way we would be able to enumerate all integers given an infinite amount of time. So, now we know that while some sets are infinite, at least some of these infinite sets are countable. An infinite set that is countable is called a *denumerable* set.

But, we have operators that we can use to construct new sets. What about :math:`N \times N` where :math:`N` is the set of natural numbers. The natural numbers are the set of non-negative integers. Is :math:`N \times N` a denumerable set? Clearly it is infinite, but can we come up with an enumeration that would let us count all the elements of :math:`N \times N`?

The proof that :math:`N \times N` is countably infinite, or denumerable, is a little trickier. The proof relies on constructing a two-dimensional, infinite, grid as shown here.

	+-----+-------+-------+-------+-------+-------+-------+-----+
	|     |   0   |   1   |   2   |   3   |   4   |   5   | etc |
	+-----+-------+-------+-------+-------+-------+-------+-----+
	| 0   | (0,0) | (0,1) | (0,2) | (0,3) | (0,4) | (0,5) | etc |
	+-----+-------+-------+-------+-------+-------+-------+-----+
	| 1   | (1,0) | (1,1) | (1,2) | (1,3) | (1,4) | (1,5) | etc |
	+-----+-------+-------+-------+-------+-------+-------+-----+
	| 2   | (2,0) | (2,1) | (2,2) | (2,3) | (2,4) | (2,5) | etc |
	+-----+-------+-------+-------+-------+-------+-------+-----+
	| 3   | (3,0) | (3,1) | (3,2) | (3,3) | (3,4) | (3,5) | etc |
	+-----+-------+-------+-------+-------+-------+-------+-----+
	| 4   | (4,0) | (4,1) | (4,2) | (4,3) | (4,4) | (4,5) | etc |
	+-----+-------+-------+-------+-------+-------+-------+-----+
	| 5   | (5,0) | (5,1) | (5,2) | (5,3) | (5,4) | (5,5) | etc |
	+-----+-------+-------+-------+-------+-------+-------+-----+
	| etc | etc   | etc   | etc   | etc   | etc   | etc   | etc |
	+-----+-------+-------+-------+-------+-------+-------+-----+

Now, the problem is reduced to finding an enumeration that would eventually count all of them. We can't proceed down a row or we would never get to the next row. Same with columns. We can't proceed down a column or we would never get to the next one. So, we count the only way we can, by proceeding on diagonals that have a beginning and an end. Our enumeration proceeds as follows.

	(0,0), (1,0), (0,1), (2,0), (1,1), (0,2), (3,0), (2,1), (1,2), (0,3), (4,0), (3,1), (2,2), (1,3), (0,4), ...

In this way we would eventually enumerate all the elements of :math:`N \times N`. So, the cross product of any denumerable set is also denumerable.
Weird things begin to happen when you consider infinite sets. We can construct a function that will take any natural number, 0, 1, 2, 3, ..., and give us a corresponding element from our enumeration of the integers. Here is that function:

	:math:`f(n) = -1^n \times [(n+1)/2]`

Likewise, we can construct a function that will take any integer and give us a corresponding natural number. Any non-negative integer will map to a corresponding even natural number, while any negative integer would map to an odd natural number. The definition of this second function proves that the set of integers is no bigger than the set of natural numbers. If the set of integers were bigger, then there would be some integer that didn't have a corresponding natural number. But the function definition shows us that every integer has a corresponding natural number.

The previous function shows us that every natural number has a corresponding integer. This is the converse of the argument that every integer has a corresponding natural number. With these two functions the set of integers and the set of natural numbers are put in one-to-one correspondence to each other. This is usually written as :math:`Z \simeq N`. What we just proved is that the integers and the naturals are the same size set. It turns out that the cross product of the natural numbers is the same size set as well. They are all denumerable or countably infinite. The integers, naturals, and cross-product of the naturals have exactly the same size.

This goes against intuitive thinking. It just doesn't seem that a set like the integers can be the same size as the set of naturals since the naturals only have 1/2 the integers as part of them. But, we are dealing with sets of infinite size and that means that intuition is not always right.

Then what about the set of real numbers? Are they countably infinite? Intuition would say that they are not, but is intuition trust-worthy when dealing with infinite sets? It turns out that the real numbers are not countably infinite. Here is the proof. It is a classic proof by contradiction.

	Are the real numbers countably infinite?

	**Proof:**

	Assume that the reals were countably infinite. Then there must be some enumeration of all the real numbers. We'll line all the numbers up in a list. To keep things simple, we'll consider only the numbers between 0 and 1.

	Now we'll construct a new number that is not in that list of all numbers between 0 and 1. We'll start with "0." and construct the numbers after the decimal point.

	Look at the first number in our list of numbers. We'll pick the first digit of our new number that does not equal the first digit after the decimal point of the first number in our enumeration. Then, we pick the second digit of our new number that does not match the digit of the second number in our enumeration. We continue in this fashion infinitely picking the next digit in our number as we proceed. To see how this works, consider this example enumeration.

		1. :math:`0.\underline{0}309...`
		2. :math:`0.1\underline{5}48...`
		3. :math:`0.18\underline{7}9...`
		4. :math:`0.952\underline{8}...`
		5. etc.

	So, in this example our new number looks something like :math:`0.5497...`. Clearly there are lots of choices for the digits of this number.
	This new number that we just built cannot be in our enumeration of all the real numbers between 1 and 0, yet it is a number between 1 and 0. It cannot be the first number, because it differs in the first digit after the decimal point from that number. It cannot be the second number because it differs from it in the second digit after the decimal point. This same argument applies to all digits in the newly constructed real number.

	So our assumption that the real numbers between 0 and 1 was countably infinite must be wrong. Therefore, the numbers between 0 and 1 are uncountably infinite. And, since the real numbers are clearly a superset of the real numbers between 0 and 1, the real numbers are uncountably infinite as well. :math:`\Box`

So, the real numbers are uncountably infinite. Are there other sets that are uncountably infinite? Yes, it turns out that :math:`2^A` is an uncountably infinite set if :math:`A` is countably infinite. The cardinality of the set of reals is the same as the cardinality of the power set of a countably infinite set.

It turns out the cardinality of the interval (0, 1), the cardinality of the real numbers, and the cardinality of the power set of any denumerable set are all the same. They are all uncountable and their cardinality is denoted as :math:`\varsigma = \arrowvert 2^N \arrowvert` where :math:`N` represents the naturals.

Exercises
-------------

Provide answers for the following exercises.

	19. Prove that the set of Rational numbers is denumerable. Don't look this up on the internet. Try it for yourself to see if you can come up with the proper argument.
	20. For any uncountably infinite set, can you come up with a set that would have greater cardinality? Again, try to do this without the internet. You have enough information here to find your answer.
	21. Prove that removing one element from a denumerable set still results in a denumerable set.
	22. Prove that the set of odd natural numbers is denumerable by putting them in 1-1 correspondence with another denumerable set.
	23. Prove that the set of all finite length strings of 0 and 1 is denumerable.
	24. Prove that the set of all finite length strings of 0 is denumerable.
	25. Prove that the union of two denumerable sets is still denumerable.
	26. Prove that the set of irrational numbers is uncountable. The irrational numbers are those numbers that cannot be represented as A/B where A and B are integers.

.. The proof of 19 is to set up a table with all x values the columns (integers) and all y values the rows (integers) and then count using the diagonalization argument for each x/y a rational number (note that some x/y will get counted twice using this argument, but all x/y will eventually be denumerated).

.. For 21 the set cannot get bigger by removing an element so it is at least denumerable. By theorem 8 of Peters we can see that removing a finite subset from a denumerable set leaves the set as a denumerable set.

.. For 23 consider any string of 0's and 1's as a binary number. Place an extra 1 at the beginning of any of these strings. Then these strings can be placed into 1-1 correspondence to the positive integers which are denumerable. So the strings of 0's and 1's are denumerable.

.. The proof of 25 is to take one from the first enumeration, then one from the second enumeration, and alternate back and forth to create the new enumeration.

.. The proof of 26 is by contradiction. If the irrationals were countable then the reals would have to be countable since the reals are made up of the rational and irrational numbers and the union of two denumerable sets is denumerable.

Functions and Relations
=========================
See pages 6-8 of Linz's text, fifth edition.

Graphs and Trees
==================
See pages 8-10 of Linz's text, fifth edition.

Three Basic Concepts
=====================
Read pages 16-27 of chapter 1, fifth edition on the three basic concepts of languages, grammars, and automata.
