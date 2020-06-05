---
layout: post
title: Analyser les tournées de livraison pour leur optimisation
date: 2020-04-05 13:32:20 +0300
description: Explication à propos de l'analyse des tournées de livraisons. # Add post description (optional)
img: pope-moysuh-ObweQkF5w30-unsplash.jpg # Add image post (optional)
fig-caption: Photo by Pope Moysuh on Unsplash # Add figcaption (optional)
tags: [GIS, Analyse, Dataviz, Mapping]
---
Un client avec un besoin concernant l'optimisation de la distribution de prospectus spécialisés. En effet, les circuits de livraison avaient été réalisés il y a plusieurs années. Les nouveaux prospects se sont peu à peu ajoutés aux livraisons mais sans politique d'optimisation. Au bout de quelques années il était nécessaire d'identifier les tournées potentiellement problématiques.

## Étape 1 : Géocodage des prospects

La première étape fut de géocoder les **260 000** prospects. Le géocodage consiste en la traduction d'une adresse postale en coordonnées XY, nécessaires pour les traitement et la représentation géographique.

Pour cela j'ai utilisé l'API de [adresse.gouv.fr](https://geo.api.gouv.fr/adresse)

## Étape 2 : L'analyse par Hexagones

Pour identifier les tournées potentiellement problématiques j'ai consolidé les données grâce à des hexagones de 300m de diagonale :
1. Grâce à Postgres/PostGIS j'ai généré cette grille de plusieurs millions d'hexagones géographiques. Puis grâce à une jointure spatiale, j'ai compté dans chaque hexagone le nombre de prospects.
2. Puis j'ai identifé les hexagones qui comportaient plusieurs tournés de livraisons différentes.
3. Puis j'ai identifié le nombre de tournées différentes pour les hexagones correspondants.

![analyse des hexagones]({{site.baseurl}}/assets/img/analyse-tournees/methode-hexagones.png)

4. Enfin j'ai calculé le nombre de prospects de la tournée la plus importante et la proportion que cette tournée représente.

![analyse des hexagones 2]({{site.baseurl}}/assets/img/analyse-tournees/methode-hexagones-2.png)

## Le résultat

Cette méthode permet de réduire considérablement le scope d'analyse et d'identifier les cas intéressants. Sur l'hexagone identifié dans l'exemple suivant, la tournée principale (identifiée par les points rouges) représente 95%. Ce qui veut dire qu'un prospect seulement ne fait pas partie de cette tournée (le point bleu). La tournée peut donc être optimisée :

![Étude de cas]({{site.baseurl}}/assets/img/analyse-tournees/etude-de-cas.png)

In fine, cette méthode a permis d'isoler :
* 59 000 prospects à étudier sur les 260 000 de départ
* 4 700 Hexagones sur 48 000
* Mais surtout réduire la surface de territoire à analyser de 32 000 km² à seulement 297 km²
