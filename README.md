# Jenkins Shared Library

> **FR** — Librairie Groovy partagée pour Jenkins : abstraction des opérations Docker (build, login, push) en étapes de pipeline réutilisables, avec encapsulation dans une classe Groovy `src/`.
>
> **EN** — Shared Groovy library for Jenkins: abstracting Docker operations (build, login, push) into reusable pipeline steps, with encapsulation in a Groovy `src/` class.

---

## Stack

![Jenkins](https://img.shields.io/badge/Jenkins-Shared%20Library-red?logo=jenkins)
![Groovy](https://img.shields.io/badge/Groovy-4.0-4298B8?logo=apachegroovy)
![Docker](https://img.shields.io/badge/Docker-Hub-blue?logo=docker)

---

## FR — Description

Une Jenkins Shared Library extrait la logique commune des pipelines dans un projet Groovy versionné et réutilisable. Au lieu de dupliquer du code dans chaque `Jenkinsfile`, les étapes sont définies une seule fois ici et appelées depuis n'importe quel pipeline.

**Étapes disponibles :**

| Étape | Fichier | Description |
|---|---|---|
| `buildJar()` | `vars/buildJar.groovy` | Compile et package le projet Maven |
| `buildImage(name)` | `vars/buildImage.groovy` | Build l'image Docker |
| `dockerLogin()` | `vars/dockerLogin.groovy` | Connexion à Docker Hub |
| `dockerPush(name)` | `vars/dockerPush.groovy` | Push l'image Docker |

**Concepts avancés démontrés :**
- Différence entre `vars/` (étapes globales simples) et `src/` (classes Groovy avec logique partagée)
- Pattern `script` : passage du contexte pipeline (`this`) à une classe `src/` pour accéder aux steps Jenkins (`sh`, `echo`, `withCredentials`)
- `implements Serializable` : requis pour la transformation CPS de Jenkins (checkpoint entre étapes)
- Librairie globale vs librairie scoped au projet (via `library identifier:` dans le Jenkinsfile)

## EN — Description

A Jenkins Shared Library extracts common pipeline logic into a versioned, reusable Groovy project. Instead of duplicating code across `Jenkinsfile`s, steps are defined once and called from any pipeline.

**Available steps:**

| Step | File | Description |
|---|---|---|
| `buildJar()` | `vars/buildJar.groovy` | Compiles and packages the Maven project |
| `buildImage(name)` | `vars/buildImage.groovy` | Builds the Docker image |
| `dockerLogin()` | `vars/dockerLogin.groovy` | Logs in to Docker Hub |
| `dockerPush(name)` | `vars/dockerPush.groovy` | Pushes the Docker image |

**Advanced concepts demonstrated:**
- Difference between `vars/` (simple global steps) and `src/` (Groovy classes with shared logic)
- `script` pattern: passing the pipeline context (`this`) to a `src/` class to access Jenkins steps (`sh`, `echo`, `withCredentials`)
- `implements Serializable`: required for Jenkins CPS transformation (checkpointing between steps)
- Global library vs project-scoped library (via `library identifier:` in Jenkinsfile)

---

## Project Structure

```
.
├── vars/
│   ├── buildJar.groovy     # mvn package step
│   ├── buildImage.groovy   # Docker build (delegates to Docker class)
│   ├── dockerLogin.groovy  # Docker Hub login
│   └── dockerPush.groovy   # Docker push
└── src/
    └── com/example/
        └── Docker.groovy   # Groovy class: build + login + push logic
```
