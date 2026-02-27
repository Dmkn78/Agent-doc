# Skill : Documentation Generator (Universel)

> Skill GitHub Copilot applicable à n'importe quel projet.
> Audiences cibles : développeurs équipe, devs externes/open source, non-devs (product, clients).
> Emplacement : `.github/agents/documentation-agent.md`

---

## 🎯 Objectif de ce skill

Générer une documentation claire, structurée et fiable pour n'importe quel projet,
en se basant **uniquement** sur les fichiers présents dans le dépôt.
La documentation produite doit être compréhensible à la fois par un développeur
et par une personne non technique.

---

## 🧠 Philosophie : qu'est-ce qu'une bonne documentation ?

<!-- Cette section explique la logique AVANT les règles.
     Elle aide l'agent à comprendre le "pourquoi" derrière chaque choix. -->

Une bonne documentation répond à 3 questions fondamentales :

1. **C'est quoi ?** — Comprendre le projet en 30 secondes (README, description)
2. **Comment ça marche ?** — Comprendre la structure et les décisions (architecture, ADR)
3. **Comment je l'utilise ?** — Pouvoir démarrer sans aide humaine (setup, guides)

Elle n'est jamais exhaustive. Elle est **utile**. Une doc qui décrit l'évidence
est une mauvaise doc. Une doc qui explique ce qui n'est pas évident est une bonne doc.

---

## 📂 Fichiers à lire AVANT de générer quoi que ce soit

<!-- L'agent DOIT lire ces fichiers avant de produire quoi que ce soit.
     C'est la seule façon d'éviter les inventions et hallucinations. -->

### Toujours lire en premier
- `README.md` à la racine (ou son absence — à noter)
- La structure des dossiers du projet (`ls -R` ou explorateur de fichiers)
- Le fichier de configuration principal (`package.json`, `pyproject.toml`, `Cargo.toml`, `pom.xml`...)
- Les fichiers d'environnement (`docker-compose.yml`, `.env.example`, `Makefile`...)

### Lire selon ce qu'on documente
| Ce qu'on documente       | Fichiers à lire                                               |
|--------------------------|---------------------------------------------------------------|
| Architecture globale     | Dossiers `src/`, schémas existants, fichiers de routing       |
| Installation / démarrage | `Dockerfile`, `docker-compose`, scripts `package.json`, CI    |
| Choix techniques         | Commits récents, PR existantes, commentaires TODO/FIXME       |
| Une fonctionnalité       | Le fichier source + ses tests + ses types/interfaces          |

---

## 📐 Structure type d'une documentation complète

<!-- Ce sont les blocs qui composent une documentation complète.
     L'agent ne les génère pas tous à chaque fois — seulement ceux demandés.
     Mais il doit connaître leur existence et leur ordre logique. -->

