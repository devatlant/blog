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
> 📙️️ Par exemple, le constructeur de cuisines pourrais utiliser son outil 3D existant et envoyer les commandes directement 
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
* [Géstion des errors](#error)
* [jUnit](#jUnit)
* [I18n](#i18n)
* [Packaging](#packaging)

## <a name="Architecture"></a> Architecture
En terme d'architecture nous avons opté pour une approche **hybride** - Microservice dans Monolithe 😱. 
Techniquement c'est un jar à part entier avec toute la logique centré API (configuration, sécurité, controllers REST), 
mais qui se trouve à l'intérieur du monolithe. 

Pourquoi une telle approche? En fait, le déploiement indépendant est un processus assez délicat dans l'échelle 
d'une grande entreprise (hébergement, sécu, réseaux, etc.) 
Mais cette approche hybride permettra dans le futur proche d'extraire ce composant sous la forme du **fat Jar** 
ou **conteneur Docker** et le déployer  comme une vraie micro-services. 

> 📙️️ Oui, en pratique l'élégance et la beauté d'une technologie avancée peut être très limitée par les circonstances rigides autour de vous. 
> Mais comme des professionnels et malgré circonstances techniques on doit trouver des compromis ⚖️ pour faire avancer le business.

Dans le reste, l'architecture est classique - le pure REST en JSON.

## <a name="Spring"></a> Spring

Le framework **Spring** est devenu un standard de facto dans le développement logiciel d'**entreprise** en **Java**. 
A notre avis ce framework, créé dans les années 2000 par 🧑‍🔬 [Rod Jonson](https://twitter.com/springrod?lang=fr), a gagné la bataille contre la norme J2EE.

Nous sommes très Spring à DevAtlant et ce framework était un composant technique central dans l'architecture du projet. 
Le sous-projet **Spring.Test** s'est avéré très pratique surtout lors du développement des tests **end-to-end** et de **rétrocompatibilité** de différentes versions de l'API.


## <a name="Security"></a> Securité

La sécurité d'utilisation de l'API était l'une de besoins **non-fonctionnelles** majeures dans le **Cahier de Charges** du projet.
> 📙️ La **performance ⏰** et l'**audit 🔎**  étaient 2 autres besoins non-fonctionnelles clairement exprimés par le client. 
>Cela constituera une article entière dans notre série consacrée à ce sujet.

La pièce centrale du module de sécurité est un **Header HTTP dédié** qui sert à authentifier et autoriser l'accès. 
Ce header contient la **clé sécrète unique**. Chaque partenaire reçoit sa propre clé pour chaque type d'environnement (dev, UAT, prod).

## <a name="Swagger"></a> Swagger
## <a name="Error"></a> Géstion des errors
## <a name="jUnit"></a> jUnit
## <a name="I18n"></a> i18n
## <a name="Packaging"></a> packaging




