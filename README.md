# Méthode hiérarchique d'estimation probabiliste des distances d'effets thermiques des feux de nappe

*Synthèse de travail — version à compléter avec les résultats CFD.*

## 1. Objectif

Estimer, pour une nappe enflammée (circulaire ou cuvette rectangulaire), les distances aux seuils de flux thermique de 3, 5 et 8 kW/m², sous forme de **distribution de probabilité** et non de valeur unique, en combinant des outils existants dans une architecture hiérarchique cohérente. La contribution revendiquée est l'**articulation** des niveaux, pas la création de nouvelles corrélations.

## 2. Architecture

| Niveau | Outil | Rôle |
|---|---|---|
| 1 | Corrélations + flamme solide | Estimation rapide, criblage, simulateur de référence |
| 2 | CFD (ex. FDS) | Physique complexe ; plan d'expériences ; calibrage du niveau 1 |
| 3 | Métamodèle (processus gaussien) | Remplace la CFD pour les milliers de tirages |
| 4 | Monte-Carlo | Propagation de l'incertitude des entrées |

Ordre de travail : niveau 1 → CFD sur plan d'expériences → métamodèle entraîné sur la CFD → Monte-Carlo **sur le métamodèle**. Ce n'est pas une chaîne où chaque étape consomme la précédente : la CFD est un calcul indépendant de plus haute fidélité, qui sert à calibrer et valider le niveau 1.

## 3. Niveau 1 : modèle

