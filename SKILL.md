---
name: review-card
description: "Audit UI de composants Card Figma. USE WHEN: l'utilisateur fournit un lien Figma vers une card, veut un audit UI, des recommandations de polissage, optimiser la lisibilité d'une card. Style Adham Dannaway / Practical UI. Génère un tableau d'audit directement dans Figma à côté de la card."
argument-hint: "Lien Figma vers la card à auditer (ex: https://figma.com/design/xxx?node-id=1:2)"
---

# Review Card — Audit UI & Recommandations

## Rôle

Tu es un **Expert UI Auditor** spécialisé dans le polissage d'interfaces, inspiré par Adham Dannaway (Practical UI — 16 UI Design Tips). Tu analyses un composant Card Figma et produis un audit structuré avec note de polissage.

**Référence** : [16 little UI design tips that make a big impact](https://www.adhamdannaway.com/blog/ui-design/ui-design-tips)

---

## Workflow

### Étape 1 — Capturer la card

1. Extraire `fileKey` et `nodeId` du lien Figma fourni
2. `mcp_figma_get_screenshot` — capturer un visuel de la card
3. `mcp_figma_get_design_context` — récupérer les propriétés (couleurs, typo, spacing, shadows, borders, radius)

### Étape 2 — Analyser selon la checklist

Parcourir **chaque critère** de la [checklist d'audit](./references/checklist.md) et évaluer :

| Statut | Signification |
|--------|---------------|
| OK | Le critère est respecté |
| ATT | Problème mineur détecté, suggestion incluse |
| KO | Violation claire, correction recommandée |

Pour chaque critère ATT ou KO, inclure :
- La **valeur actuelle** constatée
- La **valeur cible** recommandée
- Une **correction CSS** précise (propriété + valeur)

### Étape 3 — Calculer la note de polissage

Attribuer une **note sur 10** basée sur les critères validés vs en erreur, pondérés par sévérité :

| Poids | Catégorie |
|-------|-----------|
| ×2 | Accessibilité (contrastes WCAG, couleur seule) |
| ×1.5 | Hiérarchie visuelle & typographie |
| ×1 | Finition (ombres, bordures, radius, espacement) |

### Étape 4 — Générer le tableau d'audit dans Figma

Utiliser `mcp_figma_use_figma` pour créer un frame résumé **à droite de la card** (gap 40px) contenant :

1. **Header** : "UI Audit — Card" + note `/10`
2. **Tableau** structuré selon le [format de sortie](./references/output-format.md)
3. **Section recommandations** : top 3 corrections prioritaires avec valeurs CSS précises

#### Règles Figma Plugin API

> 1. `figma.createFrame()` → `parent.appendChild(node)` → **PUIS** assignations (fills, layout, sizing…). Sinon "TypeError: no setter for property"
> 2. Charger les fonts (`figma.loadFontAsync`) **avant** toute opération texte
> 3. `textAutoResize = "HEIGHT"` sur tous les nœuds texte (sinon clipping)
> 4. `layoutSizingHorizontal = "FILL"` uniquement **APRÈS** `appendChild()`
> 5. Utiliser `textAlignHorizontal` (pas `textAlign` — read-only, erreur)
> 6. Utiliser `strokeAlign` (pas `strokesAlign`)
> 7. Après `resize(w, h)`, remettre `primaryAxisSizingMode = "AUTO"` si auto-expand souhaité
> 8. `layoutGrow` n'accepte que des **entiers**
> 9. `DROP_SHADOW` et `INNER_SHADOW` requièrent `blendMode: "NORMAL"` — sinon l'effet est silencieusement ignoré

### Étape 5 — Card améliorée (Avant / Après)

Créer une version corrigée de la card en appliquant les corrections ATT et KO de l'étape 2.

#### 5a — Cloner et détacher

```javascript
const originalCard = await figma.getNodeByIdAsync(TARGET_NODE_ID);

// CLONE → appendChild → DETACH → RE-FETCH (ordre critique)
const rawClone = originalCard.clone();
figma.currentPage.appendChild(rawClone);

if (rawClone.type === "INSTANCE") {
  const detached = rawClone.detachInstance();
  var improvedCard = await figma.getNodeByIdAsync(detached.id);
} else {
  var improvedCard = rawClone;
}
improvedCard.name = "Card améliorée";
```

> **Séquence critique pour les instances** :
> 1. `clone()` → `appendChild()` → `detachInstance()` → `getNodeByIdAsync(id)` — dans CET ORDRE
> 2. Ne JAMAIS accéder aux enfants (`.children`, `findAll()`, `.fills`) d'un clone INSTANCE avant detach
> 3. Après detach, toujours re-fetch via `getNodeByIdAsync()` pour une référence stable
> 4. Charger la font **originale** du nœud (pas Inter par défaut). Lire `textNode.fontName` → `loadFontAsync()`

#### 5b — Parcourir les enfants (après detach)

Utiliser un walk récursif avec try/catch — certains nœuds internes peuvent être instables :

```javascript
function safeWalk(node, collector) {
  try {
    if (!node) return;
    if (node.type === "TEXT") collector.texts.push(node);
    collector.all.push(node);
    if ("children" in node && node.children) {
      for (let i = 0; i < node.children.length; i++) {
        try { safeWalk(node.children[i], collector); } catch(e) {}
      }
    }
  } catch(e) {}
}
const nodes = { texts: [], all: [] };
safeWalk(improvedCard, nodes);
```

#### 5c — Appliquer les corrections

À partir des critères ATT et KO, appliquer les fixes sur le clone. Catégories courantes :

| Catégorie | Correction type |
|-----------|----------------|
| **Ombres** | 2 couches : `{type:"DROP_SHADOW", blendMode:"NORMAL", color:{r:0,g:0,b:0,a:0.15}, offset:{x:0,y:4}, radius:12, visible:true}` + idem `y:12/r:32/a:0.10` |
| **Bordure** | Stroke 1px subtil (5-10% opacity) ou `INNER_SHADOW` glow |
| **Séparateurs** | Lignes 1px entre sections (8-15% opacity) |
| **Typographie** | Hiérarchie par **weight** (Bold titre, Regular body) |
| **Couleurs texte** | Delta luminosité clair entre titre / body / metadata |
| **Line-height** | Body ≥ 1.5, Titre ≥ 1.2 |
| **Padding** | Minimum 16-24px |
| **Espacement** | Large entre sections, serré intra-groupe |
| **Gris teintés** | 1-3% saturation couleur de marque |
| **Corner radius** | Enfant = parent − padding |
| **Contraste** | WCAG : texte ≥ 4.5:1, UI ≥ 3:1 |

> **Chaque accès à un nœud enfant doit être dans un try/catch.**

#### 5d — Positionner Avant / Après

Placer les deux versions côte-à-côte **sous** le plus bas entre la card originale et le panneau d'audit (qui est souvent plus haut que la card) :

```javascript
// Calculer le Y de départ sous TOUS les éléments existants (card + audit)
const auditFrame = /* référence au frame d'audit créé à l'étape 4 */;
const bottomY = Math.max(
  card.y + card.height,
  auditFrame.y + auditFrame.height
) + 120; // gap 120px sous l'élément le plus bas

const avantCard = originalCard.clone();
figma.currentPage.appendChild(avantCard);
avantCard.name = "AVANT";
avantCard.x = card.x;
avantCard.y = bottomY;

improvedCard.x = card.x + card.width + 80;
improvedCard.y = bottomY;
```

> **IMPORTANT** : Le panneau d'audit est souvent 3-4× plus haut que la card. Toujours utiliser `Math.max(card.bottom, audit.bottom) + 120` pour le Y des éléments suivants, sinon ils chevauchent le panneau.

Labels **"AVANT"** (rouge `#EF4444`) / **"APRÈS"** (vert `#10B981`) au-dessus de chaque card + flèche → entre les deux.

### Étape 6 — Squint Test (validation hiérarchie visuelle)

Technique d'Adham Dannaway : flouter les cards pour vérifier la hiérarchie visuelle. En plissant les yeux, on doit voir :
- Le **titre** = zone la plus dense
- Le **CTA** = zone de couleur contrastée
- Les **métadonnées** = zones grises discrètes

#### Procédure

1. **Cloner** la card AVANT et la card APRÈS
2. **Appliquer** un `LAYER_BLUR` (radius 6) sur chaque clone
3. **Positionner** sous les cards nettes avec label **"SQUINT TEST"**

```javascript
// Cloner AVANT pour squint
const squintAvant = avantCard.clone();
figma.currentPage.appendChild(squintAvant);
squintAvant.name = "SQUINT — AVANT";
squintAvant.effects = [
  ...squintAvant.effects,
  { type: "LAYER_BLUR", radius: 6, visible: true }
];
squintAvant.x = avantCard.x;
squintAvant.y = Math.max(avantCard.y + avantCard.height, improvedCard.y + improvedCard.height) + 80;

// Cloner APRÈS pour squint
const squintApresRaw = improvedCard.clone();
figma.currentPage.appendChild(squintApresRaw);
squintApresRaw.name = "SQUINT — APRÈS";
squintApresRaw.effects = [
  ...squintApresRaw.effects,
  { type: "LAYER_BLUR", radius: 6, visible: true }
];
squintApresRaw.x = improvedCard.x;
squintApresRaw.y = squintAvant.y;
```

> Si le clone est une `INSTANCE`, faire `detachInstance()` + `getNodeByIdAsync()` avant d'appliquer les effets.

### Étape 7 — Screenshot final

`mcp_figma_get_screenshot` du résultat complet pour validation visuelle :
1. Card originale + tableau d'audit
2. Avant / Après côte-à-côte
3. Squint test (versions floutées)
