+++
title = "Hindley-Milner Type Inference for STLC and Its Extensions"
author = ["Yichen Xu"]
date = 2021-03-11T16:01:00+08:00
tags = ["test", "type-system"]
draft = false
showDate = true
+++

In this post, we will present the principles of Algorithm W and Algorithm U,
which implement the type inference of Hindley-Milner type system.

Firstly, we describe a small calculus, the well-known Simply-Typed
Lambda Calculus _(STLC)_.


## Simply-Typed Lambda Calculus {#simply-typed-lambda-calculus}

```text
t ::= 'true'
    | 'false'
    | numericLiteral
    | 'pred' t
    | 'succ' t
    | 'iszero' t
    | 'if' t 'then' t 'else' t
    | x
    | '\' x [ ':' T ] '.' t
    | t t
    | 'let' x [ ':' T ] '=' t 'in' t
    | '(' t ')'

T ::= 'Bool'
    | 'Nat'
    | T '->' T
    | '(' T ')'
```


## Type Inference for Hindley-Milner Type System {#type-inference-for-hindley-milner-type-system}

The type inference algorithm can be seperated into two parts:

-   Algorithm W, which traverses the AST and collect constraints about types and
    type variables.

-   Algorithm U, which computes the unification of the constraints.


### Algorithm W: Constraint Generation {#algorithm-w-constraint-generation}

The typing rules are listed below. They can both derive term types and collect
constraints.

```text
Γ ⊢ true : Bool | {}                       (T-True)

Γ ⊢ false : Bool | {}                      (T-False)

Γ ⊢ 0 : Nat | {}                           (T-Zero)

Γ ⊢ t : T | C    C' = C ∪ {T = Nat}
-----------------------------------        (T-Pred)
    Γ ⊢ pred t : Nat | C'

Γ ⊢ t : T | C    C' = C ∪ {T = Nat}
-----------------------------------        (T-Succ)
    Γ ⊢ succ t : Nat | C'

Γ ⊢ t : T | C    C' = C ∪ {T = Nat}
-----------------------------------        (T-IsZero)
  Γ ⊢ iszero t : Bool | C'

Γ ⊢ t1: T1 | C1    Γ ⊢ t2 : T2 | C2
          Γ ⊢ t3 : T3 | C3
  C = C1 ∪ C2 ∪ C3 ∪ {T1 = Bool, T2 = T3}
-----------------------------------------  (T-If)
  Γ ⊢ if t1 then t2 else t3 : T2 | C

  x : T ∈ Γ
--------------                             (T-Var)
Γ ⊢ x : T | {}

  Γ, x : T1 ⊢ t : T2 | C
-----------------------------              (T-AbsGiven)
Γ ⊢ λx: T1. t : T1 -> T2 | C

Γ, x : X ⊢ t : T2 | C
      X is fresh
------------------------                   (T-AbsInfer)
Γ ⊢ λx. t : X -> T2 | C

Γ ⊢ t1 : T1 | C1    Γ ⊢ t2 : T2 | C2
X is fresh, C = C1 ∪ C2 ∪ {T1 = T2 -> X}
----------------------------------------   (T-App)
        Γ ⊢ t1 t2 : X | C
```

The rules T-True, T-False and T-Zero are simply rules for constants. The rules
T-Pred, T-Succ and T-IsZero deals with operators on Nat, and they enforce the
parameter type to be Nat by adding a constraint T = Nat.

The rule T-If types the If expression, by requiring that the condition is of
type Bool and the type of the two bodies are the same.

The rule var reads the type of variable x in the environment.

The rule T-AbsGiven deals with lambda expressions where ascription of the
parameter is given. The T-AbsInfer rule generate a new type variable for the
lambda parameter and type the lambda as in T-AbsGiven.

The T-App types lambda application. It asserts that t1 is of type T2 -> X for
some fresh type variable X throw constraints.


### Algorithm U: Constraint Unification {#algorithm-u-constraint-unification}

The constraints in our case are simply equations. Algorithm U will unify these
equations by processing them one by one to produce a substitution. For each
equation S = T,

1.  S equals to T. Do nothing.

2.  S is a type variable.
    1.  If S appears in T, produce an error, since this is an infinite recursive
        type.

    2.  Otherwise, add a substitution S -> T.

3.  If T is a variable, similar to the previous case.

4.  If both S and T are type constructors, which means they are both function
    types in our simple calculus, i.e. S1 -> S2 = T1 -> T2. We unify S1 and T1,
    S2 and T2 recursively.

5.  In other cases, it fails.


## Extensions {#extensions}


### Let Polymorphism {#let-polymorphism}

