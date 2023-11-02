---
title: "Internationalisation Et Autres Trucs"
date: 2023-11-02T16:56:15+01:00
draft: true
tags: ["dev", "internationalisation"]
---

# Internationalisation et autres sujets

Aujourd'hui, on va un peu survoler l'essentiel du travail que je fais depuis quelques semaines, car il s'agit d'un sujet fleuve, à la fois par le travail que ça représente, mais aussi par l'enjeu qui se trouve derrière. En effet, notre appli est pour le moment française et anglaise. Mais des pays divers veulent utiliser Pix, et il faut pouvoir s'adapter rapidement. Donc, il faut pouvoir traduire rapidement et facilement tout notre référentiel (ce qui constitue le coeur de Pix), mais aussi ajouter une langue facilement.

## Le référentiel - Cas d'usage

Mais d'abord, petit rappel utile pour expliquer ce qu'est le référentiel. Pix, c'est avant tout un service public. Et un service qui permet de valider des compétences numériques (en com', on dit "digitales" :D). Ces compétences sont classés dans des catégories en fonction de ce qu'elles recouvrent. Plus précisément, on a une chaîne d'éléments que je vais rapidement détailler :
On a un **référentiel** dans lequel se trouvent des **compétences** dans lesquels se trouvent des **sujets** regroupant des **acquis** affichant finalement des **épreuves**.

Pour résumé :

> Référentiel > Compétences > Sujets > Acquis > Epreuves

Jusqu'ici, seules les épreuves étaient traduites en anglais essentiellement. Mais le système était tel que 1 épreuve = 1 langue. Donc, jusqu'ici, il y a autant d'épreuves que de langues disponibles. Pour simplifier les choses, on a donc décidé de n'avoir qu'une épreuve comme modèle en quelque sorte, à partir de laquelle on ajoutera différentes traductions.

Ainsi on passera de **n épreuves pour n langues** à **n langues pour 1 épreuve**.

## La release - kesaco

Si on revient aux origines de Pix, il faut savoir qu'utiliser Airtable était un choix qui était judicieux à l'époque (petit projet, facilité d'utilisation, notamment pour les non dev). Mais en grossissant, c'est devenu plus compliqué de choisir d'y rester. Mais pas impossible : il y a eu un déploiement incroyable d'imagination et de technique pour pouvoir continuer à utiliser Airtable sans que l'utilisateur final en ressente sa lenteur. Quoiqu'il en soit, l'idée à plus long terme est de se passer d'Airtable et de revenir à du full postgreSQL.

La **release** est un moyen, entre autres choses, de pallier au souci de l'appel à Airtable, qui est une BDD qui a son intérêt mais dont le problème majeur reste la lenteur et sa limitation de 10 requêtes en une fois. Chaque jour, une nouvelle release du référentiel est créée, une sorte de snapshot du référentiel. Associée à un système de cache dans une architecture assez balèze (on y reviendra sur un autre article, gros sujet aussi la mise en cache !), l'appli devient très rapide et efficace.

Ainsi, avec notre nouvelle fonctionnalité, le schéma de la release doit être modifié pour y inclure les futures traductions, mais sans néanmoins casser le fonctionnement actuel ! Car oui, ...

## Avant, pendant et après, mais surtout pendant !

... L'autre élément compliqué à prendre en compte, c'est que comme il s'agit d'un ajout de fonctionnalité assez énorme (ajout d'une table _traductions_, prise en compte desdites traductions pour **chaque élément** du référentiel), on procède par étapes. Ce qui implique qu'il faut faire en sorte que tout continue de fonctionner avec la méthode actuelle mais aussi avec la future méthode (celle qu'on implémente, quoi).
Autrement dit, on travaille non seulement à faciliter le travail au futur (migrer les traductions existantes dans la nouvelle BDD, ajouter une nouvelle langue facilement, ajouter des traductions pas encore existantes), mais aussi à ne pas casser ce qui fonctionne actuellement. On est sur de la double lecture et double écriture !

## Comment on traduit ?

La dernière chose à prendre en compte est de choisir et connecter un outil externe de traduction qui donne la possibilité de faire des traductions automatiques mais également de laisser des traductrices et traducteurs extérieurs à Pix travailler dessus. L'objectif étant de donner la possibilité d'importer/exporter l'ensemble des traductions afin de travailler dessus en dehors des applis Pix.

## Table _Translations_

Alors pour le moment, le gros du travail se passe du côté de l'API et de la connexion avec l'outil externe de traduction choisi. Le travail se fait en mob-programming (travail en équipe, l'un ou l'une d'entre nous prend le clavier pendant 20 minutes et se fait dicter le code par les autres. Puis on alterne. Et une fois que tout le monde a fait son tour, break de 15 minutes. Comme une sorte de Pomodoro, en fait), avec parfois des surprises (Tiens, tous les containers n'ont pas mise à jour le cache, mais pourquoi donc ?), des chemins tortueux (bonjour clean architecture, je ne te connais pas, tu fais quoi dans la vie ?), mais toujours de nouvelles choses passionnantes à apprendre !

Et pour m'assurer que je comprends ce que je fais, je m'assure de participer aux minis-conférences qu'on fait parfois pour l'équipe engineering pour évoquer notre avancée (puisque c'est un sujet transverse) avec quelques points techniques ! Exercice qui peut être terrifiant, mais super bien pour se forcer à verbaliser correctement ce qu'on a compris.
