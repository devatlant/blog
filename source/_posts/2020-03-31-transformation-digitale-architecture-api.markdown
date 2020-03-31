---
layout: post
title: Transformation digitale. Partie 1 - API
date: 2020-03-31 12:01:38 +0300
comments: 
categories: french Transformation API software java
---

{% img flotte /images/API_Toolbox_for_java_developer_FR_logo.png 100 100 %}
Nous commençons une série d'articles pour partager notre expertise dans la domaine de la Transformation digitale. 
La première article présente notre retour d'expérience sur un très beau projet développé pour [Leroy Merlin Ukraine](https://leroymerlin.ua) 🇺🇦 au cours de 2019. 
Ce projet avait pour l'objectif de permettre **la création automatique des commandes web** accessibles aux partenaires via **API**❗️

<!-- more -->
**API** c'est un sigle en anglais **Application Programming Interface**. 
On peut imaginer une API comme un contrat type entre un fournisseur de services et plusieurs clients. 
Cela ouvre la multitude de possibilités pour un système eCommerce du fournisseur.
> ℹ️️ Par exemple, le constructeur de cuisines pourrais utiliser son outil 3D existant et envoyer les commandes directement 
dans le back-office du SI de Leroy Merlin. 
Ainsi le business peut engager les nouvelles collaborations tout en assurant le contrôle sur les commandes et les paiements.

Le schéma suivant met en évidence quelques outils techniques qui se trouvent dans notre boite d'outils préférés pour ce genre de projet:  
{% img /images/API_Toolbox_for_java_developer_FR.gif %}

## &#128214; Sommaire

Maintenant on expliquera plus en détail chaque brique logiciel présenté ci-dessus.

* [Architecture](#Architecture)
* [Spring](#Spring)
* [Securité](#Security)
* [Swagger](#Swagger)
* [Error](#error)
* [jUnit](#jUnit)
* [I18n](#i18n)
* [Packaging](#packaging)

## <a name="Architecture"></a> Architecture
En terme d'architecture nous avons opté pour une approche **hybride** - Microservice dans Monolithe 😱. 
Techniquement c'est un jar à part entier avec toute la logique centré API (configuration, sécurité, controllers REST), 
mais qui se trouve à l'intérieur du monolithe. 

Pourquoi une telle approche ? En fait, le déploiement indépendant est un processus assez délicat dans l'échelle 
d'une grande entreprise (hébergement, sécu, réseaux, etc.) 
Mais cette approche hybride permettra dans le futur proche d'extraire ce composant sous la forme du **fat Jar** 
ou **conteneur Docker** et le déployer  comme une vraie micro-services. 

> ℹ️️ Oui, en pratique l'élégance et la beauté d'une technologie avancée peut être très limitée par les circonstances rigides autour de vous. 
> Mais comme des professionnels et malgré les circonstances on doit accepter des inconvenients  