To be precise, the previously introduced calculus and type system is not HM. The
real HM has another feature: let-polymorphism. To see what is let polymorphism
and why it is useful, see the following example.

```haskell
let id = \x. x
    add1 = succ x
in (id add1) (id 1)
```

Here, `id` is an identity function, which is very common. However, this code
snippet will not type-check. Let's inspect the type-checking progress in
details.

Firstly, we derive that `id : X -> X` where `X` is a type variable based on the
definition of `id`. And similarly, we have `add1 : Y -> Nat | Y = Nat` when
typing the definition of `add1`. When typing `(id add1)`, we further get the
constraint `X -> X = (Y -> Nat) -> T1`. And when we type `(id 1)`, we get the
constraint `X -> X = Nat -> T2`.

During the unification process, we will first know that `X = Y -> Nat` from the
constraint `X -> X = (Y -> Nat) -> T1`, then know that `X = Nat` from the
constraint `X -> X = Nat -> T2`. We will finally try to unify `Y -> Nat = Nat`,
which will produce an error.

This is caused by the limitation of our current type system. Consider the
function `id = \x. x`. It can work on any type of parameter `x`. It ideally
should have a type like `forall a. a -> a`. The `forall a.` here is a form of
first-order polymorphism, which is the missing part of our current type system.
The technique we will use here, called let polymorphism, is a ad-hoc approach to
fuse first-order polymorphism into our type system without changing it too much.
This is why we call it an 'extension'.

We first introduce the definition of **type scheme**. It can be represented as
`forall X. T`, where `X` is a type variable bound in type `T`. Practically, it
can be represented by

```haskell
data TypeScheme =
  TypeScheme { tvars :: List[TypeVar]
             , tpe   :: Type
             }
```

Here `Type` is some working implementation for types in the system.

The second step is to introduce two new rules in the
type system.

Following is the new typing rule to be introduced.

```text
Γ ⊢ t : T | C   X does not occur in C
-------------------------------------       (T-GenVar)
      Γ ⊢ t : forall X. T | C
```

The rule T-GenVar will generalize a type by generalizing all unconstrained type
variables in the type.

For example, the definition `id = \x. x` of type `X -> X` do not have any
constrain on the type variable `X`. So it will be generalized to a type scheme
`forall X. X -> X`.

The second rule to be introduced is T-InstVar. It instantiated type
variables in a type scheme.

```text
Γ ⊢ t : forall X. T | C   X1 is fresh
--------------------------------------     (T-InstVar)
        Γ ⊢ t : T [X -> X1] | C
```

By leveraging these two new rules, we can type the above example correctly.
Firstly the `id` will be typed as `forall X. X -> X`. In the expression `(id
add1)`, we first instantiate the type of `id` to `X1 -> X1` and produce the
constraint `X1 -> X1 = (Y -> Nat) -> T1`. For expression `(id 1)`, we again
instantiate the type of `id` to `X2 -> X2`, and get the constraint
`X2 -> X2 = Nat -> T2`. The expression can then be type-checked.


### Fixpoint Operator {#fixpoint-operator}

By paying the cost of fully collapse the logic corresponding to our calculus, we
can introduce the fixpoint operator to support general recursion in our small
language. The operator is of type `fix : forall X. (X -> X) -> X`.

We may add one more typing rule to support the operator.

```text
Γ ⊢ t : T | C     C' = C ∪ {T = X -> X}
---------------------------------------    (T-Fix)
         Γ ⊢ fix t : X | C'
```

As mentioned before, this is at the cost of collapsing the logic system
correspoding to our type system. In the system with fix operator and this rule
enabled, False can be proved. Actually, we can write `fix (\x. x)`, which will
be typed as `forall X. X`, which is anything, and it is actually nothing, or the
False. This is how this rule destroy our logic completely.


### Mutually-recursive Definitions {#mutually-recursive-definitions}

Mutually-recursive definition can be implemented via `fix` operator without
adding any more power into the language. However, it definitely makes sense to
add some special structure into the language to support mutually-recursive
definition groups and invent a new rule for typing them.

Consider the `letrec` construct, which is common in many ML family languages.

```haskell
letrec even = \n. if iszero n then True else not (odd (pred n))
        odd = \n. if iszero n then False else not (even (pred n))
in even
```

We can add the following typing rule.

