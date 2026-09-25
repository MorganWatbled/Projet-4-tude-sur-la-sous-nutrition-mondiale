# Étude sur la sous-nutrition mondiale — Partie historique (2013-2017)

Analyse réalisée pour l'équipe de recherche de la FAO (Food and Agriculture Organization of the United Nations), dans le cadre d'une étude globale sur l'alimentation et la sous-nutrition dans le monde.

---

## Contexte / besoin métier

La FAO a pour mission d'aider à construire un monde libéré de la faim. Dans ce cadre, l'équipe de recherche, dirigée par Marc (chercheur en économie de la santé), conduit une étude de grande ampleur sur l'alimentation et la sous-nutrition mondiale.

L'étude est découpée en deux volets :
- **2018 à aujourd'hui** : déjà traité par Julien, l'ancien Data Analyst de l'équipe, récemment muté.
- **2013 à 2017 (partie historique)** : périmètre de ce projet, avec un focus sur l'année 2017 pour les indicateurs de sous-nutrition.

L'objectif est de quantifier la sous-nutrition mondiale, d'évaluer dans quelle mesure la production alimentaire actuelle pourrait théoriquement nourrir la population mondiale, et d'identifier les pays les plus en difficulté ainsi que les principaux bénéficiaires de l'aide alimentaire internationale.

## Données (source, qualité, limites)

**Sources :** quatre fichiers CSV fournis par la FAO —
- `population.csv` (1 416 lignes) : population par zone et par année (2013-2018).
- `dispo_alimentaire.csv` (15 605 lignes, 18 colonnes) : disponibilité alimentaire par pays et par produit (Kcal/personne/jour, quantités, production, pertes, importations/exportations, etc.).
- `aide_alimentaire.csv` (1 475 lignes) : aide alimentaire internationale par pays bénéficiaire, année et produit.
- `sous_nutrition.csv` (1 218 lignes) : taux de sous-nutrition par zone, sur des périodes glissantes de 3 ans (ex. 2016-2018).

