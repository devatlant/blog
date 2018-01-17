---
layout: post
title: "Building Telegram chatbot in 15 minutes"
date: 2017-08-13 15:25:00 +0300
comments: true
categories: technology french Telegram conference
---

{% img flotte /images/chatBotsTelegram.jpg Building chatbot for Telegram in 15 minutes by DevAtlant Software team %}

L’idée était de capter l’attention des étudiants par la création d’une application __ludique et interactive__. 
L'interaction avec les étudiants a été assurée grace aux smartphones qui sont aujourd'hui omniprésents. Le processus de création – __l’élaboration du code d'application__ dans l’IDE 
– a été montré du A à Z  avec le vidéoprojecteur.
 
Et après cela nous avons décrit les différentes opportunités offertes aux étudiants ukrainiens pour continuer leurs 
études ou la vie professionnelle en France.

<!-- more -->

## Introduction
En Avril 2017 notre équipe a organisé sa première conférence __« Building Telegram chatbot in 15 minutes »__ dont l’objectif
 était de créer un événement __medio-technologique__ pour motiver les étudiants ukrainiens. Cette motivation a 2 facettes :
 
1.	Technologique – passionner les jeunes gens par la technologie
*	Culturelle – promouvoir les riches possibilités d’évolution offertes par le gouvernement français aux étudiants 
ukrainiens (les bourses d’études, le mouvement  [#FrenchTech](http://www.lafrenchtech.com))

Cette conférence a eu lieu à l’[Université d’Etat de la Chimie et de Technologie de Dnipro](http://udhtu.edu.ua).

Nous avons consciemment choisi le titre très concret еt technologique pour attirer le plus grand nombre des étudiants. 
Les chatbots, le messenger Telegram sont très à la mode en ce moment. 

## Racines
La meilleure façon pour réussir votre présentation c’est de faire agir votre audience. Cette technique j’ai vu pour la 
première fois en 2009 au soirée [Paris JUG](https://www.parisjug.org) animée par [Didier Girard](https://twitter.com/didiergirard). 
Faire du code on live, déployer sur un serveur avec IP publique et inviter l’audience à voter en utilisant des SMS – c’étais tout 
simplement époustouflant. Cela m’a marqué à tel point que je m’en souviens en 2017 et je le raconte dans cette article.


## Chatbot
En s’inspirant de cette approche, nous avons créé le jeu « Deviner un nombre » sous forme de chatbot Telegram. 

{% img img-thumbnail img-responsive https://raw.githubusercontent.com/devatlant/chat-bot-game/master/res/telegram_screenshot.jpg %}

### principe du jeu 
Le principe est très simple :

1. Chatbot génère un nombre aléatoire entre 0 et 999, 
+	N’importe quelle personne ayant l’application Telegram peut se connecter à notre chatbot et essayer deviner le nombre.
+	Chatbot accepte uniquement des chiffres en entrée et donne en retour une réponse avec l’indice – plus petit ou plus grand. 
+	Le premier qui trouve ce nombre gagne la partie.

### Programmation

Lors de la présentation 
nous avons montré le processus de la __création de l’application du A à Z__:

* faire le fork du GitHub,
* importer le projet dans Intellij IDEA,
* écrire du code en Java,
* déployer l'application sur Telegram comme chatbot.

Programmation et toute l’activité autour étaient les composants clés de la présentation. 
En __30 minutes__ nous avons essayé de lever le rideau de la __programmation industrielle__:

* systèmes de gestion des versions (git), 
* IDEs moderne comme [Intellij Idea](https://www.jetbrains.com/idea/), 
* exemples du code industriel (__defensive programming, design pattrens__, etc.), 
* approche __TDD__, 
* systèmes de contrôle de qualité comme [Sonar](https://www.sonarqube.org),
* systèmes d'Intégration continue [Jenkins](https://jenkins-ci.org)

Le code source de ce bot est disponible sur [notre github](https://github.com/devatlant/chat-bot-game).

## Conclusion

Voici notre bref retour d'expérience:  

* les slides __PowerPoint sont inutiles__ pour ce genre d'évènement.  
* le code qui tourne et qui peut être utilisé en direct par chaque étudiant fais un __"wow"__ effet. 
La période d’interaction avec le jeu a été la plus dynamique et animée. Les étudiants sont devenus énergiques et intéressés par tout c’est qui se passait dans la salle.      

