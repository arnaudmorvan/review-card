# Format de Sortie — Tableau d'Audit Figma

## Structure du frame d'audit

Le frame d'audit est positionné **à droite de la card** (gap 40px) avec la structure suivante :

```
Frame "UI Audit" (auto-layout vertical, padding 32px, gap 24px, counterAxisSizingMode="FIXED", primaryAxisSizingMode="AUTO")
├── Header (auto-layout horizontal, gap 12px)
│   ├── Titre : "UI Audit — Card" (Inter Bold 18px)
│   └── Pill note : "X / 10" (pill arrondie, couleur selon score)
├── Résumé (auto-layout horizontal, gap 16px)
│   ├── "N OK" (vert)
│   ├── "N À améliorer" (orange)
│   └── "N Problèmes" (rouge)
├── Tableau d'audit (auto-layout vertical, gap 0)
│   ├── Row header (fond gris foncé) : # | Critère | Statut | Actuel | Cible | Fix CSS
│   ├── Row critère 1...
│   └── ...
└── Section recommandations (auto-layout vertical, gap 16px, primaryAxisSizingMode="AUTO")
    ├── Titre : "Top 3 Recommandations" (Inter Bold 16px)
    ├── Reco 1 (auto-layout vertical, padding 16px, gap 8px, primaryAxisSizingMode="AUTO")
    ├── Reco 2 ...
    └── Reco 3 ...
```

> **IMPORTANT** : Tout le frame et ses enfants utilisent `primaryAxisSizingMode = "AUTO"` pour que le contenu ne soit jamais coupé. Les textes de recommandation ont `textAutoResize = "HEIGHT"` + `layoutSizingHorizontal = "FILL"`.

## Couleurs de la note

| Score | Couleur | Hex |
|-------|---------|-----|
| 8-10 | Vert | `#10B981` |
| 5-7 | Orange | `#F59E0B` |
| 0-4 | Rouge | `#EF4444` |

## Colonnes du tableau

| Colonne | Largeur | Contenu |
|---------|---------|---------|
| **#** | 32px | Numéro du critère (ex: 1.1) |
| **Critère** | 155px | Nom court du critère |
| **Statut** | 40px | OK / ATT / KO (texte, pas d'emoji) |
| **Actuel** | 155px | Valeur constatée (ex: `#000000`, `8px`, `1.0 line-height`) |
| **Cible** | 155px | Valeur recommandée (ex: `#1A1A1A`, `16px`, `1.5`) |

## Styles du tableau

- **Fond header** : gris foncé `#374151` texte blanc
- **Lignes alternées** : blanc `#FFFFFF` / gris clair `#F9FAFB`
- **Lignes ATT** : fond jaune pale `#FEF3C7`
- **Lignes KO** : fond rouge pale `#FEE2E2`
- **Lignes OK** : fond par défaut (alternance)
- **Texte** : `Inter Regular 10.5px`, headers `Inter Bold 10px`
- **Bordure tableau** : `1px #E5E7EB`

## Lignes compactes du tableau

> **CRITIQUE** : chaque row du tableau doit avoir `counterAxisSizingMode = "AUTO"` pour que la hauteur s'adapte au contenu. Sans cela, les lignes font 100px par défaut.

```
Row : auto-layout horizontal, gap 6px, py 6px, px 8px
  counterAxisSizingMode = "AUTO"   ← hauteur compacte
  layoutSizingHorizontal = "FILL"  ← largeur pleine
  clipsContent = false
```

Texte dans chaque cellule : `textAutoResize = "HEIGHT"` + largeur fixe par colonne (resize).

## Section Recommandations

Chaque recommandation est un **frame auto-layout vertical** (padding 16px, gap 8px) avec `primaryAxisSizingMode = "AUTO"` pour ne jamais couper le contenu.

Format pour chaque recommandation :
```
Frame reco (auto-layout vertical, padding 16px, gap 8px, fill blanc, corner 8px, stroke 1px #E5E7EB)
├── Header (auto-layout horizontal, gap 8px)
│   ├── Numéro dans cercle (24px, couleur charte)
│   └── Titre (Inter Bold 14px)
├── Texte "Avant : [valeur actuelle]" (Inter Regular 13px, gris #6B7280)
├── Texte "Après : [valeur cible]" (Inter Medium 13px, couleur charte)
└── Texte "Impact : [description]" (Inter Regular 13px, gris #6B7280)
```

> **Tous les textes** : `textAutoResize = "HEIGHT"`, `layoutSizingHorizontal = "FILL"` — le texte wrap et le frame s'étend verticalement.

## UI sobre

- **Pas d'emoji** nulle part (ni dans les titres, ni dans les statuts, ni dans les recommandations)
- Statuts en texte : **OK**, **ATT** (attention), **KO**
- Numéros de recommandation : cercle plein avec chiffre blanc (pas d'emoji couronne/trophée)
- Couleurs d'accent via la charte utilisateur (ou défaut vert/orange/rouge)
