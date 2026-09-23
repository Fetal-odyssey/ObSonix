# Pré-enregistrement, campagne analyses3_anatomie4

Date : 2026-09-23 10:28

## Question
La représentation gelée des encodeurs sépare-t-elle l'anatomie (fœtal, cardiaque, thyroïde, sein) lorsque les sources
d'acquisition du test n'ont jamais été vues par la sonde ? L'analyse du 27 juillet (95 % par image, 14 % par source)
était confondue par des catégories absentes de l'entraînement ; celle-ci se restreint aux catégories présentes dans au
moins trois sources avec au moins 20 images par cellule, et chaque pli de test contient les quatre catégories.

## Critère principal
Exactitude équilibrée (moyenne des rappels par catégorie) de la sonde « sources tenues hors », prédictions hors pli
regroupées, avec intervalle de confiance bootstrap à 95 % par grappe de sources et test de permutation (N = 500).
Règle de décision, fixée avant de voir les résultats : le signal anatomique survit à la tenue hors des sources pour un
encodeur si la borne inférieure de l'IC à 95 % par grappe dépasse le 97,5e centile de la distribution de permutation
et si l'exactitude équilibrée dépasse la ligne de base « classe majoritaire ». Le verdict global porte sur
A0 ep300 (encodeur principal) et DINOv2 (témoin).

## Critères secondaires
Exactitude brute, F1 macro, rappel par catégorie, même mesure sous découpage par image (sources partagées),
identification de la source (globale et à catégorie fixée), ablation par centrage par source.

## Paramètres
{
 "campagne": "analyses3_anatomie4",
 "graine": 1789,
 "categories": [
  "foetal",
  "cardiaque",
  "thyroide",
  "sein"
 ],
 "min_cellule": 20,
 "plafond": 300,
 "k_plis": 4,
 "n_boot": 1000,
 "n_perm": 500,
 "familles": {
  "EchoNet_Dynamic": "EchoNet",
  "EchoNet_LVH": "EchoNet",
  "EchoNet_Pediatric": "EchoNet",
  "FE1_FetalEcho_T1": "FetalEcho",
  "FE2_FetalEcho_T2": "FetalEcho"
 },
 "encodeurs": [
  "RANDOM",
  "IN1K",
  "DINOv2",
  "A0 ep100",
  "A0 ep200",
  "A0 ep300",
  "FOETAL ep50",
  "FOETAL ep100"
 ],
 "principal": "A0 ep300",
 "temoin": "DINOv2",
 "pretraitement": "Resize((px,px)) + ToTensor, RGB, px=256 (I-JEPA, IN1K) ou 224 (DINOv2)",
 "features": "moyenne des tokens de patch (token de classe exclu), standardisés sur le train",
 "sonde": "régression logistique multinomiale (lbfgs, C=1, max_iter=3000), variantes équilibrée et brute"
}
