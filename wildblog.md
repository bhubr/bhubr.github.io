# Exercice "Wild Blog"

Le but de l'exercice est de réaliser une mini-page de blog. Une structure HTML très simple t'est fournie en bas de cette page, qu'il faudra enrichir, et styliser avec du CSS.

On a prévu un créneau d'1h, si c'est trop juste et que tu as une contrainte après ne t'en fais pas, mais sinon tu peux déborder un peu (15-30 min disons).

Voici les opérations à réaliser, et des ressources qui peuvent t'y aider en cas de besoin :

* Laisser de la marge à gauche et à droite de la page, et faire en sorte que le contenu soit centré ([tuto centrage](https://www.alsacreations.com/article/lire/539-Centrer-les-elements-ou-un-site-web-en-CSS.html)).
* Utiliser la police de caractères Lato sur tout le site.
* Faire en sorte que les articles se positionnent côte à côte, et pas l'un en-dessous de l'autre. La méthode importe peu.
* Les articles doivent être entourés d'une bordure continue, grise (éventuellement arrondie mais si tu as le temps). Les bordures doivent être un peu espacées, de façon à ce que deux articles ne soient pas collés. [Tuto bordures et ombres](https://openclassrooms.com/fr/courses/1603881-apprenez-a-creer-votre-site-web-avec-html5-et-css3/1605694-creez-des-bordures-et-des-ombres)
* Il n'est pas problématique que les articles n'aient pas la même hauteur.
* Ajouter une image en haut de chaque article, qui ne doit pas déborder du cadre.
* Ne pas utiliser Bootstrap ou autre framework CSS (mais tu peux faire du Sass si ça te semble nécessaire).
* Avoir un HTML correct, validé par https://validator.w3.org/#validate_by_input.

Un exemple de ce qu'on pourrait obtenir est montré ci-dessous. Le blog n'a pas besoin d'être responsive.

<img src="https://bhubr.github.io/img/WildBlog.png" style="border: 1px solid #aaa" />

Tu peux travailler comme cela t'arrange:
- Sur CodePen
- Ou dans un éditeur ou tu feras du HTML + CSS

Le template de base:

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Exercice Wild Code School</title>
</head>

<body>
  <div class="main">
    <h1>Wild Blog</h1>

    <div class="liste-articles">
      <div class="article">
        <div>
          <h2>Un article</h2>
          <p>On sait depuis longtemps que travailler avec du texte lisible et contenant du sens est source de
            distractions, et empêche de se concentrer sur la mise en page elle-même.</p>
        </div>
      </div>
      <div class="article">
        <div>
          <h2>Un autre article</h2>
          <p>L'avantage du Lorem Ipsum sur un texte générique comme 'Du texte. Du texte. Du texte.' est qu'il possède
            une
            distribution de lettres plus ou moins normale, et en tout cas comparable avec celle du français standard.
          </p>
        </div>
      </div>
      <div class="article">
        <div>
          <h2>Encore un article</h2>
          <p>De nombreuses suites logicielles de mise en page ou éditeurs de sites Web ont fait du Lorem Ipsum leur faux
            texte par défaut, et une recherche pour 'Lorem Ipsum' vous conduira vers de nombreux sites qui n'en sont
            encore qu'à leur phase de construction.</p>
        </div>
      </div>
    </div>
  </div>
</body>

</html>
```