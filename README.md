# Coq Code Style Guide 

(under development, comments / pull requests are welcome)

--- 

## Indentation

Use Emacs suggestions, but:
* Do not indent the content of a `Section`.
* Do not indent `by ...` construction on a new line.
* Reasonable indentation for multi-line expressions.

## Spaces
* All binary operators (including typing operator `:` and `:=`) are surrounded by space.  

:x:                `(fun (g:nat) => 2 * (g*g)): nat->nat`

:heavy_check_mark: `(fun (g : nat) => 2 * (g * g)) : nat -> nat`


## Goal selectors

### `[|||||]`

* Separate `|` by spaces
* No spaces after `[` and before `]` 

:x:                `apply x; [done|].`
:x:                `apply x; [ done | ].`
:x:                `apply x; [done| ].`

:heavy_check_mark: `apply x; [done |].`

:x:                `apply x; [done| |].`

:heavy_check_mark: `apply x; [done | |].`

:x:                `apply x; [done| |basic_solver].`

:heavy_check_mark: `apply x; [done | | basic_solver].`


* If the tactic within `[]` solves the subgoal, make sure it is terminating (use `by` or `done`). It will help locate the point where the proof is broken.

:x:                `apply x; [auto | rewrite IH |].`

:heavy_check_mark: `apply x; [done | by rewrite IH |].`



### 1,2,3:

* Do not separate by spaces

:x:                `1, 2, 3 : basic_solver.`

:x:                `all : basic_solver.`

:heavy_check_mark: `1,2,3: basic_solver.`

:heavy_check_mark: `all: basic_solver.`

### Subgoals
* Use `{,}` style, not `+,-`
* Do not select the last subgoal. Rule of thumb: no `}` following another `}` AND no `}` preceding `Qed`
* Space after `{` and before `}`
* To start a subgoal proof, put selecting `{` on a new line (same indent with the previous line), then start the proof after a space
* `}` goes at the end of the subgoal proof, after a space. No newline
* Within one subgoal, keep the indentation (ignoring `{`)


:x: `+,-` style :(
```
Proof.
  constructor.
  + apply foo. 
    basic_solver. 
  + desf.
Qed.
```
:x: No spaces :( Last subgoal selection :(
```
Proof.
  constructor.
  {apply foo. 
   basic_solver.}
  {desf.}
Qed.
```

:x: Wrong indent :( Wrong `{` style :(
```
Proof.
  constructor.
    { 
      apply foo.  
      basic_solver. 
    }
    desf.
Qed.
```

:x: Not keeping indentation :(
```
Proof.
  constructor.
  { apply foo. 
  basic_solver. }
  desf.
Qed.
```

:heavy_check_mark:
```
Proof.
  constructor.
  { apply foo. 
    basic_solver. }
  desf.
Qed.
```


## Statement of lemmas, theorems, and definitions

### Quantifications

* Use arguments, not `forall`

:x:

```
Lemma collect_rel_ct : forall (f : A -> B) (r : relation A),
  f □ r⁺ ⊆ (f □ r)⁺.
```


:heavy_check_mark:

```
Lemma collect_rel_ct (f : A -> B) (r : relation A) :
  f □ r⁺ ⊆ (f □ r)⁺.
```

## Arguments naming
* Lowercase for variable-like arguments
* Uppercase for property-like arguments
* First, put all variable-like arguments in one line 
* Then use a new line for every property-like argument

:x: 

```
Lemma codom_rel_helper (r : relation A) (in : codom_rel r ⊆₁ s) :
  r ≡ r ⨾ ⦗s⦘.
```

:heavy_check_mark:

```
Lemma codom_rel_helper (r : relation A)
      (IN : codom_rel r ⊆₁ s) :
  r ≡ r ⨾ ⦗s⦘.
```
