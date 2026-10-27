---
hyp: "claude-session"
session: "91bf1771-07e9-488b-832c-f5946e998ce7"
backend: "claude-code"
started: "2026-10-25T09:03:56"
updated: "2026-10-27T07:12:53"
title: "TT: Sequents"
model: "claude-opus-5-5"
tokens_in: 33503
tokens_out: 3813
cost_usd: 0.1329
---

# Claude · TT: Sequents

## You

TT: Sequents: Can you clarify the relationship between TT and Sequent calculus?  It can be confusing in part because sometimes the turnstile is used in both but, I assume, it denotes different things?

## Claude

*Grep …*



*Grep …*



Short answer: the turnstile means essentially the same thing in both. What differs is the shape of the rules that derive the judgments, and whether proofs get names (terms). Most of the confusion comes from three systems that all use `⊢`:

| System | Typical judgment | Rule style |
|---|---|---|
| Natural deduction (sequent-style presentation) | `Γ ⊢ A` | introduction / elimination |
| Sequent calculus (Gentzen LK, LJ, linear logic) | `Γ ⊢ Δ` (possibly several conclusions) | right rules / left rules, plus cut |
| Type theory | `Γ ⊢ t : A`, with `Γ = x₁:A₁, …, xₙ:Aₙ` | almost always intro/elim, i.e. natural deduction with proof terms |

## 1. What `⊢` means in all of them

In every case `⊢` belongs to the **meta-level**. It separates hypotheses from conclusion in a *hypothetical judgment*: "given Γ, this is derivable." Keep it apart from two other symbols:

- **`→` / `⊃`**: implication *inside* the logic, i.e. a formula or type.
- **`⊨`**: semantic truth in all models.

The rule that ties the object level to the meta level is the deduction theorem, or →-introduction: `Γ, A ⊢ B` iff `Γ ⊢ A → B`. In type theory this is λ-abstraction.

There is one real double use. Some authors use `⊢` both as the *sequent arrow*, a piece of syntax being manipulated, and as "is derivable." Careful sequent-calculus texts write the sequent as `Γ ⇒ Δ` and reserve `⊢` for derivability, so that `⊢ Γ ⇒ Δ` means "this sequent has a derivation." Once you see that distinction, most of the notational overlap goes away.

## 2. Type theory = natural deduction plus names for proofs

Under Curry–Howard, `Γ ⊢ t : A` is just `Γ ⊢ A` with the proof recorded as the term `t`, and each hypothesis named by a variable. The standard TT rules are natural-deduction rules:

- **Introduction** = constructor: `λ`, pairing, `inl`.
- **Elimination** = destructor: application, `fst`, `case`.
- **Normalization** = β-reduction (an introduction immediately followed by an elimination cancels out).

Dependent type theory adds more judgment forms (`Γ ⊢ A type`, `Γ ⊢ t ≡ u : A`, `Γ ctx`). They all still have the "context ⊢ conclusion" shape.

## 3. How sequent calculus differs: the rules

Sequent calculus has no elimination rules. Each connective gets a **right rule** (introduce it in the conclusion) and a **left rule** (introduce it in a hypothesis). Proofs only ever grow formulas, which is what gives cut-free proofs the *subformula property*.

Conjunction:

```
ND elim:     Γ ⊢ p : A×B                   SC left:   Γ, x:A, y:B ⊢ t : C
            ─────────────                            ───────────────────────────────────
            Γ ⊢ fst p : A                            Γ, p:A×B ⊢ let (x,y)=p in t : C
```

Implication:

```
ND elim (modus ponens):                   SC left:
Γ ⊢ f : A→B    Γ ⊢ a : A                  Γ ⊢ a : A     Γ, y:B ⊢ t : C
─────────────────────────                 ───────────────────────────────
     Γ ⊢ f a : B                           Γ, f:A→B ⊢ t[f a / y] : C
```

So a left rule is roughly an elimination turned upside down: it acts on a *variable* in the context, like pattern matching, rather than on an arbitrary term.

**Cut** says that from `Γ ⊢ A` and `Δ, A ⊢ C` you get `Γ, Δ ⊢ C`. Computationally it is `let x = a in t`, or substitution. **Cut elimination** (Gentzen's Hauptsatz) is the sequent-calculus counterpart of normalization.

Sequent calculus also has proof-term systems:
- **λμμ̃** (Curien–Herbelin): term assignment for classical LK. Cut elimination becomes evaluation on an abstract machine, and multiple conclusions correspond to continuations/control.
- **LJT / spine calculus**: a term assignment for intuitionistic sequent calculus.

## 4. Structural rules

Sequent calculus states weakening, contraction and exchange as **explicit rules**. Type theory usually leaves them **implicit** in how variables behave:
- using a variable twice is contraction;
- not using it at all is weakening;
- context order is exchange.

That is why substructural logics such as linear logic are naturally presented as sequent calculi: you just drop or restrict those rules. The linear type theories built on them have to enforce the restriction instead, by splitting contexts (`Γ, Δ`) and checking variable usage. This is the same contrast your earlier notes on no-cloning versus discarding were tracking.

## 5. The common categorical reading

In both systems, `Γ ⊢ A` (or `Γ ⊢ t : A`) reads as a morphism `⟦Γ⟧ → ⟦A⟧`:
- the identity axiom `A ⊢ A` is the identity morphism;
- cut is composition;
- the comma in the context is a product (or `⊗` in the linear case).

Natural deduction / TT and sequent calculus are two different syntaxes generating the same morphisms. That is the precise sense in which the turnstile "means the same thing" in both.
