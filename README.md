# Projet topographique

Trois mini-outils web pour des calculs topographiques classiques (en **gon**, repère où l'axe Y pointe au Nord et le gisement est mesuré dans le sens horaire à partir du Nord).

## Pages

| Page | Rôle |
|------|------|
| [topo.html](topo.html) | Distance et gisement entre deux points A et B |
| [rayonement.html](rayonement.html) | Coordonnées d'un point M obtenues par rayonnement depuis une station S avec une référence R |
| [intersection.html](intersection.html) | Coordonnées d'un point M obtenues par intersection à partir de deux stations A et B et des angles α, β |

Les trois pages partagent la feuille de style [topo.css](topo.css) et sont reliées par une barre de navigation.

## Utilisation

Ouvrir n'importe quel fichier HTML dans un navigateur — aucun serveur n'est nécessaire.

## Conventions

- Coordonnées en mètres, axe Y vers le Nord, axe X vers l'Est.
- Angles et gisements en **gon** (400 gon = un tour complet, 100 gon = 90°).
- Gisement mesuré dans le sens horaire à partir du Nord.
- Les angles α et β des observations sont mesurés en sens horaire.

## Formules utilisées

**Gisement de A vers B** (selon le quadrant de Δx = x₂ − x₁, Δy = y₂ − y₁) :

| Quadrant | Condition | Formule (gon) |
|----------|-----------|---------------|
| 1 (NE) | Δx ≥ 0, Δy > 0 | `atan(Δx/Δy) × 200/π` |
| 2 (SE) | Δx ≥ 0, Δy < 0 | `atan(\|Δy/Δx\|) × 200/π + 100` |
| 3 (SO) | Δx < 0, Δy < 0 | `atan(Δx/Δy) × 200/π + 200` |
| 4 (NO) | Δx < 0, Δy ≥ 0 | `atan(\|Δy/Δx\|) × 200/π + 300` |

**Distance** : `d = √(Δx² + Δy²)`

**Rayonnement** : G_SM = G_SR + α, puis x_M = x_S + d·sin(G_SM), y_M = y_S + d·cos(G_SM)

**Intersection** : γ = 200 − α − β, puis loi des sinus : AM = AB·sin(β)/sin(γ), G_AM = G_AB ± α (signe selon le côté de M par rapport à AB).

## Historique des modifications

### Corrections de la logique de calcul

- **[topo.html](topo.html)** — bug corrigé : l'ancienne version ajoutait `+100`, `+200`, `+300` (valeurs en gon) à un `atan` toujours en radians, puis multipliait l'ensemble par `200/π` à la fin. Les décalages explosaient à ~6366 gon. La conversion radians → gon est maintenant faite à l'intérieur de chaque branche de quadrant.
- **[intersection.html](intersection.html)** — la convention `G_AM = G_AB − α` était codée en dur (M à gauche de AB). Un sélecteur permet désormais de choisir le côté (droite / gauche). Validation ajoutée pour `α + β < 200 gon`. Les distances AM et BM sont également affichées.
- **[rayonement.html](rayonement.html)** — logique déjà correcte. Le `− 2·π` superflu dans les arguments de `sin`/`cos` (sans effet, sin/cos étant 2π-périodiques) a été retiré.
- Cas limite Δx = Δy = 0 (gisement indéfini) : message d'erreur explicite au lieu d'un `NaN`.

### Améliorations de l'interface

- Refonte de [topo.css](topo.css) (l'ancienne avait un `height` dupliqué, une grille fixe 300×300 px et un `margin-left: 400px` codé en dur sur le bouton).
- Mise en page commune aux trois pages : carte centrée, fieldsets, fond dégradé, états de focus, responsive (mobile).
- Barre de navigation reliant les trois pages avec mise en évidence de la page active.
- Résultats affichés dans un bloc dédié en lecture seule, avec unités (m, gon) et arrondis (3 à 4 décimales).
- Ligne d'erreur affichant les messages de validation (champs vides, points confondus, angles invalides).
- Placeholders donnant des exemples de valeurs attendues.

## Structure du projet

```
projet-topographique/
├── README.md
├── topo.css           # styles partagés
├── topo.html          # distance + gisement
├── rayonement.html    # rayonnement
└── intersection.html  # intersection
```
