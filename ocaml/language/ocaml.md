# OCaml
Langage statiquement, fortement typé, fonctionnel avec support de l'orienté objet.

# Essayer OCaml

[Essayer OCaml en ligne](https://try.ocamlpro.com/) (nécessite une connexion Internet).


# Commentaires

```ocaml
(* Un commentaire sur une ligne. *)
(* Un commentaire sur
plusieurs lignes *)

(* Un commentaire (* contenu dans *) un autre. *)
```

# Identifieurs

Parler de _

# Variables

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

# Les types fondamentaux

## Les entiers

Le type `int` représente des entiers relatifs codés en complément à 2. Ils prennent:
* **31** bits (et pas 32 !) sur les processeurs 32 bits.
* **63** bits (et pas 64 !) sur les processeurs 64 bits.

##### En l'occurence, un seul bit est réservé par OCaml pour déterminer si la valeur est un nombre "normal" ou un pointeur.

## Les réels

Le type `float` représente des réels relatifs codés suivant la norme *IEEE 754* (précision double).

##### (Il existe toutefois les modules `Int32` et `Int64` pour représenter des entiers natifs).

```ocaml
let x = 12.333
let y = 13. (* le . est nécessaire pour ne pas confondre avec un entier *)
```

## Écriture des nombres

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

## Les booléens

Le type `bool` représente une valeur qui est soit vraie (`true`) soit fausse (`false`).

## Les caractères

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

## Les chaînes de caractères

Le type `string` représente une suite de caractères (`char`) sous la forme d'un `array`.

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

# Les opérateurs

Les opérateurs mathématiques **diffèrent en fonction de si les opérateurs sont des entiers ou des flottants** !

## Opérateurs mathématiques sur des entiers

`+`, `-`, `*`, `/`, `mod` (infixe)  
`+`, `-` (unaires)

## Opérateurs mathématiques sur des flottants

`+.`, `-.`, `*.`, `/.` (infixe), `**` (puissance)  
`+.`, `-.` (unaires)

## Opérateurs logiques

`&&` (short-circuit), `||` (short-circuit), `not`

## Opérateurs de comparaison

|Opérateur|Commentaire|
|---------|-----------|
|=|Teste l'**égalité structurelle**, c'est-à-dire l'égalité des contenus.|
|<>|Négation de =|
|<=, <, >, >=|Teste l'**inégalité structurelle**.|
|==|Teste l'**égalité physique**, c'est-à-dire l'égalité de références.|
|!=|Inverse de ==|

## Opérateurs d'application

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

# Les conditions

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

# Les types composés

TODO: parler de la copie des types composés et personnalisés

## Les listes

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

L'opérateur `::` permet également de **déstructurer** une liste:

```ocaml
let hd :: tl = [1;2;3]
```
`hd` récupère le premier élément (ici 1) et `tl` récupère la liste composée des autres éléments (ici `[2;3]`).

**Attention**: la déstructuration échouera si elle est appliquée sur une liste vide ! L'opérateur `::` est donc un pattern **non exhaustif**.

On peut extraire un élement précis d'une liste également avec une déstructuration:

```ocaml
let [1;x;_] =  [1;2;3]
(* x = 2 *)
```

La concaténation de deux listes s'effectue avec l'opérateur `@`:
```ocaml
let l = [1;2;3] @ [4;5;6] (* l == [1;2;3;4;5;6] *)
```

## Les tuples

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

## Les arrays

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

# Les types personnalisés

## Les variants (type somme)

TODO: parler des variants récursifs, des records inline

Les **variants** sont des énumérations dont les valeurs peuvent être paramétrées.
```ocaml
type mon_type = (* le nom du variant doit commencer par une minuscule *)
    | A (* les noms des valeurs doivent commencer par une majuscule *)
    | B of int (* une valeur paramétrée *) 
    | C of int * string (* une valeur paramétrée avec plusieurs types, pas un tuple ! *)
    | C2 of (int * string) (* un tuple ! *)
    | D of {x: int} (* une valeur paramétrée par un record *)
```
Pour créer une instance d'un variant:
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
let B(_, y, "ijkl") = B ("abcd", "efgh", "ijkl") (* y = "efgh" *)
```

## Les records (type produit)

TODO: parler de la mise un jour d'un record avec with & des variants génériques & du type d'un variant générique

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
On notera que l'on a pas précisé le type de `p`. En effet, OCaml est capable de le retrouver à partir des noms des champs (ici `x` et `y`). Cependant, lorsque plusieurs *records* ont des attributs en commun, OCaml va essayer de déterminer le type en regardant quels champs sont utilisés. S'il n'y a pas assez d'informations, 


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

### Déstructuration d'un record et *field punning*

Comme pour les variants, on peut tout à fait déstructurer un record avec un pattern:
```ocaml
(* record de test *)
type un_record = {
    a: int;
    b: string;
    c: char
}

let r = {a = 12; b = "abcd"; c = 'a'}
let {a = _; b = str; c = 'a'} = r (* str == "abcd" *)
```

On peut aussi remplacer la dernière ligne du code précédent par:
```ocaml
let {b=str;_} = r
```
où `_` permet de dire explicitement: "on ignore tous les autres champs". `_` **doit être placé en dernier**. On peut aussi tout simplement ne pas mettre `_` et dans ce cas, tous les champs non présents dans la déstructuration seront ignorés.
```ocaml
let {b=str} = r (* fonctionne aussi ! *)
```

Imaginons maintenant que nous voulions extraire un (ou plusieurs) champ d'un record dans une (ou plusieurs) variable du même nom. Cela s'écrirait:
```ocaml
let r = {a = 12; b = "abcd"; c = 'a'}
let {a = a; b = b; _} (* a = 12 et b = "abcd" *)
```
OCaml propose une simplification de l'écriture dans ce cas: il n'y a plus besoin de lier la variable avec le champ que l'on veut récupérer puisqu'ils ont le même nom.
```ocaml
let r = {a = 12; b = "abcd"; c = 'a'}
let {a;b;_} (* même effet que la dernière ligne du code précédent *)
```
Cette simplification s'appelle le *field punning*.

# Gestion des erreurs

OCaml propose plusieurs mécanismes pour gérer les erreurs:

## Le type `result`

Le module [Stdlib](https://v2.ocaml.org/api/Stdlib.html) d'OCaml définit le type somme `result` de la manière suivante:
```ocaml
type ('a, 'b) result =
  | Ok of 'a
  | Error of 'b
```
Il peut donc être utilisé comme type de retour d'une fonction qui peut échouer.
```ocaml
(* Cette fonction échoue si x > 50, sinon renvoie x * 2 *)
let f x = 
    if x > 50 then Error "x doit être inférieur ou égal à 50 !"
    else Ok (x * 2)

let x = f 24 (* Ok 48 *)
let y = f 51 (* Error "..." *)
(* Utiliser le pattern matching pour vérifier la réussite (ou l'échec) *)
```

## Les exceptions

OCaml propose également des exceptions équivalentes à celles que l'on peut trouver dans des langages comme C++, Java, Python, etc.

Les exceptions ont toutes un seul type: le type somme `exn`. Il est possible de créer ses propres exceptions (paramétrées ou non) avec le mot-clé `exception`:
```ocaml
exception Foo (* exception non paramétrée *)
exception Bar of int (* exception à 1 paramètre *)
exception Baz of string * string (* exception à 2 paramètres *)
exception Quux = Baz (* renommage d'une exception, Quux et Baz ont les mêmes paramètres *)
```
##### Dans cet exemple, `Foo`, `Bar` et `Baz` viennent donc compléter (et sont de type) `exn`.  

Une fois l'exception créée, on peut la lever avec la fonction `raise`:
```ocaml
let f x = 
    if x > 100 then raise Foo
    else if x > 75 then raise (Bar 12) 
    else if x > 50 then raise (Baz ("abcd", "efgh"))
    else x * 2
```
##### La présence d'exceptions n'influe pas sur le type de la fonction; ici, `f` est toujours de type `int -> int`.
Dans le code appelant, on utilise un bloc `try` avec un filtrage pour associer une exception à une action:
```ocaml
let x = try (f 76) with
  | Foo -> -1 (* le premier | est optionnel *)
  | Bar(x) -> -x
  | _ -> -1000 (* le _ agit comme un cas par défaut, c'est-à-dire tout ce qui n'a pas été matché précédemment *)
```
Le bloc `try` est une expression qui renvoie le type de l'expression testée (entre `try` et `with`). On sépare les cas avec `|`.

## Les assertions

Le mot-clé `assert` permet de vérifier des expressions booléennes et lâche une exception `Assert_failure` si la condition n'est pas respectée.
```ocaml
let sqrt x = 
    assert (x >= 0.) (* vérification de la précondition x >= 0. pour le calcul de la racine *)
    ...
```
Contrairement à d'autres langages, il n'est pas possible de fournir un message d'erreur personnalisé affiché lors de l'échec de l'assertion.

# Pattern matching

Le *pattern matching* est l'une des fonctionnalités les plus intéressantes d'OCaml. Il permet d'analyser la structure d'une expression et de récupérer ses composants. On utilise la forme `match <expression> with` pour effectuer un pattern matching sur `expression`:
```ocaml
let t = (1, 2, 3)
let sum = match t with
    | (x, y, z) -> x + y + z (* cas appellé si t est un tuple de trois entiers *)
    | _ -> 0 (* cas par défaut *)
```
##### Un pattern matching étant une expression, on peut en récupérer une valeur. Ici, on stocke le résultat du *matching* dans `sum`.

On sépare les cas avec le caractère `|` et la flèche `->` sert à associer un *pattern* avec une action.  
Les patterns pouvant être utilisés sont:
* des constantes (de types primitifs mais aussi composés)
* tous les patterns de déconstruction vus jusqu'ici (listes, variants, records, etc.)
* un pattern d'alias: le mot-clé `as` permet de renommer une expression.
```ocaml
let paire = (1, 2)
let f p = match p with
    | (_, _) as s -> s
    | _ -> (0, 0)
```
Ce code teste `p` et s'il s'agit d'une paire d'entiers alors on la renvoie sans la modifier, sinon on renvoie la paire `(0, 0)`.
On pourrait le réécrire en
```ocaml
 let paire = (1, 2)
let f p = match p with
    | (x, y) -> (x, y)
    | _ -> (0, 0)
```
mais ce dernier code est moins efficace que le premier puisqu'on déconstruit la paire dans le premier pattern puis on la reconstruit dans la branche associée.  
**Le pattern `as` peut être utilisé en dehors d'un pattern matching.**

* un pattern "ou exclusif": il permet tout simplement de lier plusieurs patterns à une seule action associée.
```ocaml
match chiffre with
    | 0 | 3 | 6 | 9 -> print_string "chiffre multiple de 3"
    | 1 | 2 | 4 | 5 | 7 | 8 -> print_string "chiffre pas multiple de 3"
    | _ -> print_string "pas un chiffre"
```
* un pattern d'exception: le pattern matching permet, comme le mot-clé `try`, d'attraper des exceptions si elles sont levées.
```ocaml
exception Foo of int
let f x = 
    if x > 50 then raise (Foo 12)
    else x * 2

match f 51 with
    | x -> print_string "une valeur normale"
    | exception Foo x -> print_int x
```
Comme dans un bloc `try with`, on peut récupérer le(s) paramètre(s) (si applicable) de l'exception via un pattern de déconstruction.
* Un pattern `when`: le pattern `when` permet d'ajouter une condition supplémentaire à un pattern. Par exemple, le code suivant permet de vérifier si le premier élément d'une paire est égal à une valeur passée en paramètre:
```ocaml
let is_first_element_x p x = match p with
    (a, _) when a = x -> true
  | _ -> false
```
Ce pattern permet de simplifier l'écriture de conditions, mais il a un défaut: lorsqu'il est utilisé, le compilateur n'est plus capable de vérifier que le pattern prenne bien en compte tous les cas. Par exemple:
```ocaml
let is_even x = match x with
    _ when x mod 2 = 0 -> true
  | _ when x mod 2 = 1 -> false
```
ce code détermine si un nombre passé en paramètre est pair ou non avec un pattern `when`. Bien que tous les cas soient pris en compte (soit le nombre est pair soit il ne l'est pas), le compilateur affiche un avertissement car il est possible selon lui que le pattern échoue (ce qui n'est pas le cas).

# Fonctions

## Création d'une fonction

La manière la plus simple d'écrire une fonction est
```ocaml
let nom param1 param2 param3 = 
    expression
```

où `nom` représente le nom de la fonction et les `param` les différents paramètres.  
Par exemple, une fonction qui renvoie la somme de ses deux paramètres s'écrit
```ocaml
let somme a b = a + b
```
Le contenu d'une fonction devant être une expression, il n'existe pas de mot-clé `return` ou équivalent.

## Fonctions anonymes

Il est aussi possible de créer des fonctions anonymes: il s'agit de fonctions sans nom qui sont généralement utilisées dans des appels de fonction.  
La fonction `somme` de la section précédente peut donc s'écrire:
```ocaml
fun a b -> a + b
```
mais peut s'écrire aussi:
```ocaml
fun a -> fun b -> a + b
```
Ces deux écritures sont **strictement équivalentes**. La seconde forme semble inutilement complexe, mais nous y reviendrons dans la section traîtant de la notion de *currying*.  
Une fonction (anonyme) étant une expression, il est possible de la stocker dans une variable:
```ocaml
let somme = fun a b -> a + b
```

## Type d'une fonction

Le type de la fonction `somme` (qu'il s'agisse de l'écriture d'une fonction normale ou anonyme) est:
```ocaml
int -> int -> int (* déduit par le compilateur *)
```
Pour lire un type de cette forme, on commence par regarder le type après la dernière `->`: il s'agit du type de retour de la fonction. Les autres types sont, dans l'ordre, les types des paramètres.  
Prenons l'exemple de la fonction suivante, qui commence par concaténer deux chaînes de caractères puis convertit le résultat en nombre:
```ocaml
let concat_and_convert a b =
    let c = a ^ b in (* concaténation *)
    int_of_string c (* conversion en entier *)
```
Le type de cette fonction est:
```ocaml
string -> string -> int
```
On commence par regarder le dernier type (`int`): il correspond au type de retour de la fonction. Ensuite, le premier type (`string`) correspond au type du premier paramètre (`a`) et le deuxième type (`string`) correspond au type du second paramètre (`b`).

## Annotation manuelle des types des paramètres et de la valeur de retour

Parfois, les types des paramètres et/ou du type de retour déduits par le compilateurs ne sont pas assez précis ou ne correspondent pas à ce que l'on attend. Dans ce cas, on peut annoter la fonction pour préciser les types des paramètres et/ou le type de retour:
```ocaml
let somme (a: int) (b: int) = (* les () sont obligatoires ! *)
    a + b
```
##### Annotation des types des paramètres

```ocaml
let somme a b : int = (* pas de () ici *)
    a + b
```
##### Annotation du type de retour

```ocaml
let somme (a: int) (b: int) : int =
    a + b
```
##### Annotation des paramètres ET du type de retour

Les mêmes principes s'appliquent pour les fonctions anonymes:

* Pour la forme "simplifiée": 
```ocaml
let somme = fun (a: int) (b: int) : int -> a + b
```

* Pour la forme décomposée: le type de retour est annoté sur la dernière fonction
```ocaml
let somme = fun (a: int) -> fun (b: int) : int -> a + b
```

## Le mot-clé `function`

## Fonctions sans arguments et procédures

## Fonctions récursives et *tail-call optimization*

## Application partielle et notion de *currying*

## Arguments nommés et arguments optionnels

# Création d'opérateurs personnalisés

# Modules

**Le module [Pervasives](https://v2.ocaml.org/releases/4.02/htmlman/libref/Pervasives.html) est le module ouvert de base.**

# Interfaçage avec le C 

# Ressources

* [Site officiel](https://v2.ocaml.org/manual/)
* [Spécification officielle](https://v2.ocaml.org/manual/language.html)
* [Cheat sheets officiels](https://github.com/OCamlPro/ocaml-cheat-sheets)
* [RealWorldOCaml](https://dev.realworldocaml.org)
* [Cours de l'université de Chicago](https://www2.lib.uchicago.edu/keith/ocaml-class)
* [CS3110, cours de l'université de Cornell](https://cs3110.github.io/textbook/cover.html)