**Qualité :**
- De nombreuses valeurs manquantes dans `dispo_alimentaire.csv` (ex. seules 2 720 lignes sur 15 605 renseignées pour « Aliments pour animaux »), traitées par remplacement des NaN par 0 — un choix qui suppose une absence réelle plutôt qu'une donnée manquante, à documenter comme hypothèse.
- Plusieurs zones nécessitent une harmonisation de nom entre fichiers (ex. « République populaire démocratique de Corée » → « Corée du Nord », « États-Unis d'Amérique » → « Amérique ») pour que les jointures fonctionnent correctement.
- La colonne `sous_nutrition` contient des valeurs non numériques converties en erreurs (`errors="coerce"`), donc certaines zones se retrouvent sans valeur exploitable après conversion.

**Limites (bugs identifiés dans les calculs actuels, à corriger avant diffusion) :**
- Le calcul de la population « nourrissable » (section 3.2) aboutit à une **population non nourrie négative** (-1 046 454 340), ce qui est incohérent : la moyenne des Kcal par pays est calculée sans pondération par la population, ce qui biaise le résultat.
- Les tableaux « pays avec le moins de disponibilité alimentaire » et « pays avec le plus de disponibilité alimentaire » (sections 3.9 et 3.10) affichent exactement le même résultat (Autriche, Belgique, Turquie…) : le tri semble avoir été fait deux fois dans le même sens, une des deux analyses reste donc à refaire.
- Le calcul du pourcentage de sous-nutrition par pays (section 3.6) produit des valeurs aberrantes (jusqu'à plusieurs dizaines de millions de %), signe d'une erreur d'unité (mélange de valeurs déjà multipliées par 1 000 000 avec une nouvelle multiplication) — le classement des pays reste indicatif mais les pourcentages affichés ne sont pas fiables en l'état.
- La dernière cellule sur le manioc en Thaïlande se termine sur une `TypeError` non résolue (appel de méthode `.sum` mal formé), la section reste donc incomplète.

## Démarche (choix, outils, étapes)

1. Importation des quatre fichiers CSV et exploration initiale (dimensions, types, valeurs manquantes) pour chacun.
2. Nettoyage et harmonisation : renommage de colonnes (`Valeur` → `Population` / `sous_nutrition`), remplacement des NaN par 0 dans `dispo_alimentaire`, conversion des unités (population multipliée par 1000, aide alimentaire multipliée par 100 pour passer en kg).
3. Harmonisation des noms de zones entre les quatre fichiers avant toute jointure.
4. **Proportion de personnes en sous-nutrition** : jointure population/sous-nutrition sur l'année 2017, calcul du ratio global.
5. **Population théoriquement nourrissable** : estimation à partir de la disponibilité calorique moyenne mondiale rapportée à un besoin de 2 500 Kcal/jour/personne, avec une variante limitée aux produits d'origine végétale.
6. **Répartition des usages de la disponibilité intérieure** : ventilation entre alimentation animale, alimentation humaine directe, pertes, transformation, semences et autres usages.
7. **Focus céréales** : part des céréales destinée à l'alimentation animale plutôt qu'humaine.
8. **Classements par pays** : pays les plus touchés par la sous-nutrition, pays les plus/moins pourvus en disponibilité alimentaire par habitant, pays les plus bénéficiaires de l'aide alimentaire (total et évolution 2013-2016).
9. Étude de cas ciblée sur la Thaïlande et le manioc (production vs exportation).

**Outil :** Python (Jupyter Notebook, pandas, matplotlib, numpy).

## Résultats + impact / recommandations

- **Sous-nutrition mondiale (2017)** : environ **7,10 %** de la population mondiale est en sous-nutrition.
- **Population théoriquement nourrissable par la seule production végétale** : environ **91,7 %** de la population mondiale, un résultat cohérent avec le message classique de ce type d'étude (la production suffit largement, la sous-nutrition relève davantage de la répartition que de la disponibilité globale) — voir toutefois la limite ci-dessus sur le calcul incluant les produits animaux, à corriger.
- **Répartition des usages de la disponibilité intérieure mondiale** : Production/stockage (48,9 %), Nourriture directe (24,3 %), Transformation industrielle (11,1 %), Alimentation animale (6,2 %), Autres usages (3,8 %), Pertes (2,4 %), Semences (0,8 %).
- **Céréales** : en moyenne **17,5 %** de la disponibilité en céréales est orientée vers l'alimentation animale plutôt qu'humaine — un axe de réflexion pertinent sur l'usage des ressources céréalières.
- **Pays les plus touchés par la sous-nutrition (classement, hors fiabilité du % affiché)** : Haïti, Madagascar, Tchad, Libéria, Rwanda, Mozambique, Lesotho, Timor-Leste, Venezuela, Afghanistan.
- **Principaux bénéficiaires de l'aide alimentaire internationale (cumul 2013-2016)** : Syrie (1 858 943 t), Éthiopie (1 381 294 t), Yémen (1 206 484 t), Soudan du Sud (695 248 t), Soudan (669 784 t) — l'évolution annuelle de ces cinq pays montre une forte hausse de l'aide au Yémen en fin de période, contrastant avec une baisse pour le Soudan et le Soudan du Sud.
- **Pays à la disponibilité alimentaire par habitant la plus élevée** : Autriche (3 770 Kcal/pers/jour), Belgique (3 737), Turquie (3 708), Amérique (3 682) — loin au-dessus des besoins théoriques de 2 500 Kcal/jour.

**Impact attendu :** fournir à l'équipe de recherche de la FAO une base d'analyse chiffrée sur la période historique, complémentaire du volet 2018-aujourd'hui, mettant en évidence que la sous-nutrition relève principalement d'un problème de répartition et d'accès plutôt que de disponibilité globale de la production alimentaire.

## Limites + prochaines pistes

- Plusieurs calculs identifiés ci-dessus (population non nourrie négative, doublon des classements haute/basse disponibilité, pourcentages de sous-nutrition aberrants, cellule Thaïlande en erreur) doivent être corrigés avant toute diffusion des résultats en présentation.
- Le remplacement systématique des NaN par 0 dans `dispo_alimentaire` mérite d'être requestionné produit par produit : une donnée manquante n'est pas toujours une donnée nulle.
- Analyse limitée à la période 2013-2017 ; une mise en perspective avec le volet 2018-aujourd'hui (déjà traité par Julien) permettrait une vision continue dans le temps.
- Le focus Thaïlande/manioc, une fois l'erreur corrigée, pourrait être étendu à d'autres pays/produits stratégiques pour enrichir l'étude.

---

*Projet réalisé dans le cadre de la mission Data Analyst au sein de l'équipe de recherche de la FAO.*
