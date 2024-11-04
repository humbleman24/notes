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

### Modern Description Logic

The previous notation is not standard.

With the standardisation, DL has become a fundamental part of the semantic web, through the development **Web Ontology Language(OWL)**

#### ABox and TBox

Formulas can be partitioned into two distinguished sets: 

- one that contains all concept inclusions (TBox) 

  contains the terminological knowledge:

  concept definitions: 用于给一些concept下定义的

  axioms： 给concept做限制的

- one that contains all concept assertions and roles assertion (ABox)

  contains assertions about individuals: 相当于给上面定义的concept进行赋值，形成KB

#### $FL_0$ definition

We define the logic $FL_0$  as a tuple ($Sym_{FL_0}, For_{FL_0}, Int_{FL_0}, \vDash_{FL_0}$)

- $Sym_{FL_0}$: 表示符号集合，包括原子符号和构造符号
- $ For_{FL_0}$: 表示公式集合，所有能够表达的逻辑公式  （包含的是ABox的集合）
- $Int_{FL_0}$:  表示解释的集合，如何在特定语境中解释这些符号 （Interpretation）
-  $\vDash_{FL_0}$:   表示逻辑推导关系

#### Extention from $FL_0$

$FL_{\bot}$ : 表示添加了$\bot$ 符号的 $FL_0$ 语言

$FL_{\neg}$ : 表示添加了$\neg$ 符号的 $FL_{\bot}$ 语言

### AL

$AL$ ：等同于 $FL_{\exist}$ -------表示添加了${\exist}$ 符号$FL_{\neg}$ 语言 

$AL$ is often considered as the archetypical description logic, from which others are defined.

Each construct is assigned **a letter** that can be concatenated to $AL$ to form **the name of a particular DL** ! 

#### AL extension

**concept constructs** are new syntatic features that can make new kinds of complex concepts

| Symbol        | Name                       | Syntax  | Semantics                                                    |
| ------------- | -------------------------- | ------- | ------------------------------------------------------------ |
| $\mathcal{A}$ | Value restriction          | ∀r.c    | {x ∈ Δ \|for all y ∈ Δ such that (x, y) ∈ I(r), y ∈ I(c)}    |
| $\mathcal{E}$ | General existential        | ∃r.c    | {x ∈ Δ \| there exists y ∈ Δ such that (x, y) ∈ I(r) and y ∈ I(c)} |
| $\mathcal{U}$ | Concept disjunction        | c ⊔ d   | I(c) ∪ I(d)                                                  |
| $\mathcal{C}$ | General concept complement | ¬c      | Δ \ I(c)                                                     |
| $\mathcal{O}$ | Nominal                    | {a}     | {I(a)}                                                       |
| $\mathcal{N}$ | Cardinality restriction    | ≤ n.r   | {x ∈ Δ \|card({y ∈ Δ \|(x, y) ∈ I(r)}) ≤ n}                  |
| $\mathcal{N}$ | Cardinality restriction    | ≥ n.r   | {x ∈ Δ \|card({y ∈ Δ \|(x, y) ∈ I(r)}) ≥ n}                  |
| $\mathcal{Q}$ | Qualified cardinality      | ≤ n.r.c | {x ∈ Δ \|card({y ∈ Δ \|(x, y) ∈ I(r) and y ∈ I(c)}) ≤ n}     |
| $\mathcal{Q}$ | Qualified cardinality      | ≥ n.r.c | {x ∈ Δ \|card({y ∈ Δ \|(x, y) ∈ I(r) and y ∈ I(c)}) ≥ n}     |

**role constructs** are syntactic features than can make complex roles./

| Symbol        | Name             | Syntax | Semantics                        |
| ------------- | ---------------- | ------ | -------------------------------- |
| $\mathcal{J}$ | Inverse role     | r⁻     | {(x, y) ∈ Δ × Δ \|(y, x) ∈ I(r)} |
| (∩)           | Role conjunction | r ⊓ᵣ s | I(r) ∩ I(s)                      |
| (∪)           | Role disjunction | r ⊔ᵣ s | I(r) ∪ I(s)                      |
| (¬)           | Role complement  | ¬ᵣ r   | Δ × Δ \ I(r)                     |

Axiom constructs are new types of formulas / sentences.

| Symbol | Name                    | Syntax             | Semantics                                                    |
| ------ | ----------------------- | ------------------ | ------------------------------------------------------------ |
| 𝓕      | Role functionality      | ⊤ ⊑ ≤ 1.r          | if (x, y) ∈ I(r) and (x, z) ∈ I(r) then y = z                |
| 𝓗      | Role hierarchy          | r ⊑ᵣ s             | I(r) ⊆ I(s)                                                  |
| 𝓢      | 𝓐𝐿𝐶 + Role transitivity | Trans(r)           | if (x, y) ∈ I(r) and (y, z) ∈ I(r) then (x, z) ∈ I(r)        |
| ℛ      | Role chain              | r₁ ∘ ... ∘ rₙ ⊑ᵣ s | if (x, y₁) ∈ I(r₁), ..., (yᵢ, yᵢ₊₁) ∈ I(rᵢ₊₁), ... then (x, yₙ) ∈ I(s) |

