---
title: "Composant React re-écrit en Vue"
date: 2023-08-03T18:59:11+02:00
draft: true
tags: ["dev", "what I learned today"]
---

J'inaugure un type d'article qui sera tagué _"What I learned today"_. C'est court, concis, et c'est souvent un truc nouveau que j'ai découvert, de type astuce.
Si ça vous dit quelque chose, c'est que je reprends (au propre) ce que j'ai déjà écrit sur Twitter, pardon pour le doublon. Bientôt, ce sera totalement inédit (mais ce moment n'est pas arrivé, je suis encore en vacances !).

# C'est quoi l'histoire ?

C'est l'histoire d'un composant écrit en React qu'on a besoin de ré-écrire en VueJS par souci de cohérence.

# C'est quoi le use case ?

La page dans laquelle se trouve ce composant a un bouton "Réinitialiser", impliquant donc de pouvoir réinitialiser ledit composant.
Concrètement, ça signifie qu'on doit re-rendre le composant.

# C'est comment qu'on fait ?

> Alors, tout d'abord, c'est mon collègue qui m'a montré ça, il ne garantit pas le fait que ce soit une bonne pratique, néanmoins, sur la doc de Vue, ça semble être la bonne chose à faire.

Et ben en React comme en Vue, il faut utiliser key en attribut d'une balise, il est recommandé d'ajouter une key qui soit unique. Ca permet à Vue (et à React) d'identifier le composant en question. Alors, pour recharger (ou reset) notre composant, il suffira de changer la valeur de la key, pour "forcer" à le recharger. Attention, une key, on a tendance à y mettre un index, ou un id, bref, un _integer_, mais ici, je parle d'une key vraiment unique comme par exemple le nom de notre element, donc

```javascript
<MyComponent :key="{element.name}" />
```

Mais puisqu'une key est unique et sert à identifier le composant, comment on va changer sa valeur, c'est contre-productif ? Et bien, tout simplement en ajoutant par exemple un chiffre. Donc on va créer un compteur, et à chaque fois qu'on cliquera, ça activera la méthode reset qui, elle, va juste ajouter + 1 à notre key !

Par exemple, on aura :

```html
<button :src="url" :key="url + resetCounter" /> (Je triche un peu, sur le type
de balise là, mais l'important, c'est la key !)
```

## Addendum

D'après un des twittos ayant participé à la conversation autour du fil Twitter, il utilise cette méthode, mais en ajoutant un `timestamp` au lieu d'un compteur. L'idée est bonne, cela garantit l'unicité absolue de la key en ajoutant la date et l'heure du moment ^^

Une autre source, en anglais, indique également que c'est la meilleure méthode de forcer le re-render d'un composant et que la technique s'appelle [Key-Changing Technique](https://michaelnthiessen.com/force-re-render)
