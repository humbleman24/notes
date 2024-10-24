## Knowledge representation

### First order logic

1. ##### 为什么选择FOL，而不是propositional logic？

   - simple， servers as a good representation to KRR
   - expressive

logical reasoning methods are designed to work **no matter what meanings or values are assigned to the logical variables** used in sentences.

**Satisfiability**: an interpretation i satisfies a sentence **iff** it is true under that interpretation

**Validity**: A sentence is valid **iff** it is satisfied by every interpretation.

Relational Logic (FOL), provides us a way of talking about **individual objects and their interrelationships**.

#### Syntax

logical-symbols and non-logical symbols

逻辑符号是有**固定含义**并且在任何情况下都有相同解释的符号，这些符号定义了逻辑系统的**基本规则和结构**。

非逻辑符号是那些**没有固定含义**，需要根据具体应用场景来解释的符号。用于表示具体的对象、关系和性质。

一个考点是，变量的作用域，是否为bound variable

#### Semantics

Interpretation （D, I): where D is the domain of discourse and I is the interpretations function

### Knowledge

Facts: 事实通常是用来描述世界中某个具体的情况或状态。

rules: 规则是一些逻辑条件，描述某种情况下会发生的事情，通常使用蕴涵符号来表示

relations：关系是描述两个或多个对象之间的关联。关系通常用谓词来表示

#### Entailment

a set of sentences $$\Delta$$ logically entails a sentence $$\phi$$ **iff** every interpretation that satisfies $$ \Delta$$ also satisfies $$ \phi $$.

也就是说当 $$\Delta$$ 解释为真的时候， $$\phi$$ 也必须解释为真。

Inference：推理是计算蕴含的过程，即从一组已知的句子出发，推导出蕴含的句子。

​	soundness（可靠性）：get only entailments 系统只会得出正确的蕴涵结论，即所有推导出来的结论都是蕴涵

​	Completeness(完备性)：get all entailments 系统能够推导出所有可能蕴涵的结论，不会遗漏任何合理的蕴涵



### Resolution

归结：是一种用于命题逻辑和一阶逻辑的推理方法，它特别常见于自动推理系统中。归结是一种通过归结规则进行的推理技术。

Given a set of assumptions and a query, an automated reasoning system should be able to make logical inferences towards that query automatically.

First-order logic resolution is **not guaranteed to terminate**!

automated theorem prover: give a proof system, assumptions and a goal to prove, and automated theorem prover will show steps between the assumptions and the goal (used to check for proofs)

SAT Solver: look for a satisfying interpretation for clauses that are satisfiable.



### Horn Clauses

Horn clauses is a particular subset of FOL that remains expressive for most problems, but becomes more manageable for automated resolution procedures.

A clause in resolution based system is **a disjunction of literals**.

A horn clause is a clause with **at most** **one positive literal**, which by definition can be represented as an implication

Types of Horn Clause:

- Definite (positive) clause: Horn clause with **exactly one** positive literal
- Fact: definite clause with no negative literals and without variables

- Goal (negative) clause: Horn clause with no positive literal

#### Resolution

1. Takes two horn clauses and produces another clause
2. the new clause is implied by the two previous clauses
3. removes complementary literals from the new clause
4. terminates when the empty clause is produced ([])

#### SLD Resolution

选择性线性确定性子句归结

For Horn Clause, we can restrict ourselves to SLD-Resolution. This will considerably simplify the search for derivations.

### Logic programming

- statement of facts and propositions that must be satisfied
- specify knowledge (facts) and how the knowledge can be applied
- programming environment uses built-in procedures to **reason** about the knowledge and apply resolution and unification for proving statements or answering questions

### Prolog

#### Basic syntax

everything is considered a term; there are different types of terms

1. **Atoms**: sequence of characters **beginning with lower case**, or a sequence of characters **enclosed in single quotes**, or a sequence of special characters.
2. **Number**
3. **Variables**: sequence of characters beginning with **upper case or underscore** (_ anonymous variable)
4. **Compound terms**: 
   - A functor followed by a sequence of arguments
   - Functor must be an atom (follow the same rule as atom!!!!)
   - arguments can be anything, arity represents the number of arguments of a compound term
5. **List**: 
   - ordered set of terms
   - follow the structure of **[H|T]**, where H is the head of the list and T is the tail

#### Negation

To prove not P, we try to prove P. If proving P fails, the not P is True; if we prove P then not P fails.

\\+ is the sign of Negation!

#### Unification

?? Unification是在两个项（term）之间寻找一种匹配方式，使得它们能够相互一致。涉及**变量的绑定，原子的匹配，结构的比较**。

作用：Prolog通过Unification 将 查询 与 事实、规则 进行匹配，从而找到满足条件的解。

**$t_1$  unifies** with  $t_2$ if there is a subsititution $S$ such that $apply(S, t_1) = apply(S, t_2)$.

A subsititution is a function mapping variables to terms.































