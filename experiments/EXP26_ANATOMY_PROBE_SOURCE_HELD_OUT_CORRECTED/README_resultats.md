# Résultats de la campagne analyses3_anatomie4b

Généré le 2026-09-24 11:35. Données : `metrics/` (CSV et Parquet), tables publiables : `tables/` (CSV, Markdown, LaTeX), figures : `figures/` (PNG 300 dpi et SVG), journal : `JOURNAL.md`, pré-enregistrement : `S0_preenregistrement.md`, manifeste : `MANIFESTE.csv`.

## Sources exclues avant tirage
- STMUS_NDA : échographie musculaire (biceps, gastrocnémien) enregistrée comme thyroïde dans l index
- FPUS23 : fantôme fœtal, pas des images de patientes

## Verdict pré-enregistré
Pour l encodeur principal (OBSonix A0 ep300), le signal anatomique SURVIT à la tenue hors des sources
(critère : borne inférieure de l IC à 95 % par grappe de sources > 97,5e centile de permutation, et estimation > ligne de base majoritaire).
Détail par encodeur dans `tables/T6_preregistered_verdict.csv`.
Attention : l encodeur à initialisation aléatoire satisfait lui aussi la règle ; une partie du signal « sources tenues hors » vient donc de statistiques d image de bas niveau, pas des représentations apprises. La comparaison utile est celle entre encodeurs (IC par grappe de sources, larges avec 16 groupes).
Ablation par centrage (T5, F5) : non informative ici, chaque source ne contenant qu une catégorie, le centrage par source retire la catégorie par construction.

## Lecture rapide (exactitude équilibrée, sonde équilibrée)
            Encoder Image-wise balanced accuracy [95% CI, image bootstrap] Source-held-out balanced accuracy [95% CI, source-cluster bootstrap]
       Random init.                                   0.940 [0.928; 0.952]                                                 0.464 [0.340; 0.594]
        ImageNet-1k                                   0.993 [0.987; 0.997]                                                 0.723 [0.564; 0.903]
             DINOv2                                   0.986 [0.979; 0.993]                                                 0.747 [0.636; 0.912]
   OBSonix A0 ep100                                   0.987 [0.981; 0.993]                                                 0.672 [0.542; 0.758]
   OBSonix A0 ep200                                   0.987 [0.980; 0.993]                                                 0.664 [0.517; 0.770]
   OBSonix A0 ep300                                   0.983 [0.976; 0.990]                                                 0.669 [0.530; 0.767]
 OBSonix fetal ep50                                   0.982 [0.975; 0.989]                                                 0.669 [0.515; 0.807]
OBSonix fetal ep100                                   0.987 [0.980; 0.993]                                                 0.659 [0.524; 0.758]

## Paragraphe prêt pour le manuscrit (anglais)
Methods. From the 255,887-image pretraining corpus index, we retained the four anatomical categories present in at least three source datasets with at least 20 images per (category, source) cell (fetal, cardiac, thyroid, breast), sampled at most 300 images per cell (5,656 images from 19 sources). Two sources were excluded before sampling after image-level verification of source labels: STMUS_NDA (skeletal-muscle ultrasound recorded as thyroid in the index); FPUS23 (a fetal phantom rather than patient images). We assigned whole sources (datasets from the same institution or acquisition campaign grouped together: EchoNet, FetalEcho) to 3 folds so that every fold held out at least one source of each category (Table T3). Frozen encoders produced mean-pooled patch-token features (Resize((px,px)) + ToTensor, RGB, px=256 (I-JEPA, IN1K) ou 224 (DINOv2)); a multinomial logistic-regression probe with class-balanced weights was trained on features standardized with training statistics. Two designs were compared on the same images: an image-wise split (70/30 within each source, sources shared between training and test) and a source-held-out design, in which each image is predicted by a probe that never saw its source. The primary outcome was balanced accuracy of pooled out-of-fold predictions, with 95% bootstrap confidence intervals by image and by source cluster (1000 resamples) and a within-fold label-permutation null (500 permutations). As controls, a probe was trained to identify the acquisition source on held-out images of the same sources, overall and within each category, and the source-held-out probe was repeated after subtracting each source's mean feature vector.

Results. For OBSonix A0 ep300, balanced accuracy was 0.983 [0.976; 0.990] under the image-wise split and 0.669 [0.530; 0.767] with sources held out (permutation null 97.5th percentile 0.260; majority baseline 0.125). For DINOv2, the corresponding values were 0.986 and 0.747 [0.636; 0.912]; for a randomly initialized encoder, 0.940 and 0.464 [0.340; 0.594]. Acquisition source was identified among 19 sources on held-out images with accuracy 0.975 for OBSonix A0 ep300 and 0.912 for the random encoder (chance 0.053). Per-category recall, per-fold dispersion, within-category source identification and the centering ablation are given in Tables T2 to T5 and Figures F1 to F5. Because every source in this sample contains a single anatomical category, per-source centering removes the category signal by construction (balanced accuracy 0.25 for all encoders) and cannot separate acquisition signature from anatomy here. A randomly initialized encoder also exceeded the permutation null with sources held out, so part of the source-held-out signal is available from low-level image statistics rather than learned representations.

Post hoc like-for-like comparison (added on 24 September 2026 after the preregistered analysis; not preregistered). Because the image-wise split of the primary analysis evaluates only the 30% of images held out within each cell whereas the source-held-out design evaluates every image, the image-wise evaluation was repeated as a 3-fold cross-validation stratified by (category, source) cell on the same 5,656 images, so that each image received one out-of-fold prediction under each design and differences could be estimated by paired bootstrap (the same resampled images, or the same resampled source clusters, in both terms). For OBSonix A0 ep300, balanced accuracy was 0.986 [0.983; 0.990] under image-wise cross-validation versus 0.669 with sources held out, a paired difference of 0.317 [0.306; 0.329] by image bootstrap and [0.217; 0.459] by source-cluster bootstrap; for DINOv2, 0.986 versus 0.747. With sources held out, the paired contrast OBSonix A0 ep300 minus DINOv2 was -0.078 [-0.254; +0.013] (source-cluster bootstrap) and OBSonix A0 ep300 minus the random encoder +0.205 [+0.076; +0.307]. Tables T9 and T10 and Figure F6 give all encoders and both designs.

## Fichiers
- T1_anatomy_probe_main : table principale (les deux pondérations de sonde, lignes de base)
- T2_recall_by_category_source_held_out : rappel par catégorie, sources tenues hors
- T3_fold_design : plan des plis (sources tenues hors et effectifs)
- T4_source_identification_control : identification de la source, globale et à catégorie fixée
- T5_per_source_centering_ablation : effet du centrage par source (si activé ; dégénéré quand chaque source n a qu une catégorie)
- T6_preregistered_verdict : application de la règle de décision
- T9_like_for_like_image_cv_vs_source_held_out : analyse post hoc (S4b, 24 septembre 2026, non pré-enregistrée) ; validation croisée par image à 3 plis sur les mêmes images, différence appariée entre plans par encodeur
- T10_paired_encoder_contrasts : différences appariées entre encodeurs (référence : témoin et aléatoire) sous chaque plan, IC par image et par grappe de sources
- F1 à F6 : figures correspondantes (F6 : différences appariées)
