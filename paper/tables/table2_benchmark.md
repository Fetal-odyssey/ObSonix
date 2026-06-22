| Method | Dice | HC MAE (px) | Fail (%) | Inference (ms) |
|---|---|---|---|---|
| [INIT] init=IN1K  (AttResUNet) | 0.9781+/-0.0107 | 8.3 | 0.0% | 25.6 |
| [INIT] init=LeJEPA(AttResUNet) | 0.9599+/-0.0401 | 12.8 | 0.0% | 26.3 |
| [INIT] init=DAPT14(AttResUNet) | 0.9650+/-0.0322 | 11.7 | 0.0% | 25.6 |
| [INIT] init=OBSonix(AttResUNet) | 0.9653+/-0.0373 | 11.5 | 0.0% | 26.4 |
| [DEC]  dec=UNet         (EXP-15) | 0.6718+/-0.0966 | 204.6 | 0.0% | 26.1 |
| [DEC]  dec=AttResUNet   (EXP-16) | 0.9650+/-0.0341 | 12.2 | 0.0% | 25.6 |
| [DEC]  dec=+SynthAug    (EXP-17) | 0.9662+/-0.0347 | 12.4 | 0.0% | 26.4 |