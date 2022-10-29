# Kotlin

# Commentaires
* `//`, fin de ligne
* `/* */`, multi-lignes (nestable)

# Variables

## Définition
### Variables immutables
```kt
val nom: Int //déclaration et typage
nom = 12

val nom: Int = 12 //déclaration et initialisation en une ligne

val nom //déclaration et omission du type
nom = 12

val nom = 12 //omission du type
```
### Variables mutables
Utilisation du mot-clé `var` à la place de `val`.

**Une assignation n'est PAS une expression !!!**

# Types
## Types numériques
### Nombres entiers signés
Codés en **complément à 2**.
|Nom|Taille (octets)|
|-|-|
|Byte|1|
|Short|2|
|Int|4|
|Long|8|
### Nombres entiers non signés
Tous les nombres signés préfixés par un **U** (`UByte`, `UShort`, `UInt`, `Ulong`).

### Nombres flottants
Suivent la norme **IEEE-754**.
|Nom|Taille (octets|
|-|-|
|Float|32|
|Double|64|

### Nombres et inférence de type
* Pour les entiers: `Int` est déterminé sauf si la valeur est trop grande: `Long` sera alors utilisé.
* Pour les flottants: `Double` est utilisé.

### Suffixes
* `f`/`F` pour un `Float`
* `u`/`U` pour un nombre non signé
* `L` pour un `Long`

### Lisibilité
Les nombres entiers comme flottants peuvent contenir des `_` pour améliorer la lisibilité:
```kt
val v = 1_2_3 //v == 123
```

### Ecriture dans d'autres bases
**Uniquement pour les nombres entiers:**
```kt
val a = 0xFF
val b = 0b10111
```

## Booléens
Type `Boolean`, valeurs `true` et `false`.

## Caractères
Représente un caractère UTF-16.
### Définition
```kt
val c: Char = 'A' //utilisation de ' obligatoire 
```
### Caractères spéciaux
`\t`, `\b`, `\n`, `\r`, `\'`, `\"`, `\\`, `\$`
### Caractères Unicode
```kt
val c = '\uCODE`
```

# Opérateurs
## Opérateurs arithmétiques
* `+`, `-`, `*`, `/`, `%`
* `+=`, `-=`, `*=`, `/=`, `%=`
* `++`, `--` (**préfixes et postfixes**)

## Opérateurs d'égalité
### Egalité structurelle
`==`, `!=` (correspondent à des appels à `equals`)
### Egalité référentielle
`===`, `!==`

## Opérateurs de comparaison
`<`, `>`, `<=`, `>=` (correspondent à des appels à `compareTo`)

## Opérateurs logiques
`&&` (et **paresseux**), `||` (ou **paresseux**), `!` (non)

# Packages
Le package d'un fichier se déclare comme en Java; cependant, il n'y a pas besoin de placer le fichier dans l'arborescence correspondante comme en Java

# Imports


# Point d'entrée
Fonction `main`:
```kt
fun main() {
    //sans les arguments
}
```
```kt
fun main(args: Array<String>) {
    //avec les arguments
}
```