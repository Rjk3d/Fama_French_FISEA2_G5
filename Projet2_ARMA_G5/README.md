# Projet 2 — Modélisation temporelle, backtesting et prévision face au marché (Groupe 5)

Ce projet fait suite au Projet 1 sur le fonds Fidelity Magellan (FMAGX). On cherche à répondre à deux questions principales :

1. Volet A : les résidus du modèle Fama–French issus du Projet 1 se comportent-ils comme un bruit blanc (test de Ljung–Box à l'ordre 12) ?
2. Volets B à D : un modèle ARMA ajusté sur les 4 premières années de rendements mensuels parvient-il à battre une prévision naïve (moyenne historique) sur la dernière année ?

## Contenu du répertoire

| Fichier | Description |
|---|---|
| `Projet2_G5.ipynb` | Notebook complet exécuté avec l'ensemble des sorties et commentaires |
| `data/projet1_donnees_et_residus.csv` | Données exportées du Projet 1 : log-rendements, facteurs de risque et résidus FF3 |
| `figures/` | Graphiques générés lors de l'exécution du notebook |
| `resultats/` | Tableaux récapitulatifs exportés (prévisions, métriques, grille AIC/BIC) |

## Données utilisées

* 60 log-rendements mensuels du fonds FMAGX (cours ajustés Yahoo Finance) et facteurs de la Kenneth French Data Library couvrant la période d'août 2021 à juillet 2026.
* Découpage temporel : 48 mois d'entraînement (août 2021 à juillet 2025, soit 80 %) et 12 mois de test (août 2025 à juillet 2026, 20 %) réservé pour l'évaluation finale.

## Structure du notebook

1. Problématique et lien avec l'efficience informationnelle
2. Données et traçabilité des séries
3. Définition des variables (rendements, log-prix, résidus et split 80/20)
4. Traitement préalable (valeurs manquantes, calendrier et bornes IQR sur le train)
5. Analyse descriptive (statistiques, test de Jarque–Bera et visualisations)
6. Volet A : diagnostic des résidus FF3 (Ljung–Box et grille ARMA)
7. Volet B : stationnarité ADF, corrélogrammes et sélection AIC/BIC
8. Volet C : prévision à 12 pas et intervalles de confiance
9. Volet D : backtesting, comparaison aux benchmarks naïfs, data snooping et analyse en valeur liquidative
10. Synthèse critique des limites de la démarche

## Principaux résultats

| Étape | Résultat obtenu |
|---|---|
| Volet A | Ljung-Box(12) sur résidus FF3 : p = 0,61 (hypothèse de bruit blanc validé) |
| Test ADF | Log-cours non stationnaires (p = 0,98) ; rendements stationnaires (p < 0,001), d = 0 |
| Sélection ARMA | ARMA(0, 0) retenu par AIC et BIC : simple constante (0,87 %/mois) sans dynamique |
| Résidus du modèle | Bruit blanc confirmé par Ljung-Box (p = 0,79) mais présence d'hétéroscédasticité ARCH |
| Volet C | Prévision constante à 0,87 %/mois avec un intervalle à 95 % très étalé ([-10,0 % ; +11,7 %]) |
| Volet D | Erreurs RMSE et MAE strictement équivalentes à la moyenne empirique (gain nul) |

## Exécution

Dépendances :

```bash
pip install statsmodels scipy matplotlib pandas numpy jupyter
```

Pour relancer le notebook :

```bash
jupyter nbconvert --to notebook --execute --inplace Projet2_G5.ipynb
```

Le notebook ne télécharge rien et n'utilise aucun tirage aléatoire : il redonne exactement les mêmes résultats à partir
du seul contenu du répertoire. Les paramètres (découpage, grille, seuils) se trouvent dans la 2ᵉ cellule de code.
