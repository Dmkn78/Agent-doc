# 🧠 Méthode complète — Créer un Agent déterministe avec un Skill

> Objectif : être capable de construire un Skill robuste, testable et maintenable, de zéro à production.

---

## PHASE 0 — Choisir sa méthode de construction

Avant de commencer, une seule question : tu pars de quoi ?

| Situation | Méthode à utiliser |
|---|---|
| Tu as déjà fait la tâche une fois avec l'agent | **Bottom-up** : demande à l'agent de "sauver en Skill" |
| Tu conçois un workflow nouveau, complexe, ou en équipe | **Top-down** : conçois d'abord la structure complète |

> **Règle** : Bottom-up = rapidité. Top-down = robustesse. Pour un agent déterministe (comportement prévisible), préfère **Top-down**.

---

## PHASE 1 — Identifier le sujet du Skill

Avant d'écrire une seule ligne, réponds à ces 4 questions :

### 1.1 Objectif principal
> Que fait ce Skill, et quel est le livrable exact ?

Exemple : *"Transforme des notes de réunion en compte-rendu structuré (décisions + actions + échéances)"*

### 1.2 Utilisateurs cibles
> Pour qui ? Un humain seul ? Une équipe ? Un process automatisé ?

### 1.3 Cas d'usage & déclencheurs
> Quand exactement ce Skill doit-il s'activer ?

Exemples de déclencheurs précis :
- Mots-clés : "compte rendu", "minutes", "todo réunion"
- Type de contenu reçu : transcription, notes brutes, export de chat
- Situation : l'utilisateur colle un texte désorganisé

### 1.4 Livrables attendus
> Quel est le format exact de sortie ? Texte ? Tableau ? Fichier ?

---

## PHASE 2 — Structurer le SKILL.md (le cerveau de l'agent)

Le `SKILL.md` est la **SOP (Standard Operating Procedure)** de l'agent. Il doit contenir **exactement 7 sections** + un frontmatter YAML.

### 2.0 Frontmatter YAML (métadonnées)

```yaml
---
name: nom-du-skill-en-kebab-case
description: >
  [Ce que fait le Skill en 1-2 phrases.
  Inclure les cas d'usage et déclencheurs principaux.]
metadata:
  author: ton-nom
  version: "1.0"
---
```

> **Règle critique** : La `description` est ce que l'agent lit en **Niveau 1 (Progressive Disclosure)**. Elle détermine si le Skill sera déclenché ou non. Elle doit être précise, pas vague.

---

### 2.1 Section — Quand utiliser ce Skill

```markdown
## Quand utiliser ce Skill
Utilise ce Skill si :
- [condition 1 : type de contenu ou de demande]
- [condition 2 : mots-clés ou situation]
- ou si l'utilisateur demande "[mot-clé A]", "[mot-clé B]"
```

> **Règle** : Sois exhaustif sur les déclencheurs. Si tu oublies un cas, l'agent ne déclenchera pas le Skill.

---

### 2.2 Section — Entrées attendues

```markdown
## Entrées attendues
- [Input obligatoire 1] (ex : notes/transcription, même désordonnées)
- [Input obligatoire 2]
- (Optionnel) [Input facultatif] : [description]
```

> **Règle** : Si un input est manquant, l'agent doit le **demander à l'humain** avant de continuer (= point HITL, Human-In-The-Loop).

---

### 2.3 Section — Workflow étape par étape

C'est la section la plus importante. Elle définit le **comportement déterministe** de l'agent.

```markdown
## Procédure (étapes)
1. **[Nom de l'étape]** : [description précise de ce que l'agent fait]
   - [Sous-action si nécessaire]
   - ⏸️ PAUSE HUMAINE : [ce que l'agent demande avant de continuer]

2. **[Nom de l'étape]** : [...]

3. **[Nom de l'étape]** : [...]
```

> **Règles pour un agent déterministe :**
> - Chaque étape = **un verbe d'action unique** (Normaliser, Extraire, Dédupliquer, Rendre)
> - Aucune ambiguïté sur l'ordre d'exécution
> - Marquer explicitement les **points de validation humaine** (HITL) avec ⏸️
> - Si SKILL.md > 200 lignes → **signal d'alarme** : externalise dans des ressources

---

### 2.4 Section — Format de sortie

```markdown
## Format de sortie (obligatoire)
- **[Section 1]** : [description]
- **[Section 2]** : [description]
- **[Section 3]** (table : Colonne A | Colonne B | Colonne C)
```

> **Règle** : Spécifie le format **de manière contraignante**. L'agent ne doit pas avoir le choix du format de sortie. C'est ce qui garantit la cohérence entre chaque exécution.

---

### 2.5 Section — Edge cases

```markdown
## Edge cases
- Si [situation limite 1] → [comportement attendu]
- Si [situation limite 2] → [comportement attendu]
- Si l'input est incomplet → [demander X à l'humain]
```

> **Règle** : Cette section est ce qui distingue un Skill basique d'un Skill robuste. Chaque erreur rencontrée en test doit devenir un edge case documenté ici.

---

### 2.6 Section — Règles

```markdown
## Règles
- ❌ INTERDIT : [comportement à ne jamais faire]
- ✅ OBLIGATOIRE : [comportement non négociable]
- ❌ INTERDIT : [...]
```

> **Règle** : Les règles sont les **garde-fous non négociables**. Exemple : "Ne jamais inventer une décision qui n'est pas dans le texte source." ou "Toujours demander une validation avant d'envoyer un email."

---

### 2.7 Section — Chargement de fichiers (ressources)

```markdown
## Fichiers à charger
- Lire `references/regles-style.md` pour les contraintes de format
- Lire `templates/template-sortie.md` pour le format de livrable
- Si MCP utilisé : se référer à `references/mcp-instructions.md`
```

