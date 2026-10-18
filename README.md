# David Quintard

**Développement full stack · Architecture DDD · IA & agents · Cloud & DevOps**

Je conçois et développe des projets personnels de bout en bout : modélisation métier, API, interfaces web, intégration d’agents IA et exploitation en production. Je travaille principalement en **TypeScript**, avec **NestJS**, **Angular** et **React**.

Mes projets sont un terrain d’application pour construire des logiciels maintenables : isoler les règles métier, rendre les intégrations remplaçables, automatiser la qualité et maîtriser le déploiement comme les données.

## Mes principaux projets

| Projet | Objectif | Axes techniques |
| --- | --- | --- |
| [Infra](#infra) | Mutualiser l’infrastructure des applications | Docker, Traefik, S3, PostgreSQL, Redis, CI/CD |
| [Masterflow](#masterflow) | Transformer un ticket en modification de code vérifiée | Agents IA, CLI, workflow déterministe, intégrations Git |
| [Jobs4u](#jobs4u) | Accompagner la recherche d’emploi | Agrégation d’offres, matching, agents IA, OAuth |
| [Intimity](#intimity) | Réunir live vidéo, contenus et boutique de créatrices | DDD, architecture hexagonale, WebRTC, transactions |
| [OryanHair](#oryanhair) | Présenter un salon et proposer une assistance vocale | React, SEO local, reconnaissance vocale, LLM, synthèse vocale |
| [Missebony](#missebony) | Développer une plateforme de rencontres | DDD, multi-tenant, Angular SSR, messagerie temps réel |

### Infra

**Socle d’hébergement et d’exploitation.** Une infrastructure indépendante des dépôts applicatifs pour partager les services techniques et centraliser leur exploitation sur VPS.

- **Conteneurisation et exposition** : Docker Compose, routage HTTP/HTTPS par Traefik et certificats TLS automatiques avec Let’s Encrypt.
- **Services mutualisés** : PostgreSQL pour les données relationnelles, Redis pour le cache et les files applicatives, **Garage compatible S3** pour le stockage objet.
- **Identité et communications** : Authelia pour le SSO, le MFA et OpenID Connect ; coturn pour les échanges TURN/STUN nécessaires aux appels audio/vidéo.
- **CI/CD auto-hébergée** : runners GitHub Actions dédiés aux projets, déploiement automatisé des services et vérification du provisionnement des bases et buckets.
- **Exploitation** : scripts de sauvegarde et restauration PostgreSQL/S3, consultation des journaux et opérations de maintenance à distance.

**Stack :** Docker · Docker Compose · Traefik · PostgreSQL · Redis · Garage/S3 · Authelia · coturn · GitHub Actions · Shell

[Dépôt](https://github.com/davidquintard/infra)

### Masterflow

**Orchestration d’agents IA pour le développement.** Un orchestrateur qui prend en charge le cycle **ticket → code → contrôles qualité → commit → pull request**, depuis une CLI, un terminal web ou une API.

- **Workflow déterministe** : étapes explicites pour charger le ticket, préparer la branche, lancer l’agent, contrôler les changements et ouvrir la PR/MR. Les échecs de lint, tests ou analyse statique alimentent une boucle de correction bornée.
- **Agents interchangeables** : intégration des CLI de Codex, Claude, Copilot, Cursor et d’autres agents, avec les sessions des utilisateurs. Le même workflow sert aux exécutions interactives et en arrière-plan.
- **Intégrations de forges et backlogs** : GitHub, GitLab et Azure DevOps ; détection du dépôt et de sa stack, analyse de dette technique et revue de PR.
- **Architecture modulaire** : monorepo séparant CLI, assemblage des prompts, contrats partagés, API NestJS et interface React. Suivi des exécutions et journaux en temps réel via WebSocket.
- **Exécution et secrets** : workers exécutés sous l’identité système du demandeur, configuration centralisée et chiffrement AES-256-GCM des secrets en base.
- **Livraison** : tests automatisés, images Docker identifiées par le SHA du commit, tests de démarrage des images avant publication dans GHCR et contrôles après déploiement.

**Stack :** JavaScript ESM · TypeScript · Node.js · NestJS · React · Vite · Prisma · PostgreSQL · WebSocket · Docker · GitHub Actions

[Dépôt](https://github.com/davidquintard/masterflow) · [Site](https://masterflow.dev)

### Jobs4u

**CV, recherche d’emploi et préparation d’entretiens.** Une application qui réunit l’édition du CV, la recherche d’offres, le suivi des candidatures et la préparation d’entretiens assistée par IA.

- **CV structuré** : édition au format JSON Resume, persistance par utilisateur et partage public activable ou révocable.
- **Agrégation et matching** : fournisseurs d’offres derrière une interface commune, dont France Travail, Adzuna et Remotive ; classement selon le titre, les compétences et les technologies du CV.
- **Suivi des candidatures** : archivage des offres postulées et conservation d’un instantané de l’annonce.
- **Recruteur IA** : préparation de questions et débrief par un agent CLI configurable ; voix via les API du navigateur, enregistrement audio/vidéo et indicateurs locaux de posture, de mouvements du visage et de temps de parole avec MediaPipe et Web Audio. L’exécution nécessite un agent installé et authentifié côté serveur.
- **Identité et messagerie** : comptes intégrés, sessions révocables et connexions Google/GitHub optionnelles. Intégration Gmail développée avec OAuth, jetons chiffrés et notifications Google Cloud Pub/Sub ; son activation dépend de la configuration et des autorisations Google.
- **Déploiement traçable** : vérifications sur les PR, publication des images dans GHCR, déploiement du SHA exact et attente des contrôles de santé Docker et HTTPS.

**Stack :** Nx · TypeScript · NestJS · React · Vite · PostgreSQL · OAuth 2.0/OIDC · Google Cloud Pub/Sub · MediaPipe · Docker · GitHub Actions

[Dépôt](https://github.com/davidquintard/jobs4u) · [Site](https://jobs4u.fr)

### Intimity

**Plateforme de créatrices et de live vidéo.** Une plateforme adulte en développement réunissant profils de créatrices, diffusion en direct, messagerie, contenus premium et boutique, avec un back-office de modération.

- **DDD et architecture hexagonale** : contextes métier explicites — identité, portefeuille, cam, médias, boutique, modération — avec domaine, cas d’usage, ports et adaptateurs. Les règles métier sont séparées de la persistance et des fournisseurs externes.
- **Modélisation transactionnelle** : agrégat de portefeuille, objets valeur pour les crédits, invariant de solde positif ou nul, clés d’idempotence et gestion des conflits de concurrence pour éviter les doubles débits.
- **Temps réel** : diffusion WebRTC via **LiveKit auto-hébergé** et événements SSE pour les interactions ; séparation entre les règles des salles et l’adaptateur vidéo.
- **Front structuré par contexte** : bibliothèques Angular chargées à la demande, ports injectables, adaptateurs HTTP/SSE/LiveKit et état en signals. Site en SSR et back-office en SPA.
- **Médias et traitements différés** : stockage dans un bucket S3 privé, conversion des fichiers en arrière-plan, modération et file d’emails avec pg-boss ; envoi par Amazon SES.
- **Contrôles d’accès** : sessions révocables, TOTP obligatoire pour les administrateurs, intégrations de vérification d’âge et d’identité, journal des décisions de modération.
- **Qualité** : tests du domaine et des cas d’usage, tests d’intégration PostgreSQL, parcours Playwright et pipeline vérifiant types, migrations et démarrage réel.

Les parcours de paiement et de vérification d’identité dépendent aussi de l’activation des prestataires concernés.

**Stack :** Nx · TypeScript · Angular · NestJS · Prisma · PostgreSQL · LiveKit/WebRTC · SSE · Garage/S3 · pg-boss · Amazon SES · Playwright

[Dépôt](https://github.com/davidquintard/intimity) · [Site](https://intimity.fr)

### OryanHair

**Site métier et assistant vocal IA.** Un site de salon de coiffure associant présentation des prestations, référencement local et assistant vocal capable de répondre à partir des informations du salon.

- **Interface web** : React et TypeScript, composants shadcn/ui et Radix UI, Tailwind CSS et formulaires validés avec Zod.
- **SEO local** : métadonnées par page, URL canoniques et données structurées Schema.org pour l’établissement et ses prestations.
- **Interaction vocale** : capture audio dans le navigateur, envoi au service vocal et restitution de la transcription, de la réponse et de l’audio généré.
- **Chaîne IA auto-hébergée**, dans le dépôt distinct `voice` : **faster-whisper → Ollama → Piper**, soit reconnaissance de la parole, génération de réponse par LLM, puis synthèse vocale. Un gateway Express relie les services FastAPI.
- **Contexte métier** : base de connaissances du salon injectée dans le prompt et identification du site par tenant pour mutualiser le service vocal.
- **Livraison** : build Vite, frontend statique servi par Nginx dans Docker et déploiement automatisé sur VPS via GitHub Actions.

**Stack :** React · TypeScript · Vite · Tailwind CSS · shadcn/ui · Node.js/Express · Python/FastAPI · faster-whisper · Ollama · Piper · Nginx · Docker

[Dépôt](https://github.com/davidquintard/oryanhair) · [Site](https://www.oryanhair.fr)

### Missebony

**Plateforme de rencontres sur un socle multi-tenant.** Une application développée dans le **monorepo Pangoria**, qui mutualise l’API et les bibliothèques métier de plusieurs sites.

- **Architecture DDD** : modules par domaine — authentification, membres, profils, relations, conversations, médias et abonnements — avec cas d’usage, interfaces de repositories et adaptateurs de persistance Prisma.
- **Multi-tenant** : contexte de site propagé par requête avec `nestjs-cls`, base PostgreSQL et bucket S3 propres à chaque site, API et code métier partagés. Les transactions utilisent le client du tenant courant.
- **Web et mobile** : Angular, Ionic et Capacitor ; bibliothèques fonctionnelles partagées, façades applicatives et repositories HTTP injectés.
- **Temps réel** : messagerie Socket.IO et appels audio/vidéo WebRTC avec signalisation sur socket et infrastructure TURN/STUN.
- **SSR et référencement** : rendu serveur et génération des titres, métadonnées, URL canoniques et balises Open Graph selon la route.
- **Traitements asynchrones et exploitation** : événements métier, files Bull/Redis, stockage Garage/S3, API documentée avec Swagger, endpoints de santé et métriques Prometheus.
- **Évolution du produit** : gestion des offres et abonnements, adaptateurs de paiement Stripe/CinetPay en développement, tests métier et pipeline Docker/GHCR.

**Stack :** Nx · TypeScript · Angular · Ionic · Capacitor · NestJS · Prisma · PostgreSQL · Redis/Bull · Socket.IO · WebRTC · Garage/S3 · Prometheus

[Dépôt Pangoria](https://github.com/davidquintard/pangoria) · [Site](https://www.missebony.fr)

## Les pratiques que je développe au fil de ces projets

| Axe | Mise en pratique |
| --- | --- |
| **Architecture métier** | DDD, agrégats, objets valeur, cas d’usage, ports et adaptateurs ; monolithes modulaires organisés par contexte métier. |
| **IA & agents** | Orchestration de CLI d’agents, boucles de correction guidées par les tests, préparation d’entretiens et chaîne vocale STT → LLM → TTS. |
| **Cloud & auto-hébergement** | Applications conteneurisées sur VPS, stockage compatible S3, Amazon SES et intégration événementielle Google Cloud Pub/Sub. |
| **CI/CD** | GitHub Actions, runners auto-hébergés, GHCR, images identifiées par commit, migrations et vérifications de démarrage selon les projets. |
| **Données & asynchronisme** | PostgreSQL, Prisma, isolation par tenant, transactions, idempotence, files Redis/Bull et pg-boss. |
| **Sécurité** | OAuth 2.0/OIDC, PKCE, MFA/TOTP, sessions révocables, chiffrement des secrets et contrôle des accès aux médias. |
| **Qualité & exploitation** | Tests unitaires, intégration et E2E selon les projets, contrôle des types, journaux, métriques, sauvegarde et restauration. |