#### ALC Reasoning Services

**Ontology design** (本体设计) : we are bulding a **conceptual model** (TBox) for our domain; at this design stage we haven't yet **included the data**(ABox).

本体：本体是一种形式化的知识表示，定义了**某个特定的领域中的概念**（对象，类别，属性）以及这些概念中的关系。它类似于一个**词汇表**，但不仅仅是词汇，还包括这些术语之间的结构化关系。

TBox should be : **error free** and **suffficiently detailed**

drive implicit information is detectable by solving concept subsumption (TBox)

### Web Ontology Language

In computer science, ontologies are countable, and an ontology is a formal and explicit specification of the concepts, entities, and relationships that exist within a particular domain or knowledge base.

comprise the following parts:

- logical theory: formal axioms that are true of the domain （约束和关系）
- A documentation: definitions of what the terms in the theory are; what is their kind (class? relation?)

OWL is an ontology language standard for web application ontologies (the semantic web)

#### Main Components of an OWL Ontology

- **classes**: concept，表示一个对象的类型或类别
- **properties**：object properties （roles）用于连接两个个体；datatype properties连接个体与数据类型；annotation properties （ontology explanation for human users）
- **individuals**：类的实例，表示具体的对象或实体

#### Syntax

**classes, hierarchies, individuals**

- $ClassAssertion(:Woman :Mary)$ 做匹配

- $SubClassOf(:Woman :Person)$ 

- $EquivalentClasses(:Person :Human)$

- $DisjointClasses(:Woman :Man)$

- $SameIndividual(:James :Jim)$

- $DifferentIndividuals(:John :Bill)$

- $EquivalentClasses(:MyBirthdayGuests \\ObjectOneOf(:Bill :John :Mary))$

  通过列出类的**所有成员**来定义该类

- $EquivalentClasses(:Parent \\ ObjectUnionOf(:Mother :Father))$

  通过多个类的**并集**来定义一个新的类

- $EquivalentClasses(:Mother \\ ObjectIntersectionOf(:Parent :Woman))$

  通过多个类的**交集**来定义一个新的类

- $ClassAssertion(ObjectIntersectionOf(\\ :Person \\ ObjectComplementOf(:Parent)\\)\\ :Jack)$  

  Complement: Jack is a person but not a parent

**Quantification for the functional syntax**

- Universal: [ALL r c] $EquivalentClasses(:HappyPerson \\ ObjectAllValueFrom(:hasChild :HappyPersion))$
- Existential: [EXIST n r] $EquivalentClasses(:Parent \\ ObjectSomeValuesFrom(:hasChild :Person))$ 
- Max cardinality: $ClassAssertion(ObjectMaxCardinality(4 :hasChild :Person)\\ :John)$
- Min cardinality: $ClassAssertion(ObjectMinCardinality(2 :hasChild :Person)\\ :John)$
- Exact cardinality: $ClassAssertion(ObjectExactCardinality(3 :hasChild :Person)\\ :John)$

Other properties

- Value restrictions: $EquivalentClasses(:JohnsChild \\ ObjectHasValue(:hasParent :John))$ 
- self restrictions: $EquivalentClasses(:NarcissticPerson \\ ObjectHasSelf(:loves))$ 

**Property types**

- each object property **(roles)** must be declared: Declaration(ObjectProperty(:hasPet))
- named individuals **(individuals)** must be declared: Declaration(NamedIndividual(:Dumbledore))
- datatype property must be declared: Declaration(DataProperty(:age))

----------------------------

- Inverse properties: $InverseObjectProperties(:hasParent :hasChild)$

- symmetric and asymmetric: $SymmetricObjectProperty(:hasSpouse) \\ AsymmetricObjectProperty(:hasChild)$

- disjointness: $DisjointObjectProperties(:hassParent :hasSpouse)$

- reflexive and irreflexive: $ReflexiveObjectProperty(:hasRelative) \\ IrreflexiveObjectProperty(:parentOf)$

- Transitve properties: $TransitiveObjectProperty(:hasAncestor)$

- Functional properties: $FunctionalObjectProperty(:hasHusband)$

  功能性属性指的是每个个体在该属性下至多只能有一个值

- Inverse-functional properties: $InverseFunctionalObjectProperty(:hasHusband)$

