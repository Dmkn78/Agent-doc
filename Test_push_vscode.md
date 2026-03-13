# Documentation Technique — Jira Epic Creator

> **Version** 1.0.0 · **Stack** Java 17 · Spring Boot 3.2 · HTML/CSS/JS · Jira REST API v3 · **Mars 2026**

---

## Table des matières

1. [Vue d'ensemble du projet](#1-vue-densemble-du-projet)
2. [Configuration — pom.xml & application.properties](#2-configuration--pomxml--applicationproperties)
3. [Backend Java — Analyse fichier par fichier](#3-backend-java--analyse-fichier-par-fichier)
4. [Frontend — index.html](#4-frontend--indexhtml)
5. [Guide de démarrage](#5-guide-de-démarrage)
6. [Pistes d'évolution](#6-pistes-dévolution)

---

## 1. Vue d'ensemble du projet

### 1.1 Objectif

Jira Epic Creator est une application web qui automatise la création d'Epics dans Jira et génère automatiquement un ensemble de sous-tâches pré-configurées via des templates.

L'objectif : éliminer la saisie manuelle répétitive. Un développeur remplit un formulaire, clique sur *Créer*, et l'application crée l'Epic + toutes ses tâches associées dans Jira en quelques secondes.

### 1.2 Architecture générale

Le projet suit une architecture client-serveur classique en **3 couches** :

| Couche | Technologie | Rôle |
|---|---|---|
| **Présentation (Frontend)** | HTML / CSS / JS | Page statique, formulaire utilisateur |
| **Métier (Backend Java)** | Spring Boot 3.2 | API REST, orchestration de la logique |
| **Externe (API Jira)** | Atlassian REST API v3 | Création physique des tickets |

```
┌─────────────────────────────────────────────────────────┐
│                      NAVIGATEUR                         │
│   index.html  ←→  JavaScript (fetch API)                │
└───────────────────────┬─────────────────────────────────┘
                        │  HTTP POST /api/epic
                        │  { epicName, template, labels }
                        ▼
┌─────────────────────────────────────────────────────────┐
│              SERVEUR JAVA (Spring Boot)                  │
│                                                         │
│  EpicController → JiraService → TaskTemplate            │
│  (reçoit HTTP)    (logique)     (templates)             │
└───────────────────────┬─────────────────────────────────┘
                        │  POST /rest/api/3/issue (×N)
                        │  Authorization: Basic <token>
                        ▼
┌─────────────────────────────────────────────────────────┐
│                  API JIRA (Atlassian)                    │
│  Crée l'Epic → récupère son ID → crée N tâches liées    │
└─────────────────────────────────────────────────────────┘
```

### 1.3 Structure des fichiers

```
jira-epic-creator/
├── frontend/
│   └── index.html                     # Interface utilisateur complète
│
└── backend/
    ├── pom.xml                        # Configuration Maven & dépendances
    └── src/main/
        ├── resources/
        │   └── application.properties # Configuration & credentials Jira
        └── java/com/epic/
            ├── EpicCreatorApplication.java   # Point d'entrée Spring Boot
            ├── controller/
            │   └── EpicController.java       # Endpoints HTTP REST
            ├── service/
            │   └── JiraService.java          # Logique métier & appels Jira
            └── model/
                ├── EpicRequest.java          # DTO de la requête entrante
                └── TaskTemplate.java         # Templates de tâches
```

### 1.4 Flux de données complet

Voici le flux exact d'un clic utilisateur jusqu'à la création dans Jira :

1. L'utilisateur remplit le formulaire et clique **Créer**.
2. Le JavaScript appelle `fetch('http://localhost:8080/api/epic', { method: 'POST', body: JSON })`.
3. `EpicController` reçoit la requête sur `POST /api/epic`. Spring désérialise automatiquement le JSON en objet `EpicRequest` via Jackson.
4. Le controller délègue à `JiraService`, qui construit un payload JSON et envoie un `POST /rest/api/3/issue` à Jira avec l'authentification Basic Auth.
5. Jira répond HTTP **201** avec `{"id": "10042", "key": "DEV-1"}`. Le service extrait l'ID.
6. Le service récupère la liste de tâches dans `TaskTemplate`. Pour chaque tâche, il envoie un nouveau `POST` avec le champ `"parent": {"id": "10042"}` — c'est ce champ qui crée le lien hiérarchique.
7. Une fois tous les tickets créés, le service retourne un objet résultat au controller, qui le sérialise en JSON et répond HTTP 200 au frontend.
8. Le JavaScript affiche le message de succès.

---

## 2. Configuration — pom.xml & application.properties

### 2.1 pom.xml — Le manifeste du projet Java

Le fichier `pom.xml` (Project Object Model) est le fichier de configuration de Maven, l'outil de build Java. Il joue un rôle équivalent à un `package.json` en Node.js.

#### Identité du projet

```xml
<groupId>com.epic</groupId>
<artifactId>jira-epic-creator</artifactId>
<version>1.0.0</version>
<packaging>jar</packaging>
```

- **`groupId`** : l'organisation propriétaire, en nom de domaine inversé. Ici `com.epic`.
- **`artifactId`** : Maven génère `jira-epic-creator-1.0.0.jar` lors de la compilation.
- **`packaging: jar`** : le projet se compile en un JAR exécutable autonome contenant le serveur Tomcat embarqué.

#### Héritage Spring Boot

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.2.0</version>
</parent>
```

Déclarer Spring Boot comme parent active l'héritage de plusieurs centaines de configurations par défaut : versions des dépendances pré-alignées, plugins Maven pré-configurés, propriétés de compilation. Sans ce bloc, il faudrait déclarer et aligner manuellement les versions de chaque dépendance Spring.

#### Dépendances

| Dépendance | Scope | Rôle |
|---|---|---|
| `spring-boot-starter-web` | compile | Embarque Tomcat + Spring MVC. Permet `@RestController` et la sérialisation JSON via Jackson. |
| `lombok` | optional | Génère getters/setters/constructeurs à la compilation via annotations. |
| `spring-boot-starter-test` | test | JUnit 5 + Mockito + Spring Test. Absent du JAR de production. |

### 2.2 application.properties — La configuration du serveur

Spring Boot charge ce fichier automatiquement au démarrage depuis `src/main/resources/`. Il sépare la configuration du code : les credentials ne sont **jamais** écrits dans le code source.

```properties
# Port d'écoute du serveur Tomcat embarqué
server.port=8080

# URL racine de l'instance Jira
jira.base-url=https://TON-PROJET.atlassian.net

# Email du compte Jira (pour Basic Auth)
jira.email=ton.email@exemple.com

# Token API Atlassian (PAS le mot de passe)
# Générer sur : https://id.atlassian.com/manage-profile/security/api-tokens
jira.token=TON_TOKEN_JIRA_ICI

# Clé courte du projet Jira (visible dans l'URL des tickets : DEV-123)
jira.project-key=DEV

# Origines autorisées à appeler le serveur (CORS)
# En production : remplacer * par l'URL exacte du frontend
cors.allowed-origins=*
```

| Propriété | Valeur défaut | Description |
|---|---|---|
| `server.port` | `8080` | Port d'écoute. Accessible sur `http://localhost:8080` en local. |
| `jira.base-url` | *(à remplir)* | URL racine Jira. Utilisée par `JiraService` pour construire les URLs d'appel. |
| `jira.email` | *(à remplir)* | Email du compte Jira. Combiné avec `jira.token` pour l'auth Basic. |
| `jira.token` | *(à remplir)* | Token API Atlassian. À générer sur le portail Atlassian. |
| `jira.project-key` | `DEV` | Clé du projet Jira. Détermine où les tickets sont créés. |
| `cors.allowed-origins` | `*` | En production, remplacer par l'URL précise du frontend. |

> ⚠️ **Sécurité** — Ne jamais committer `application.properties` contenant un vrai token dans Git. Ajouter ce fichier dans `.gitignore`. En production, passer les valeurs via des variables d'environnement : `JIRA_TOKEN=xxx java -jar app.jar`

---

## 3. Backend Java — Analyse fichier par fichier

### 3.1 EpicCreatorApplication.java — Point d'entrée

```java
@SpringBootApplication
public class EpicCreatorApplication {
    public static void main(String[] args) {
        SpringApplication.run(EpicCreatorApplication.class, args);
        System.out.println("✅ Serveur démarré sur http://localhost:8080");
    }
}
```

C'est le fichier le plus simple mais le plus fondamental. La méthode `main()` est le point d'entrée standard de tout programme Java. Quand on exécute `mvn spring-boot:run`, la JVM appelle cette méthode en premier.

L'annotation `@SpringBootApplication` est un raccourci qui active trois mécanismes Spring :

- **Auto-configuration** : Spring Boot lit le classpath et configure automatiquement tout ce qu'il détecte — Jackson pour JSON, Tomcat pour HTTP, etc.
- **Scan des composants** : Spring scanne le package `com.epic` et tous ses sous-packages à la recherche de classes annotées `@Controller`, `@Service`, etc.
- **Activation des configurations** : Spring lit `application.properties`.

L'appel `SpringApplication.run(...)` démarre le conteneur IoC (Inversion of Control), crée tous les beans, puis démarre le serveur Tomcat embarqué. À la fin de cette ligne, le serveur est opérationnel.

---

### 3.2 EpicRequest.java — Le Data Transfer Object (DTO)

```java
public class EpicRequest {
    private String epicName;         // Titre de l'Epic (obligatoire)
    private String epicDescription;  // Description (optionnel)
    private String projectKey;       // Clé du projet Jira ("DEV")
    private String template;         // "FEATURE", "BUG" ou "INFRA"
    private List<String> labels;     // Labels à appliquer sur toutes les tâches
    // + getters et setters
}
```

Un **DTO (Data Transfer Object)** est un objet dont le seul rôle est de transporter des données entre deux couches. `EpicRequest` représente exactement la structure du JSON que le frontend envoie au backend :

```json
{
  "epicName": "Refonte page de connexion",
  "epicDescription": "Améliorer l'UX du login",
  "projectKey": "DEV",
  "template": "FEATURE",
  "labels": ["sprint-3", "frontend"]
}
```

Quand Spring reçoit cette requête HTTP POST, il utilise **Jackson** (inclus dans `spring-boot-starter-web`) pour désérialiser automatiquement ce JSON en une instance de `EpicRequest`. Ce processus s'appelle le *data binding* : Spring lit chaque clé JSON, trouve le setter correspondant, et l'appelle. Aucun code de parsing manuel n'est nécessaire.

| Champ | Type Java | Description |
|---|---|---|
| `epicName` | `String` | Titre de l'Epic. Mappé sur le champ `summary` de l'API Jira. |
| `epicDescription` | `String` | Description longue. Optionnel (null accepté). Format ADF pour Jira. |
| `projectKey` | `String` | Clé du projet Jira. Actuellement fixée côté frontend. |
| `template` | `String` | Nom du template. Utilisé comme clé dans `TaskTemplate.TEMPLATES`. |
| `labels` | `List<String>` | Labels appliqués sur l'Epic et toutes les sous-tâches. |

> 💡 Les getters et setters sont obligatoires pour que Jackson puisse accéder aux champs privés. Une alternative est d'utiliser Lombok (déjà dans les dépendances) avec `@Data` pour les générer automatiquement à la compilation.

---

### 3.3 TaskTemplate.java — Le registre des templates

```java
public class TaskTemplate {

    public static final Map<String, List<Map<String, String>>> TEMPLATES = Map.of(

        "FEATURE", List.of(
            Map.of("name", "Analyse et cadrage",      "type", "Task",  "points", "2"),
            Map.of("name", "Maquettes UX/UI",         "type", "Task",  "points", "3"),
            Map.of("name", "Développement backend",   "type", "Story", "points", "5"),
            Map.of("name", "Développement frontend",  "type", "Story", "points", "5"),
            Map.of("name", "Tests unitaires",         "type", "Task",  "points", "3"),
            // ... 7 autres tâches
        ),

        "BUG",   List.of( /* 7 tâches */ ),
        "INFRA", List.of( /* 7 tâches */ )
    );

    public static List<Map<String, String>> getTasksForTemplate(String name) {
        return TEMPLATES.getOrDefault(name.toUpperCase(), TEMPLATES.get("FEATURE"));
    }
}
```

C'est le **fichier de configuration fonctionnelle** du projet. Il contient la définition de tous les templates de tâches sous forme d'une `Map` Java statique.

**Décryptage du type** `Map<String, List<Map<String, String>>>` :
- Clés de la Map externe → noms des templates (`"FEATURE"`, `"BUG"`, `"INFRA"`)
- Valeurs → listes de Maps, chaque Map représentant une tâche avec ses attributs (`name`, `type`, `points`)

**`static final`** signifie que la Map est créée une seule fois au chargement de la classe (`static`) et ne peut pas être réassignée (`final`). Les objets retournés par `Map.of()` et `List.of()` sont **immutables** — toute tentative de modification lève une `UnsupportedOperationException`.

**`getTasksForTemplate()`** appelle `.toUpperCase()` avant la recherche, rendant la correspondance insensible à la casse. Le fallback sur `"FEATURE"` via `getOrDefault()` garantit qu'un template inconnu ne provoque jamais de `NullPointerException`.

> ✅ **Étendre les templates** — Ajouter un nouveau template se résume à ajouter une entrée dans la Map `TEMPLATES` dans ce fichier, puis ajouter le bouton correspondant dans le frontend. Aucun autre fichier Java n'est à modifier.

---

### 3.4 EpicController.java — La porte d'entrée HTTP

```java
@RestController
@RequestMapping("/api")
@CrossOrigin(origins = "*")
public class EpicController {

    @Autowired
    private JiraService jiraService;

    @PostMapping("/epic")
    public ResponseEntity<Map<String, Object>> createEpic(
            @RequestBody EpicRequest request) {
        try {
            Map<String, Object> result = jiraService.createEpicWithTasks(request);
            return ResponseEntity.ok(result);                    // HTTP 200
        } catch (Exception e) {
            return ResponseEntity.internalServerError().body(
                Map.of("success", false, "message", e.getMessage())
            );                                                   // HTTP 500
        }
    }

    @GetMapping("/templates")
    public ResponseEntity<Map<String, Object>> getTemplates() { ... }

    @GetMapping("/health")
    public ResponseEntity<Map<String, String>> health() { ... }
}
```

#### Annotations de classe

| Annotation | Rôle |
|---|---|
| `@RestController` | Combine `@Controller` (gestionnaire HTTP) et `@ResponseBody` (retour sérialisé en JSON). |
| `@RequestMapping("/api")` | Préfixe toutes les URLs par `/api`. Ainsi `@PostMapping("/epic")` répond sur `/api/epic`. |
| `@CrossOrigin(origins = "*")` | Configure la politique CORS. Sans ça, le navigateur refuserait les requêtes `fetch` cross-origin. |

#### Injection de dépendance

`@Autowired` demande à Spring d'injecter automatiquement une instance de `JiraService`. C'est le principe d'**Inversion of Control (IoC)** : le controller ne crée pas lui-même le service (pas de `new JiraService()`), Spring le fournit. Cela facilite les tests unitaires (on peut injecter un mock).

#### Endpoints exposés

| Méthode | URL | Code retour | Description |
|---|---|---|---|
| `POST` | `/api/epic` | 200 / 500 | Crée l'Epic + toutes ses sous-tâches. Corps JSON requis. |
| `GET` | `/api/templates` | 200 | Retourne les templates disponibles et leurs descriptions. |
| `GET` | `/api/health` | 200 | Vérification que le serveur est opérationnel. |

#### Gestion des erreurs

Le bloc `try/catch` intercepte toutes les exceptions remontées par `JiraService` (erreur réseau, token invalide, clé de projet incorrecte, etc.) et les transforme en une réponse HTTP 500 avec un corps JSON structuré. Cela évite de retourner une stack trace Java brute au frontend.

---

### 3.5 JiraService.java — Le cerveau de l'application

C'est le fichier le plus important du projet. Il se décompose en 4 sections.

#### Section 1 — Injection de configuration

```java
@Service
public class JiraService {

    @Value("${jira.base-url}")
    private String jiraBaseUrl;

    @Value("${jira.email}")
    private String jiraEmail;

    @Value("${jira.token}")
    private String jiraToken;

    private final HttpClient httpClient = HttpClient.newHttpClient();
}
```

Les annotations `@Value("${jira.base-url}")` lisent les valeurs dans `application.properties` et les injectent dans les champs au démarrage. Si une propriété est absente, Spring lève une exception au démarrage (*fail-fast* : le serveur refuse de démarrer plutôt que de planter lors du premier appel).

Le `HttpClient` est instancié une seule fois au niveau du champ (pas dans chaque méthode) pour des raisons de **performance** : la création d'un client HTTP est coûteuse (pools de threads, connexions). Partager une seule instance est la bonne pratique.

#### Section 2 — Méthode principale

```java
public Map<String, Object> createEpicWithTasks(EpicRequest request) throws Exception {

    // Étape 1 : créer l'Epic, récupérer son ID
    String epicId = createEpic(request);

    // Étape 2 : récupérer les tâches du template choisi
    List<Map<String, String>> tasks =
        TaskTemplate.getTasksForTemplate(request.getTemplate());

    // Étape 3 : créer chaque tâche en la liant à l'Epic
    List<String> createdTaskIds = new ArrayList<>();
    for (Map<String, String> task : tasks) {
        String taskId = createTask(task, epicId, request);
        createdTaskIds.add(taskId);
    }

    // Étape 4 : retourner le résultat
    return Map.of(
        "success", true,
        "epicId", epicId,
        "tasksCreated", createdTaskIds.size(),
        "taskIds", createdTaskIds
    );
}
```

Cette méthode est le **chef d'orchestre**. Elle est synchrone : les appels Jira s'exécutent séquentiellement, l'un après l'autre. Pour un template FEATURE (12 tâches), le serveur effectue **13 appels HTTP à Jira** (1 pour l'Epic + 12 pour les tâches).

> 💡 **Piste d'amélioration** — Les appels de création de tâches sont indépendants entre eux et pourraient être parallélisés avec `CompletableFuture.allOf()` pour réduire le temps d'exécution de ~60%.

#### Section 3 — Construction des payloads JSON

```java
private String createEpic(EpicRequest request) throws Exception {
    String json = """
        {
            "fields": {
                "project": { "key": "%s" },
                "summary": "%s",
                "description": {
                    "type": "doc", "version": 1,
                    "content": [{"type": "paragraph",
                        "content": [{"type": "text", "text": "%s"}]
                    }]
                },
                "issuetype": { "name": "Epic" },
                "labels": %s
            }
        }
        """.formatted(
            request.getProjectKey(),
            escapeJson(request.getEpicName()),
            escapeJson(request.getEpicDescription()),
            formatLabels(request.getLabels())
        );

    return sendToJiraAndGetId(json);
}
```

Les **text blocks** Java (syntaxe `"""..."""`, disponibles depuis Java 15) permettent d'écrire des strings multilignes sans concaténation. La méthode `.formatted()` remplace chaque `%s` dans l'ordre par les arguments fournis.

Le format de la description utilise l'**Atlassian Document Format (ADF)**, le format JSON natif des champs texte riches dans Jira Cloud.

La méthode `createTask()` est similaire mais ajoute le champ critique :

```java
"parent": {
    "id": "<epicId>"   // ← ce champ crée le lien hiérarchique avec l'Epic
}
```

Sans ce champ, la tâche serait créée mais flottante, sans lien avec l'Epic.

#### Section 4 — sendToJiraAndGetId() : l'appel HTTP

```java
private String sendToJiraAndGetId(String jsonBody) throws Exception {
    String url = jiraBaseUrl + "/rest/api/3/issue";

    // Basic Auth = Base64(email:token)
    String auth = Base64.getEncoder().encodeToString(
        (jiraEmail + ":" + jiraToken).getBytes()
    );

    HttpRequest httpRequest = HttpRequest.newBuilder()
        .uri(URI.create(url))
        .header("Authorization", "Basic " + auth)    // authentification
        .header("Content-Type", "application/json")  // corps en JSON
        .header("Accept", "application/json")        // réponse en JSON
        .POST(HttpRequest.BodyPublishers.ofString(jsonBody))
        .build();

    HttpResponse<String> response = httpClient.send(
        httpRequest, HttpResponse.BodyHandlers.ofString()
    );

    // Jira retourne 201 Created (pas 200 OK) pour une création réussie
    if (response.statusCode() != 201) {
        throw new RuntimeException(
            "Erreur Jira [" + response.statusCode() + "] : " + response.body()
        );
    }

    // Extraire l'ID du ticket depuis {"id": "10042", "key": "DEV-123", ...}
    return extractJsonValue(response.body(), "id");
}
```

**Authentification** — Jira Cloud utilise le schéma *Basic Auth* : la chaîne `email:token` est encodée en Base64 et transmise dans le header `Authorization`. Ce n'est **pas** le mot de passe du compte, mais un token API généré sur le portail Atlassian.

**Réponse Jira** — Jira retourne HTTP **201 Created** (et non 200 OK) pour toute création réussie. Tout autre code lève une `RuntimeException` qui remonte jusqu'au `try/catch` du controller.

#### Méthodes utilitaires

| Méthode | Rôle |
|---|---|
| `formatLabels(List<String>)` | Convertit une List Java en tableau JSON string. Ex: `["sprint-3","frontend"]`. |
| `extractJsonValue(String, String)` | Extraction d'une valeur JSON par clé via `indexOf()`, sans parser le JSON complet. À remplacer par `Jackson ObjectMapper` si le parsing devient plus complexe. |
| `escapeJson(String)` | Échappe `\`, `"` et `\n` dans les strings insérées dans les text blocks. Critique : sans ça, un titre contenant des guillemets casserait le JSON et Jira retournerait une erreur 400. |

---

## 4. Frontend — index.html

Le frontend est une page HTML autonome, **sans framework, sans dépendance externe, sans étape de build**. Elle s'ouvre directement dans un navigateur.

### 4.1 Architecture de la page

La page est divisée en trois zones fonctionnelles :

- **Formulaire de saisie** : nom de l'Epic, description, sélection du template, gestion des labels.
- **Prévisualisation live** : affiche en temps réel les tâches qui seront créées selon le template sélectionné.
- **Zone de résultat** : affiche le succès ou l'erreur après la réponse du serveur.

### 4.2 État de l'application

```javascript
let selectedTemplate = "FEATURE";  // template actif
let labels = [];                    // liste des labels saisis
```

Tout l'état est stocké dans deux variables JavaScript. Pas de framework de gestion d'état (Redux, MobX, etc.) car la complexité ne le justifie pas à ce stade.

### 4.3 Gestion des templates

```javascript
const TEMPLATES = {
    FEATURE: [
        { name: "Analyse et cadrage",    type: "Task",  points: 2 },
        { name: "Développement backend", type: "Story", points: 5 },
        // ... 10 autres tâches
    ],
    BUG:   [ /* 7 tâches */ ],
    INFRA: [ /* 7 tâches */ ]
};

function selectTemplate(btn) {
    document.querySelectorAll('.template-btn')
            .forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    selectedTemplate = btn.dataset.template;  // lit data-template="FEATURE"
    renderTaskPreview();                       // re-rend la prévisualisation
}
```

L'objet `TEMPLATES` est le **miroir côté frontend** de `TaskTemplate.java` côté backend. Il sert uniquement à l'affichage de la prévisualisation — le backend recalcule lui-même les tâches à partir du nom du template reçu.

### 4.4 Gestion des labels

```javascript
document.getElementById('label-input')
        .addEventListener('keydown', function(e) {
    if (e.key === 'Enter' && this.value.trim()) {
        e.preventDefault();          // empêche la soumission du formulaire
        addLabel(this.value.trim());
        this.value = '';             // vide l'input
    }
});

function addLabel(text) {
    if (labels.includes(text)) return;  // déduplication automatique
    labels.push(text);
    renderLabels();
}
```

Les labels sont saisis dans un input texte. L'appui sur *Entrée* déclenche l'ajout avec déduplication automatique. `renderLabels()` reconstruit entièrement le DOM de la zone labels à chaque modification.

### 4.5 La fonction createEpic() — l'appel HTTP

```javascript
async function createEpic() {
    const payload = {
        epicName:        document.getElementById('epic-name').value.trim(),
        epicDescription: document.getElementById('epic-desc').value.trim(),
        projectKey:      "DEV",
        template:        selectedTemplate,
        labels:          labels
    };

    // Requête HTTP vers le serveur Java
    const response = await fetch('http://localhost:8080/api/epic', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload)
    });

    const data = await response.json();
    showResult(data.success, data.message, ...);
}
```

La fonction est déclarée `async` pour pouvoir utiliser `await`. L'API `fetch()` est native au navigateur (pas de dépendance externe).

- Le **premier `await fetch(...)`** attend la réception des headers HTTP.
- Le **deuxième `await response.json()`** attend que le corps soit complètement lu et parsé. Ces deux étapes sont nécessaires car HTTP peut envoyer les headers avant que le corps soit prêt.

> ⚠️ **Limitation** — L'URL du backend est codée en dur comme `http://localhost:8080`. Pour la production, externaliser dans une variable de configuration (`window.CONFIG.apiUrl`).

---

## 5. Guide de démarrage

### 5.1 Prérequis

| Outil | Version minimale | Vérification |
|---|---|---|
| Java JDK | 17+ | `java -version` |
| Maven | 3.6+ | `mvn -version` |
| Compte Jira | Cloud ou Server | Droits de création de tickets |
| Token API Atlassian | — | [id.atlassian.com/manage-profile/security/api-tokens](https://id.atlassian.com/manage-profile/security/api-tokens) |

### 5.2 Installation et démarrage

**1.** Cloner ou copier le dossier du projet.

**2.** Configurer les credentials Jira dans `backend/src/main/resources/application.properties` :

```properties
jira.base-url=https://MON-EQUIPE.atlassian.net
jira.email=mon.email@company.com
jira.token=MON_TOKEN_API
jira.project-key=DEV
```

**3.** Démarrer le serveur Java :

```bash
cd backend
mvn spring-boot:run
# Maven télécharge les dépendances au premier lancement (~1-2 min)
```

**4.** Vérifier que le serveur tourne :

```bash
curl http://localhost:8080/api/health
# Attendu : {"status":"UP","message":"Serveur opérationnel ✅"}
```

**5.** Ouvrir `frontend/index.html` dans Chrome ou Firefox.

### 5.3 Débogage courant

| Erreur | Cause & Solution |
|---|---|
| `HTTP 401` depuis Jira | Token invalide ou expiré. Régénérer sur Atlassian et mettre à jour `application.properties`. |
| `HTTP 400` depuis Jira | Clé de projet incorrecte, ou issuetype *Epic* non disponible dans ce projet. Vérifier dans Jira. |
| `CORS error` (navigateur) | Le serveur Java n'est pas démarré, ou `@CrossOrigin` manquant. Vérifier que le serveur tourne sur `:8080`. |
| Port 8080 déjà utilisé | Changer `server.port=8081` dans `application.properties` et adapter l'URL dans `index.html`. |
| Epic créé, tâches sans lien | Jira Server < 8.x utilise `"epic link"` au lieu de `"parent"`. Adapter `JiraService.createTask()`. |

### 5.4 Déploiement serveur

```bash
# Compiler en JAR autonome
cd backend
mvn package -DskipTests

# Lancer sur le serveur avec variables d'environnement
JIRA_TOKEN=mon_token \
JIRA_EMAIL=user@company.com \
java -jar target/jira-epic-creator-1.0.0.jar

# Frontend : copier index.html sur un serveur web statique (nginx, Apache...)
# Mettre à jour l'URL du backend dans index.html avant le déploiement
```

---

## 6. Pistes d'évolution

### Court terme

- **Parallélisation des appels Jira** — Utiliser `CompletableFuture.allOf()` pour créer les sous-tâches en parallèle et réduire le temps d'exécution de ~60%.
- **Sécurisation CORS** — Remplacer `@CrossOrigin(origins = "*")` par l'URL précise du frontend en production.
- **Validation d'entrée** — Ajouter `@Valid` et des annotations Bean Validation (`@NotBlank`, `@Size`) sur `EpicRequest` pour rejeter les requêtes malformées avant d'appeler Jira.
- **ProjectKey dynamique** — Alimenter une liste déroulante via `GET /rest/api/3/project` depuis Jira.
- **Externaliser l'URL backend** — Remplacer l'URL codée en dur dans `index.html` par `window.CONFIG.apiUrl`.

### Moyen terme

- **Génération de tâches par IA** — Appeler l'API Claude/OpenAI avec le titre de l'Epic pour générer des tâches contextuelles plutôt que des templates fixes.
- **Persistance** — Ajouter Spring Data JPA + PostgreSQL pour logger les Epics créés et permettre l'audit.
- **Interface React** — Migrer le frontend vers React pour une gestion d'état plus robuste si le formulaire gagne en complexité.
- **Docker** — Conteneuriser avec un Dockerfile multi-stage pour simplifier le déploiement.
- **Authentification** — Ajouter Spring Security + OAuth2 pour se connecter avec le compte Atlassian de l'utilisateur plutôt qu'un token global.

---

*Documentation Technique — Jira Epic Creator v1.0.0 — Mars 2026*


# Documentation explicative — Fonctionnement général du projet Jira Epic Creator

## 1. Introduction

Le projet **Jira Epic Creator** est une petite application web dont le but est d'automatiser la création d’Epics et de sous-tâches dans Jira.

Normalement, lorsqu’un développeur ou un chef de projet veut créer un Epic dans Jira, il doit :

1. créer l’Epic
2. créer plusieurs tâches associées
3. relier chaque tâche à l’Epic
4. ajouter des labels, descriptions, etc.

Ce processus est **répétitif et long**.

L’objectif de ce projet est donc simple :

> permettre à un utilisateur de remplir un formulaire, cliquer sur un bouton, et laisser l’application créer automatiquement l’Epic et toutes ses tâches dans Jira.

---

# 2. Vue d’ensemble de l’architecture

Le projet est organisé selon une architecture classique **client / serveur**.

Il y a **trois parties principales** :

```
Interface utilisateur (Frontend)
        ↓
Serveur Java (Backend)
        ↓
API Jira (service externe)
```

Chaque partie a un rôle précis.

---

# 3. L’interface utilisateur (Frontend)

## Rôle

L’interface utilisateur est la partie visible par l’utilisateur.

Elle est composée d’une simple page web contenant :

- du **HTML** pour la structure
- du **CSS** pour l’apparence
- du **JavaScript** pour la logique

Cette interface sert principalement à :

- récupérer les informations saisies par l’utilisateur
- afficher les templates disponibles
- envoyer les données au serveur
- afficher le résultat de l’opération

Important :

> l’interface ne communique **jamais directement avec Jira**.

Elle communique uniquement avec le **serveur Java**.

---

## Fonctionnement du formulaire

L’utilisateur remplit un formulaire contenant par exemple :

```
Nom de l'Epic
Description
Template de tâches
Labels
```

Exemple :

```
Epic name : Refonte page login
Template : FEATURE
Labels : sprint-3, frontend
```

Ces informations servent à construire un **objet de données** qui sera envoyé au serveur.

---

## Envoi de la requête au serveur

Lorsque l’utilisateur clique sur le bouton **Créer**, le JavaScript de la page effectue une requête HTTP vers le serveur.

Cette requête contient les informations du formulaire sous forme de **JSON**.

Exemple de données envoyées :

```json
{
  "epicName": "Refonte login",
  "epicDescription": "Améliorer UX login",
  "projectKey": "DEV",
  "template": "FEATURE",
  "labels": ["frontend", "sprint-3"]
}
```

Cette requête est envoyée à l’adresse :

```
POST /api/epic
```

qui correspond à un endpoint exposé par le serveur Java.

---

# 4. Le serveur Java (Backend)

## Rôle

Le serveur Java est le **cœur de l’application**.

C’est lui qui va :

1. recevoir les requêtes envoyées par l’interface
2. interpréter les données reçues
3. appliquer la logique métier
4. appeler l’API Jira
5. renvoyer le résultat à l’interface

Le serveur est construit avec **Spring Boot**, un framework très utilisé pour créer des applications Java web.

Spring Boot permet notamment de :

- créer facilement des API REST
- gérer les requêtes HTTP
- convertir automatiquement les données JSON en objets Java
- configurer rapidement un serveur web

---

# 5. Le démarrage du serveur

Lorsque l’application démarre, Spring Boot lance automatiquement un serveur web.

Ce serveur écoute les requêtes HTTP sur un port (par exemple **8080**).

Cela signifie que l’application devient accessible via :

```
http://localhost:8080
```

À partir de ce moment, le serveur peut recevoir des requêtes provenant de l’interface.

---

# 6. Le rôle du Controller

Dans une application Spring Boot, les **Controllers** sont responsables de la gestion des requêtes HTTP.

Un controller agit comme une **porte d’entrée** vers l’application.

Dans ce projet, le controller expose plusieurs endpoints :

```
POST /api/epic
GET  /api/templates
GET  /api/health
```

Le plus important est :

```
POST /api/epic
```

Cet endpoint est appelé lorsque l’utilisateur veut créer un Epic.

Le controller reçoit la requête et transmet les informations au **service métier**.

---

# 7. Le transfert des données

Lorsque le serveur reçoit les données JSON envoyées par l’interface, Spring Boot les transforme automatiquement en **objet Java**.

Ce type d’objet s’appelle un **DTO (Data Transfer Object)**.

Dans ce projet, l’objet s’appelle :

```
EpicRequest
```

Il contient les informations envoyées par le frontend :

```
epicName
epicDescription
projectKey
template
labels
```

Le but du DTO est simplement de transporter les données entre les différentes couches de l’application.

---

# 8. La logique métier (Service)

La logique principale de l’application se trouve dans le **service**.

Le service est responsable de :

1. créer l’Epic dans Jira
2. récupérer les tâches correspondant au template choisi
3. créer les sous-tâches
4. relier les tâches à l’Epic
5. renvoyer le résultat final

On peut représenter cette logique de manière simplifiée :

```
Créer Epic
      ↓
Récupérer template
      ↓
Créer chaque tâche
      ↓
Associer les tâches à l’Epic
      ↓
Retourner le résultat
```

---

# 9. Les templates de tâches

Pour éviter de recréer les mêmes tâches à chaque fois, le projet utilise un système de **templates**.

Un template correspond à une liste de tâches prédéfinies.

Par exemple :

```
FEATURE
 ├ Analyse
 ├ Développement backend
 ├ Développement frontend
 ├ Tests
 └ Déploiement
```

Chaque template correspond donc à une liste de tâches qui seront créées automatiquement.

Lorsque l’utilisateur choisit un template, le serveur récupère la liste correspondante et crée chaque tâche dans Jira.

---

# 10. Communication avec l’API Jira

Le serveur Java doit ensuite communiquer avec **Jira**.

Jira expose une **API REST**, qui permet de créer des tickets via des requêtes HTTP.

Le serveur envoie donc une requête vers l’API Jira :

```
POST /rest/api/3/issue
```

avec un JSON décrivant le ticket à créer.

Exemple simplifié :

```json
{
  "fields": {
    "project": {
      "key": "DEV"
    },
    "summary": "Refonte login",
    "issuetype": {
      "name": "Epic"
    }
  }
}
```

Jira crée alors l’Epic et renvoie une réponse contenant son identifiant.

---

# 11. Création des sous-tâches

Une fois l’Epic créé, le serveur récupère son identifiant.

Ensuite, pour chaque tâche du template, il envoie une nouvelle requête à Jira.

Chaque tâche contient une référence vers l’Epic afin de créer la relation hiérarchique.

On obtient donc une structure comme ceci :

```
Epic
 ├ Task 1
 ├ Task 2
 ├ Task 3
 ├ Task 4
```

Toutes les tâches sont automatiquement liées à l’Epic.

---

# 12. Réponse du serveur

Une fois toutes les opérations terminées, le serveur renvoie une réponse à l’interface.

Exemple :

```json
{
  "success": true,
  "epicId": "DEV-123",
  "tasksCreated": 12
}
```

Le navigateur peut alors afficher un message de succès à l’utilisateur.

---

# 13. Flux complet de l’application

On peut résumer tout le fonctionnement avec ce schéma :

```
Utilisateur
     ↓
Interface web
     ↓
Requête HTTP
     ↓
Serveur Java
     ↓
Service métier
     ↓
Appel API Jira
     ↓
Création Epic + tâches
     ↓
Réponse JSON
     ↓
Affichage du résultat
```

---

# 14. Résumé général

Ce projet repose sur un principe simple :

- l’interface collecte les informations
- le serveur applique la logique
- l’API Jira crée les tickets

On peut donc résumer l’architecture ainsi :

```
Frontend
   ↓
Backend Java
   ↓
Jira API
```

Chaque couche a une responsabilité claire :

| Couche | Rôle |
|------|------|
| Interface | interaction avec l’utilisateur |
| Backend | logique et automatisation |
| API Jira | création réelle des tickets |

---

# 15. Conclusion

Le projet **Jira Epic Creator** est une application simple mais très représentative des architectures modernes.

Il montre comment :

- créer une interface web
- construire une API backend
- interagir avec un service externe
- automatiser des tâches répétitives

Même si le projet reste relativement petit, il illustre plusieurs concepts fondamentaux du développement web moderne :

- architecture client-serveur
- APIs REST
- séparation des responsabilités
- automatisation des processus métier
