# OCaml
Langage statiquement, fortement typé, fonctionnel avec support de l'orienté objet.

## Commentaires

```ocaml
(* Un commentaire sur une ligne. *)
(* Un commentaire sur
plusieurs lignes *)

(* Un commentaire (* contenu dans *) un autre. *)
```

## Identifieurs

Parler de _

## Variables

Utilisation du mot-clé `let` pour la création d'une variable:

```ocaml
let x = 12
```

(opération appellée `let binding`).  
Dans cet exemple, le type de la variable n'est pas précisé, mais il est possible de le faire:

```ocaml
let x: int = 12
```

Une variable crééé de cette manière existera de l'endroit où elle est définie jusqu'à la fin du bloc dans lequel elle est déclarée.

Il est possible de créer une variable existante dans une expression particulière avec le mot-clé `in`:

```ocaml
let y = 
    let x = 3 * 2 in
    x * 2
(* x n'existe plus ici ! *)
```

L'expression `3*2` est évaluée en premier, puis le résultat est "injecté" dans le bloc après le `in`.  
Il est évidemment possible de combiner plusieurs blocs `in` les uns dans les autres.

# Les variables sont **immutables** !!!

## Les types fondamentaux

### Les entiers

Le type `int` représente des entiers relatifs codés en complément à 2. Ils prennent:
* **31** bits (et pas 32 !) sur les processeurs 32 bits.
* **63** bits (et pas 64 !) sur les processeurs 64 bits.

##### En l'occurence, un seul bit est réservé par OCaml pour déterminer si la valeur est un nombre "normal" ou un pointeur.

### Les réels

Le type `float` représente des réels relatifs codés suivant la norme *IEEE 754* (précision double).

#### (Il existe toutefois les modules `Int32` et `Int64` pour représenter des entiers natifs).

```ocaml
let x = 12.333
let y = 13. (* le . est nécessaire pour ne pas confondre avec un entier *)
```

### Écriture des nombre

* Pour plus de lisibilité, on peut "aérer" les nombres avec des `_`:

```ocaml
let n = 1_000_000
let o = 12_345.67_8_9
```

* Les **entiers** peuvent être exprimés dans différentes bases:

```ocaml
let p = 0x123F (* hexadécimal *)
let q = 0o126 (* octal *)
let r = 0b1011 (* binaire *)
```

avec `x`, `o`, et `b` pouvant être aussi en majuscule.

### Les booléens

Le type `bool` représente une valeur qui est soit vraie (`true`) soit fausse (`false`).

### Les caractères

Le type `char` permet de représenter un caractère suivant le charset ISO-8859-1. Un `char` occupe un octet.

```ocaml
let c: char = 'a' (* Un caractère se crée avec des guillemets simples *)
```

Certains caractères peuvent être échappés avec `\`:

|Séquence|Signification|
|--------|-------------|
|`\n`|LF|
|`\r`|CR|
|`\t`|Tabulation|
|`\b`|Backspace|
|`\'`|Guillemet simple|
|`\"`|Guillemet double|
|`\\`|Antislash|

### Les chaînes de caractères

Le type `string` représente une suite de caractères (`char`) sous la forme d'un `array`. Une `string` est donc mutable !

```ocaml
let s: string = "bonjour"
```

Il est possible de stocker des caractères **Unicode** dans une chaîne avec l'échappement `u{XXXX}` où `XXXX` représente le code en hexadécimal du caractère en question.

```ocaml
let s = "\u{207A}"
```

## Les types composés

### Les listes

Le type `list` représente une collection **immutable** d'éléments **du même type** sous la forme d'une liste simplement chaînée.

```ocaml
let l: int list = [1;2;3]
let empty = [] (* Polymorphique *)
let one_elem = [1] (* Ou alors: let one_elem = [1;] *)
```
Il est aussi possible d'écrire une liste avec l'opérateur `::` (associatif à droite):

```ocaml
let l = 1 :: 2 :: 3 :: []
```

Il est aussi possible d'utiliser l'opérateur `::` pour **déstructurer** une liste:

```ocaml
let hd :: tl = [1;2;3]
```
`hd` récupère le premier élément (ici 1) et `tl` récupère la liste composée des autres éléments (ici `[2;3]`).

**Attention**: la déstructuration échouera si elle est appliquée sur une liste vide ! L'opérateur `::` est donc un pattern **non exhaustif**.

Enfin, on peut extraire un élement précis d'une liste en la reproduisant:

```ocaml
let [1;x;_] =  [1;2;3]
(* x = 2 *)
```

### Les tuples

Un tuple est une collection immutable d'éléments de types **qui peuvent être différents**:

```ocaml
let t = (1, 'c', "abcd", 2)
```

En OCaml, le type d'un tuple est `t1 * t2 * t3... * tn`. Dans l'exemple ci-dessus, le type de `t` est `int * char * string * int`.

Comme les listes, un tuple peut être déconstruit avec un pattern:

```ocaml
let (x, _) = (1, 2)
(* x = 1 *)
```

### Les arrays

Les arrays sont des tableaux d'éléments **mutables** du même type sous la forme d'une zone de mémoire continue.

```ocaml
let a = [|1;2;3;4|] (* il ne s'agit pas d'une liste mais bien d'un array ! *)
let b = a.(0) (* accès à un élément, b vaut 1 *)
a.(1) <- 12 (* modification du deuxième élément du tableau, a vaut maintenant [|1;12;3;4|] *)
```

Il est possible d'extraire des éléments d'un array à l'aide d'un pattern:

```ocaml
let [|_;a;b;_|] = [|1;2;3;4|]
(* a = 2 et b = 3 *)

## Ressources

* [Site officiel](https://v2.ocaml.org/manual/)
* [Cheat sheets officiels](https://github.com/OCamlPro/ocaml-cheat-sheets)
* [RealWorldOCaml](https://dev.realworldocaml.org)
* [DLDC OCaml](https://www2.lib.uchicago.edu/keith/ocaml-class)
