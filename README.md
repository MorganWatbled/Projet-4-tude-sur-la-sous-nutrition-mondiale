# BottleNeck — Rapprochement et analyse des ventes de vin

Projet mené pour BottleNeck, marchand de vin prestigieux, visant à fiabiliser les données produits/ventes/stocks et à en tirer des analyses pour le comité de direction (CODIR).

---

## Contexte / besoin métier

BottleNeck utilise aujourd'hui des outils artisanaux et plusieurs exports non reliés entre eux pour piloter son activité, ce qui rend l'analyse des données et la gestion des stocks complexes. Nicolas, responsable vente, a confié une mission en deux phases :

- **Phase 1** : agréger les différents fichiers pour rendre les données exploitables.
- **Phase 2** : analyser ces données pour une présentation au CODIR (chiffre d'affaires, tops références, valeurs aberrantes, marges, rotation des stocks, corrélations).

Le rapport produit servira également de point de départ à un futur projet de data visualisation : au-delà des chiffres, Nicolas attend un récapitulatif des erreurs rencontrées et des corrections à mettre en place pour disposer d'une base de données propre, avec une formalisation du processus de travail et une justification de la conformité RGPD.

## Données (source, qualité, limites)

**Sources :**
- Extraction de l'**ERP** : référence produit, prix, état du stock.
- Extraction du **site web** (Wordpress) : SKU, quantités vendues, description des produits.
- **Table de liaison** entre les références ERP et Wordpress, mise à jour par le stagiaire avec les nouveaux produits.
- Périmètre temporel : extraction au 31 octobre, ventes couvrant la période du 1ᵉʳ au 31 octobre.

**Qualité :**
- Les numéros de référence ne correspondent pas nativement entre l'ERP et le site web : le rapprochement dépend entièrement de la fiabilité de la table de liaison.
- Au moins 8 erreurs sont attendues dans les données (erreurs de saisie, de type, de calcul, de jointure). [À compléter : liste précise des erreurs identifiées après exploration]

**Limites :**
- Données limitées au mois d'octobre : pas de recul temporel pour analyser une tendance ou une saisonnalité.
- La table de liaison ayant été mise à jour manuellement par un stagiaire, elle est une source potentielle d'erreurs de jointure supplémentaires à surveiller en priorité.
[À compléter : autres limites constatées à l'exploration, ex. produits présents dans un système mais absents de l'autre]

## Démarche (choix, outils, étapes)

**Phase 1 — Agrégation et fiabilisation des données**
1. Rapprochement de l'extraction du site web avec la base ERP via la table de liaison.
2. Identification systématique des erreurs dans les données (saisie, type, calcul, jointure) — au moins 8 attendues.
3. Formulation de propositions de solutions pour améliorer la qualité des données dans les systèmes sources.
4. Formalisation du processus de nettoyage mis en œuvre, avec justification des choix au regard de la conformité RGPD.

**Phase 2 — Analyses pour le CODIR**
1. Calcul du chiffre d'affaires par produit et du chiffre d'affaires total.
2. Analyse des tops références et de la répartition selon la loi du 20/80.
3. Détection des valeurs aberrantes de saisie via Z-Score ou écart interquartile.
4. Extraction et visualisation (boxplot) des valeurs aberrantes de prix, avec conclusion sur d'éventuelles erreurs de prix.
5. Analyse de l'état des stocks, des taux de marge, de la rotation des stocks et du nombre de mois de stock.
6. Étude des corrélations entre variables quantitatives (prix, prix d'achat, stock, ventes, prix HT, taux de marge).
7. Analyses complémentaires jugées pertinentes, au-delà de la liste initiale de Nicolas.

**Outil :** notebook [À compléter : Python ou R], en repartant du notebook déjà entamé par Nicolas.

## Résultats + impact / recommandations

- Un jeu de données rapproché et fiabilisé entre ERP et site web, avec la liste documentée des erreurs corrigées.
- [À compléter : chiffre d'affaires total et par produit, principaux constats sur les tops références et le 20/80]
- [À compléter : conclusions sur les valeurs aberrantes de prix et de saisie identifiées]
- [À compléter : constats sur la rotation des stocks, les taux de marge et les corrélations entre variables quantitatives]
- **Impact attendu :** une base de données propre et documentée, présentée au CODIR, servant de socle fiable au futur projet de data visualisation de l'entreprise.

## Limites + prochaines pistes

- L'analyse porte sur un instantané d'un seul mois (octobre) : une consolidation sur plusieurs mois serait nécessaire pour des conclusions sur les tendances de vente ou de marge.
- Les corrections proposées en phase 1 concernent les données déjà extraites ; une action corrective sur les systèmes sources eux-mêmes (ERP, Wordpress) resterait à mettre en œuvre pour éviter la récurrence des erreurs.
[À compléter : pistes complémentaires identifiées pendant l'analyse, notamment en vue du futur projet de data visualisation]

---

*Projet réalisé dans le cadre de la mission Analyste chez BottleNeck, sous la responsabilité de Nicolas, responsable vente.*
