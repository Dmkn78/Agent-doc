Bonne question. On repart de zéro, mode débutant complet.

---

## La vraie première question à se poser

Avant d'écrire une seule ligne de code : **est-ce que tu as bien compris ce que l'utilisateur veut ?**

La réponse est presque toujours : **non, pas encore.**

Pas parce que tu es mauvais. Parce que l'utilisateur lui-même ne sait pas toujours exactement ce qu'il veut. Il a une douleur, une frustration, une idée vague. Ton boulot c'est de transformer ça en quelque chose de précis.

---

## Les 4 étapes dans l'ordre

```
1. COMPRENDRE        → Qu'est-ce qu'on veut vraiment ?
2. DÉCOUPER          → Ça se fait en combien de morceaux ?
3. ESTIMER           → Combien de temps chaque morceau ?
4. CODER             → Seulement maintenant on touche au clavier
```

La plupart des débutants sautent directement à l'étape 4. C'est là que tout dérape.

---

## Étape 1 — Comprendre (1 à 2 jours)

Tu poses des questions. Beaucoup. Voici les questions à poser pour ce projet :

**Sur le problème :**
- Aujourd'hui, comment vous créez vos Epics ? Combien de temps ça prend ?
- Qu'est-ce qui vous énerve dans ce processus ?
- C'est qui qui va utiliser cet outil ? Des devs ? Des POs ?

**Sur la solution :**
- Les templates de tâches, c'est vous qui les définissez ou l'outil doit les suggérer ?
- Vous êtes sur Jira Cloud ou Jira Server ?
- L'outil doit être accessible depuis n'importe où ou juste en interne ?

**Sur les contraintes :**
- Vous avez déjà un serveur pour héberger ça ?
- Il y a une deadline ?
- Combien de personnes vont utiliser ça ?

À la fin de cette étape tu écris **une page** qui résume ce que tu as compris. Tu la montres à l'utilisateur. Il te dit si c'est bon ou pas.

---

## Étape 2 — Découper (quelques heures)

Tu prends tout ce que tu veux faire et tu le coupes en petits morceaux indépendants. On appelle ça des **User Stories**.

Une User Story ça s'écrit toujours comme ça :

> En tant que **[qui]**, je veux **[quoi]** pour **[pourquoi]**.

Pour ce projet ça donne par exemple :

```
Story 1 : En tant que PO, je veux créer un Epic avec un nom
          pour qu'il apparaisse dans Jira.

Story 2 : En tant que PO, je veux choisir un template
          pour que les sous-tâches soient créées automatiquement.

Story 3 : En tant que PO, je veux ajouter des labels
          pour que toutes les tâches soient bien catégorisées.

Story 4 : En tant que dev, je veux que le token Jira
          soit sécurisé pour ne pas l'exposer dans le code.
```

Tu continues jusqu'à ce que tu aies tout découpé. Chaque story doit être **testable** — tu dois pouvoir dire "c'est fait" ou "c'est pas fait".

---

## Étape 3 — Estimer (quelques heures)

Pour chaque story, tu estimes en **jours** (pas en heures, c'est trop précis pour un débutant).

| Story | Estimation |
|---|---|
| Créer un Epic via l'API Jira | 2 jours |
| Créer les sous-tâches liées | 1 jour |
| Interface formulaire HTML | 2 jours |
| Gestion des templates | 1 jour |
| Sécuriser le token | 0.5 jour |
| Tests + débogage | 2 jours |
| Déploiement serveur | 1 jour |
| **Total** | **~10 jours** |

Ensuite tu **multiplies par 1.5 ou 2**. Toujours. Parce que :
- Tu vas bloquer sur des trucs imprévus
- Il y aura des aller-retours avec l'utilisateur
- Les débutants sous-estiment systématiquement

Donc 10 jours estimés → tu annonces **3 semaines**.

---

## L'ordre dans lequel tu codes

Tu ne codes pas tout en même temps. Tu construis **par couches** :

```
Semaine 1 : Le squelette qui marche
            → Appeler l'API Jira et créer UN ticket (rien d'autre)
            → Pas d'interface, juste du code qui tourne

Semaine 2 : La logique métier
            → Ajouter les templates
            → Créer l'Epic + les sous-tâches liées

Semaine 3 : L'interface + les finitions
            → Le formulaire HTML
            → La gestion des erreurs
            → Le déploiement
```

À la fin de la semaine 1, tu as quelque chose qui marche. C'est moche, c'est basique, mais ça marche. **C'est ça le plus important.**

---

## Pour ce projet précis, par où commencer demain matin

1. Installe Java 17 et Maven sur ta machine
2. Va sur Jira, génère un token API
3. Ouvre Postman (outil gratuit) et essaie de créer un ticket à la main sans coder, juste avec l'interface Postman
4. Si ça marche → tu sais que ton token et ta config sont bons
5. Seulement après → tu reproduis ça en Java

L'idée c'est de **valider chaque hypothèse avant de coder**. Est-ce que l'API Jira répond ? Est-ce que mon token est bon ? Est-ce que le type "Epic" existe dans mon projet ?

---

C'est quoi ton niveau actuel en Java ? Et tu as déjà utilisé Postman ou une API REST ?q
