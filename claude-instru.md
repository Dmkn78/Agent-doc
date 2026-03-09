# Transcription mot pour mot — 15 images

---

## Image 1 (Capture_d_écran_2026-03-09_061309_-_Copie.png)

**5.1 — Deux façons de construire un Skill**

**Méthode 1 : Bottom-up**

**Faire la tâche une fois** avec l'agent, puis lui demander de la « sauver en Skill ». Rapide, concret, basé sur un cas réel. Idéal pour démarrer.

**Méthode 2 : Top-down**

**Concevoir directement** : nom, déclencheurs, process, fichiers, règles. Plus structuré, meilleur pour les workflows complexes ou les équipes.

---

## Image 2 (Capture_d_écran_2026-03-09_061402.png)

**MODULE 1 · SUITE**

**1.3 — Progressive Disclosu… : le super-pouvoir**

Le principe fondamental : **l'agent ne charge pas tout d'un coup**. Ce mécanisme en trois niveaux est ce qui rend les Skills performants à grande échelle, sans saturer la fenêtre de contexte.

**Niveau 1 · Métadonnées**

Nom + description du Skill uniquement. L'agent « voit » le Skill mais ne le charge pas encore.

**Niveau 2 · Corps du SKILL.md**

Chargé uniquement si le Skill est déclenché. Le process complet devient disponible.

**Niveau 3 · Ressources**

Chargées uniquement si `SKILL.md` le demande explicitement. Templates, références, activities…

*(Barre de progression vidéo visible — 10:24 / 18:22)*

**Conclusion pratique** : votre SKILL.md doit **forcer** explicitement l'usage des bons fichiers. Si vous ne le dites pas, l'agent les ignorera — même s'ils sont dans le dossier.

---

## Image 3 (Capture_d_écran_2026-03-09_061338.png)

**PROMPTER U… LL CLAUDE**
*(titre partiellement masqué)*

**Les questions essentielles po… t améliorer des Skills**
*(titre partiellement masqué)*

---

### Quelles questions se poser ?

→ Quand déclencher ce Skill ?
→ Qu'est-ce que ce Skill doit accomplir ?
→ Quels connecteurs utiliser ?
→ Quelles étapes suivre pas-à-pas ?

**Pour chaque étape :**
✓ Où mettre l'humain dans la boucle (HITL) ?
✓ Quelle référence utiliser ici ?
✓ Quel format final je veux ?

---

**Nom & Déclenchement**

Le nom du Skill doit être **Infographie Pro** et se déclencher à chaque fois qu'une personne souhaite générer une infographie.

**L'objectif**

→ Le Skill doit créer des infographies de qualité selon ma charte graphique pour Linkedin / newsletters.

**Connecteurs**

→ Ce Skill utilise le connecteur **Banana Orange MCP** spécifique.

**Processus pas-à-pas**

1. **Demander le texte source** : Utiliser l'interface de Q&A ✅
2. **Donner des suggestions de visualisation** : Lire la référence `<exemples.md>` ✅
   - Quelle référence utiliser ici ? ✅ 🐱
3. **Produire 5 versions à valider** : Suivre les valeurs et le style désirés ✅

↓

**Pour chaque Skill :**

Pense à reprendre et améliorer les règles dictées, les fichiers référence et l'exemple final au fur & à mesure des itérations.

*(Barre de progression vidéo — 9:26 / 18:22)*

---

## Image 4 (Capture_d_écran_2026-03-09_061309.png)

**5.1 — Deux façons de construire un Skill**

**Méthode 1 : Bottom-up**

**Faire la tâche une fois** avec l'agent, puis lui demander de la « sauver en Skill ». Rapide, concret, basé sur un cas réel. Idéal pour démarrer.

**Méthode 2 : Top-down**

**Concevoir directement** : nom, déclencheurs, process, fichiers, règles. Plus structuré, meilleur pour les workflows complexes ou les équipes.

---

## Image 5 (Capture_d_écran_2026-03-09_061611.png)

**ANATOMIE DU SKILL (Claude)**
*(titre partiellement masqué)*

Comment structurer un Skill pour gui… gent (Instructions + **Données** + Outils)

---

### 1. Sujet du Skill

**Nom**
((slug))

**Description**
- Quand l'uxiliser ?
- Objectif / Résultat

**Mots-clés**
déclencheurs

---

### 2. Prompt Optimisé (SKILL.md)

**Frontmatter (YAML)**

```
name: nom-du-skill
description: ...
version: ...
author: ...
```

**Instructions Core**
- Rôle & objectif
- Étapes / Workflow
- Contraintes & règles

**Déclenchement**
- Quand activer ?

---

### 3. Ressources (chargées à la demande)

**Références**
Docs, PDF, MD, JSON…

**Contexte & Exemples**
Guides, Cas, Style, Prompts

**Instructions MCP**
Intégrations / Outils

**Assets**
Images, Vidéos, Fichiers (mp4., mp3…)

**Scripts & Code**
.js, .py, API, Fonctions

