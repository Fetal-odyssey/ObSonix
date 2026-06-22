# EXP20_OBSONIX_PRETRAIN
*2026-06-22 15:51*

## Metriques
| Metrique | Valeur |
|---|---|
| corpus | 24430 |
| epochs | 50 |
| best_loss | 6.5733 |
| init | DINOv2-B |
| prototypes | 8192 |
| base_lr | 1.25e-04 |
| archi | OBSonix-ViT-B16 |

## Notes
DINO+iBOT stabilise (warmup LR/teacher-temp, freeze last-layer ep0, EMA 0.996->1.0). Corpus 24430, init=DINOv2-B. Data: cache local /content (I/O Drive elimine).
