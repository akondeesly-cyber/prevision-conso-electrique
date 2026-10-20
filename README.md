# Prévoir la consommation électrique de demain : de la donnée à la décision

**Projet final — S7MOI1, Science des Données, concepts avancés — ECAM-EPMI, 2AE**  
**Équipe :** AKONDE Esly · DELBARD-MAZZOLA Paul-Baptiste · DIOP Abdou Aziz

## Objectif
À partir du jeu UCI *Individual Household Electric Power Consumption*, le projet traite deux problèmes : (1) prévoir la consommation journalière du lendemain (supervisé) et (2) regrouper les journées selon leurs profils horaires (non supervisé). Le notebook 03 examine ensuite des scénarios économiques et les limites de leur interprétation.

## Résultats observés dans les notebooks
Sur le jeu de test chronologique de 2010, la MAE indiquée pour la baseline « comme hier » est de 5,10 kWh/jour et celle de la Random Forest de 4,22 kWh/jour, soit une baisse de 17,2 % par rapport à cette baseline. Le notebook 02 compare aussi la baseline saisonnière : le critère de réussite doit être évalué par rapport à la meilleure baseline, comme le fait le code. K-Means produit deux groupes, avec un score silhouette rapporté de 0,203, ce qui indique une séparation limitée. Les résultats du ROI dépendent d'hypothèses explicites et ne prouvent pas la rentabilité sur un parc réel, car le jeu ne couvre qu'un foyer.

## Ouvrir dans Google Colab
Exécuter dans l'ordre **01 → 02 → 03** :

- [Notebook 01 — cadrage et EDA](https://colab.research.google.com/github/akondeesly-cyber/prevision-conso-electrique/blob/main/notebooks/01_cadrage_eda.ipynb)
- [Notebook 02 — modélisation](https://colab.research.google.com/github/akondeesly-cyber/prevision-conso-electrique/blob/main/notebooks/02_modelisation.ipynb)
- [Notebook 03 — décision et ROI](https://colab.research.google.com/github/akondeesly-cyber/prevision-conso-electrique/blob/main/notebooks/03_decision_roi.ipynb)

Les notebooks 02 et 03 cherchent d'abord les CSV dans `data/`, puis téléchargent les fichiers manquants depuis le dossier `data/` de ce dépôt GitHub. **Les CSV doivent donc être publiés dans GitHub avant de lancer ces notebooks.** Dans Colab, ouvrez chaque notebook depuis le lien ci-dessus et faites *Exécution → Tout exécuter*. Si une erreur 404 apparaît, vérifiez que le dépôt est public et que les fichiers existent aux chemins indiqués.

## Structure du dépôt

```text
prevision-conso-electrique/
├── README.md
├── requirements.txt
├── data/
│   ├── features_journalieres.csv
│   ├── table_journaliere.csv
│   ├── profils_horaires.csv
│   ├── predictions_test.csv
│   ├── clusters_journaliers.csv
│   ├── resultats_roi.csv
│   └── sensibilite_roi.csv
└── notebooks/
    ├── 01_cadrage_eda.ipynb
    ├── 02_modelisation.ipynb
    └── 03_decision_roi.ipynb
```

Les trois premiers CSV sont produits par le notebook 01, `predictions_test.csv` et `clusters_journaliers.csv` par le notebook 02, puis les deux fichiers ROI par le notebook 03.

## Exécution locale
Python 3.10 ou plus récent. Depuis la racine du dépôt :

```bash
python -m venv .venv
# macOS/Linux : source .venv/bin/activate
# Windows PowerShell : .venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
jupyter notebook
```

Exécutez les notebooks dans l'ordre. Les chemins locaux sont relatifs à la racine du dépôt.

## Données et méthode
Source : [UCI Machine Learning Repository — Individual Household Electric Power Consumption (ID 235)](https://archive.ics.uci.edu/dataset/235/individual+household+electric+power+consumption). Les données sont des mesures à la minute d'un seul foyer de décembre 2006 à novembre 2010. `Global_active_power` est une puissance en kW ; l'énergie journalière est estimée à partir de la puissance moyenne journalière multipliée par 24 heures, sous réserve du seuil de couverture défini dans le notebook 01.

La prévision repose sur une séparation chronologique, des baselines, une régression linéaire, un arbre et une Random Forest réglée. La segmentation utilise les profils horaires, la standardisation, la PCA, K-Means et DBSCAN. Le notebook 03 applique une fonction de coût et des scénarios de ROI.

## Limites
Les données portent sur un seul foyer et ne comportent pas de météo ni d'informations de présence. La généralisation à un parc de foyers et le foisonnement des erreurs doivent être évalués par un pilote multi-foyers. Les résultats économiques sont des scénarios, pas une garantie de rentabilité.

## Usage de l'IA générative
Claude (Anthropic) a été utilisé pour structurer la démarche, expliquer des notions, proposer une première version de code et aider à formuler des interprétations à partir des résultats exécutés. L'équipe doit être en mesure d'expliquer le code, les choix méthodologiques, les résultats et les limites.
