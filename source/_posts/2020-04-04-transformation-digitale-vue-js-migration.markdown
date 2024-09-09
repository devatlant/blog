---
layout: post
title: "Transformation digitale. Partie 2 - migration sur vue.js"
date: 2020-04-04 14:28:51 +0200
comments: true
categories: french Transformation software JS Vue.JS migration logiciel
published: true
author: Denis Bezuhlyi
---

{% img flotte /images/Migration_vue_js_logo.png 100 100 %}
Dans cet article [Denys Bezuhlyi](https://www.linkedin.com/in/denys-bezuhlyi/) présentera notre expérience de passage sur VueJS depuis JS 
pure et JQuery. **VueJS** c’est un framework _Javascript_ qui est devenu très à la mode,
 car il propose une architecture assez simple, mais en 
 même temps très puissant, ayant un support très fort de côte de communauté. 
 Jétez un oeil  le nombre des étoiles sur Github (plus de *161K* au moment
 de l'écriture de l’article, si on compare avec React JS, il n’a que *146K*).

<!-- more -->

## <a name="Choix de la technologie"></a> Choix de la technologie
Au début de l'année 2019 on comprenait bien que l’architecture des fichiers JS 
n’est pas très bonne, on utilisait [Grant](https://gruntjs.com/) qui est peu supportable maintenant, 
[Bower](https://bower.io/) qui est aussi tres peu utilisé le dernier temps par les développeurs 
Front-end. En plus, l'équipe de [Leroy Merlin Ukraine](https://leroymerlin.ua) pensait d’une grosse changement de 
design de site, ce qui n'était pas possible avec les technologies qu’on utilisait 
sur le site. Bien évidemment on a pensé de **VueJS**, on en parle partout, donc on a 
décidé d’essayer, comprenant le risque de ce passage.

## <a name="Remplacement des managers des packages"></a> Remplacement des managers des packages
Alors, au début il fallait nettoyer un peu notre projet, on avait deux manageurs 
des packages: [npm](https://www.npmjs.com/) et [Bower](https://bower.io/), donc vu que [Bower](https://bower.io/) est mal soutenu par la communauté 
on a laissé [npm](https://www.npmjs.com/) comme le manager principale, en plus il est beaucoup plus puissant. 
On pouvait utiliser Yarn aussi, mais on ne gagne presque rien si on change [npm](https://www.npmjs.com/) par [Yarn](https://yarnpkg.com/), 
donc on a choisi [npm](https://www.npmjs.com/), parce qu’on l’avait déjà. Après, il fallait supprimer [Grant](https://gruntjs.com/) 
qui est devenu ancien et mal soutenu par la communauté aussi. Pour le remplacer on 
a choisi [Webpack](https://webpack.js.org/), qui tout le monde utilise. La seule chose que [Grant](https://gruntjs.com/) fait maintenant 
dans notre projet - c’est la génération de la liste de changement à partir des commits 
sur git après la mise en production, on n’a pas toujours le temps pour le refaire en 
créant un script sur [NodeJS](https://nodejs.org/).

## <a name="Nettoyage du projet"></a> Nettoyage du projet
Après avoir commencé la migration sur [Webpack](https://webpack.js.org/) on a décidé 
de laisser juste 3 fichiers qui vont être généré (comme les fichiers principale, 
les points d'entrée de l’application): le fichier JS avec les bibliothèque utilisées, 
le fichier JS avec le code VueJS et le code ancien et bien évidemment le fichier CSS. 
Malheureusement, on n’a pas pu utilisant la fameuse [Vue CLI](https://cli.vuejs.org/), 
car ce n'était pas le début du projet, on avait déjà une grande partie de code ancien 
qu’il fallait supporter, on devait avancer pas à pas, donc on a créé notre propre configuration 
[Webpack](https://webpack.js.org/) avec toutes les règles nécessaires. Le problème 
suivant est que toutes les fichiers JS/CSS étaient inclus dans les différents 
fichiers HTML, en ayant des centaines des fichiers c’est assez difficile à maintenir. 
Donc, il fallait examiner chaque fichier et inclure les fichiers JS dans le bon endroit. 
Mais bien sur on a rencontré les difficultés: ces fichiers étaient des scripts JS simple, 
écrit sous ES5, il fallait les reécrire selon le modèle des modules JS pour pouvoir les utiliser 
comme les modules. En plus il y avait souvent les conflits entre les fichiers différents, donc il 
fallait dépenser des heures pour comprendre d'où vient le bug, en corrigeant le problème. 
Une autre situation on avait avec les fichiers CSS. Avant la migration on écrivait les 
feuilles de styles en utilisant SCSS, mais toutes ces fichiers se compilaient en fichiers 
différents, alors qu’on voulait déployer sur la prod un seul fichiers css pour gagner un 
peu de temps (en le perdant lors de la première chargement de site). A vrai dire, on 
avait énormément des conflits liés aux fichiers SCSS, et on passé encore des heures en 
essayant de réorganiser et refactoriser les fichiers SCSS pour que le site ait le même 
vue qu’avant. Au bout d’un moment on a pu résoudre tous les conflits et [Webpack](https://webpack.js.org/) 
était bien mise en place.

## <a name="Implementation de VueJs"></a> Implementation de VueJs
Après avoir préparé la nouvelle infrastructure on a commencé à intégrer le framework [VueJS](https://vuejs.org/) 
dans notre projet. Cela n'était pas très évident aussi, car on voulait agir pas à pas, par 
itérations, on voulait le code ancien et créer les nouvelles fonctionnalités en 
utilisant [VueJS](https://vuejs.org/), en faisant le refactoring du code ancien 
au fur et à mesure. Le moteur de “templating” qu’on a pour le moment c’est [Themeleaf](https://www.thymeleaf.org/), 
il marche très bien avec Java (la partie Backend est écrit utilisant Java, notamment [Spring](https://spring.io/)), 
mais on ne pouvait pas l’utiliser en même temps avec VueJS (dans le meme fichier), 
car il y a des conflit des tags VueJS et [Themeleaf](https://www.thymeleaf.org/), en plus les fichiers thymeleaf 
sont traités par le serveur, alors que les fichiers VueJS sont traités par le Front. 
Donc on devait toujours séparer ces deux technologies, et en réalité cela ne pose pas 
beaucoup de problèmes. Mais cela pose des problèmes pour SEO (maintenant la situation 
s’est changée, mais au début de 2019 cela n'était pas le cas). Le problème était 
qu’au moment Google bot scanne le site - il n’attend pas la chargement des fichiers JS, 
alors tout le contenu qui est affiché par VueJS - il n’est pas visible pour Google Bot, 
comme le résultat - on perd notre SEO, bien sur c’est une très grosse perte pour le site 
ecommerce et on devait proposer une solution pour notre client. La première chose dont on 
a pensé c’est d’utiliser et d'intégrer l'approche SSR. Cela marche parfaitement si on 
utilise NodeJS comme le serveur, bien qu’il y a certaines contraintes (il y a plus d’info 
sur le site officiel [Server-Side Rendering](vuejs.org/v2/guide/ssr.html)), mais nous, 
on utilise Java comme le serveur, alors cela ne nous convenait pas. Pour Java, 
il y a une autre solution - un projet Open Source - Nashorn. Cet outil permet d'intégrer 
SSR sur le serveur Java, mais le problème principal est qu’il est assez lent, 
en plus c'était assez difficile de l’ajouter dans notre infrastructure, donc on a décidé de 
rejeter cette idée. 

## <a name="La nouvelle architecture"></a> La nouvelle architecture
En comprenant qu’on a des gros problemes avec SEO, on a commencé à chercher les 
solutions alternatives, en gardant l'interprétation de code JS par le Front. Alors, 
en fait on a décidé de créer une structure hybride: les parties du code qui sont très 
importantes pour SEO - on les écrit en utilisant Thymelef, les parties dynamique et 
qui ne sont pas très importantes pour SEO - on les écrit sous [VueJS](https://vuejs.org/), en plus on modifie 
le code ancien (soit on le supprimait, soit on le modifiait en suivant le nouveau syntax ES6). 
Donc, au finale on ne pouvait pas avoir une seule point d'entrée dans l’application, ce qui est 
le cas pour une grande majorité des projet sous [VueJS](https://vuejs.org/), on a plusieurs. En comprenant que 
potentiellement on peut avoir une centaine des points d'entrée, on a écrit un dispatcher, 
qui gère toutes ces points d'entrée. L'idée de ce dispatcher est assez simple: il a un 
container dans lequel on peut ajouter des composant VueJS dérivé de l’Initializer Abstrait
 et le deuxième méthode - il ne fait qu'appeler les composant enregistrés en boucle. A son tour, 
 l’Initializer abstrait c’est une classe JS qui determine les preference de composant [VueJS](https://vuejs.org/) 
 par défaut (localisation, router, vuex store, etc.) et en plus permet configurer l’appel 
 de composant a partir de l’identifiant parent de chaque page, cela nous permet de filtrer les composant 
 sur chaque page. Donc chaque composant doit maintenant être dérivé de ce Initializer Abstrait 
 et être enregistré dans le container de Dispatcher relie avec un “marker” spécifique 
 (l’identifiant de la page). C’est très simple, mais tout cela nous permet de construire 
 la structure hybride de l’application [VueJS](https://vuejs.org/) qui a plusieures point de l'entrée grâce à ce Dispatcher.

En plus, il faut dire qu'à la fin de l'année 2019 Google a changé la version de 
Google crawler, maintenant il utilise la version du Chrome plus récent, et 
normalement maintenant il attend le chargement et l’affichage de fichiers JS.

## <a name="Conclusion"></a> Conclusion
En conclusion, il faut dire que la migration depuis JS+JQuery sur VueJS 
n’est pas une tache simple, il faut résoudre beaucoup de conflits dans le 
code et mettre en place une belle architecture pour pouvoir faire ce passage 
d’une manière indolore et lisse pour le client, on ne peut pas le faire tout de suite, 
mais on a réussi de le faire et maintenant l’architecture est devenu plus moderne avec 
la possibilité d'entrer étendu d’une manière beaucoup plus facile et rapide qu’avant, 
ce qui est très intéressant pour chaque client.
	
