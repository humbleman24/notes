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

#### Matching

matching: 应该就是赋值，$=$ its role is to **match Variables with Atoms** in KB.

variable doesn't have to be on the left side of $=$

A variable in Prolog can only have one value during the execution of a query.

##### Instantiation & Matching & Unification

$=$ has two facets, instantiation and matching.

Both these processes are internally used by Prolog to **do Unification**.

- unification requires a term on either side of it
- Variable = atom or atom = Variable is called instantiation
- atom = atom or Variable1 = Variable2 is called matching

##### Matching without unification

use $==$ to match without unification（不做统一的匹配，表示直接对于值做匹配）

This matches two terms, but to succeed, the two terms have to already have identical values.

#### Structured object

专门是对于compund form来说的，特点是，可以根据结构进行递归式的匹配，通过一个例子来进行说明

```prolog
parents(mother('npongo'), father(jambo))
query1: ?- parents(mother(Mum), father(Dad))
query2: ?- parents(Mum, Dad)
query3: ?- parents(mother('npongo'), father(Dad))
```

##### Querying > 1 Goal through Variables

在一次查询中，如果有多个term需要进行匹配，可以使用同一个变量来限制多个term之间的关系，可以理解为外键。使得unification的时候，多个term中的同一个Variable必须一致！

When you would use more than one goal, use comma "," to connect them (often called the conjunction)

#### Arithmetic

- **is/2**: 用于求值数学表达式。它的作用是计算右边的数学表达式，并将结果赋给左边的变量
- **=:=**：用于比较两个数学表达式是否相等
- **=\\=**：用于比较两个数学表达式是否不相等
- is 与 = 的区别， 一定要用于计算一个**数学表达式**，并且左边需要是变量

#### Loops

No traditional loops!

we can obtain recursive calls by using recursive predicates.

#### Lists

Lists in Prolog begin with "[" and end with "]"

values are separated by ","

use recursive to find a member in the list

manipulating list:

- append(L1, L2, LR); 将两个列表连接成一个列表
- nth0(Index, List, Element); 获取列表的第n个元素
- nth0(Index, LR, Element, L)；将一个元素插入到列表的指定位置
- delete(L, Element, LR); 删除列表中的指定元素
- length(L, N); 计算列表的长度

#### Cut Operator（!）

the ! operator is a very useful operator for stopping backtracking

can be used in the body of a rule

It succeeds the first time when called, but then fails upon any backtracking attempt

**Green Cut**: 如果cut的使用不会改变程序的逻辑，只是用于提高效率，称之为green cut

**Red Cut**：如果cut的使用改变了程序的逻辑，可能会导致不正确的行为，因为可能组织了合法解的回溯

#### If-then-else

（A->B;C）：if A then B else C

try and prove A. If you can, go on to prove B and ignore C.

If proving A fails, go on to prove C and ignoring B.

### Description Logic

简单理解一下为什么需要Description Logic，与FOL的区别是什么

- FOL是一种逻辑主要应用的是facts, rules并且注意在procedures and inferences，所有应用于逻辑推理以及一些数学的定理证明
- description logic则比较注重于对于实际物体的表述，基于概念和角色，关注类别，关系和属性

1. Objects naturally fall into categories and are often thought of as being members of multiple categories
2. categories can be more general or more specific than others
3. categories is natural for those with more complex descriptions
4. objects have parts, somtimes in multiples
5. the relationships among an object's parts is essential to its being considered a member of a category(不同的内部结构会影响对象的类别归属)

#### Concept, roles and constants

These three expressions are considered nonlogical symbols which means they are application-dependent.

- **concept(classes, unary predicates)** are like category nouns ( groups of objects that share similar characteristics )
- **roles(relations, binary predicates)** are like relational nouns ( predicates that have more than one argument representing a relation between them)
- **constants(instances, constants)** are like proper nouns ( the instances of concepts )

#### Some properties & Meanings

- if r is a role, and d is a concept, then [ALL r d] is a concept. 如果r是一个角色，d是一个概念，那么得到的也是一个概念，ALL是全量约束（Universal Restriction）所有r关系的对象都是d类型的，用来表示对象的某种关系所关联的所有对象都属于特定的概念。
- if r is a role, and n is a positive integer, the [EXISTS n r] is a concept. 如果r是一个角色，n是一个整数，那么得到的是一个概念。EXIST是至少约束（At Least Restriction）表示至少存在n个r关系的对象，用来表示一个对象在r关系中至少关联n个对象
- if r is a role, and c is a constant, then [FILLS r c] is a concept. 如果r是一个角色，c是一个常量，那么得到的是一个概念，FILL是值约束（Value Restriction），表示对象在r关系中唯一地关联到c，表示该对象的r关系唯一地与c相连。
- if d_1 ... d_n are concepts, then [AND d_1 ... d_n] is a concept. 如果d_1到 d_n是一些概念，那么得到的也是一个概念。这个规则定义了交集，表示对象同时满足的所有条件。
- if d_1 and d_2 are concepts, then $d_1 \sqsubseteq d_2$ is a sentence. 意思就是所有d_1中的实例，都是d_2中的实例
- if d_1 and d_2 are concepts, then $d_1 \dot= d_2$ is a sentence. 意思就是d_1与d_2是完全相等的
- if d is a concept and c is a constant, then (c -> d) is a sentence. 意思就是如果一个实例满足c，那他必然是d的一个实例

#### semantics of DL

interpretation is the mapping from the nonlogical symbols of DL to elements and relations over the set of objects called the domain of interpretation.

1. for every constant c, $\varphi(c) \in \delta $.
2. for every atomic concept a, $\varphi(a) \subseteq \delta$
3. for every role r, $\varphi(r) \subseteq \delta \times \delta$

#### entailment in DL

Teo basic sorts of reasoning we consider:

- determining whether or not some constant c satisfies a certain concept d ( c -> d)

- determining whether or not a concept d in subsumed by another concept d' ($d \subseteq d'$)

#### Computing entailment in DL

to  compute entailment, we will still use the above two basic sorts of reasoning.

Before computing entailments, a normalisation needs to be performed.

**Normalisation**

1. Expand definitions: Any atomic concept that appears as the left-hand side of a sentence in the KB is replaced by its definition. 这里的意思就是对左边的atomic下了一个定义，所以以后但凡使用到这个concept都应该等价的转换到这个定义上做判断。
2. Flatten the AND operators: AND 中不应该有嵌套的AND，这些都应该被提出来重新组合
3. Combine the ALL operators: 在一个AND中，如果有对一个r的多个ALL的定义，应该将对应的d进行组合，形成一个符合的ALL来定义物体
4. combine the EXISTS operators: 在一个AND中，如果对一个r有多个EXISTS的定义，应该取最大的n值做保留
5. Deal with T: 当sentence中存在T，应该单独对这个句子进行处理，因为有时候T会让整个句子不在有用因为他不做任何的限定也就是没有任何的知识或description的提供；有时候又是无用的出现，可以在句子中之间删除这个概念
6. Remove redundant expressions

#### Computing Satisfaction

如何在描述逻辑中通过知识库中的**子集关系**和**角色填充**等信息进行推理，即使结论并非直接显现，而是通过若干关联概念间接得到的。这种推理方式强调了描述逻辑在处理复杂知识关系和类推中的应用。







































