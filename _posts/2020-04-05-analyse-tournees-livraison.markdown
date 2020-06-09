---
layout: post
title: Analyser les tournées de livraison pour leur optimisation
date: 2020-04-05 13:32:20 +0300
description: Explication à propos de l'analyse des tournées de livraisons. # Add post description (optional)
img: pope-moysuh-ObweQkF5w30-unsplash.jpg # Add image post (optional)
fig-caption: Photo by Pope Moysuh on Unsplash # Add figcaption (optional)
tags: [GIS, Analyse, Dataviz, Mapping]
---

Un client avait besoin d'identifier les tournées de livraisons de prospectus spécialisés qui pouvait être optimisées.

## Étape 1 : Géocodage des clients

La première étape fut de géocoder (transformer une adresse postale en coordonnées XY) les **260 000** clients. 

Pour cela j'ai utilisé l'API de [adresse.gouv.fr](https://geo.api.gouv.fr/adresse)

## Étape 2 : L'analyse par Hexagones

Pour identifier les tournées potentiellement problématiques j'ai consolidé les données grâce à des hexagones de 300m de diagonale :
1. En **SQL** Grâce à **Postgres/PostGIS** j'ai généré cette grille de plusieurs millions d'hexagones géographiques. Puis grâce à une jointure spatiale, j'ai compté dans chaque hexagone le nombre de clients.
2. Puis j'ai identifé les hexagones qui comportaient plusieurs tournés de livraisons différentes.
3. Puis j'ai identifié le nombre de tournées différentes pour les hexagones correspondants.
![analyse des hexagones]({{site.baseurl}}/assets/img/analyse-tournees/methode-hexagones.png)
4. Enfin j'ai calculé le nombre de clients de la tournée la plus importante et la proportion que cette tournée représente.

![analyse des hexagones 2]({{site.baseurl}}/assets/img/analyse-tournees/methode-hexagones-2.png)

## Le résultat

In fine, cette méthode a permis d'isoler :
* 59 000 clients à étudier sur les 260 000 de départ
* 4 700 Hexagones sur 48 000
* Mais surtout réduire la surface de territoire à analyser de 32 000 km² à seulement 297 km²

Par ailleurs, cela permet de réduire considérablement le scope d'analyse et d'identifier les cas intéressants. Sur l'hexagone identifié dans l'exemple suivant, la tournée principale (identifiée par les points rouges) représente 95%. Ce qui veut dire qu'un client seulement ne fait pas partie de cette tournée (le point bleu). La tournée peut donc être optimisée :

![Étude de cas]({{site.baseurl}}/assets/img/analyse-tournees/etude-de-cas.png)


