# OBSonix 🔬

**Obstetric Ultrasound Neural Intelligence eXtended**

> Premier modèle de fondation fœtal open source — DINOv2 + iBOT pré-entraîné sur ~84 000 images échographiques obstétricales.

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-green.svg)](https://python.org)
[![PyTorch 2.0+](https://img.shields.io/badge/PyTorch-2.0+-red.svg)](https://pytorch.org)
[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/FetalOdyssey/OBSonix/blob/main/notebooks/OBSonix_foundation_model.ipynb)

---

## 🎯 Objectif

OBSonix est un **Visual Transformer (ViT-B/16)** pré-entraîné de manière auto-supervisée sur un corpus consolidé d'images échographiques fœtales :

| Dataset | Images | Licence |
|---------|--------|---------|
| FETAL_PLANES_DB | ~12 400 | CC BY 4.0 |
| IUGC 2024 (frames) | ~68 106 | CC BY 4.0 |
| PSFHS | 1 358 | CC BY 4.0 |
| HC18 | 1 334 | CC BY 4.0 |
| JNU-IFM | ~1 200 | Open |
| **Total** | **~84 000** | |

**Architecture** : ViT-B/16, objectif DINOv2 (distillation CLS) + iBOT (masked patch prediction)  
**Init** : FetalCLIP ViT-L/14 ou DINOv2-B  
**Résolution** : 224×224 px (pré-entraînement), 384×384 px (fine-tuning)

---

## 🏆 Performances cibles

| Métrique | Seuil | Cible OBSonix |
|----------|-------|---------------|
| Dice HC18 | ≥ 0.93 | > 0.95 |
| HC MAE | ≤ 2.5 mm | < 2.0 mm |
| Taux d'échec | < 5 % | < 3 % |
| Dice PSFHS (FH+PS) | ≥ 0.90 | > 0.92 |

---

## 🚀 Démarrage rapide

### Option 1 — Google Colab (recommandé)

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/FetalOdyssey/OBSonix/blob/main/notebooks/OBSonix_foundation_model.ipynb)

1. Ouvrir le notebook dans Colab (GPU A100/L4 recommandé)
2. Remplir la cellule **CONFIG** (Google Drive + GitHub token)
3. Exécuter dans l'ordre : EXP-18 → EXP-19 → EXP-20 → EXP-21 → EXP-22 → EXP-23 → EXP-24

### Option 2 — Local

```bash
git clone https://github.com/FetalOdyssey/OBSonix.git
cd OBSonix
pip install -r requirements.txt
# Télécharger les données (voir scripts/download_data.sh)
bash scripts/download_data.sh
# Lancer le pré-entraînement
python src/train_pretrain.py --config configs/obsónix_pretrain.yaml
```

---

## 📁 Structure du dépôt

```
OBSonix/
├── notebooks/
│   └── OBSonix_foundation_model.ipynb   # Notebook principal Colab
├── configs/
│   ├── obsónix_pretrain.yaml            # Hyperparamètres pré-entraînement
│   └── obsónix_finetune.yaml            # Hyperparamètres fine-tuning
├── src/
│   ├── data/
│   │   ├── datasets.py                  # FetalDS, HCRegDS
│   │   └── augmentations.py             # OBSAug (speckle, shadow, CLAHE)
│   ├── models/
│   │   ├── obsónix.py                   # Architecture ViT-B/16 + têtes
│   │   └── decoder.py                   # AttentionResUNetDecoder
│   ├── train/
│   │   ├── pretrain.py                  # Boucle DINOv2 + iBOT
│   │   └── finetune.py                  # Fine-tuning segmentation
│   └── eval/
│       ├── metrics.py                   # Dice, HC MAE, MC-Dropout
│       └── benchmark.py                 # Benchmark multi-modèles
├── scripts/
│   ├── download_data.sh                 # Téléchargement 5 datasets
│   └── download_weights.sh              # Téléchargement poids fondation
├── requirements.txt
├── LICENSE                              # Apache 2.0
└── README.md
```

---

## 📊 Datasets

Tous les datasets sont open data et téléchargés automatiquement par EXP-18 :

- **FETAL_PLANES_DB** : [Zenodo 3904280](https://zenodo.org/records/3904280)
- **IUGC 2024** : [Zenodo 16869288](https://zenodo.org/records/16869288)
- **PSFHS** : [Zenodo 10969427](https://zenodo.org/records/10969427)
- **HC18** : [Zenodo 1322001](https://zenodo.org/records/1322001)
- **JNU-IFM** : [Figshare](https://figshare.com/s/5a18a5e54e6c6f62b37a)

---

## 📜 Licence

Code : **Apache 2.0**  
Données : CC BY 4.0 (datasets tiers — voir licences individuelles)  
Poids pré-entraînés OBSonix : **Apache 2.0**

---

## 🔗 Citation

Si vous utilisez OBSonix dans vos travaux :

```bibtex
@software{obsónix2026,
  title   = {OBSonix: Obstetric Ultrasound Neural Intelligence eXtended},
  author  = {FetalOdyssey contributors},
  year    = {2026},
  url     = {https://github.com/FetalOdyssey/OBSonix},
  license = {Apache-2.0}
}
```

---

## 🤝 Contribution

Les contributions sont les bienvenues ! Voir [CONTRIBUTING.md](CONTRIBUTING.md).

---

*OBSonix fait partie du projet [FetalOdyssey](https://github.com/FetalOdyssey).*
