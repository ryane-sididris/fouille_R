# Projet : Classification Texte IA vs Humain

---

## Objectif du Projet

Ce projet vise à développer un système capable de distinguer des textes rédigés par des humains de ceux générés par une Intelligence Artificielle (IA).

L’hypothèse  repose sur l’existence de différences linguistiques, stylométriques et statistiques entre ces deux types de textes.

---

## Auteurs

- Ryane SID IDRIS
- Yacine OUALIKEN
- Sofiane MOUHOUB

12/02/2026  

---

# Pipeline du Projet

Le projet suit une démarche de Data Science :

1. Chargement et fusion des datasets
2. Analyse exploratoire des données (EDA)
3. Extraction de caractéristiques stylométriques
4. Analyse de lisibilité (Score de Flesch)
5. Vectorisation TF-IDF (unigrammes + bigrammes)
6. Réduction de dimension via AFD
7. Classification avec Naïve Bayes
8. Validation croisée et évaluation avancée

---

# Données

Le dataset d'entraînement est composé de 4 fichiers CSV :

- `train_drcat_01.csv`
- `train_drcat_02.csv`
- `train_drcat_03.csv`
- `train_drcat_04.csv`

Les fichiers sont fusionnés pour former un seul et unique fichier `dataset_reduced_final.csv`.

---

# Analyse Exploratoire (EDA)

Nous avons commencé par vérifier :

- L’équilibre des classes
- La distribution des textes
- Les bigrammes les plus fréquents

L' objectif étant d'éviter tout biais lié à un déséquilibre des données.

---

# Extraction des Caractéristiques Stylométriques

Nous somme ensuite partie de l'hypothèse que les textes IA ont souvent une structure plus régulière et une lisibilité plus homogène et avons extrait plusieurs indicateurs de style :

- `word_count` : Nombre total de mots  
- `sentence_var` : Variabilité de la longueur des phrases  
- `punc_ratio` : Densité de ponctuation  
- `flesch_score` : Score de lisibilité de Flesch  

---

# Vectorisation TF-IDF

Transformation des textes en :

- Unigrammes (1 mot)
- Bigrammes (2 mots)

Avec :

- Suppression de la ponctuation
- Suppression des stopwords
- Pondération TF-IDF
- Filtrage des termes rares

---

# Réduction de Dimension : Analyse Factorielle Discriminante

L’AFD permet de :

- Maximiser la séparation entre les classes
- Réduire des milliers de variables en un score unique *LD1*

Formule optimisée :

```
J(w) = (wᵀ Sb w) / (wᵀ Sw w)
```

On obtient un score discriminant `lda_score` synthétisant toute l’information utile.

---

# Modélisation : Naïve Bayes

Nous utilisons un Classifieur Naïf Bayésien basé sur :

- `lda_score`
- `sentence_var`
- `punc_ratio`
- `flesch_score`

---

## Validation

- Validation croisée stratifiée
- 10-fold Cross-Validation
- Optimisation sur la métrique ROC

---

# Résultats

| Métrique | Score |
|----------|--------|
| Accuracy | ~90% |
| Sensibilité (IA) | 96.5% |
| Spécificité (Humain) | 74% |
| AUC | 0.96 |
| Score de Brier | 0.07 |

---

## Interprétation

- Le modèle détecte presque toutes les IA  
- Certains textes humains complexes peuvent être classés IA  
- L' AUC = 0.96 montre une excellente capacité de discrimination
- Score de Brier faible indicte des prédictions fiables

---

# Pistes d’Amélioration

## Compréhension sémantique BERT

TF-IDF ne prend pas en compte le sens du texte, car il se base uniquement sur la fréquence des mots. 
À l’inverse, des modèles comme les Transformers ou BERT utilisant des embeddings contextuels permettent de capter la cohérence logique, la profondeur argumentative et des patterns sémantiques plus subtils.

---

## Frontières non linéaires Kernel LDA

L’AFD actuelle est linéaire.

Si les textes générés par l’IA et ceux rédigés par des humains deviennent trop similaires, il serait pertinent d’utiliser des modèles plus complexes comme le Kernel LDA, 
des SVM non linéaires ou encore des réseaux neuronaux afin de capturer des frontières de décision plus sophistiquées.

---

# Conclusion

Ce projet montre qu’une combinaison de stylométrie, de mesures de lisibilité, de TF-IDF, de réduction de dimension et d’un modèle probabiliste permet d’obtenir d’excellentes performances. 
L’Analyse Factorielle Discriminante a joué un rôle clé en transformant un problème de haute dimension en un score discriminant unique.

---

# Comment Exécuter le Projet

1. Placer les 4 fichiers CSV dans le même dossier que le `.Rmd`
2. Ouvrir le fichier dans RStudio
3. Installer les packages si nécessaire
4. Exécuter le script complet
5. Le fichier `dataset_reduced_final.csv` sera généré automatiquement
6. Appuyer sur knit dans Rstudio pour générer le site Html contenant le rapport
