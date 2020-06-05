---
layout: post
title: Une application pour trouver le centre de dépistage le plus proche de chez vous
date: 2020-05-30 13:32:20 +0300
description: Explication à propos du site web de dépistage du Covid. # Add post description (optional)
img: national-cancer-institute-NFvdKIhxYlU-unsplash.jpg # Add image post (optional)
fig-caption: Photo by National Cancer Institute on Unsplash # Add figcaption (optional)
tags: [Front, API]
---
Voici un site web pour trouver le centre de dépistage du Covid le plus proche de chez vous : [Le site](https://prrojet-tests-covid.nw.r.appspot.com/)

[![Accéder au site]({{site.baseurl}}/assets/img/homepage-test-covid.png)](https://prrojet-tests-covid.nw.r.appspot.com/)

## Comment cette application a été construite ? 

### Source des données 

Les données sont des données publiques disponibles sur data.gouv.fr. C'est une base au format CSV qui recense les centres de dépsitages avec leur localisation et des informations complémentaires (numéro de téléphone, site internet ...) :

> [https://www.data.gouv.fr/fr/datasets/sites-de-prelevements-pour-les-tests-covid/](https://www.data.gouv.fr/fr/datasets/sites-de-prelevements-pour-les-tests-covid/)

### Enrichissement et stockage des données

Les données sont stockées sur une base **BigQuery (Google Cloud Platform)**. Grâce aux champs de latitude et de longitude, un nouveau champ Geometry a été créé. Celui-ci est indispensable pour retrouver les centres de dépistage les plus proches dans l'application.

### Fonctionnement de l'application

L'application est contruite en Python avec **Flask** et le Framework **Materialize** pour le rendu.

#### Étape 1 : Récupération des coordonnées géographiques

Une fois l'adresse postale envoyée, une requête est envoyée à l'API de [adresse.gouv.fr](https://geo.api.gouv.fr/adresse) pour récupérer les coordonnées XY.

#### Étape 2 : Requêter les centres de dépistage les plus proches

Une fois les coordonnées XY récupérées, La base **BigQuery** est requêtée grâce à la fonction *ST_DISTANCE* pour renvoyer les 10 résultats les plus proches, leurs attributs, ainsi que la distance calculée.

#### Étape 3 : Rendu dans l'application

Une fois les centres de dépistage les plus proches récupérés, Flask créé un JSON et le renvoie sur la page de résultats. La page est alors générée grâce au moteur de rendu de Flask (template Jinja2) et une card est créée pour chaque établissement.

Il est également possible de récupérer les résultats au format JSON. [Voir le fonctionnement de l'API](https://prrojet-tests-covid.nw.r.appspot.com/api)

### Hébergement

L'application Flask est hégergée sur **Google App Engine**