- subproperty relations and property chain: $SubjectPropertyOf(ObjectPropertyChain(:hasParent :hasParent) \\ :hasGrandparent)$

- keys: identify uniquely individuals by values of key properties

  $HasKey(:RegisteredParent :hasWaitingListN)$

- Object property value: $ObejectPropertyAssertion(:hasWife :John :Mary) \\ NegativeObjectPropertyAssertion(:hasWife :Bill :Mary)$

- Domain / Range restrictions: $ObjectPropertyDomain(:hasWife :Man) \\ ObjectPropertyRange(:hasWife :woman)$

  domain 是定义域，range是值域

### Non-Monotonic Reasoning

Non-monotonic reasoning deals with the problem of deriving plausible conclusions, **but not infallible**, from a knowledge base.

Since the conclusions are not certain, it must be possible to retract some of them if new information shows that they are wrong.

**Closed World Assumption**: only positive information is represented explicitly. Negative information is not represented explicitly. understand the following statement:

you can prove something (negative) because **we can't conclude it** not because **we can prove it**!

CWA is a non-monotonic!

**Frame Problem**: 我理解的frame problem，指的是，由于你要描述物体改变状态后，某些属性是否改变，则你需要把所有的改变状态对属性的影响全部写出来，否则CWA会认为有改变，这是一个棘手的问题，应为很大情况下，你并不能把所有的变化操作给列举出来。

#### Default Logic

default logic adds the concept of 'generality'

Goal: be able to say a P is a Q in general, but not necessarily 

it is reasonable to conclude Q(a) given P(a), unless there is a good reason not to

**Varieties of Defaults**

- general statements:

  - prototypocal: the prototypical P is a Q    e.g. Owls typically hunt at night
  - Normal: Under typical sircumstances, P's are Q's    e.g. People work close to where they live
  - Statistical: Most P's are Q's.    e.g. The people in the waiting room are growing impatient

- Lack of information to the contrary

  - Group confidence: All known P's are Q's  e.g. Natural languages are easy for children to learn
  - Familiarity:  If a P was not a Q, you would know it. An older brother

- Conventioanl

  - conversational:  Unless I tell you otherwise, a P is a Q   

    e.g. There is a gas station two blocks east   ----> the gas station is open

  - Representational: Unless otherwise indicated, a P is a Q    e.g. the speed limit in a city

- Persistence

  - Inertia: a P is a Q if it is used to be a Q   e.g. Colours of objects, locations of parked cars

Default Logic is a nonmonotonic reasoning **formalism** desgined to handle reasoning with defaults, or typical **assumptions** (above mentioned), **in situations where complete information may not be available**.

It extends classical logic by **non-standard inference rules**.
$$
\begin{align}
\frac{bird(x)\ \ \ \ \ \ :fly(x)}{fly(x)}
\end{align}
$$
interpreted as: if x is a bird and we can consistently assume that x flies then we can infer that x flies!
$$
\begin{align}
\frac{P(x) :\   Q_1(x) Q_2(x).....Q_n(x)}
{R(x)}
\end{align}
$$
General form: if $P(x)$ holds and $Q_1(x) Q_2(x).....Q_n(x)$ can be consistently assumed then we can conclude $R(x)$.

Terminology:

- $P(x)$: the prerequisite, a condition that must be true for the default to be considered
- $Q_1(x) Q_2(x).....Q_n(x)$: the justification, conditions that are assumed to be true unless there is evidence to the contrary.
- $R(x)$ the consequent, what can be concluded if the prerequisite is true and the justifications are consistent with the current knowledge

**default theory**

a default theory is a pair $<D,W>$, where D is a set of default rules and W is a set of first-order formulas.

#### Extensions 

The notion of extension of a default theory: the theory obtained by extending W by the rules in D. Defined by E

E is deductively closed: $E = Th(E)$

##### Semi-inductive definition

The order in which defaults are considered in step Deriving is significant:

different orders give rise to different extensions

##### Normal default

default logic is considered to have normal default rules.

a normal default is a default rule where the **justification and the conclusion are the same**

a default d is normal if has the form $\frac{\alpha : \beta}{\beta}$

A normal default theory is a default theory where all defaults in D are normal.

**Theorem**: A normal default theory always has an extension

##### Inference relation

- credulous inference: if there **exists** an extension of default theory such that A is in E

  If it is possible, i will believe it.

- skeptical inference: if for **all** extensions E of default theory, we have A in E. 

  I will only believe it if it is guaranteed to be true in every possible extension

#### Problems with default logic

unwanted transitivity

semi-normal: the justification implies the consequent

A semi-normal default theory (= a theory where all default are semi-normal) may have no extensions

Add priority to the default defaults

There is no notion of entailment amongst defaults

**Auto epistemic logic ** defaults are represented as sentences and can be reasoned with 

