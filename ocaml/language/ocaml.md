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
|`\n`|LF (aller à la ligne)|
|`\r`|CR (retour en début de ligne)|
|`\t`|Tabulation|
|`\b`|Retour arrière|
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

|Opérateur|Signification|
|---------|-----------|
|=|Teste l'**égalité structurelle**, c'est-à-dire l'égalité des contenus.|
|<>|Négation de =|
|<=, <, >, >=|Teste l'**inégalité structurelle**.|
|==|Teste l'**égalité physique**, c'est-à-dire l'égalité de références.|
|!=|Inverse de ==|

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

Il est même possible d'utiliser `::` plusieurs fois dans une même déstructuration:
```ocaml
let first :: second :: third :: tl = [1;2;3;4;5]
```

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
let t: (int * char * string * int) = (1, 'c', "abcd", 2)
```

En OCaml, le type d'un tuple est `t1 * t2 * t3... * tn` où `tx` représente le type de l'élément n°`x`. Dans l'exemple ci-dessus, le type de `t` est `int * char * string * int`.

Comme les listes, un tuple peut être déconstruit avec un pattern:

```ocaml
let (x, _) = (1, 2)
(* x = 1 *)
```

## Les arrays

Les arrays sont des tableaux d'éléments **mutables** du même type sous la forme d'une zone de mémoire continue.

```ocaml
let a: int array = [|1;2;3;4|] (* il ne s'agit pas d'une liste mais bien d'un array ! *)
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

### Création et utilisation d'un variant

Les **variants** sont des énumérations dont les valeurs peuvent être paramétrées.
```ocaml
type mon_type = (* le nom du variant doit commencer par une minuscule *)
    | A (* les noms des valeurs doivent commencer par une majuscule *)
    | B of int (* une valeur paramétrée *) 
    | C of int * string (* une valeur paramétrée avec plusieurs types, pas un tuple ! *)
    | C2 of (int * string) (* un tuple ! *)
    | D of {x: int} (* une valeur paramétrée par un record ("inline record" ) *)
```
Pour créer une instance d'un variant:
```ocaml
let x: mon_type = A (* précision du type *)
let y = B 12
let z = C (12, "12") (* parenthèses nécessaires ! *)
let z2 = C2 (12, "12")
let a = D {x = 12}
```

### Déstructuration d'un variant

Il est possible de déstructurer des variants pour récupérer le contenu.
```ocaml
type mon_variant = 
    | A of int
    | B of string * string

let A(x) = A 12 (* x = 12 *)
let B(_, y, "ijkl") = B ("abcd", "efgh", "ijkl") (* y = "efgh" *)
```

### Variants génériques

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
L'utilisation se fait de la même manière que pour les variants non génériques:
```ocaml
let a: (int, char) double_option = Some (12, 'a') (* précision du type *)
let b: (string, float) double_option = Some ("abcd", 12.3) (* a et b sont de types différents ! *)
```

### Les variants récursifs

Un variant peut contenir un champ qui est lui-même une instance du même variant ! Cette méthode est utilisée pour représenter des structures de données.
```ocaml
type 'a liste = 
  | Nil (* cas terminal (pas de récursivité ici) *)
  | Cons of 'a * 'a liste (* cas non terminal (récursif) *)
```
##### Création d'un type `liste` générique: `Nil` correspond à la liste vide `[]`, et `Cons` correspond à l'opérateur `::`. 
```ocaml
let l: int liste = Cons(12, (Cons(13, Cons(14, Nil))))
```
##### Utilisation du type `liste` pour créer d'une liste d'entiers.

## Les records (type produit)

### Création et utilisation d'un record

Les **records** sont des types structurés.
```ocaml
type point2d = { (* le nom du record doit commencer par une minuscule *)
    x: int; (* le nom d'un champ doit commencer par une minuscule *)
    y: int (* le ; du dernier champ est optionnel *)
}
```

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

### Désambiguïsation du type d'un record

On notera que l'on a pas précisé le type de `p` dans l'exemple précédent. En effet, OCaml est capable de le retrouver à partir des noms des champs (ici `x` et `y`). Cependant, lorsque plusieurs *records* ont des attributs en commun, OCaml va essayer de déterminer le type en regardant quels champs sont utilisés. S'il n'y a pas assez d'informations, OCaml utilisera le dernier *record* défini dans l'ordre du code.
```ocaml
type t1 = {
  a: int;
  b: int
}

type t2 = {
  a: int;
  b: int
}

type t3 = {
  a: int;
  b: int
}

let t = {a = 1; b = 2} (* t est de type t3 car il s'agit du dernier record compatible défini *)
```
### Champs `mutable` d'un record
Les records peuvent contenir des champs modifiables, marqués `mutable`:
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

### Les records génériques

Tout comme les variants, les records peuvent être paramétrés par des types génériques:
```ocaml
type ('a, 'b) paire = {
    premier: 'a;
    second: 'b
}
```
##### Représentation d'une paire d'éléments de types différents.

```ocaml
let p1: (int, string) paire = {premier = 12; second = "13}
```
Le typage se fait de la même manière que pour les variants.

### Mise à jour fonctionnelle d'un record
Le mot-clé `with` permet de créer une nouvelle instance d'un record à partir d'une instance existante tout en modifiant un ou plusieurs champs:
```ocaml
type record = {
  a: char;
  b: int;
  c: string;
  d: float
}

let r = {
  a = 'a';
  b = 12;
  c = "c";
  d = 12.1
}

(* création d'un nouveau record r' à partir de r mais en changeant les valeurs de c et de d *)
let r' = {r with c = "d"; d = 15.1}
```
Lorsque l'on utilise la mise à jour fonctionnelle avec un record générique, le nouveau record créé peut être d'un type différent de l'ancien:
```ocaml
type 'a t = {
    a: int;
    b: 'a
}

let r = {a = 12; b = "13"} (* r est de type "string t"*)
let r' = {r with b = 13} (* r' est de type "int t" *)
```

## Alias de type
```ocaml
type nouveau_nom = ancien_nom
```
Les valeurs de type alias sont compatibles avec le type original.

# Gestion des erreurs

## Le type `result`
Défini dans l e module [Stdlib](https://v2.ocaml.org/api/Stdlib.html):
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
La présence d'exceptions n'influe pas sur le type de la fonction; ici, `f` est toujours de type `int -> int`.  
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
##### Contrairement à d'autres langages, il n'est pas possible de fournir un message d'erreur personnalisé affiché lors de l'échec de l'assertion.

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

## Utilisation d'une fonction

```ocaml
let result = fonction arg1 arg2...
```
En cas d'ambigüité, les `()` permettent de séparer les arguments.
```ocaml
let res = somme (a + b) (c + d)
```


## Opérateurs d'application
L'opérateur `|>` est l'opérateur d'**application inverse**: écrire:
```ocaml
x |> f |> g
```
est équivalent à:
```ocaml
g (f x)
```

//TODO: parler du lien avec l'application partielle

L'opérateur `@@` est l'opérateur d'**application**: écrire:
```
g @@ f @@ x
```
est équivalent à:
```
g (f x)
```

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
```ocaml
let somme (a: int) (b: int) = (* les () sont obligatoires ! *) a + b
```
##### Annotation des types des paramètres

```ocaml
let somme a b : int = (* pas de () ici *) a + b
```
##### Annotation du type de retour

```ocaml
let somme (a: int) (b: int) : int = a + b
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

Le mot-clé `function` permet d'effectuer directement un pattern-matching sur le dernier argument de la fonction. Par exemple:
```ocaml
let prod_eq x = function
  | (a, b) when a * b = x -> true
  | _ -> false
```
la fonction `prod_eq` vérifie que le produit des deux éléments d'une paire d'entiers soit égal à `x`. La paire n'est pas présente explicitement dans la liste d'arguments puisqu'elle est introduite en tant que dernier argument par le mot-clé `function`. C'est sur ce paramètre qu'est effectué le *pattern matching* dans la fonction. On remarquera qu'on emploie pas dans ce cas la construction `match ... with`.

## Fonctions sans argument et procédures

### Fonctions sans argument
On utilise `()` pour remplacer les paramètres:
```ocaml
let f () = (* lors de la définition *)
    42

let i = f () (* lors de l'appel *)
```

### Procédures
```ocaml
(* définition de la fonction *)
let print_hello name = 
  print_string ("Hello " ^ name)

(* appel de la fonction *)
let () = print_hello "Paul"
```

## Fonctions récursives et *tail-call optimization*

Définition d'une fonction récursive avec le mot-clé `rec`:
```ocaml
let rec (* <---- ici *) factorial n = 
    if n = 0 then 1
    else if n = 1 then n
    else n * factorial (n - 1)
```
##### Calcul de la factorielle de `n` de manière récursive.

### Récursion terminale
Les fonctions récursives terminales sont optimisées par le compilateur (transformées en boucles).

## Application partielle
L'application partielle est possible car en OCaml, toutes les fonctions currifiées:
```ocaml
let somme a b = a + b
let somme_2 = somme 2 (* application partielle *)
(* somme est de type int -> int -> int *)
(* somme_2 est de type int -> int *)
```

## Arguments nommés

### Définition d'arguments nommés:
```ocaml
let ratio ~num ~denom = 
    num /. denom (* pas besoin de ~ à l'utilisation *)
```
Lors de l'appel, l'ordre de précision des arguments nommés n'importe pas:
```ocaml
ratio ~denom:20. ~num:12.
```
Mais ils doivent être tous renseignés, sinon il s'agit d'une application partielle.

### Label punning
Au lieu d'écrire:
```ocaml
let num = 12. in
let denom = 20. in
ratio ~num:num ~denom:denom
```
on peut écrire
```ocaml
let num = 12. in
let denom = 20. in
ratio ~num ~denom (* changements ici *)
```
lorsque les arguments ont les mêmes noms que les paramètres formels.

### Type d'une fonction possédant des arguments nommés
Précision du nom de l'argument nommé dans la signature:
```ocaml
num:float -> denom:float -> float
```
##### Type de la fonction `ratio`.

### Typage explicite d'un argument nommé

### Renommage d'arguments nommés
```ocaml
let ratio' ~num:n ~denom:d =
    n /. d
```
`num` a été renommé en `n` et `denom` en `d`. `num` et `denom` ne sont plus utilisables dans le corps de la fonction.  
Cependant, le type contient toujours les noms `num` et `denom`:
```ocaml
num:float -> denom:float -> float
```

### Combinaisons d'arguments positionnels et d'arguments nommés
Les arguments positionnels doivent être précisés dans l'ordre **entre eux** et les arguments nommés peuvent être précisés dans n'importe quel ordre.

## Arguments optionnels
Un argument optionnel est un argument nommé de type `option` qui peut ne pas être renseigné.

### Définitions d'arguments optionnels
```ocaml
let concat ?sep first second = (* ? pour définir un argument optionnel *)
    let sep' = match sep with (* ? inutile dans le corps de la fonction *)
        | None -> ""
        | Some s -> s
    in
    first ^ sep' ^ second
```
Lors de l'appel, on peut:
* ne pas préciser de valeur pour l'argument optionnel: il vaut alors `None`
* lui préciser une valeur avec `~` (**label punning applicable**):
```ocaml
concat ~sep:" " "abcd" "efgh"
```
* lui préciser une valeur avec `?` (la valeur doit être de type `'a option`, **label punning applicable**)
```ocaml
concat ?sep:(Some " ") "abcd" "efgh"
```

### Type d'une fonction possédant des arguments optionnels

Comme pour les arguments nommés, le nom du paramètre optionnel est présent dans la signature mais avec `?` au lieu de `~`:
```ocaml
?sep:string -> string -> string -> string
```
##### Type de la fonction `concat`.

### Valeur par défaut d'un argument optionnel

```ocaml
let concat ?(sep=" ") a b = 
    a ^ sep ^ b
```
La valeur par défaut est utilisée lorsque le paramètre vaut `None` (n'est pas renseigné ou fixé explicitement).

# Création d'opérateurs personnalisés

## Opérateurs préfixes (=> unaires)

Doit commencer par `!`, `?` ou `~` (peut être suivi par un ou plusieurs symboles parmi `~ ! ? $ & * + - / = > @ ^ |`)

## Opérateurs infixes (=> binaires)

Doit commencer par `$`, `&`, `*`, `+`, `-`, `/`, `=`, `>`, `@`, `^`, `|`, `%`, `<` (peut être suivi par un ou plusieurs symboles parmi `~ ! ? $ & * + - / = > @ ^ |`)

## Définition d'une fonction opérateur

Mettre le nom entre ().
```ocaml
let r = 
    let (<=>) = Stdlib.compare in
    -1 <=> 13
```
**Si l'opérateur commence par \*, mettre un espace entre `(` et `*`, sinon l'opérateur sera parsé comme un commentaire.**

# Modules

**Le module [Pervasives](https://v2.ocaml.org/releases/4.02/htmlman/libref/Pervasives.html) est le module ouvert de base.**

## Utilisation de modules
Accéder au contenu d'un module `M` avec `.`:
```ocaml
M.fonction
```

### Import global
```ocaml
open Module (* se place dans le bloc global *)
(* tout le contenu du module Module est disponible jusqu'à la fin du module *)
```

### Import local
* Avec `let open`:
```ocaml
let open Module in
<expression>
(* le contenu de Module est importé jusqu'à la fin du bloc *)
```
* Avec `Module.()`
```ocaml
Module.(
    (* code dans lequel le contenu de Module est importé *)
)
```
Lorsque deux modules sont ouverts, les définitions du dernier masquent celles du premier (si applicable).

### Renommage d'un module

```ocaml
module NouveauNom = AncienNom
```

## Définition d'un module

Le nom d'un module doit commencer par une majuscule:
```ocaml
module NomModule = struct
    (* définitions... *)
end
```
Si ce module se trouve dans un fichier `a.ml`, on y accède depuis un autre fichier par:
`A.NomModule`.  
Un module peut contenir:
* des valeurs (variables et fonctions)
* des valeurs externes (`external`)
* des types
* des exceptions
* des instructions `open`
* des instructions `include`
* d'autres modules
* des classes

## Définition d'un module signature

Un module signature est une interface implémentable par un (ou plusieurs) module(s):
```ocaml
module type Signature = sig
    val f: int -> int (* il existe une fonction f prenant un entier et renvoyant un entier *)

    type enum (* il existe un type nommé enum *)

    (* d'autres déclarations... *)
end
```

Implémentation d'un module signature:
```ocaml
module M: Signature = struct
    let f x = x

    type enum = A | B


    (* toutes les déclarations de la signature doivent être implémentées + ce qu'on veut *)
end
```

## Définition d'un module ET de sa signature en même temps
```ocaml
module M: sig
    (* déclarations *)
end
=
struct
    (* implémentation *)
end
```
Mais ceci n'est pas idiomatique, on préfère séparer signature et implémentation.

## Masquage de valeurs
Est accessible par un autre module seulement ce qui est défini dans l'interface du module actuel:
```ocaml
module M: sig
    val f: int -> int
end
=
struct
    let f x = 2 * x (* n'existe pas dans la signature: elle est donc accessible que dans ce module *)
    let g x = x
end
```

## Le mot-clé `include`
Permet d'ajouter et d'exporter dans un module les déclarations d'un autre module:
```ocaml
module M = struct
    include AutreModule
    (* ... *)
end
```
Cette fonctionnalité permet d'étendre les fonctionnalités de modules existants et même de les remplacer:
```ocaml
module M = struct
  include List
  let ma_fonc i = 2*i
end
```
##### Ajout d'une fonction au contenu du module List

```ocaml
module M = struct
  include List
      
  let mem x l = 
    match l with
      [] -> false
    | hd :: tl -> if hd = x then true else mem x tl
end

open M
(* ... *)
```
Notre fonction `mem` remplace la fonction `mem` du module `List` !  
**Attention: si l'une des fonctions du module `List` fait appel à `mem`, ce sera la fonction originale qui sera appelée.** 

# Les foncteurs

# Modules et compilation séparée

Un fichier `.ml` représente un module, où le nom du module est le nom du fichier avec la première lettre en majuscule:
```
monmodule.ml -> Monmodule
```

S'il existe un fichier d'extension `.mli` du même nom, alors le contenu de ce fichier servira de signature.

## Unité de compilation
Une paire `.ml`/`.mli` constitue une *unité de compilation*. (S'il n'y a pas de fichier `.mli`, alors le fichier `.ml` sera une unité de compilation à lui tout seul).

//TODO: http://wide.land/modules/structures.html

# Ressources

* [Site officiel](https://v2.ocaml.org/manual/)
* [Spécification officielle](https://v2.ocaml.org/manual/language.html)
* [Cheat sheets officiels](https://github.com/OCamlPro/ocaml-cheat-sheets)
* [RealWorldOCaml](https://dev.realworldocaml.org)
* [Cours de l'université de Chicago](https://www2.lib.uchicago.edu/keith/ocaml-class)
* [CS3110, cours de l'université de Cornell](https://cs3110.github.io/textbook/cover.html)
