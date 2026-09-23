# Résultats de la campagne analyses3_anatomie4

Généré le 2026-09-23 11:16. Données : `metrics/` (CSV et Parquet), tables publiables : `tables/` (CSV, Markdown, LaTeX), figures : `figures/` (PNG 300 dpi et SVG), journal : `JOURNAL.md`, pré-enregistrement : `S0_preenregistrement.md`, manifeste : `MANIFESTE.csv`.

## Verdict pré-enregistré
Pour l encodeur principal (OBSonix A0 ep300), le signal anatomique SURVIT à la tenue hors des sources
(critère : borne inférieure de l IC à 95 % par grappe de sources > 97,5e centile de permutation, et estimation > ligne de base majoritaire).
Détail par encodeur dans `tables/T6_preregistered_verdict.csv`.
Attention : l encodeur à initialisation aléatoire satisfait lui aussi la règle ; une partie du signal « sources tenues hors » vient donc de statistiques d image de bas niveau, pas des représentations apprises. La comparaison utile est celle entre encodeurs (IC par grappe de sources, larges avec 18 groupes).
Ablation par centrage (T5, F5) : non informative ici, chaque source ne contenant qu une catégorie, le centrage par source retire la catégorie par construction.

## Lecture rapide (exactitude équilibrée, sonde équilibrée)
            Encoder Image-wise balanced accuracy [95% CI, image bootstrap] Source-held-out balanced accuracy [95% CI, source-cluster bootstrap]
       Random init.                                   0.953 [0.943; 0.964]                                                 0.519 [0.405; 0.606]
        ImageNet-1k                                   0.995 [0.992; 0.998]                                                 0.716 [0.578; 0.848]
             DINOv2                                   0.987 [0.981; 0.992]                                                 0.736 [0.648; 0.884]
   OBSonix A0 ep100                                   0.991 [0.986; 0.995]                                                 0.648 [0.515; 0.748]
   OBSonix A0 ep200                                   0.993 [0.988; 0.997]                                                 0.643 [0.537; 0.735]
   OBSonix A0 ep300                                   0.991 [0.986; 0.996]                                                 0.655 [0.519; 0.762]
 OBSonix fetal ep50                                   0.990 [0.985; 0.995]                                                 0.683 [0.546; 0.789]
OBSonix fetal ep100                                   0.990 [0.985; 0.995]                                                 0.635 [0.513; 0.737]

## Paragraphe prêt pour le manuscrit (anglais)
Methods. From the 255,887-image pretraining corpus index, we retained the four anatomical categories present in at least three source datasets with at least 20 images per (category, source) cell (fetal, cardiac, thyroid, breast), sampled at most 300 images per cell (6,256 images from 21 sources), and assigned whole sources (datasets from the same institution or acquisition campaign grouped together: EchoNet, FetalEcho) to 4 folds so that every fold held out at least one source of each category (Table T3). Frozen encoders produced mean-pooled patch-token features (Resize((px,px)) + ToTensor, RGB, px=256 (I-JEPA, IN1K) ou 224 (DINOv2)); a multinomial logistic-regression probe with class-balanced weights was trained on features standardized with training statistics. Two designs were compared on the same images: an image-wise split (70/30 within each source, sources shared between training and test) and a source-held-out design, in which each image is predicted by a probe that never saw its source. The primary outcome was balanced accuracy of pooled out-of-fold predictions, with 95% bootstrap confidence intervals by image and by source cluster (1000 resamples) and a within-fold label-permutation null (500 permutations). As controls, a probe was trained to identify the acquisition source on held-out images of the same sources, overall and within each category, and the source-held-out probe was repeated after subtracting each source's mean feature vector.

Results. For OBSonix A0 ep300, balanced accuracy was 0.991 [0.986; 0.996] under the image-wise split and 0.655 [0.519; 0.762] with sources held out (permutation null 97.5th percentile 0.286; majority baseline 0.155). For DINOv2, the corresponding values were 0.987 and 0.736 [0.648; 0.884]; for a randomly initialized encoder, 0.953 and 0.519 [0.405; 0.606]. Acquisition source was identified among 21 sources on held-out images with accuracy 0.971 for OBSonix A0 ep300 and 0.917 for the random encoder (chance 0.048). Per-category recall, per-fold dispersion, within-category source identification and the centering ablation are given in Tables T2 to T5 and Figures F1 to F5. Because every source in this sample contains a single anatomical category, per-source centering removes the category signal by construction (balanced accuracy 0.25 for all encoders) and cannot separate acquisition signature from anatomy here. A randomly initialized encoder also exceeded the permutation null with sources held out, so part of the source-held-out signal is available from low-level image statistics rather than learned representations.

## Fichiers
- T1_anatomy_probe_main : table principale (les deux pondérations de sonde, lignes de base)
- T2_recall_by_category_source_held_out : rappel par catégorie, sources tenues hors
- T3_fold_design : plan des plis (sources tenues hors et effectifs)
- T4_source_identification_control : identification de la source, globale et à catégorie fixée
- T5_per_source_centering_ablation : effet du centrage par source (si activé ; dégénéré quand chaque source n a qu une catégorie)
- T6_preregistered_verdict : application de la règle de décision
- F1 à F5 : figures correspondantes
