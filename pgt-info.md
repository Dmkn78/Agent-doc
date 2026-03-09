## Image 1 — temps non visible

### Texte lu

**5.1 — Deux façons de construire un Skill**

#### Bloc gauche
**Méthode 1 : Bottom-up**

**Faire la tâche une fois** avec l’agent, puis lui demander de la « sauver en Skill ». Rapide, concret, basé sur un cas réel. Idéal pour démarrer.

#### Bloc droit
**Méthode 2 : Top-down**

**Concevoir directement** : nom, déclencheurs, process, fichiers, règles. Plus structuré, meilleur pour les workflows complexes ou les équipes.

### Vérification
Je ne vois pas de temps affiché sur cette capture.  
Le texte est lisible et semble complet.

---

## Image 2 — 9:26 / 18:22

### Texte lu

**PROMPTER UN SKILL CLAUDE**

**Les questions essentielles pour créer et améliorer des Skills**

#### Colonne gauche
**Quelles questions se poser ?**

- Quand déclencher ce Skill ?
- Qu’est-ce que ce Skill doit accomplir ?
- Quels connecteurs utiliser ?
- Quelles étapes suivre pas-à-pas ?

#### Encadré bas gauche
**Pour chaque étape :**

- Où mettre l’humain dans la boucle (HITL) ?
- Quelle référence utiliser ici ?
- Quel format final je veux ?

#### Colonne droite
**Nom & Déclenchement**  
Le nom du Skill doit être **Infographie Pro** et se déclencher à chaque fois qu’une personne souhaite générer une infographie.

**L’objectif**  
Le Skill doit créer des infographies de qualité selon ma charte graphique pour LinkedIn / newsletters.

**Connecteurs**  
Ce Skill utilise le connecteur **Banana Orange MCP** spécifique.

**Processus pas-à-pas**

1. Demander le texte source : Utiliser l’interface de Q&A ✅
2. Donner des suggestions de visualisation : Lire la référence « exemples.md » ✅
   - Quelle référence utiliser ici ? ✅
3. Produire 5 versions à valider : Suivre les valeurs et le style désirés ✅

#### Bas de slide visible partiellement
**Pour chaque Skill :**  
Pense à reprendre et améliorer les règles dictées, les fichiers référence et l’exemple final au fur & à mesure des itérations.

### Vérification
Deux points sont un peu ambigus visuellement :

- la ligne sous le point 2 ressemble à **« Quelle référence utiliser ici ? »**, mais la définition de l’image n’est pas parfaite ;
- le bas de la slide est partiellement masqué par la barre vidéo, mais la phrase reste globalement lisible.

---

## Image 3 — 10:24 / 18:22

### Texte lu

**MODULE 1 · SUITE**

**1.3 — Progressive Disclosure, le super-pouvoir**

Le principe fondamental : **l’agent ne charge pas tout d’un coup.** Ce mécanisme en trois niveaux est ce qui rend les Skills performants à grande échelle, sans saturer la fenêtre de contexte.

#### Niveau 1
**Niveau 1 · Métadonnées**  
Nom + description du Skill uniquement. L’agent « voit » le Skill mais ne le charge pas encore.

#### Niveau 2
**Niveau 2 · Corps du SKILL.md**  
Chargé uniquement si le Skill est déclenché. Le process complet devient disponible.

#### Niveau 3
**Niveau 3 · Ressources**  
Chargées uniquement si **SKILL.md** le demande explicitement. Templates, références, activities…

#### Bandeau du bas visible partiellement
**Conséquence pratique :** votre **SKILL.md** doit forcer explicitement l’usage des bons fichiers. Si vous ne le dites pas, l’ag…

### Vérification
La dernière phrase est **tronquée** sur la droite et en bas.  
On lit clairement jusqu’à **« Si vous ne le dites pas, l’ag… »** mais pas la suite complète.

---

## Image 4 — temps non visible

### Texte lu

**ANATOMIE D’UN SKILL (Claude)**

**Comment structurer un Skill pour guider l’agent (Instructions + Données + Outils)**

### 1. Sujet du Skill

**Nom**  
`((slug))`

**Description**

- Quand l’utiliser ?
- Objectif / Résultat

**Mots-clés**  
Déclencheurs

### 2. Prompt Optimisé (SKILL.md)

#### Frontmatter (YAML)

```yaml
name: nom-du-skill
description: ...
version: ...
author: ...
```

#### Instructions Core

- Rôle & objectif
- Étapes / Workflow
- Contraintes & règles

#### Déclenchement

- Quand activer ?

### 3. Ressources (chargées à la demande)

**Références**  
Docs, PDF, MD, JSON...

**Contexte & Exemples**  
Guides, Cas, Style, Prompts