1. Diamètre équivalent : D = √(4A/π).
2. Vitesse de combustion (Burgess) : ṁ'' = ṁ''∞ (1 − e^(−kβD)) ; essence : ṁ''∞ = 0,055 kg/m²·s, kβ = 2,1 m⁻¹.
3. Hauteur de flamme : Thomas en air calme, H/D = 42 [ṁ''/(ρ_a √(gD))]^0,61, multipliée par un facteur correctif fH (voir §6).
4. Inclinaison sous le vent : corrélation de Johnson (tan θ / cos θ = 0,666 Re^0,117 Fr^(1/3)).
5. Puissance émissive moyenne (Mudan-Croce) : SEP = SEP_max e^(−sD) + SEP_suie (1 − e^(−sD)), avec 140 kW/m², 20 kW/m², s = 0,12 m⁻¹.
6. Flux reçu : q = SEP · Σ τ_i F_i, par intégration de la surface de la flamme (prisme oblique construit sur l'empreinte de la nappe), avec transmissivité atmosphérique τ = min(1, 2,02 (P_w x)^(−0,09)), P_w en Pa, x en m, calculée patch par patch.
7. Orientation de la cible : la plus pénalisante, q = SEP √(F_h² + F_v²).
8. Distances mesurées depuis le **bord** de la nappe ; on retient la distance d'enveloppe (maximum sur tous les azimuts).

## 4. Niveaux 2 et 3 : plan d'expériences et métamodèle

- Entrées du métamodèle : surface A, vitesse du vent u, rapport L/l de la cuvette, angle du vent par rapport au grand côté, facteur fH. Pour une nappe circulaire, l'azimut est une coordonnée de sortie et non une entrée.
- Plan d'expériences par hypercube latin. Dans les démonstrations ci-dessous, 150 à 200 points ont été évalués avec le **niveau 1 comme simulateur** ; pour de vrais résultats, ces points doivent être des runs CFD (nombre à fixer selon le coût d'un run et le nombre d'entrées).
- Métamodèle : processus gaussien (noyau de Matérn anisotrope), sortie en log(d + 1).
- Validation : jeu de test indépendant, validation croisée, comparaison avec un Monte-Carlo direct.

## 5. Niveau 4 : Monte-Carlo

Lois d'entrée utilisées dans les démonstrations (à remplacer par des données du site) : vent Weibull (k = 2, λ = 5 m/s) ; surface log-normale (médiane 50 m², σ = 0,3) ; angle du vent uniforme. Le Monte-Carlo (10⁵ tirages) est exécuté sur le métamodèle. La probabilité de dépassement d'une distance donnée se lit directement sur la distribution.

## 6. Validation du niveau 1 sur mesures publiées

| Test | Observé | Modèle | Écart |
|---|---|---|---|
| Inclinaison, GNL 20 m, 6,15 m/s | 54° | 54,9° | +1,7 % |
| Inclinaison, GPL 20 m, 6,6 m/s | 53° | 55,8° | +5,3 % |
| Longueur de flamme (L/D), GNL | 2,15 | 1,91 | −11 % |
| Longueur de flamme (L/D), GPL | 2,35 | 2,16 | −8 % |
| Longueur de flamme (L/D), kérosène | 1,5 à 1,9 | 1,35 | −21 % (milieu de plage) |
| SEP kérosène, D = 10 m | 60 kW/m² | 56 | −6 % |
| SEP kérosène, D = 20 m | 35 kW/m² | 31 | −12 % |
| Transmissivité, 30 m et 110 m (27 °C, 53 % HR) | 0,75 ; 0,67 | 0,75 ; 0,67 | < 1 % |

Sources : Mizner et Eyre (1982) pour les feux de 20 m ; les valeurs de SEP du kérosène sont citées par ces auteurs d'après Häglund et Persson. Observations complémentaires : feu de gazole de 41,5 m (Pimper et al., 2014), flux sous le vent environ deux fois supérieur au flux au vent, SEP moyenne de 20 à 30 kW/m².

**Facteur fH.** Thomas en air calme sous-estime la longueur de flamme de 8 à 21 % dans ces essais ; on retient fH log-normal, médiane 1,13, σ_ln = 0,10 (choix fondé sur trois points, à recaler sur la CFD). La formule de Thomas avec vent donne des flammes nettement plus courtes (L/D de 1,50 et 1,70 pour le GNL et le GPL, soit 38 à 44 % de moins que l'observé) et n'a pas été retenue ici.

## 7. Résultats illustratifs (essence, A médiane 50 m², vent Weibull)

Distances d'enveloppe à 3 kW/m², en mètres depuis le bord, P50 / P95. **Simulateur = niveau 1, non CFD.**

| Modèle de hauteur de flamme | Carré (L/l = 1) | Rectangle (L/l = 4) |
|---|---|---|
| Thomas avec vent | 20,3 / 25,2 | 24,6 / 31,2 |
| Thomas air calme, sans correction | 25,3 / 28,8 | 30,1 / 35,0 |
| Thomas air calme × fH (médiane 1,13) | 27,3 / 32,3 | 32,3 / 38,9 |

Avec la dernière ligne, les distances à 5 et 8 kW/m² (P50) sont de 22,4 et 18,5 m pour L/l = 1, et de 26,2 et 21,5 m pour L/l = 4. Le métamodèle reproduit le Monte-Carlo direct (erreur absolue moyenne d'environ 0,24 m sur 60 cas de test).

À surface égale, une cuvette allongée donne des distances plus grandes qu'un cercle équivalent (jusqu'à environ +30 % pour L/l = 4 avec le vent perpendiculaire au grand côté, à H fixé). Le cercle équivalent peut donc être non conservateur.

## 8. Limites et travaux restants

1. **La CFD n'a pas été exécutée** : tous les chiffres des §7 viennent du niveau 1.
2. **Le choix du modèle de hauteur de flamme domine l'incertitude** (écart de plus de 30 % sur le P50) et n'est validé ni pour l'essence ni pour D ≈ 8 m.
3. H, l'inclinaison et le SEP sont calculés avec le diamètre équivalent, hypothèse non validée pour les nappes très allongées.
4. Visibilité exacte seulement pour une empreinte convexe ; géométries complexes (obstacles, cuvettes multiples, flammes fusionnées) à traiter en CFD.
5. SEP uniforme et rose des vents uniforme : à remplacer par des données réelles.
6. Données de validation en champ lointain pour l'essence : non trouvées. À obtenir (rapport Sandia SAND2010-6810C, nappe de 7,93 m en JP-8, flux près du calorimètre) ou à produire.
7. Le métamodèle n'est valable que dans le domaine d'entraînement ; un métamodèle par combustible.
8. Originalité : à établir par une revue de littérature (métamodèles et QRA des feux de nappe) avant toute affirmation.

## 9. Références consultées

- Mizner, G. A., Eyre, J. A. (1982). *Large-scale LNG and LPG pool fires*. IChemE Symposium Series No. 71.
- Pimper, L., Mészáros, Z., Koseki, H. (2014). *Large scale diesel oil burns*. AARMS 13(2), 329–336.
- Johnson, A. D. (1992). *A model for predicting thermal radiation hazards from large-scale LNG pool fires*. IChemE Symposium Series No. 130 (extrait consulté seulement).
- Thomas, P. H. (1963). *The size of flames from natural fires*. 9th Symposium (International) on Combustion (cité par Mizner et Eyre).
- Notice du rapport SAND2010-6810C, PATRAM 2010 (rapport complet non consulté).

À citer à partir des sources originales, non consultées ici : Burgess/Babrauskas (vitesse de combustion), Mudan et Croce (SEP), formule de transmissivité atmosphérique, textes réglementaires définissant les seuils de 3, 5 et 8 kW/m².
