# Pre-entrainement LeJEPA ViT-B/16 (in-domain) - EXP19B_LEJEPA_PRETRAIN

## Contexte
Aucun checkpoint ViT-B/16 LeJEPA officiel n'est publie : le depot officiel
(rbalestr-lab/lejepa) fournit le code, pas de poids pre-entraines pour cette
architecture. Pre-entrainement auto-supervise *in-domain* sur le corpus
echographique foetal, methode LeJEPA (Balestriero & LeCun, 2025, arXiv:2511.08544).

## Methode
- Backbone : ViT-B/16 (timm vit_base_patch16_224, 768-dim).
- Objectif LeJEPA = (1 - lambda) * invariance inter-vues + lambda * SIGReg.
- Hyperparametre unique lambda = 0.05 ; sans teacher-student / stop-gradient.
- Vues / image : 2. Resolution : 224 px. AdamW (lr=0.002, wd=5e-2), bf16.
- Pipeline : staging local /content (decode .mha unique + pre-resize 256 px)
  pour s'affranchir de l'I/O Google Drive a chaque epoch.

## Corpus (24430 images)
FETAL_PLANES_DB, IUGC2024 (frames), HC18, PSFHS (image_mha), JNU_IFM ;
masques exclus (mask/, mask_enhance/, *_Annotation, label_mha).

## Resultats
- Epochs : 100 ; perte LeJEPA finale : 0.0811
- Artefact : weights/LeJEPA/lejepa_vitb16.pth (backbone), init de EXP-14 (DAPT).

## Reference
Balestriero R., LeCun Y. LeJEPA: Provable and Scalable Self-Supervised Learning
Without the Heuristics. arXiv:2511.08544 (2025).