> **Règle critique (Progressive Disclosure)** : L'agent **ne charge pas automatiquement** les fichiers du dossier. Tu dois **nommer explicitement** chaque fichier à charger et **à quelle étape**. Si tu ne le dis pas → l'agent ignore le fichier, même s'il est dans le dossier.

---

### 2.8 Section — Exemple (optionnel mais fortement recommandé)

```markdown
## Exemple
### Input (résumé)
"[Texte brut d'exemple]"

### Output attendu (extrait)
**[Section]**
- [Élément 1]

| Colonne A | Colonne B | Colonne C |
|---|---|---|
| [valeur] | [valeur] | [valeur] |
```

> **Règle** : Un exemple concret réduit drastiquement les hallucinations et les écarts de format. C'est l'outil le plus puissant pour calibrer l'output.

---

## PHASE 3 — Architecture des fichiers (structure "99%")

Le `SKILL.md` = le **process**. Tout le reste = les **ressources**.

```
nom-du-skill/
├── SKILL.md                        ← Le process (SOP de l'agent)
├── references/                     ← La source de vérité
│   ├── regles-metier.md            ← Règles, contraintes, ICP
│   ├── exemples.md                 ← Exemples de bonne sortie
│   └── mcp-instructions.md         ← Si MCP/outil externe
├── templates/                      ← Les modèles de sortie
│   └── template-livrable.md        ← Format exact du livrable
├── activities/                     ← Sous-procédures HITL
│   └── activity-validation.md      ← Checklist de validation humaine
├── assets/                         ← Fichiers non-texte
│   └── exemple-visuel.png
└── scripts/                        ← Code exécutable
    └── normalisation.py
```

### Règle de séparation des responsabilités

| Quoi | Où le mettre | Pourquoi |
|---|---|---|
| Quoi faire + dans quel ordre | `SKILL.md` | C'est le process, pas le contenu |
| Règles métier, exemples, données | `references/` | Modifiable sans toucher au process |
| Format du livrable | `templates/` | Cohérence garantie à chaque run |
| Checklists, validations humaines | `activities/` | Séparation UX / logique |
| Images, JSON, binaires | `assets/` | Contexte non-textuel |
| Scripts, appels API | `scripts/` | Actions exécutables |

> **Règle d'or** : Tu dois pouvoir mettre à jour le contexte (references/) **sans toucher au process** (SKILL.md) — et inversement.

---

## PHASE 4 — Prompter Claude pour construire le Skill

Utilise cette checklist de questions avant de demander à Claude de générer le SKILL.md :

| Question | Ce que ça génère dans le SKILL |
|---|---|
| Quand déclencher ce Skill ? | Section "Quand utiliser" + description YAML |
| Qu'est-ce que ce Skill doit accomplir ? | Objectif + Format de sortie |
| Quels connecteurs / outils utiliser ? | Section "Fichiers à charger" + mcp-instructions |
| Quelles étapes suivre pas-à-pas ? | Section "Procédure" |
| Où mettre l'humain dans la boucle ? | Points ⏸️ HITL dans le workflow |
| Quelle référence utiliser à quelle étape ? | Chargement explicite de fichiers |
| Quel format final exact ? | Section "Format de sortie" + template |

---

## PHASE 5 — Tester & Itérer

### Arbre de diagnostic

| Symptôme observé | Cause probable | Correction |
|---|---|---|
| L'agent ne déclenche pas le Skill | Description YAML trop vague | Reécrire la `description` avec des mots-clés précis |
| L'agent ne suit pas les étapes | Workflow ambigu ou trop long | Réécrire la procédure avec verbes d'action clairs |
| Output de mauvaise qualité | Pas d'exemple ni de template | Ajouter `templates/` + section Exemple dans SKILL.md |
| L'agent fait quelque chose d'interdit | Règle manquante | Ajouter une règle ❌ dans la section Règles |
| L'agent utilise mal un outil/MCP | Pas d'instructions MCP | Créer `references/mcp-instructions.md` et le référencer |
| Output incohérent entre les runs | Format non contraint | Rendre la section "Format de sortie" obligatoire et précise |
| L'agent ignore des fichiers du dossier | Chargement non explicite | Nommer les fichiers **mot pour mot** dans le SKILL.md |

> **Principe** : Chaque erreur = une occasion d'améliorer le Skill. Chaque itération rend l'agent plus déterministe.

---

## Checklist finale avant de déployer un Skill

```
☐ Frontmatter YAML complet (name, description, metadata)
☐ Description précise avec déclencheurs explicites
☐ Section "Quand utiliser" exhaustive
☐ Entrées attendues listées (obligatoires + optionnelles)
☐ Workflow en étapes numérotées avec verbes d'action
☐ Points HITL marqués ⏸️ dans le workflow
☐ Format de sortie défini de manière contraignante
☐ Edge cases documentés (au moins 3)
☐ Règles ❌/✅ définies
☐ Fichiers ressources nommés explicitement dans SKILL.md
☐ Au moins 1 exemple Input → Output
☐ SKILL.md < 200 lignes (sinon externaliser)
☐ Testé sur 3 inputs différents
☐ Chaque erreur de test a généré une correction dans le Skill
```

---

## Résumé visuel des 5 phases

```
PHASE 0 → Choisir : Bottom-up ou Top-down ?
    ↓
PHASE 1 → Définir : Objectif, utilisateurs, déclencheurs, livrable
    ↓
PHASE 2 → Écrire le SKILL.md : 7 sections + YAML + Exemple
    ↓
PHASE 3 → Architecturer : SKILL.md = process / reste = ressources
    ↓
PHASE 4 → Prompter Claude avec la checklist de questions
    ↓
PHASE 5 → Tester → Diagnostiquer → Corriger → Itérer
```
