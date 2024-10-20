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









































