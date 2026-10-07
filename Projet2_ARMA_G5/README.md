# Projet 2 — Modélisation temporelle, backtesting et prévision face au marché (Groupe 5)

Suite du Projet 1 sur le **Fidelity Magellan Fund (FMAGX)**. Deux questions :

1. **Volet A** : les résidus du modèle Fama–French du Projet 1 sont-ils un bruit blanc (Ljung–Box à l'ordre 12) ?
2. **Volets B à D** : un modèle ARMA calibré sur les 4 premières années de rendements mensuels du fonds prévoit-il la
   dernière année mieux qu'une prévision naïve (moyenne historique) ?

## Contenu

| Fichier | Description |
|---|---|
| `Projet2_G5.ipynb` | Notebook complet, exécuté, avec les résultats et les commentaires |
| `data/projet1_donnees_et_residus.csv` | Données exportées par le Projet 1 : log-rendements du fonds, facteurs, résidus FF3 et Carhart |
| `figures/` | Figures du rapport, générées par le notebook |
| `resultats/` | Tableaux exportés : prévisions, métriques, grille AIC/BIC |

## Données

- 60 log-rendements mensuels de FMAGX (VL ajustée des distributions, Yahoo Finance), **août 2021 – juillet 2026**,
  et les facteurs de la Kenneth French Data Library : ce sont exactement les données du Projet 1.
- Découpage chronologique : **entraînement** = 48 mois (août 2021 – juillet 2025, 80 %) ;
  **test** = 12 mois (août 2025 – juillet 2026, 20 %), jamais utilisés pour l'estimation.

## Structure du notebook

1. Problème et objectifs (lien avec l'efficience faible des marchés)
2. Données et traçabilité
3. Construction des variables (rendement, log-prix, résidus FF3, découpage 80/20)
4. Prétraitement (valeurs manquantes, calendrier, IQR avec des bornes calculées sur l'entraînement)
5. Analyse exploratoire (statistiques descriptives, Jarque–Bera, graphiques)
6. **Volet A** : Ljung–Box et corrélogramme des résidus FF3, grille ARMA sur les résidus
7. **Volet B** : test ADF (log-prix et rendements), FAC/FACP, grille AIC/BIC (p, q ≤ 3), estimation et diagnostic des résidus
8. **Volet C** : `get_forecast(steps=12)` et intervalles de confiance à 95 %
9. **Volet D** : graphique de confrontation, RMSE/MAE face aux prévisions naïves ; pour aller plus loin : backtest
   glissant à 1 pas, piège du *data snooping*, traduction en valeur liquidative face au marché
10. Bilan critique

## Résultats clés

| Étape | Résultat |
|---|---|
| Volet A | Ljung–Box(12) sur les résidus FF3 : Q = 10,0, **p = 0,61** ⇒ bruit blanc, modèle FF3 validé |
| ADF | log-prix : p = 0,98 (racine unitaire) ; rendements : p < 0,001 (stationnaires) ⇒ d = 0 |
| Choix de (p, q) | **ARMA(0,0)** retenu par le BIC (et par l'AIC) : constante 0,87 %/mois + bruit blanc (σ = 5,5 %/mois) |
| Résidus ARMA | Ljung–Box p = 0,79, Jarque–Bera p = 0,37 ; effets ARCH détectés (p = 0,005, bonus hors programme) |
| Volet C | prévision 0,87 %/mois, IC à 95 % [−10,0 % ; +11,7 %], 11 mois de test sur 12 dans l'intervalle |
| Volet D | RMSE = 4,23 % et MAE = 2,91 %, **identiques à la moyenne historique** ⇒ aucune valeur ajoutée prédictive |

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
