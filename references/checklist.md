# Checklist d'Audit UI — Card

Basée sur les 16 UI Design Tips d'Adham Dannaway (Practical UI).

---

## 1. Espacement & Groupement

| # | Critère | Règle |
|---|---------|-------|
| 1.1 | Groupement par proximité | Les éléments liés sont proches, les sections distinctes ont un espacement plus large |
| 1.2 | Padding intérieur | Minimum 16-24px entre le contenu et les bords de la card |
| 1.3 | Espacement inter-sections | L'espace entre groupes logiques > l'espace intra-groupe (ratio ≥ 2:1) |
| 1.4 | Alignement des icônes | Les icônes sont alignées avec le texte adjacent (baseline ou middle) |

## 2. Cohérence

| # | Critère | Règle |
|---|---------|-------|
| 2.1 | Style d'icônes | Toutes les icônes ont le même style (outline OU filled, pas un mélange) |
| 2.2 | Stroke weight icônes | Trait uniforme (2pt recommandé) avec coins arrondis cohérents |
| 2.3 | Tailles d'éléments similaires | Les éléments de même rôle ont la même taille |

## 3. Affordance visuelle

| # | Critère | Règle |
|---|---------|-------|
| 3.1 | Éléments interactifs vs statiques | Les éléments cliquables se distinguent visuellement des éléments statiques |
| 3.2 | Couleur = interactif | La couleur de marque est réservée aux éléments interactifs (liens, boutons) |
| 3.3 | Pas de faux affordances | Les éléments non-cliquables ne ressemblent pas à des boutons |

## 4. Hiérarchie visuelle

| # | Critère | Règle |
|---|---------|-------|
| 4.1 | Squint test | En plissant les yeux : le titre est la zone la plus dense, le CTA est la zone de couleur contrastée |
| 4.2 | Hiérarchie par weight | Privilégier le **font-weight** (Bold vs Regular) à la font-size pour distinguer titre/body |
| 4.3 | Delta de luminosité texte | Le texte secondaire est au moins 2 crans plus clair que le titre |
| 4.4 | Prominence du CTA | L'action principale = l'élément le plus visible de la card |

## 5. Simplification

| # | Critère | Règle |
|---|---------|-------|
| 5.1 | Pas de styles superflus | Aucune bordure, fond ou couleur sans rôle fonctionnel |
| 5.2 | Bruit visuel minimal | Pas de décoration purement esthétique qui n'aide pas la compréhension |

## 6. Couleur intentionnelle

| # | Critère | Règle |
|---|---------|-------|
| 6.1 | Couleur avec un but | Chaque couleur a une signification (statut, interaction, hiérarchie) |
| 6.2 | Gris teintés | Les gris neutres doivent avoir 1-3% de saturation de la couleur de marque |
| 6.3 | Pas de noir pur | Aucun texte en `#000000` — viser `#1A1A1A` ou un gris foncé teinté |

## 7. Contraste — Éléments UI (WCAG)

| # | Critère | Règle |
|---|---------|-------|
| 7.1 | Contraste UI ≥ 3:1 | Les composants UI (boutons, icônes, bordures de champs) ont un ratio ≥ 3:1 |
| 7.2 | Icônes sur images | Les icônes superposées à des images ont un fond solide ou un contraste garanti |

## 8. Contraste — Texte (WCAG)

| # | Critère | Règle |
|---|---------|-------|
| 8.1 | Petit texte ≥ 4.5:1 | Texte ≤ 18px : ratio de contraste ≥ 4.5:1 |
| 8.2 | Grand texte ≥ 3:1 | Texte > 18px bold ou > 24px regular : ratio ≥ 3:1 |
| 8.3 | Labels discrets mais lisibles | Les métadonnées/labels sont discrets mais respectent le WCAG |

## 9. Couleur comme seul indicateur

| # | Critère | Règle |
|---|---------|-------|
| 9.1 | Pas de couleur seule | Les liens, statuts, erreurs utilisent un indice autre que la couleur (underline, icône, shape) |

## 10. Typographie — Typeface

| # | Critère | Règle |
|---|---------|-------|
| 10.1 | Une seule famille sans-serif | Utiliser 1 typeface sans-serif pour toute la card |
| 10.2 | X-height élevé | Privilégier une police avec des minuscules hautes (Inter, SF Pro, etc.) |

## 11. Uppercase

| # | Critère | Règle |
|---|---------|-------|
| 11.1 | Limiter l'uppercase | Pas d'uppercase sur le body text ou les descriptions |
| 11.2 | Sentence case par défaut | Les titres et labels utilisent le sentence case |

## 12. Font weights

| # | Critère | Règle |
|---|---------|-------|
| 12.1 | Regular + Bold seulement | Utiliser maximum 2 poids : Regular pour le corps, Bold pour les titres |
| 12.2 | Pas de Light/Thin petit | Aucun texte < 16px en Light ou Thin |

## 13. Alignement texte

| # | Critère | Règle |
|---|---------|-------|
| 13.1 | Left-align par défaut | Le body text est aligné à gauche |
| 13.2 | Centre limité | Le centrage est réservé aux titres courts ou éléments isolés |

## 14. Line height

| # | Critère | Règle |
|---|---------|-------|
| 14.1 | Body ≥ 1.5 | Le line-height du texte courant est ≥ 150% de la font-size |
| 14.2 | Titre ≥ 1.2 | Le line-height des titres est ≥ 120% |

## 15. Ombres & Profondeur

| # | Critère | Règle |
|---|---------|-------|
| 15.1 | Ombre 2 couches | L'ombre idéale = 1 couche diffuse large + 1 couche sombre proche |
| 15.2 | Pas d'ombre boueuse | L'ombre ne doit pas être gris foncé opaque — utiliser faible opacité |

## 16. Bordures & Radius

| # | Critère | Règle |
|---|---------|-------|
| 16.1 | Bordure subtile | Préférer 1px à 5-10% opacity plutôt qu'un gris solide |
| 16.2 | Radius imbriqué | Radius enfant = radius parent − padding (jamais supérieur) |
| 16.3 | Cohérence des radius | Tous les éléments de même niveau ont le même border-radius |