```text
Γ, x1 : X1, ..., xn : Xn ⊢ t1 : T1 | C1
Γ, x1 : X1, ..., xn : Xn ⊢ t2 : T2 | C2
 ...
Γ, x1 : X1, ..., xn : Xn ⊢ tn : Tn | Cn
Γ, x1 : X1, ..., xn : Xn ⊢ t : T | C
C' = C ∪ C1 ∪ C2 ∪ ... ∪ Cn ∪ {X1 = T1, ..., Xn = Tn}
X1, X2, ..., Xn are fresh
----------------------------------------------------- (T-LetRec)
    Γ ⊢ letrec x1 = t1 ... xn = tn in t : T | C'
```

To type the above example, we first assume that `even : X1` and `odd : X2`. When
we type `even` and `odd`, we can get the following judgements:

-   odd : Nat -> T2 | T2 = Bool
-   even : T1 -> Bool | T1 = Nat
-   even : Nat -> T3 | T3 = Bool
-   odd : T4 -> Bool | T4 = Nat
-   X1 = T1 -> Bool
-   X1 = Nat -> T3
-   X2 = Nat -> T2
-   X2 = T4 -> Bool

Via unification, we can derive that

-   T2 = Bool
-   T1 = Nat
-   T3 = Bool
-   T4 = Nat
-   X1 = Nat -> Bool
-   X2 = Nat -> Bool

Therefore, the `letrec` expression can be typed as `Nat -> Bool`.

Note that, to show that `letrec` and the newly introduced typing rule is just
some 'syntax sugars', we can express what have done only with `fix` with tuple types.

```haskell
let metaEvenOdd = \(even, odd) ->
    ( \n -> if iszero n then True else not (odd (succ n))
    , \n -> if iszero n then False else not (even (succ n))
    )
in fix metaEvenOdd
```

We can type that
`metaEvenOdd : (Nat -> Bool, Nat -> Bool) -> (Nat -> Bool, Nat -> Bool)`
and know that
`fix : (Nat -> Bool, Nat -> Bool)`,
which is the desired mutually-recursive functions.


### Record Types {#record-types}

We can add record types into the system. Following is the additional constructs.

```text
t ::= { d* }
    | t '.' x

d ::= x [ ':' T ] '=' t

T ::= { D* }

D ::= x ':' T
```

And we add new typing rules into the type system.

```text
Γ, x1 : X1, ..., xn : Xn ⊢ t1 : T1 | C1
Γ, x1 : X1, ..., xn : Xn ⊢ t2 : T2 | C2
 ...
Γ, x1 : X1, ..., xn : Xn ⊢ tn : Tn | Cn
C' = C1 ∪ C2 ∪ ... ∪ Cn ∪ { X1 = T1, ..., Xn = Tn }
X1, X2, ..., Xn are fresh
--------------------------------------------------------------  (T-Rec)
Γ ⊢ { x1 = t1, ..., xn = tn } : { x1 : T1, ..., xn : Tn } | C'

Γ ⊢ t : T | C
C' = C ∪ { T <: { x : X } }
---------------------------                                     (T-Select)
Γ ⊢ t.x : X | C'
```

Note that the new subtyping relation is introduced in rule T-Select. `S <: T`
means that `S` is a subtype of `T`. We do not have to introduce subtyping into
our system currently. `T <: { x : X }` in the constraint simply means that the
type T is a record type and its member `x` is of type `T`.

We can slightly modify Algorithm U to support record types and the subtyping
relation in the constraint. When we encounter a constraint `T <: { x : X }`, we
simply record the constraint in the type variable. Then, when we are going to
substute a type variable into a concrete type, we check whether they are
compatible.

Consider an example.

```haskell
let f = \p -> plus p.x p.y
in f { x = 1, y = 1 }
```

When typing the function `f`, we derive that
`f : T0 { x : T1, y : T2 } -> Int | T1 = Nat, T2 = Nat`.
And we can type that
`{ x = 1, y = 1 } : { x : Nat, y : Nat }`.
From `f { x = 1, y = 1}`, we get the constraint that
`T1 { x : T1, y : T2 } -> Int = { x : Nat, y : Nat } -> T3`.

During the unification, we will try to unify
`T0 { x : T1, y : T2 }` and `{ x : Nat, y : Nat }`, which will further try to
unify `T1` and `Nat`, `T2` and `Nat`.

Finally, `f` will be typed as `{ x : Nat, y : Nat } -> Nat`, as expected.

Actually, in the `let` form, for type variables that are only constrained by
subtyping relations, we can generalize them just as if they are not constrained,
given that we record the required subtyping constraints into the type scheme. In
this way, the `f` will be typed as
`forall X <: { x : Nat, y : Nat } -> Nat`,
which will handle outputs like `{ x = 1, y = 1, z = true }`.
Such technique can be named as _let subtyping polymorphism_.


## References {#references}

FOS 2020: <https://fos2020.github.io>
