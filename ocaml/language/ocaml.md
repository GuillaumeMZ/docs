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

### Écriture des nombres

* Pour plus de lisibilité, on peut "aérer" les nombres avec des `_`:

```ocaml
let n = 1_000_000
let o = 12_345.67_8_9
```

* Les nombres entiers peuvent être exprimés dans différentes bases:

```ocaml
let p = 0x123F (* hexadécimal *)
let q = 0o126 (* octal *)
let r = 0b1011 (* binaire *)
```

avec `x`, `o`, et `b` pouvant être aussi en majuscule.

# TODO: parler des réels

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

L'opérateur `^` permet de concaténer deux chaînes de caractères:
```ocaml
let abcd = "ab" ^ "cd" (* abcd = "abcd" *)
``` 

## Les opérateurs

Les opérateurs mathématiques **diffèrent en fonction de si les opérateurs sont des entiers ou des flottants** !

### Opérateurs mathématiques sur des entiers

`+`, `-`, `*`, `/`, `mod` (infixe)  
`+`, `-` (unaires)

### Opérateurs mathématiques sur des flottants

`+.`, `-.`, `*.`, `/.` (infixe), `**` (puissance)  
`+.`, `-.` (unaires)

### Opérateurs logiques

`&&` (short-circuit), `||` (short-circuit), `not`

### Opérateurs de comparaison

|Opérateur|Commentaire|
|---------|-----------|
|=|Teste l'**égalité structurelle**, c'est-à-dire l'égalité des contenus.|
|<>|Négation de =|
|<=, <, >, >=|Teste l'**inégalité structurelle**.|
|==|Teste l'**égalité physique**, c'est-à-dire l'égalité de références.|
|!=|Inverse de ==|

### Opérateurs d'application

L'opérateur `|>` est l'opérateur d'**application inverse**: écrire:
```
x |> f |> g
```
est équivalent à:
```
g (f x)
```

L'opérateur `@@` est l'opérateur d'**application**: écrire:
```
g @@ f @@ x
```
est équivalent à:
```
g (f x)
```

## Les conditions

En OCaml, les conditions s'écrivent avec les mots-clés `if`, `then` et `else`:

```ocaml
let x = ...
let y = ...

if x < y then
    ...
else (* le else est obligatoire ! *)
    ...
```

Un `else` peut être suivi d'une autre condition afin de former une condition alternative.

Une condition étant une expression, il est possible d'en récupérer une valeur:
```ocaml
let a = if condition then 1 else 2
```

**Les types retournés dans les différentes branches doivent être identiques !**

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
```
Mais il n'est pas possible d'extraire une partie du tableau comme avec les listes.

## Les types personnalisés

### Les variants (type somme)

Les **variants** sont des énumérations dont les valeurs peuvent être paramétrées.

```ocaml
type mon_type = (* le nom du variant doit commencer par une minuscule *)
    | A (* les noms des valeurs doivent commencer par une majuscule *)
    | B of int (* une valeur paramétrée *) 
    | C of int * string (* une valeur paramétrée avec plusieurs types, pas un tuple ! *)
    | C2 of (int * string) (* un tuple ! *)
    | D of {x: int} (* une valeur paramétrée par un record *)
```
Pour initialiser une variable:

```ocaml
let x = A
let y = B 12
let z = C (12, "12") (* parenthèses nécessaires ! *)
let z2 = C2 (12, "12")
let a = D {x = 12}
```

Les variants peuvent être paramétrés par des types génériques:

```ocaml
type 'a option = 
    | Some of 'a
    | None
```
##### Un exemple d'implémentation du type option présent dans la bibliothèque standard d'OCaml.

Lorsqu'il y a plusieurs types génériques, on les déclare de la manière suivante:
```ocaml
type ('a, 'b) double_option = (* les () et les , sont nécessaires ! *)
    | Some of 'a * 'b
    | None
```
##### Pas très utile...

Il est possible de déstructurer des variants pour récupérer le contenu.
```ocaml
type mon_variant = 
    | A of int
    | B of string * string

let A(x) = A 12 (* x = 12 *)
let B(_, y) = B ("abcd", "efgh") (* y = "efgh" *)
```

### Les records (type produit)

Les **records** sont des types structurés.

```ocaml
type point2d = { (* le nom du record doit commencer par une minuscule *)
    x: int; (* le nom d'un champ doit commencer par une minuscule *)
    y: int (* le ; du dernier champ est optionnel *)
}
```
##### Création d'un record.

L'instanciation d'un record se fait de la manière suivante:
```ocaml
let p = {
    x = 12;
    y = 13 (* le ; du dernier champ est toujours optionnel *)
}
```

On peut accéder aux champs individuellement avec la notation `.`:
```ocaml
let x1 = p.x
```

Enfin, les records peuvent contenir des champs modifiables, marqués `mutable`:
```ocaml
type a = {
    mutable x: int
}
```

On les met à jour avec la flèche `<-`:
```ocaml
let l = {x = 12}
l.x <- 13 (* mise à jour du champ x de l *)
```

Comme pour les variants, on peut tout à fait déstructurer un record avec un pattern:

```ocaml

```

## Exceptions

## Pattern matching

## Fonctions

## Modules

**Le module [Pervasives](https://v2.ocaml.org/releases/4.02/htmlman/libref/Pervasives.html) est le module ouvert de base.**

## Ressources

* [Site officiel](https://v2.ocaml.org/manual/)
* [Spécification officielle](https://v2.ocaml.org/manual/language.html)
* [Cheat sheets officiels](https://github.com/OCamlPro/ocaml-cheat-sheets)
* [RealWorldOCaml](https://dev.realworldocaml.org)
* [DLDC OCaml](https://www2.lib.uchicago.edu/keith/ocaml-class)
