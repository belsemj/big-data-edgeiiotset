# Big Data – Détection d'intrusion IoT/IIoT (Edge-IIoTset)

Projet du module **Big Data** – 3ᵉ année Télécom, **ENIT**.
**Semaine 2 (W2) :** préparation des données et classification supervisée sur un seul poste (local), avant le passage à l'échelle distribuée (Spark) en W3.

---

## Objectif

Entraîner un système de **détection d'intrusion** (IDS) capable de reconnaître
automatiquement le **type de trafic** d'un réseau IoT/IIoT à partir des
caractéristiques des flux réseau — du trafic normal ou l'une de 14 attaques.

## Dataset

**Edge-IIoTset** (Ferrag et al., *IEEE Access*, 2022).
Généré sur une plateforme de test à 7 couches avec de vrais capteurs IoT/IIoT, puis
injection de 14 attaques (DDoS, injection, MITM, malware, reconnaissance).

- Slug Kaggle : `mohamedamineferrag/edgeiiotset-cyber-security-dataset-of-iot-iiot`
- Fichier utilisé : `ML-EdgeIIoT-dataset.csv` (version sélectionnée pour le ML traditionnel, ~157 800 lignes)
- **Type de tâche :** classification supervisée **multi-classes** (15 classes)
- **Cible :** `Attack_type` (Normal + 14 attaques) — classes **déséquilibrées**

> Le dataset n'est **pas inclus** dans ce dépôt (volumineux, public). Voir « Installation » ci-dessous.

## Pipeline (notebook W2)

1. **Chargement** du dataset depuis Kaggle.
2. **Exploration & nettoyage (EDA)** : types, valeurs manquantes, doublons, équilibre
   des classes, suppression des colonnes identifiants / à fuite, encodage des catégorielles.
   → 152 196 lignes × 74 features après préparation.
3. **Entraînement** : découpage 80/20 stratifié, normalisation, puis 3 modèles comparés
   (Régression logistique, Arbre de décision, Random Forest).
4. **Analyse de performance** : accuracy, precision, recall, F1, rapport par classe,
   matrice de confusion.

## Résultats (test, F1 pondéré)

| Modèle | Accuracy | F1 |
|---|---|---|
| Régression logistique | 0,77 | 0,77 |
| Arbre de décision | 0,93 | 0,93 |
| **Random Forest** | **0,93** | **0,93** |

**Meilleur modèle : Random Forest.** La régression logistique sous-apprend (données non
linéaires) ; l'arbre de décision surapprend (0,999 en train vs 0,93 en test) ; le Random
Forest offre le meilleur compromis. Les attaques applicatives (Password, SQL injection,
Uploading, XSS) et Fingerprinting restent les plus difficiles à distinguer.

## Installation

```bash
pip install jupyter pandas scikit-learn matplotlib seaborn
```

Télécharger `ML-EdgeIIoT-dataset.csv` depuis Kaggle et le placer dans le dossier du notebook.

## Lancer

```bash
python -m notebook
```

Ouvrir `W2_EdgeIIoTset.ipynb`, puis **Kernel → Restart Kernel and Run All Cells**.
Astuce : mettre `SAMPLE_FRAC = 0.2` en haut du notebook pour une exécution plus rapide.

## Structure du dépôt

```
.
├── W2_EdgeIIoTset.ipynb   # notebook de la W2
├── README.md
└── .gitignore
```

## Étapes suivantes

- **W3 :** adapter le pipeline à **Apache Spark** (traitement distribué sur l'ensemble des données).
- **W5–W6 :** stockage des résultats sur **Elasticsearch**.
- **W7 :** visualisation via un **dashboard Kibana**.

## Auteur

Belsem — ENIT, Télécommunications.
