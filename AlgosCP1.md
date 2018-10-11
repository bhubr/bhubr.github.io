# Solutions pour les algos 1 et 2 du checkpoint

## Algo 1

L'énoncé pouvait receler une petite "ambigüité", ou plutôt laisser une petite place à l'interprétation, ce qui a donné lieu à quelques variantes entre vos solutions.

Tel qu'il était formulé, l'énoncé ne précisait pas si la fonction à écrire devait recevoir des paramètres pour indiquer le nombre de colonnes, et le nombre de sièges par colonnes. Si on s'en tient strictement à l'énoncé, la fonction n'est pas paramétrable. On peut écrire la fonction comme ceci :

```javascript
function getSeatNumbers() {

  // Le tableau doit être initialisé avant la boucle, et DANS
  // la fonction (s'il est initialisé en dehors, le résultat sera
  // faussé après le 1er appel, à moins de réinitialiser le tableau
  // avant chaque appel)
  const seatNumbers = [];

  // Bien faire attention aux "limites" de la boucle. Comme
  // les numéros commencent à 1, on initialise les compteurs à 1
  for (let col = 1 ; col <= 26 ; col++) {
    for (let seat = 1 ; seat <= 100 ; seat++) {
      // Variante plus "élégante" que col + '-' + seat
      // avec les backticks comme délimiteurs
      // (on appelle cette syntaxe template string - nouveauté d'ES6)
      seatNumbers.push(`${col}-${seat}`);
    }
  }

  // Renvoyer le résultat après la fin de la boucle
  return seatNumbers;
}
```

Dans "la vraie vie", ce type de fonction serait probablement écrit de façon à être plus "générique", c'est-à-dire réutilisable dans différents contextes : par exemple pour des cinémas ou théâtres ayant un nombre de colonnes et un nombre de sièges par colonnes différents.

On écrirait alors la fonction avec deux paramètres attendus, pour gérer tous les cas possibles, et on utiliserait ces paramètres pour définir les conditions d'arrêt des boucles.

```javascript
function getSeatNumbers(numColumns, numSeatsPerColumn) {
  const seatNumbers = [];
  for (let col = 1 ; col <= numColumns ; col++) {
    for (let seat = 1 ; seat <= numSeatsPerColumn ; seat++) {
      seatNumbers.push(`${col}-${seat}`);
    }
  }
  return seatNumbers;
}
```

## Algo 2

Il y avait plusieurs points, plus ou moins critiques, à corriger. Vous les avez bien identifiés :
* Enlever la double-flèche dans la déclaration ! Tel qu'elle est écrite initialement, `minMax` est une *fonction qui renvoie une fonction*. Alors, on n'a pas encore vu cela, mais ce genre de chose existe en JS. Mieux, c'est très puissant, et on s'en sert souvent une fois qu'on comprend comment ça marche ! Mais ici, il fallait effectivement enlever `() => ` sur la ligne de la déclaration.
* Affecter la fonction à une `const` plutôt qu'à une `var` (bien qu'utiliser une `var` ne pose pas réellement de problème, on veut garder la cohérence avec du code moderne, c'est-à-dire basé sur ES6).
* Déclarer `min` et `max` avec `let` au lieu de `const`.
* Initialiser un tableau vide pour stocker les résultats.
* "Pousser" `min` et `max` dans ce tableau et pas dans le tableau d'entrée.
* Retourner ce tableau tout à la fin, et pas dans la boucle.
* Corriger `i = array.length - 1` en `i < array.length` (on aurait aussi bien pu écrire `i <= array.length - 1`).
* Corriger `i+1` dans la déclaration du `for` en `i += 1` ou en `i++`.
* Corriger `if (array(i) < min)` en `if (array[i] < min)`.
* Corriger `min = array` en `min = array[i]`.
* Corriger `if (array[i] = max)` en `if (array[i] > max)`.
* Eventuellement, et *suivant la solution adoptée*, écrire `let i = 1` au lieu de `let i = 0` pour initialiser la boucle. J'insiste, cela dépendait de votre solution !

