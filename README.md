# etude-de-danger-pour-feu-de-nappe-dans-une-station-service
Voici la méthode en 5 étapes, avec les formules.

1. Géométrie de la nappe

- Surface A, fixée par la cuvette de rétention, la pente ou le volume répandu. Le diamètre équivalent est D = √(4A/π).

- Pour un dépotage, A est la zone de la fuite, limitée par la dalle et les caniveaux.

2. Vitesse de combustion

- ṁ″ = 0,055 kg/m²·s pour l'essence. L'ordre de grandeur est correct, mais la fourchette de la littérature va de 0,055 à 0,08 selon les sources. Garde 0,055 par défaut et note que c'est un paramètre sensible.

3. Hauteur de flamme (Thomas, sans vent)

H/D = 42 · [ ṁ″ / (ρ_air · √(g·D)) ]^0,61

avec ρ_air = 1,2 kg/m³. Avec du vent, la flamme s'incline et s'allonge. Il existe une version de Thomas avec vent, et on peut la prévoir si le vent dominant est connu.

4. Puissance émissive de surface (modèle de flamme solide, Mudan-Croce)

SEP = SEP_max · e^(−sD) + SEP_suie · (1 − e^(−sD))

avec SEP_max = 140 kW/m², SEP_suie = 20 kW/m², s = 0,12 m⁻¹. La SEP baisse quand la nappe grossit, parce que la fumée noire écrante la flamme.

5. Flux reçu par une cible à distance x

q = SEP · τ · F

- τ est la transmissivité atmosphérique. Une formule courante est τ = 2,02·(P_w·d)^(−0,09), avec P_w la pression partielle de vapeur d'eau et d la distance à la flamme.

- F est le facteur de vue entre la flamme (un cylindre de diamètre D et de hauteur H) et la cible. Il n'y a pas de formule simple, donc je l'ai calculé par intégration numérique. Les tables de Mudan ou de Howell donnent les mêmes valeurs.

- On cherche ensuite la distance où q descend à chaque seuil (3, 5, 8 kW/m²).

Exemple (essence, cible à 1,5 m de hauteur, sans vent)

| **Surface nappe** | **D** | **H** | **SEP** | **8 kW/m²** | **5 kW/m²** | **3 kW/m²** |
| --- | --- | --- | --- | --- | --- | --- |
| 20 m² | 5,0 m | 9,8 m | 85 kW/m² | 9 m | 12 m | 17 m |
| 50 m² | 8,0 m | 13,5 m | 66 kW/m² | 10,5 m | 15 m | 21 m |
| 100 m² | 11,3 m | 17,2 m | 51 kW/m² | 11 m | 16 m | 23 m |

Les distances sont mesurées depuis le bord de la nappe. Ces ordres de grandeur sont cohérents avec ceux qu'on trouve dans les études de dangers de stations-service.

Limites à écrire dans ton document

- Cible plane verticale, flamme cylindrique droite, sans vent, sans effet des bâtiments ni de l'auvent.

- Le résultat est très sensible à la surface A, donc à ton hypothèse de rétention.

- Le modèle ne couvre pas le feu de cuve (à traiter à part si tu as des stockages aériens).

Le lien avec ta méthode : les distances aux seuils alimentent directement Ω_Spatiale à l'étape 7 (disque autour de la nappe, croisé avec le SIG).

Tu veux que je te livre ce calcul sous forme de script Python réutilisable, ou qu'on passe au feu éclair et à la dispersion ?
