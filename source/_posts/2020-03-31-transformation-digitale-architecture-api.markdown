---
layout: post
title: Transformation digitale. Partie 1 - API
date: 2020-03-31 12:01:38 +0300
comments: 
categories: french Transformation API software java spring logiciel
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
* [Géstion des errors avec Vnd.Errors](#error)
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

Le framework [Spring](https://spring.io) est devenu un standard de facto dans le développement logiciel d'**entreprise** en **Java**. 
A notre avis ce framework, créé dans les années 2000 par 🧑‍🔬 [Rod Jonson](https://twitter.com/springrod?lang=fr), a gagné la bataille contre la norme J2EE.

Nous sommes très Spring à DevAtlant et ce framework était un composant technique central dans l'architecture du projet. 
Le sous-projet **Spring.Test** s'est avéré très pratique surtout lors du développement des tests **end-to-end** et de **rétrocompatibilité** de différentes versions de l'API.


## <a name="Security"></a> Securité

La sécurité d'utilisation de l'API était l'une des besoins **non-fonctionnelles** majeures dans le **Cahier de Charges** du projet.
> 📙️ La **performance ⏰** et l'**audit 🔎**  étaient 2 autres besoins non-fonctionnelles clairement exprimés par le client. 
>Cela constituera une article entière dans notre série consacrée à ce sujet.

La pièce centrale du module de sécurité est un **Header HTTP dédié** qui sert à authentifier et autoriser l'accès. 
Ce header contient la **clé sécrète unique** 🔑. Chaque partenaire reçoit sa propre clé pour chaque type d'**environnement** (dev, recette, production).

```
SECRET_API_KEY=A9890-C65B899-AA00011
```

Le problématique majeur c'est la provenance de cette clé sécrète. Qui doit le générer et attribuer à chaque partenaire ? 
Pour les solutions automatisées nous recommandons l'utilisation de la technologie 
[oAuth 2.0 boosté par Spring](https://docs.spring.io/spring-security-oauth2-boot/docs/current/reference/html/boot-features-security-oauth2-authorization-server.html). 
Pour le nombre limité des clients de vos API, vous pouvez générer et stocker la clé à la demande dans la configuration de serveur.   

## <a name="Swagger"></a> Swagger

Sur tous nos projets on applique les principes d'**industrialisation 🏗 du génie logiciel**. L'un de ces principes est l'**automatisation**. 
>Pour le code source on utilise ```Maven, GitLab, Jenkins, Sonar Qube, Nexus```. Rien de nouveau ici.

Mais la question de la **qualité de la documentation** se pose très souvent dans tous les projets plus ou moins complexes. 
Et cette question devient cruciale quand on parle de l'API, où la qualité de la documentation doit être toujours **impeccable**, 
car la doc est utilisée par plusieurs équipes **externes**❗️ De l'autre coté, soyons franc, les développeurs n'aiment pas écrire 🤮 еt 
surtout maintenir à jour la documentation du projet 🆘. Nous avons trouvé une solution à ce paradoxe - le projet [Swagger](https://swagger.io) ✅

Swagger permet d'incorporer les ```annotations``` spécialisées autour de vos méthodes du ```@RestController```. 
Et ces annotations sont utilisés lors de la génération de la documentation. 
Un immense avantage du Swagger  c'est la documentation que les partenaires peuvent exécuter lors de la phase d'exploration de votre API. 
Et nous avons remarqué que nos développeurs sont très à l'aise avec cette approche qui ne ressemble pas du tout à l'élaboration du document Word fade et sans etat d'âme. 
Comme résultat on a une documentation:

1. générée automatiquement lors du build Jenkins à partir du code source✅
2. précise et à jour ✅
3. se déploie automatiquement sur le serveur d'intégration ✅
4. les équipes de dévs des partenaires peuvent faire leurs tests en autonomie ✅

## <a name="error"></a> Géstion des erreurs avec Vnd.Errors

Nous pensons que c'est inutile d'expliquer l'importance de communication des erreurs aux client de votre API. 
Bien sûr, vous pouvez inventer votre _propre structure ou modèle des codes et messages de retour_, 
mais si vous utilisez Spring, nous vous recommandons de jeter un œil sur le projet [VND.errors](https://github.com/blongden/vnd.error) (qui fait partie du project [Spring Hateoas](https://docs.spring.io/spring-hateoas/docs/current/reference/html/)).

{% img /images/consoleVnd.png %}

Maven config
```xml
        <!-- VndError for REST API error handling -->
        <dependency> 
            <groupId>org.springframework.hateoas</groupId>
            <artifactId>spring-hateoas</artifactId>
            <version>0.25.1.RELEASE</version>
        </dependency>
```

Voici quelques règles de base qu'on suit lors de cette phase:
Séparer les erreurs techniques et les erreurs métiers en introduisant    

https://docs.spring.io/spring-hateoas/docs/0.25.3.BUILD-SNAPSHOT/api/org/springframework/hateoas/VndErrors.VndError.html

## <a name="jUnit"></a> jUnit
## <a name="I18n"></a> i18n
## <a name="Packaging"></a> packaging