---

**Bonnes Pratiques** —
✅ Clair & Modulaire &nbsp;&nbsp; ✅ Liens relatifs &nbsp;&nbsp; ✅ Exemples concrets &nbsp;&nbsp; • Scripts robustes &nbsp;&nbsp; ✅ Sécurité & Tests

---

## Image 6 (Capture_d_écran_2026-03-09_062003.png)

Le SKILL.md orchestre : les ressources alimentent. Gardez cette discipline et vos Skills resteront maintenables même après des dizaines d'itérations.

```
compte-rendu-reunion/
└── skill.md   # (ou SKILL.md pour la spec)

triage-support/
├── SKILL.md
├── activities/
│   ├── activity-intake.md
│   ├── activity-investigation.md
│   └── activity-escalation.md
├── templates/
│   ├── template-reponse-client.md
│   └── template-note-interne.md
```

---

## Image 7 (Capture_d_écran_2026-03-09_061947.png)

**MODULE 3 · SUITE**

**3.3 — L'astuce structurante … er process et contexte**
*(titre partiellement masqué)*

C'est un principe simple mais que presque personne n'applique correctement. La séparation des responsabilités est la clé d'un Skill maintenable et performant.

---

**SKILL.md = le process**

Propre, lisible, focalisé. Il décrit **quoi faire** et **dans quel ordre**. Aucune donnée métier, aucun exemple long, aucune règle détaillée ici.

Si votre SKILL.md fait plus de 200 lignes, c'est un signal d'alarme 🚨

---

**Le reste = les ressources**

`references/` porte le contexte métier, les règles, les données de vérité.
`templates/` porte la qualité de sortie. `activities/` porte l'UX.

Cette séparation permet de mettre à jour le contexte **sans toucher au process** — et inversement.

---

*(Diagramme en bas de slide — partiellement visible)*

**Scripts**
Commandes et automations

**Processus central**
Étapes et déclencheurs

**Activités**
Tâches à exécuter

**SKILL.md → Ressources**

**Context séparé**
Conserver contexte hors process

*(Barre de progression vidéo — 15:28 / 18:22)*

---

## Image 8 (Capture_d_écran_2026-03-09_062023.png)

```
### Output (extrait)
**Décisions**
- Lancement V2 planifié lundi 09:00.

**Actions**
| Action | Responsable | Échéance | Statut |
```

*(Barre de progression vidéo — 15:27 / 18:22)*

---

## Image 9 (Capture_d_écran_2026-03-09_061918.png)

```
- Notes/transcription (même désordonnée)
- (Optionnel) Contexte : nom du projet, date, participants

## Procédure (étapes)
1. **Normaliser** : identifier la date, les participants,
2. **Extraire** :
   - Décisions (phrases actées, arbitrages)
   - Actions (verbe + responsable + échéance)
   - Points ouverts / risques
3. **Dédupliquer** les actions similaires et clarifier si ambigu.
4. **Rendre** un CR au format ci-dessous.

## Format de sortie (obligatoire)
- **Contexte** : date, participants, objectif
- **Décisions**
- **Actions** (table : Action | Responsable | Échéance | Statut)
- **Points ouverts / questions**
- **Risques**
- **Annexes** (extraits utiles)

## Exemple
### Input (résumé)
"On lance la V2 lundi. Paul fait la checklist. Marie contacte le client d'ici mercredi…"

### Output (extrait)
**Décisions**
- Lancement V2 planifié lundi 09:00.

**Actions**
```

---

## Image 10 (Capture_d_écran_2026-03-09_061859.png)

```
---
name: compte-rendu-reunion
description: Transforme notes/transcriptions de réunion en compte rendu structuré (décisions, actions, échéances). À utiliser pour réunions, calls, ateliers.
metadata:
  author: team-ops
  version: "1.0"
---

# Compte rendu de réunion (CR)

## Quand utiliser ce skill
Utilise ce skill si l'utilisateur fournit :
- une transcription, des notes brutes, un export de chat
- ou demande "compte rendu", "minutes", "actions", "todo réunion"

## Entrées attendues
- Notes/transcription (même désordonnée)
- (Optionnel) Contexte : nom du projet, date, participants

## Procédure (étapes)
1. **Normaliser** : Identifier la date, les participants, l'objectif.
```

*(Barre de progression vidéo — 15:08 / 18:22)*

---

## Image 11 (Capture_d_écran_2026-03-09_061836.png)

**MODULE 3 · SUITE**

**3.2 — Le corps du SKILL.md… + UX human-in-the-loop**
*(titre partiellement masqué)*

Le corps de votre `SKILL.md` est la véritable SOP de l'agent. Il doit couvrir **sept sections essentielles** pour garantir une exécution fiable et prévisible :

**1 — Quand utiliser**
Les déclencheurs précis : quels mots, quelles situations activent ce Skill ?

**2 — Entrées attendues**
Ce que l'agent doit recevoir (ou demander) pour commencer l'exécution.

