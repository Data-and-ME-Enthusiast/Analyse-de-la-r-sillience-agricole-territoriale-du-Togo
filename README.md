# Analyse de la résillience agricole territoriale
## 1. Contexte et objectif
L’agriculture togolaise est le premier employeur du pays (plus de 60% de la population active) et un pilier essentiel de l’économie. Pourtant, elle reste très dépendante des aléas climatiques, avec une irrigation limitée et des infrastructures de transformation insuffisantes. Face à l’urgence de renforcer la résilience des territoires, le Togo IA Lab a organisé un défi visant à identifier les zones vulnérables et à prioriser les investissements publics et privés.

L'Objectif principal du challenge est de concevoir un tableau de bord analytique sous Power BI ou python permettant de : 

- Construire un Indice de Résilience Agricole Territoriale (IRAT) .

- Cartographier l’accès aux infrastructures agricoles (eau, élevage, abattoirs, pisciculture).

- Identifier les localités les plus vulnérables.

- Établir un Top 10 des zones prioritaires pour de nouveaux investissements.

Le projet a été mené en trois phases : collecte, préparation et chargement des données (ETL), modélisation des indicateurs et développement du dashboard interactif.

2. Sources de données
Toutes les données proviennent de sources ouvertes et ont été téléchargées en juillet 2026 :

| Thématique | Source | URL |
|------------|--------|-----|
| Établissements d'élevage | geodata.gouv.tg | (lien) |
| Abattoirs | geodata.gouv.tg | (lien) |
| Zones de pisciculture | geodata.gouv.tg | (lien) |
| Retenues d'eau collinaires | geodata.gouv.tg | (lien) |
| Digues et petits barrages | geodata.gouv.tg | (lien) |
| Données agricoles générales | opendata.gouv.tg | (lien) |
| Indicateurs économiques (Banque mondiale) | worldbank.org | (liens) |

Les données étaient hétérogènes (formats CSV, GeoJSON, Excel) et ont nécessité un important travail de nettoyage et de standardisation.

## 3. Préparation et modélisation des données
### 3.1 Nettoyage et structuration
- Excel : consolidation initiale des fichiers, suppression des doublons, uniformisation des codes canton.
- OpenRefine : correction des noms de localités (variantes orthographiques), détection des valeurs aberrantes, et géocodage partiel.
- Power Query (Power BI) : fusion des tables, création d’une table de référence unique des cantons (cantons) par empilement (UNION) de quelques colonnes (ccanton_nom_bdd, region_nom_bdd, prefecture_nom_bdd et commune_nom_bdd) issues de chaque infrastructure, puis suppression des doublons.

Cette table (363 lignes) a été reliée en relation Un-à-Plusieurs avec toutes les tables d’infrastructures via canton_nom_bdd.

### 3.2 Calcul des infrastructures par canton
Quatre mesures DAX comptent le nombre d’infrastructures de chaque type pour un canton donné. Ces mesures sont : Nb_Eau, Nb_Elevage, Nb_Abattoirs et Nb_Pisciculture.

## 4. Construction des indicateurs composites
### 4.1 Normalisation des notes
Pour chaque pilier, une note normalisée entre 0 et 1 est calculée via la méthode Min-Max (sur l’ensemble des cantons). 

## 4.2 Indice de Résilience Agricole Territoriale (IRAT)
L’IRAT est la moyenne pondérée des quatre notes normalisées. Les poids ont été définis selon les caractéristiques de l'agricultures togolaises. La formule de ce d'indice se présente comme suit :  

IRAT = (Normₑₐᵤ × 0,35) + (Normₑₗₑᵥₐgₑ × 0,25) + (Normₐbₐₜₜₒᵢᵣₛ × 0,25) + (Normₚᵢₛcᵢcᵤₗₜᵤᵣₑ × 0,15)

Les poids de référence de chaque pilier utilisés sont présenté dans le tableau aui suit : 

| Pilier | Poids |
|--------|-------|
| Accès à l'eau | 35 % |
| Élevage | 25 % |
| Abattoirs | 25 % |
| Pisciculture | 15 % |

Notons que la sommes des poids doit donner 1 ou 100%. Le tableau de bord affre une possibilité de varier ces poids pour créer des scénario. 

### 4.3 Indice d’Investissement (Score de priorité)
Il combine la vulnérabilité (besoin) et le potentiel économique (opportunité). Vulnérabilité est égal à 1 – IRAT (plus l’IRAT est faible, plus le besoin est grand) et le poids de reéférence attribuer cette dimension est 60%. Potentiel économique est égal à la moyenne des quatre notes normalisées et le poids de référence est 40%. Notons que la sommes des poids doit donner 1 ou 100%. Le tableau de bord affre une possibilité de varier ces poids pour créer des scénario. 
La formule de l’indice d’investissement se présente comme suit : 



## 5. Résultats marquants

- 181 cantons plus vulnérables (IRAT = 0), soit 46 % du territoire.

- Top 10 des cantons les plus résilients (IRAT le plus élevé) : Gadja (0,83), Notsé (0,48), Gamé-Sèva (0,46), Vogan (0,42), Kpédomé (0,38), Bè-Est (0,36), Commune de Kara (0,36), Amoussou Kopé (0,36), Baguida (0,35), Blitta (0,34).

- Top 10 des cantons prioritaires pour l’investissement (score ~0,61) : Agbodrafo, Anfoin, Glidji, Attitogon, Gbodjomé, Tohoun, Ganavé, Hahotoé, Kaboli, Kouma et Togoville.

- Accès aux infrastructures : élevage (93,66 % des cantons), retenues d’eau (53,99 %), digues/barrages (37,74 %), pisciculture (19,83 %), abattoirs (9,37 %).

## 6. Recommandations stratégiques
- Investir massivement dans l’eau (l'agriculture togolaise est pluviale) : multiplier les retenues collinaires et les barrages, en priorité dans les 181 cantons les vulnérables.

- Densifier le réseau d’abattoirs : passer de 9 % à 30 % des cantons couverts, en ciblant les zones à fort potentiel d’élevage.

- Valoriser les retenues existantes pour la pisciculture : 611 retenues pourraient accueillir des fermes piscicoles permettant de générer des revenus complémentaires.

- Concentrer 60 % des investissements sur les 10 cantons prioritaires identifiés, pour un effet de levier maximal.

- Mettre en place un observatoire territorial : suivi trimestriel de l’IRAT, mise à jour des données et révision des priorités


---------------------------------
Sanoussi Moudjibou
  Mail : mjquiet@outlook.fr
     Tel : +228 92281025 
          Analyste statisticien, suivi-évaluateur / Consultant.



