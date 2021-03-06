## Solution for Problem 2

1. 
   ```ocaml 
   let f x y z = if x y then y else z + 1
   ```

   If `f` is well-typed, its type must have the shape:
   ```ocaml
   f: 'x -> 'y -> 'z -> 'h
   ```
   for some type variables `'x`, `'y`, `'z`, and `'h`.

   Type variables associated with free variables in body of `f`:
   ```
   x: 'x
   y: 'y
   z: 'z
   ```
   
   Derived constraints from body of `f`
   ```
   'a = 'x
   'b = 'y
   'x = 'b -> 'c
   'c = bool
   'd = 'y
   'e = 'z
   'f = int
   'e = int
   'f = int
   'g = int
   'h = 'd
   'h = 'g
   ```
   
   Unifier exists:
   σ = `{'a ↦ (int -> bool), 'b ↦ int, 'x ↦ (int -> bool), 'c ↦ bool, 'd ↦ int, 
         'e ↦ int, 'f ↦ int, 'z ↦ int 'g ↦ int, 'h ↦ int, 'y ↦ int}`

   Thus, `f` is well typed. Applying the unifier to type of `f` yields
   ```ocaml
   f: (int -> bool) -> int -> int -> int
   ```
   
2. 
   ```ocaml
   let g x y z = if x > 0 then x + y else y z
   ```

   If `g` is well-typed, its type must have the shape:
   ```ocaml
   g: 'x -> 'y -> 'z -> 'j
   ```
   for some type variables `'x`, `'y`, `'z`, and `'j`.

   Type variables associated with free variables in body of `g`:
   ```
   x: 'x
   y: 'y
   z: 'z
   ```
   
   Derived constraints from body of `g`
   ```
   'a = 'x
   'b = int
   'a = int
   'b = int
   'c = bool
   'd = 'x
   'e = 'y
   'd = int
   'e = int
   'f = int
   'g = 'y
   'h = 'z
   'g = 'h -> 'i
   'j = 'f
   'j = 'i
   ```
   
   Processing the constraints up to but excluding `'g = 'h -> 'i`
   yields the partial unifier
   
   σ = `{'a ↦ int, 'b ↦ int, 'x ↦ int, 'c ↦ bool, 'd ↦ int, 'e ↦ int, 'y ↦ int, 'f ↦ int, 'g ↦ int, 'h ↦ 'z}`
   
   Applying this unifier to the constraint `'g = 'h -> 'i` yields
   
   ```
   int = 'z -> 'i
   ```
   
   which is not satisfiable regardless of what `'z` and `'i`
   are. Hence, the function is not well-typed. The problem is that `y`
   is used both as an operand of `+`, which forces its type to be
   `int`, as well as in the function application `y z`, which forces
   its type to be `'z -> 'i` for some `'z` and `'i`.


3.
   ```ocaml
   let h x y z = z x (y x)
   ```
   
   If `h` is well-typed, its type must have the shape:
   ```ocaml
   g: 'x -> 'y -> 'z -> 'g
   ```
   for some type variables `'x`, `'y`, `'z`, and `'g`.

   Type variables associated with free variables in body of `h`:
   ```
   x: 'x
   y: 'y
   z: 'z
   ```

   Derived constraints from body of `h`
   ```
   'a = 'z
   'b = 'x
   'a = 'b -> 'c
   'd = 'y
   'e = 'x
   'y = 'e -> 'f
   'c = 'f -> 'g
   ```
   
   Unifier exists:
   σ = `{'a ↦ ('x -> 'f -> 'g), 'b ↦ 'x, 'z ↦ ('x -> 'f -> 'g), 'd ↦ ('x -> 'f), 'e ↦ 'x, 'y ↦ ('x -> 'f), 'c ↦ ('f -> 'g)}`
   
   Thus, `h` is well typed. Applying the unifier to type of `h` yields
   ```ocaml
   h: 'x -> ('x -> 'f) -> ('x -> 'f -> 'g) -> 'g
   ```
   Consistent renaming of the type variables yields the type
   ```ocaml
   h: 'a -> ('a -> 'b) -> ('a -> 'b -> 'c) -> 'c
   ```
   which is the type that the OCaml compiler infers.
   