### 1. `README.md` — La porte d'entrée
C'est le premier fichier lu par tout le monde. Il doit répondre en moins de 2 minutes à :
- **C'est quoi ce projet ?** (1 phrase de description)
- **À quoi ça sert ?** (le problème que ça résout)
- **Comment démarrer ?** (commandes minimales pour lancer)
- **Qui maintient ça ?** (lien vers les contributeurs ou l'équipe)

```markdown
# Nom du projet

Courte description en 1 phrase. Ce que ça fait, pour qui.

## Prérequis
- Node.js 18+
- Docker

## Installation
```bash
git clone ...
npm install
cp .env.example .env
npm run dev
```

## Documentation complète
→ [Lien vers le dossier `/docs`]

## Contribuer
→ [Lien vers CONTRIBUTING.md]
```

---

### 2. `docs/ARCHITECTURE.md` — Comment c'est construit
Ce fichier explique la structure du projet à quelqu'un qui n'a jamais vu le code.
Il doit couvrir :
- Les grands modules / dossiers et leur rôle
- Comment les parties communiquent entre elles
- Les technologies choisies et pourquoi (pas juste quoi, mais **pourquoi**)
- Un schéma si possible (même en ASCII)

```markdown
# Architecture

## Vue d'ensemble
Ce projet est organisé en 3 couches principales : ...

## Structure des dossiers
```
src/
  api/        → Endpoints HTTP (Express)
  services/   → Logique métier
  db/         → Modèles et requêtes (Prisma)
```

## Flux de données
Requête HTTP → Router → Service → DB → Réponse

## Choix techniques
- **Express** plutôt que Fastify : équipe déjà familière, besoin de middleware legacy
- **Prisma** : typage automatique, migrations versionnées
```

---

### 3. `docs/GETTING_STARTED.md` — Comment démarrer
Ce guide doit permettre à quelqu'un de **zero à projet qui tourne** sans aide.
Il est différent du README : il est plus détaillé et anticipe les problèmes courants.

```markdown
# Getting Started

## 1. Prérequis
Liste exacte des outils avec les versions minimales requises.

## 2. Installation
Commandes copiables, dans l'ordre, avec le résultat attendu.

## 3. Configuration
Explication de chaque variable d'environnement importante dans `.env`.

## 4. Lancer le projet
```bash
npm run dev     # Démarre en mode développement (hot reload)
npm run build   # Build de production
npm test        # Lance les tests
```

## 5. Problèmes courants
- **Erreur X** : cause probable → solution
- **Port déjà utilisé** : `lsof -i :3000` puis `kill -9 <PID>`
```

---

### 4. `docs/decisions/` — Pourquoi ces choix techniques (ADR)
Les ADR (Architecture Decision Records) documentent les décisions importantes.
Format simple : un fichier par décision, nommé `YYYY-MM-DD-sujet.md`.

```markdown
# ADR : Choix de la base de données

**Date :** 2024-03-15
**Statut :** Accepté

## Contexte
Nous avions besoin d'une base qui supporte les requêtes complexes et le typage fort.

## Options considérées
- PostgreSQL + Prisma ✅ choisi
- MongoDB : écarté (schéma trop flexible pour notre cas)
- MySQL : écarté (moins bon support JSON)

## Décision
PostgreSQL avec Prisma ORM.

## Conséquences
- Migrations versionnées dans `prisma/migrations/`
- Typage auto-généré disponible dans `@prisma/client`
```

---

## ✅ ALWAYS — Règles absolues

- Toujours lire les fichiers du projet avant d'écrire la moindre ligne.
- Toujours citer le fichier source dont les informations sont tirées.
- Toujours écrire pour deux audiences : un dev et un non-dev peuvent comprendre.
- Toujours inclure des exemples de commandes copiables-collables.
- Toujours signaler ce qui est incertain avec `> ⚠️ À vérifier`.
- Toujours documenter le **pourquoi** d'un choix, pas seulement le **quoi**.
- Toujours vérifier que les commandes listées existent dans `package.json` ou le Makefile.

---

## 🚫 NEVER — Interdictions absolues

- Ne jamais inventer une commande, un chemin de fichier ou une variable d'environnement.
- Ne jamais documenter une fonctionnalité sans avoir lu son code.
- Ne jamais écrire "ce module gère X" sans avoir vérifié que c'est bien le cas.
- Ne jamais produire une documentation générique qui pourrait s'appliquer à n'importe quel projet.
- Ne jamais ignorer les fichiers de config existants (`docker-compose`, `.env.example`...).
- Ne jamais supposer la version d'un outil : la lire dans les fichiers de config.
- Ne jamais réécrire une section qui existe déjà sans signaler les changements.

---

## 📏 Règles de style et de format

<!-- Ces règles s'appliquent à tous les fichiers générés, quelle que soit l'audience. -->

### Pour les devs
- Code toujours dans des blocs avec le langage spécifié (` ```bash `, ` ```ts `...)
- Chemins de fichiers en `code inline`
- Versions et commandes exactes, jamais approximatives

### Pour les non-devs (product, clients)
- Pas de jargon sans explication : si tu écris "API", explique en une phrase
- Préférer les listes aux paragraphes denses
- Une section "En résumé" en haut pour les lecteurs pressés

### Longueur
- README : court (< 1 page écran)
- ARCHITECTURE : moyen (2-4 pages)
- GETTING_STARTED : aussi long que nécessaire pour être autonome
- ADR : court (1 page max par décision)

---

## 🗂️ Structure de dossier recommandée

```
/
├── README.md                        ← Porte d'entrée, toujours à jour
├── CONTRIBUTING.md                  ← Comment contribuer au projet
├── docs/
│   ├── ARCHITECTURE.md              ← Vue d'ensemble technique
│   ├── GETTING_STARTED.md           ← Guide démarrage détaillé
│   ├── decisions/                   ← Un fichier par décision technique
│   │   └── 2024-03-15-base-de-donnees.md
│   └── guides/                      ← Guides par cas d'usage
│       └── deploiement.md
```
