# Projet 1 — Audit quantitatif et analyse de style d'un fonds (Groupe 5)

Le talent du gérant du **Fidelity Magellan Fund (FMAGX)** est-il réel (alpha de Jensen), ou la performance du fonds
s'explique-t-elle par son exposition aux facteurs de Fama–French ?

$$R_{it} - R_{ft} = \alpha_i + \beta_{i1}(R_{mt}-R_{ft}) + \beta_{i2}SMB_t + \beta_{i3}HML_t + \varepsilon_{it}$$

## Contenu

| Fichier | Description |
|---|---|
| `Projet1_G5.ipynb` | Notebook complet, exécuté, avec les résultats et les commentaires |
| `Projet1_G5_brouillon.ipynb` | Ancien brouillon, conservé pour mémoire |
| `data/FMAGX_quotidien.csv` | VL quotidiennes brutes et ajustées (Yahoo Finance) |
| `data/FF_Research_Data_Factors_mensuel.csv` | Mkt-RF, SMB, HML, RF (Kenneth French Data Library) |
| `data/FF_Momentum_Factor_mensuel.csv` | Facteur momentum (Kenneth French Data Library) |
| `data/FRED_DTB3_quotidien.csv` | Taux des T-bills à 3 mois (FRED), utilisé pour la robustesse |
| `data/projet1_donnees_et_residus.csv` | Variables du modèle et résidus FF3 / Carhart, **à utiliser pour le Projet 2** |

## Données

- Période : **août 2021 – juillet 2026**, soit 60 rendements mensuels. C'est le dernier mois publié par K. French au 25/09/2026.
- Rendements : log-rendements calculés à partir de la VL **ajustée des distributions**, prise en fin de mois.
- Excès de rendement : rendement du fonds moins RF (K. French).

## Structure du notebook

1. Problème et objectifs
2. Collecte des données, avec leur traçabilité
3. Construction des variables
4. Prétraitement : valeurs manquantes, valeurs aberrantes (IQR), cours ajusté vs cours brut
5. Analyse exploratoire : statistiques descriptives, Jarque–Bera, histogrammes, corrélations
6. Estimation MCO du modèle FF3 (comparé au CAPM)
7. Diagnostic : VIF, Breusch–Pagan/White + HC1, normalité, autocorrélation, distance de Cook
8. Validation hors échantillon (48/12 mois) et régressions glissantes sur 36 mois
9. Extension Carhart (4 facteurs) : R² ajusté, AIC, BIC, test F
10. Robustesse : taux sans risque FRED, rendements simples
11. Bilan critique
12. Export des résidus pour le Projet 2

## Résultats clés (FF3)

| Coefficient | Estimation | p-value | Lecture |
|---|---|---|---|
| α | −0,23 %/mois (≈ −2,8 %/an) | 0,22 | pas de talent démontré |
| β marché | 1,06 | < 0,001 | risque ≈ celui du marché |
| β SMB | −0,10 | 0,15 | biais grandes capitalisations (non significatif) |
| β HML | −0,25 | < 0,001 | biais *growth* marqué |
| R² | 0,94 | | |

- Breusch–Pagan : pas d'hétéroscédasticité détectée (p = 0,55).
- VIF : tous inférieurs à 1,2.
- Momentum (Carhart) : significatif (p = 0,02), mais ne réduit le BIC que de 1,9, ce qui est une preuve faible. Le FF3 reste le modèle de référence.

## Exécution

Dépendances :

```bash
pip install yfinance pandas-datareader statsmodels scipy matplotlib pandas numpy jupyter
```

Pour relancer le notebook :

```bash
jupyter nbconvert --to notebook --execute --inplace Projet1_G5.ipynb
```

Tous les paramètres (fonds, période, seuil) se trouvent dans la 2ᵉ cellule de code. Si vous relancez le notebook plus
tard, Yahoo ou K. French peuvent avoir révisé légèrement leurs données : les chiffres cités dans les textes
d'interprétation peuvent alors différer un peu des résultats affichés.
