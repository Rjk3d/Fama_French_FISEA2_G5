# Projet 1 — Audit quantitatif et analyse de style d'un fonds

Nous cherchons à savoir si les performances du Fidelity Magellan Fund sont dues aux choix de son gérant ou si elles s’expliquent plutôt par son exposition aux facteurs de Fama-French.

$$R_{it} - R_{ft} = \alpha_i + \beta_{i1}(R_{mt}-R_{ft}) + \beta_{i2}SMB_t + \beta_{i3}HML_t + \varepsilon_{it}$$

## Contenu de notre projet

| Fichier | Description |
|---|---|
| `Projet1_G5.ipynb` | Notebook complet, exécuté, avec les résultats et les commentaires |
| `Projet1_G5_brouillon.ipynb` | Ancien brouillon, conservé pour mémoire (à ne pas mettre dans le .zip rendu) |
| `figures/` | Figures du rapport, générées par le notebook |
| `data/FMAGX_quotidien.csv` | VL quotidiennes brutes et ajustées (Yahoo Finance) |
| `data/FF_Research_Data_Factors_mensuel.csv` | Mkt-RF, SMB, HML, RF (Kenneth French Data Library) |
| `data/FF_Momentum_Factor_mensuel.csv` | Facteur momentum (Kenneth French Data Library) |
| `data/FRED_DTB3_quotidien.csv` | Taux des T-bills à 3 mois (FRED), utilisé pour la robustesse |
| `data/projet1_donnees_et_residus.csv` | Variables du modèle et résidus FF3 / Carhart, **à utiliser pour le Projet 2** |

## Données

- Période : août 2021 – juillet 2026, soit 60 rendements mensuels. C'est le dernier mois publié par K. French au 25/09/2026.
- Rendements : log-rendements calculé à partir de la VL ajustée des distributions, prise en fin de mois.
- Excès de rendement : rendement du fonds moins RF.

## Structure de notre notebook

Notre notebook est composé de 12 étapes

1.Présenter notre problématique et les objectifs de l’étude.

2.Récupérer les données nécessaires et vérifier leur origine.

3.Préparer les variables utilisées dans notre analyse.

4.Nettoyer les données en vérifiant les valeurs manquantes, les valeurs aberrantes et les différences entre cours ajustés et bruts.

5.Étudier les données à l’aide de statistiques descriptives, d’histogrammes, de tests de normalité et de corrélations.

6.Estimer le modèle Fama-French à trois facteurs (FF3) et comparer les résultats avec le CAPM.

7.Vérifier la fiabilité du modèle grâce aux différents tests statistiques : multicolinéarité, hétéroscédasticité, normalité des résidus, autocorrélation, points influents, forme fonctionnelle et stabilité des coefficients.

8.Tester le modèle sur les 12 derniers mois, après une estimation sur les 48 premiers, puis suivre l’évolution des coefficients sur des fenêtres de 36 mois.

9.Ajouter le facteur momentum avec le modèle de Carhart, comparer les modèles et réaliser une sélection de variables.

10.Vérifier si nos conclusions changent en utilisant un autre taux sans risque et des rendements simples.

11.Présenter les principales conclusions de notre analyse ainsi que ses limites.

12.Exporter les résidus nécessaires à la réalisation du Projet 2.

## Résultats clés (FF3)

| Coefficient | Estimation | p-value | Lecture |
|---|---|---|---|
| α | −0,23 %/mois (≈ −2,8 %/an) | 0,22 | pas de talent démontré |
| β marché | 1,06 | < 0,001 | risque ≈ celui du marché |
| β SMB | −0,10 | 0,15 | biais grandes capitalisations (non significatif) |
| β HML | −0,25 | < 0,001 | biais *growth* marqué |
| R² | 0,94 | | |

Le test de Breusch–Pagan ne détecte pas d’hétéroscédasticité (p = 0,55) et les VIF restent tous inférieurs à 1,2.

Les tests de Breusch–Godfrey, RESET et Chow ne révèlent pas de problème majeur d’autocorrélation, de spécification ou de stabilité.

Le momentum est significatif dans le modèle de Carhart (p = 0,02), mais l’amélioration du BIC reste faible (1,9 point). Nous conservons donc le FF3 comme modèle de référence.

## Exécution

Dépendances :

```bash
pip install yfinance pandas-datareader statsmodels scipy matplotlib pandas numpy jupyter
```

Pour relancer le notebook :

```bash
jupyter nbconvert --to notebook --execute --inplace Projet1_G5.ipynb
```
Nous avons regroupé les paramètres de l’analyse dans la deuxième cellule de code. Pour retrouver les mêmes résultats que dans notre rapport, nous utilisons les données enregistrées dans data/, téléchargées le 25/09/2026. Si nous désactivons "USE_LOCAL_DATA", le notebook télécharge à nouveau les données depuis Yahoo, K. French et la FRED, ce qui peut entraîner de légères différences si les séries ont été mises à jour.