**3 — Workflow étape par étape**
La séquence d'actions, avec les points de validation humaine clairement marqués.

**4 — Format de sortie**
La structure exacte du livrable : sections, longueur, ton, variables.

**5 — Edge cases**
Les situations limites et comment les traiter — c'est ce qui distingue un Skill robuste.

**6 — Règles**
Ce qui est interdit, ce qui est obligatoire. Les garde-fous non négociables.

**7 — Chargement de fichiers**
*(suite tronquée hors champ)*

---

## Image 12 (Capture_d_écran_2026-03-09_061825.png)

**Tester… érer**
*(titre partiellement masqué)*

| Problème identifié | Correction |
|---|---|
| Ne suit pas le processus | → Mettre à jour SKILL.md |
| Résultats de mauvaise qualité | → Ajouter infos ou exemples |
| Commet une erreur | → Ajouter une règle |
| Utilise mal outil / MCP | → Créer fichier de référence MCP |

**Si Skill imparfait → Ajouter ce qui manque**

---

## Image 13 (Capture_d_écran_2026-03-09_061756.png)

**Comment structu… LL (Agent / Claude) ?**
*(titre partiellement masqué)*

1) Identifier le sujet → 2) Créer un promp… → 3) Architecture du Skill (infographie)

---

### 1. Identifier le Sujet du Skill

**Définir clairement :**

🎯 **Objectif principal**
Que fait le skill ?

👥 **Utilisateurs cibles**
Pour qui ?

💡 **Cas d'usage / Déclencheurs**
Quand l'utiliser ?

📄 **Livrables attendus**
Quel résultat ?

---

### 2. Créer un Prompt Optimisé

**Structure du prompt :**

🔵 **Rôle**
Tu es un expert en …

🟣 **Contexte**
Objectif + Contraintes

🟠 **Étapes**
Procédure à suivre

🟡 **Ressources**
Fichiers / Références

🟢 **Format de sortie**
Livrable attendu

📘 **Exemples** (optionnel)
Cas concrets

💡 Conseil : Sois clair, précis et modulaire
→ l'agent charge ce dont il a besoin !

---

### 3. Anatomie d'un Skill (Structure de fichiers)

**SKILL.md**
Instructions principales (Core Instruction)

**References/**
→ Docs, PDF, MD, JSON…
→ Docs de contexte, exemples, guides
→ Consignes de style, bonnes pratiques
MCP Instructions MCP

**Assets/**
→ Images, vidéos, binaires…
→ Médias (.mp4, .mp3…)
→ Exemples visuels, présentations

**Scripts/**
→ Code (.js, .py, etc.)
→ Exécutions, API, fonctions

**Templates/**
→ Modèles de sortie ( emails, docs, rapports…)

---

**Résultat :**
✅ Skill portable
✅ Réutilisable
✅ Testable & évolutif

---

## Image 14 (Capture_d_écran_2026-03-09_061711.png)

**MODULE 2**

**Architecture « 99 % » : struc… répertoires**
*(titre partiellement masqué)*

**2.1 — Structure minimale vs recommandée**

Vous pouvez commencer avec un simple dossier et un `SKILL.md`. Mais pour atteindre le niveau « 99 % », la structure cloud-style apporte modularité, maintenabilité et qualité de sortie supérieure.

---

**`references/`**
Docs, guides, exemples, règles de style, ICP. La **source de vérité** qui réduit les hallucinations.

**`templates/`**
Modèles de sortie : emails, comptes-rendus, rapports. Garantit la **cohérence** du format.

**`activities/`**
Sous-procédures et checklists. Idéal pour le **human-in-the-loop** : l'agent sait quoi demander.

**`assets/`**
Images, JSON, exemples visuels. Tout ce qui n'est pas du texte mais enrichit le contexte.

**`scripts/`**
Code exécutable, appels API, normalisation de données. Les **actions** du Skill.

*(Barre de progression — / 18:22)*

Exercice (15 min) — Pour votre tâche identifiée au module 1, choisissez **au moins** : 1 fichier `templates/`, 1 fichier `references/`, et 1 fichier `activities/` si le workflow est multi-étapes.

---

## Image 15 (Capture_d_écran_2026-03-09_062023.png — doublon probable)

**La méthode « build & iterat… e que 99 % ratent**
*(titre partiellement masqué)*

---

**TESTER & AMÉLIORER UN SKILL**
Identifier les erreurs et appliquer les corrections

| Problèmes identifiés | Corrections à apporter |
|---|---|
| 🔧 Ne suit pas le process | ✏️ Mettre à jour le Skill.md |
| 🚫 Mauvaises sorties | 📦 Ajouter infos ou exemples |
| ✏️ Fait quelque chose de mal | ⚠️ Ajouter une règle |
| 🔧 Utilise mal un outil / MCP | 🔧 Créer fichier référence MCP |
| 🔧 Utilise mal un outil / MCP | ✏️ Créer fichier référence MCP |

**Chaque erreur est une occasion d'améliorer votre Skill et de le rendre plus robuste.**