Ensuite, parmi les points où vos solutions pouvaient varier, il y avait l'initialisation de `min` et `max`. Vous avez pu être influencés par les deux premières lignes de la fonction, les affectation `min = 0` et `max = array[0]`.

Il fallait d'une certaine façon "choisir" entre ces deux façons d'initialiser `min` et `max` (avec des variantes possibles).

Quelques statistiques sur les différentes solutions proposées:
* 5 d'entre vous ont intialisé à la fois `min` et `max` ; quelques-uns laissant `max` à zéro, quelques-uns initialisant `min` et `max`, respectivement à une valeur positive très élevée, et à une valeur négative très basse.
* 5 d'entre vous ont initialisé `min` et `max` avec `array[0]`.
* 1 d'entre vous a utilisé une façon très différente.

Je vous détaille tout cela !

### Solution 1 (imparfaite)

Celle-ci marche dans la plupart des cas, du moins avec tous les exemples de tableaux d'entrée proposés.

**Mais** elle ne marche plus si le tableau d'entrée comporte :
* uniquement des nombres supérieurs à `min`
* uniquement des nombres inférieurs à `max`

Ce [post sur StackOverflow](https://stackoverflow.com/questions/307179/what-is-javascripts-highest-integer-value-that-a-number-can-go-to-without-losin) demande quelle est la valeur entière la plus élevée en JS. En résumé :
* la valeur positive la plus élevée est "2 puissance 53 - 1", c'est à dire 2 multiplié par lui-même 53 fois, moins 1. On peut écrire cela `Math.pow(2, 53) - 1` ou depuis ES7 `2 ** 53 - 1` (*exponentiation operator*). La valeur négative la plus basse est simplement son inverse.
* en ES6, elle sont accessibles via les constantes `Number.MAX_SAFE_INTEGER` et `Number.MIN_SAFE_INTEGER`

Vous pouvez coller ce code dans un fichier `.js` et l'exécuter avec Node, ou directement le coller dans la console.

```javascript
// On peut enlever les parenthèses autour d'array car c'est le seul paramètre
const minMax = array => {
  // Tableau de résultats
  const result = [];

  // On initialise min et max avec des valeurs très élevées,
  // en positif et négatif respectivement.
  // De cette façon, il est "très probable" qu'on ait au moins
  // un nombre inférieur à min, et au moins un supérieur à max.
  let min = 1000000000;
  let max = - 1000000000;

  // Dans ce cas, on DOIT commencer avec l'index i = 0, et
  // parcourir tous les indices jusqu'à array.length - 1 (inclus)
  for (let i = 0; i <= array.length - 1; i += 1) {
    if (array[i] < min) {
      min = array[i];
    }
    if (array[i] > max) {
      max = array[i];
    }
  }
  result.push(min, max);
  return result;
}

// Les exemples de l'énoncé fonctionnent
console.log(minMax([4, 6, 35, -65, -9, 0, 67]));
console.log(minMax([-30, 5, 43, 108, -5, -7, 89]));
console.log(minMax([56, 7, 63, 9, 7, 12, 85]));

// Mais ceux-ci ne fonctionnent pas...

// Avec des nombres positifs très (TRÈS) élevés
// Devrait renvoyer [140737488355328, 1125899906842624]
console.log(minMax([2 ** 50, 2 ** 47, 2 ** 48]));

// Avec des nombres négatifs très (TRÈS) bas
// Devrait renvoyer [-1125899906842624, -140737488355328]
console.log(minMax([- (2 ** 50), - (2 ** 47), - (2 ** 48)]));
```

En résumé, c'est une solution plutôt astucieuse, mais qui peut échouer.

### Solution 2

Cette solution marche à tous les coups. On initialise `min` et `max` avec des valeurs qui ne sont pas "arbitraires", mais issues du tableau à tester.

En utilisant `array[0]` pour initialiser `min` et `max`, on est certain d'avoir un point de référence, pour les comparaisons qui sont faites dans la boucle.

La version la plus optimale fait démarrer la boucle à partir de l'index 1. Il est en effet inutile de démarrer à l'index 0, car on sait que `array[0]` ne sera ni inférieur, ni supérieur à lui-même. On "gagne" ainsi une itération, mais la version qui démarre de l'index 0 reste correcte.

```javascript
const minMax = array => {
  const result = [];

  // On initialise min et max avec une valeur servant de point
  // de référence fiable pour les comparaisons à suivre.
  let min = array[0];
  let max = array[0];

  // Ici, on pouvait donc laisser let i = 1
  for (let i = 1; i <= array.length - 1; i += 1) {
    if (array[i] < min) {
      min = array[i];
    }
    if (array[i] > max) {
      max = array[i];
    }
  }
  result.push(min, max);
  return result;
}

// Tout fonctionne !
console.log(minMax([4, 6, 35, -65, -9, 0, 67]));
console.log(minMax([-30, 5, 43, 108, -5, -7, 89]));
console.log(minMax([56, 7, 63, 9, 7, 12, 85]));
// Renvoie [140737488355328, 1125899906842624]
console.log(minMax([2 ** 50, 2 ** 47, 2 ** 48]));
// Renvoie [-1125899906842624, -140737488355328]
console.log(minMax([- (2 ** 50), - (2 ** 47), - (2 ** 48)]));
```

### Solution 3

Cette solution marche également à tous les coups. Elle comporte deux variantes.

Les deux font intervenir l'opérateur OU (`||`) dans les conditions testées par des `if`.

L'idée commune, c'est d'affecter une valeur à `min` et `max` dans la 1ère itération de la boucle.

#### Variante 1 : tester la valeur de l'index dans les `if`

Ici, on sait qu'on doit initialiser une valeur lors de la 1ère itération (passage) dans la boucle. Donc, si `i` vaut zéro, on affecte `array[0]` à `min` comme à `max`. Cela revient presque au même que la solution 2 !

```javascript
const minMax = array => {
  const result = [];
  // Ici, peu importe qu'on affecte ou pas une valeur initiale
  let min = 0;
  let max = 0;

  // Mais il faut alors démarrer à i = 0
  for (let i = 0; i <= array.length - 1; i += 1) {
    // Lors du 1er passage, on affecte la valeur trouvée à min
    if (i === 0 || array[i] < min) {
      min = array[i];
    }
    // Idem pour max
    if (i === 0 || array[i] > max) {
      max = array[i];
    }
  }
  result.push(min, max);
  return result;
}

// Tout fonctionne !
console.log(minMax([4, 6, 35, -65, -9, 0, 67]));
console.log(minMax([-30, 5, 43, 108, -5, -7, 89]));
console.log(minMax([56, 7, 63, 9, 7, 12, 85]));
// Renvoie [140737488355328, 1125899906842624]
console.log(minMax([2 ** 50, 2 ** 47, 2 ** 48]));
// Renvoie [-1125899906842624, -140737488355328]
console.log(minMax([- (2 ** 50), - (2 ** 47), - (2 ** 48)]));
```

#### Variante 2 : tester la valeur de `min` et `max` dans les `if`

Ici, on ne *doit pas* affecter de valeur initiale à `min` et `max`, car on repose sur l'absence de valeur (`undefined`) dans les `if`.

```javascript
const minMax = array => {
  const result = [];
  let min;  // undefined par défaut
  let max;  // undefined par défaut

  for (let i = 0; i <= array.length - 1; i += 1) {
    // Lors du 1er passage, min vaut encore undefined, on lui affecte array[0]
    if (min === undefined || array[i] < min) {
      min = array[i];
    }
    // Idem pour max
    if (max === undefined || array[i] > max) {
      max = array[i];
    }
  }
  result.push(min, max);
  return result;
}

// Tout fonctionne !
console.log(minMax([4, 6, 35, -65, -9, 0, 67]));
console.log(minMax([-30, 5, 43, 108, -5, -7, 89]));
console.log(minMax([56, 7, 63, 9, 7, 12, 85]));
// Renvoie [140737488355328, 1125899906842624]
console.log(minMax([2 ** 50, 2 ** 47, 2 ** 48]));
// Renvoie [-1125899906842624, -140737488355328]
console.log(minMax([- (2 ** 50), - (2 ** 47), - (2 ** 48)]));
```