**Instructions MCP**  
Intégrations / Outils

**Assets**  
Images, Vidéos, Fichiers  
(mp4., mp3...)

**Scripts & Code**  
.js, .py, API, Fonctions

### Bandeau bas
**Bonnes Pratiques**

- Clair & Modulaire
- Liens relatifs
- Exemples concrets
- Scripts robustes
- Sécurité & Tests

### Vérification
Le texte est très lisible.  
Je note seulement que **« mp4., mp3... »** apparaît tel quel à l’écran, même si la ponctuation semble étrange.

---

## Image 5 — temps non visible

### Texte lu

**MODULE 2**

**Architecture « 99 % » : structure répertoires**  
**2.1 — Structure minimale vs recommandée**

Vous pouvez commencer avec un simple dossier et un **SKILL.md**. Mais pour atteindre le niveau « 99 % », la structure cloud-style apporte modularité, maintenabilité et qualité de sortie supérieure.

### Liste des dossiers

**references/**  
Docs, guides, exemples, règles de style, ICP. **La source de vérité** qui réduit les hallucinations.

**templates/**  
Modèles de sortie : emails, comptes-rendus, rapports. Garantit la cohérence du format.

**activities/**  
Sous-procédures et checklists. Idéal pour le **human-in-the-loop** : l’agent sait quoi demander.

**assets/**  
Images, JSON, exemples visuels. Tout ce qui n’est pas du texte mais enrichit le contexte.

**scripts/**  
Code exécutable, appels API, normalisation de données. **Les actions du Skill.**

#### Bandeau bas visible partiellement
**Exercice (15 min)** — Pour votre tâche identifiée au module 1, choisissez au moins : **1 fichier templates/**, **1 fichier references/**, et **1 fichier** …

### Vérification
La fin de la phrase d’exercice est **coupée**.  
Tout le reste est lisible.

---

## Image 6 — temps non visible

### Texte lu

**Comment structurer un SKILL (Agent / Claude) ?**

**1) Identifier le sujet → 2) Créer un prompt → 3) Architecture du Skill (infographie)**

### 1. Identifier le Sujet du Skill

**Définir clairement :**

**Objectif principal**  
Que fait le skill ?

**Utilisateurs cibles**  
Pour qui ?

**Cas d’usage / Déclencheurs**  
Quand l’utiliser ?

**Livrables attendus**  
Quel résultat ?

### 2. Créer un Prompt Optimisé

**Structure du prompt :**

**Rôle**  
Tu es un expert en …

**Contexte**  
Objectif + Contraintes

**Étapes**  
Procédure à suivre

**Ressources**  
Fichiers / Références

**Format de sortie**  
Livrable attendu

**Exemples (optionnel)**  
Cas concrets

**Conseil : Sois clair, précis et modulaire**  
→ l’agent charge ce dont il a besoin !

### 3. Anatomie d’un Skill (Structure de fichiers)

**SKILL.md**  
Instructions principales  
(Core Instruction)

**References/**

- Docs, PDF, MD, JSON...
- Docs de contexte, exemples, guides
- Consignes de style, bonnes pratiques
- MCP Instructions MCP

**Assets/**

- Images, vidéos, binaires...
- Médias (.mp4, .mp3...)
- Exemples visuels, présentations

**Scripts/**

- Code (.js, .py, etc.)
- Exécutions, API, fonctions

**Templates/**

- Modèles de sortie ( emails, docs, rapports...)

#### Encadré résultat
**Résultat :**

- Skill portable
- Réutilisable
- Testable & évolutif

### Vérification
Le texte est globalement très lisible.  
Je note juste que la ligne **« MCP Instructions MCP »** apparaît telle quelle, même si elle semble redondante.

---

## Image 7 — temps non visible

### Texte lu

**Tester / itérer**

#### Colonne gauche → colonne droite

- **Ne suit pas le processus** → **Mettre à jour SKILL.md**
- **Résultats de mauvaise qualité** → **Ajouter infos ou exemples**
- **Commet une erreur** → **Ajouter une règle**
- **Utilise mal outil / MCP** → **Créer fichier de référence MCP**

#### Encadré bas
**Si Skill imparfait → Ajouter ce qui manque**

### Vérification
Texte court, lisible, complet.

---

## Image 8 — temps non visible

### Texte lu

**MODULE 3 · SUITE**

**3.2 — Le corps du SKILL.md + UX human-in-the-loop**

Le corps de votre **SKILL.md** est la véritable SOP de l’agent. Il doit couvrir sept sections essentielles pour garantir une exécution fiable et prévisible :

1. **Quand utiliser**  
   Les déclencheurs précis : quels mots, quelles situations activent ce Skill ?

2. **Entrées attendues**  
   Ce que l’agent doit recevoir (ou demander) pour commencer l’exécution.

3. **Workflow étape par étape**  
   La séquence d’actions, avec les points de validation humaine clairement marqués.

4. **Format de sortie**  
   La structure exacte du livrable : sections, longueur, ton, variables.

5. **Edge cases**  
   Les situations limites et comment les traiter — c’est ce qui distingue un Skill robuste.

6. **Règles**  
   Ce qui est interdit, ce qui est obligatoire. Les garde-fous non négociables.

7. **Chargement de fichiers**

### Vérification
Le point 7 est **coupé** : seul le titre **« Chargement de fichiers »** est visible, pas son explication.

---

## Image 9 — 15:08 / 18:22

### Texte lu

```yaml
name: compte-rendu-reunion
description: Transforme notes/transcriptions de réunion en compte rendu structuré (décisions, actions, échéances). À utiliser pour réunions, calls, ateliers.
metadata:
  author: team-ops
  version: "1.0"
---
```

# Compte rendu de réunion (CR)

## Quand utiliser ce skill
Utilise ce skill si l’utilisateur fournit :

- une transcription, des notes brutes, un export de chat
- ou demande “compte rendu”, “minutes”, “actions”, “todo réunion”

## Entrées attendues

- Notes/transcription (même désordonnée)
- (Optionnel) Contexte : nom du projet, date, participants

## Procédure (étapes)

### Vérification
Le bloc est lisible.  
La partie suivante du document continue sur l’image suivante.

---

## Image 10 — temps non visible

### Texte lu

## Procédure (étapes)

1. **Normaliser** : identifier la date, les participants, l’objectif.
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
“On lance la V2 lundi. Paul fait la checklist. Marie contacte le client d’ici mercredi...”

### Output (extrait)

**Décisions**

- Lancement V2 planifié lundi 09:00.

**Actions**

### Vérification
Texte bien lisible.  
La table après **Actions** continue sur l’image suivante.

---

## Image 11 — 15:27 / 18:22

### Texte lu

### Output (extrait)

**Décisions**

- Lancement V2 planifié lundi 09:00.

**Actions**

| Action | Responsable | Échéance | Statut |
|---|---|---|---|

### Vérification
Le reste de la table est **coupé**.  
Seule l’en-tête du tableau est visible.

---

## Image 12 — 15:28 / 18:22

### Texte lu

**MODULE 3 · SUITE**

**3.3 — L’astuce structurante : séparer process et contexte**

C’est un principe simple mais que presque personne n’applique correctement. La séparation des responsabilités est la clé d’un Skill maintenable et performant.

#### Bloc gauche
**SKILL.md = le process**

Propre, lisible, focalisé. Il décrit **quoi faire et dans quel ordre**. Aucune donnée métier, aucun exemple long, aucune règle détaillée ici.

Si votre **SKILL.md** fait plus de 200 lignes, c’est un signal d’alarme 🚨

#### Bloc droit
**Le reste = les ressources**

**references/** porte le contexte métier, les règles, les données de vérité.  
**templates/** porte la qualité de sortie. **activities/** porte l’UX.

Cette séparation permet de mettre à jour le contexte **sans toucher au process** — et inversement.

#### Blocs du schéma visibles

**Scripts**  
Commandes et automatisations

**Processus central**  
Étapes et déclencheurs

**Activités**  
Tâches à exécuter

**Contexte séparé**  
Conserver le contexte hors process

#### Centre du schéma
**SKILL.md → Ressources**

### Vérification
Certains blocs du schéma sont partiellement rognés, mais les textes visibles ci-dessus sont lisibles.

---

## Image 13 — temps non visible

### Texte lu

Ligne d’introduction visible en haut :  
**Le SKILL.md orchestre : les ressources alimentent. Gardez cette discipline et vos Skills resteront maintenables même après des dizaines d’itérations.**

#### Arborescence visible

```text
compte-rendu-reunion/
└── skill.md    # (ou SKILL.md pour la spec)

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

### Vérification
La partie basse de l’arborescence est **coupée**.  
Il peut manquer d’autres dossiers ou fichiers sous `triage-support/`.

---

## Contrôle global

### Certain

- J’ai retranscrit tout ce qui est **lisible** sur les 13 captures.
- J’ai gardé les termes affichés, y compris quand ils semblent un peu inhabituels.
- J’ai indiqué les zones **tronquées**, **partiellement masquées** ou **ambiguës**.

### Limites liées aux images

- Certaines phrases du bas des slides sont coupées par la barre vidéo.
- Quelques zones sont masquées par la vignette du présentateur.
- Certaines arborescences ou tableaux continuent hors cadre.
