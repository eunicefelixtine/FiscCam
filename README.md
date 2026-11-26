# FiscCam — Gestion fiscale pour les commerçants camerounais

FiscCam est une application web responsive installable (PWA) destinée aux commerçants du Cameroun. Elle centralise les obligations fiscales, échéances, paiements et justificatifs, et fournit des estimations indicatives. L’application suit les paiements : elle ne les effectue pas dans le MVP. Toute estimation fiscale doit être vérifiée au regard des textes officiels en vigueur et afficher clairement qu’elle n’est pas officielle.

> **État du dépôt :** le dépôt contient actuellement cette spécification, pas encore le code applicatif. Les technologies, commandes et répartition ci-dessous constituent le plan de réalisation ; les commandes d’installation seront opérationnelles après l’initialisation du projet.

## Sommaire

1. [Objectifs et périmètre](#objectifs-et-périmètre)
2. [Utilisateurs et interfaces](#utilisateurs-et-interfaces)
3. [Fonctionnalités à réaliser](#fonctionnalités-à-réaliser)
4. [Exigences transversales](#exigences-transversales)
5. [Architecture et technologies prévues](#architecture-et-technologies-prévues)
6. [Données principales](#données-principales)
7. [Répartition du travail et consignes](#répartition-du-travail-et-consignes)
8. [Feuille de route](#feuille-de-route)
9. [Initialisation prévue](#initialisation-prévue)

## Objectifs et périmètre

L’application doit aider les commerçants à connaître leurs obligations, éviter les retards, conserver les preuves de paiement et estimer les montants à prévoir. La cible prioritaire est constituée des petits et moyens commerçants, en commençant par l’Impôt Général Synthétique (IGS), puis le régime du réel simplifié. Une interface d’administration web est également prévue.

- **P1 — MVP :** fonctions indispensables au premier produit utilisable.
- **P2 — Évolutions importantes :** fonctions à livrer après le MVP.
- **P3 — Évolutions avancées :** fonctions à planifier après P2.

Les utilisateurs peuvent enregistrer dans FiscCam un paiement effectué par les canaux officiels. Le paiement direct Mobile Money est une évolution P2, et le rapprochement automatique est P3.

## Utilisateurs et interfaces

- **Commerçant :** crée son profil, consulte ses obligations, suit ses paiements et gère ses documents.
- **Comptable (P3) :** accès délégué aux données d’un ou plusieurs commerçants.
- **Administrateur :** gère les règles fiscales, les comptes et les communications.

### Interfaces à concevoir et livrer

1. **Accès et profil :** inscription/connexion par téléphone et OTP, création et modification du profil, choix du régime, préférences et déconnexion.
2. **Espace commerçant :** tableau de bord ; liste, détail et calendrier des obligations ; ajout d’obligation ; réglages des rappels ; fiches fiscales ; historique et formulaires de paiement ; coffre-fort documentaire ; simulateur et aide aux déclarations.
3. **Interfaces P2/P3 commerçant :** saisie hors ligne des ventes et dépenses, graphiques, rapports et exports, préférences WhatsApp, factures/reçus, scan OCR, aide/FAQ, assistant et annuaire.
4. **Back-office administrateur :** gestion versionnée des régimes et paramètres fiscaux, gestion des comptes, rédaction/diffusion de messages et statistiques de supervision.
5. **Écrans communs :** états de chargement, listes vides, erreurs, confirmations, validation de formulaire, accès refusé et mode hors ligne/synchronisation. Les interfaces doivent être responsives et disponibles en français et en anglais.

## Fonctionnalités à réaliser

Les identifiants ci-dessous servent de référence pour le développement, les tests et la répartition des responsabilités. Aucun besoin fonctionnel de la spécification ne doit être abandonné.

### Compte et profil fiscal

| ID | Besoin | Priorité |
|---|---|---|
| BF-01 | Inscription et connexion par numéro de téléphone camerounais avec code OTP SMS. | P1 |
| BF-02 | Profil commerçant : nom/raison sociale, NIU validé, activité, ville et centre des impôts. | P1 |
| BF-03 | Choix du régime (IGS, réel simplifié, réel) déterminant les obligations. | P1 |
| BF-04 | Modification du profil, déconnexion sécurisée et historisation des modifications. | P1 |
| BF-05 | Plusieurs établissements/points de vente avec obligations et paiements propres. | P2 |
| BF-06 | Comptes multi-utilisateurs et rôles (propriétaire, comptable, caissier) avec droits associés. | P3 |

### Obligations fiscales

| ID | Besoin | Priorité |
|---|---|---|
| BF-07 | Générer les obligations annuelles selon le régime : impôt, période, échéance et estimation. | P1 |
| BF-08 | Ajouter manuellement une obligation, par exemple une taxe communale. | P1 |
| BF-09 | Gérer les statuts à venir, à payer, payé, partiellement payé et en retard selon paiements et date. | P1 |
| BF-10 | Fiches simples des impôts (qui, quand, combien, comment payer), en français et en anglais. | P2 |

### Calendrier et rappels

| ID | Besoin | Priorité |
|---|---|---|
| BF-11 | Calendrier et liste des échéances, filtrables par mois et impôt. | P1 |
| BF-12 | Rappels SMS et push à J-7, J-3 et J-1, une seule fois par échéance et palier. | P1 |
| BF-13 | Alerte le lendemain d’une échéance non soldée. | P1 |
| BF-14 | Rappels WhatsApp activables dans les préférences. | P2 |
| BF-15 | Estimation des pénalités de retard affichée sur l’obligation concernée. | P2 |

### Paiements et tableau de bord

| ID | Besoin | Priorité |
|---|---|---|
| BF-16 | Enregistrer obligation, montant, date, mode (espèces, banque, Mobile Money) et référence ; recalculer le solde. | P1 |
| BF-17 | Joindre et consulter une preuve de paiement (photo ou PDF). | P1 |
| BF-18 | Modifier ou annuler un paiement avec trace de qui, quoi et quand. | P1 |
| BF-19 | Historique avec recherche et filtres par impôt, période et statut ; résultat en moins de 2 secondes. | P1 |
| BF-20 | Tableau de bord : total payé, reste à payer, prochaine échéance et obligations en retard. | P1 |
| BF-21 | Graphiques d’évolution des paiements par année et par impôt. | P2 |
| BF-22 | Paiement direct MTN MoMo ou Orange Money, confirmé et enregistré automatiquement. | P2 |
| BF-23 | Rapprocher automatiquement les relevés Mobile Money et proposer les paiements détectés. | P3 |

### Documents

| ID | Besoin | Priorité |
|---|---|---|
| BF-24 | Ajouter, classer et consulter attestations, patentes, déclarations et quittances, avec dates/type. | P1 |
| BF-25 | Rappel 30 jours avant l’expiration d’un document. | P1 |
| BF-26 | Exporter/télécharger un document en PDF depuis sa fiche. | P2 |

### Calcul et simulation

| ID | Besoin | Priorité |
|---|---|---|
| BF-27 | Estimer l’impôt à partir du chiffre d’affaires et du régime, sur la base de taux paramétrés ; afficher la date de référence de la loi de finances. | P1 |
| BF-28 | Afficher « estimation non officielle » sur chaque résultat. | P1 |
| BF-29 | Préparer une déclaration avec préremplissage des montants et récapitulatif à reporter. | P2 |
| BF-30 | Comparer les montants estimés de deux régimes fiscaux. | P3 |

### Ventes, dépenses et factures

| ID | Besoin | Priorité |
|---|---|---|
| BF-31 | Saisir simplement ventes et dépenses hors ligne ; synchroniser au retour du réseau. | P2 |
| BF-32 | Catégoriser les opérations, identifier les dépenses déductibles et calculer leur total par période. | P2 |
| BF-33 | Générer factures et reçus avec calcul automatique de TVA et export PDF. | P3 |
| BF-34 | Scanner des factures par OCR et préremplir les champs principaux. | P3 |

### Rapports et exports

| ID | Besoin | Priorité |
|---|---|---|
| BF-35 | Rapport de paiements mensuel, trimestriel ou annuel cohérent avec l’historique. | P2 |
| BF-36 | Export PDF et Excel téléchargeable. | P2 |
| BF-37 | Partage sécurisé avec un comptable/agent des impôts via lien limité dans le temps et révocable. | P3 |

### Assistance

| ID | Besoin | Priorité |
|---|---|---|
| BF-38 | FAQ et guides pas à pas (NIU, déclaration, patente), accessibles du menu et des fiches fiscales. | P2 |
| BF-39 | Assistant conversationnel fiscal reposant sur une base de connaissances validée. | P3 |
| BF-40 | Annuaire des centres des impôts et centres de gestion agréés, avec recherche par ville. | P3 |

### Administration

| ID | Besoin | Priorité |
|---|---|---|
| BF-41 | Administrer régimes, types d’impôts, taux, seuils et échéances sans modifier le code, avec historique des versions. | P1 |
| BF-42 | Consulter et suspendre des comptes utilisateurs ; journaliser les actions. | P1 |
| BF-43 | Envoyer un message à tous les utilisateurs ou à un groupe par notification/SMS. | P1 |
| BF-44 | Statistiques et tableau de supervision : utilisateurs, paiements enregistrés, rappels envoyés. | P2 |

## Exigences transversales

- **Hors ligne :** consultation de l’historique et enregistrement local d’un paiement sans connexion, puis synchronisation ; les saisies de ventes/dépenses hors ligne sont également synchronisées (BF-31).
- **Performance et appareils :** interface fluide sur Android d’entrée de gamme et réseau lent, pages légères, historique filtré en moins de deux secondes.
- **Ergonomie et accessibilité :** interface épurée, vocabulaire courant, icônes compréhensibles, contrastes suffisants, texte lisible et navigation clavier.
- **Langues :** français et anglais pour l’application et ses contenus fiscaux.
- **Sécurité et confidentialité :** HTTPS, protection des données au repos, authentification, droits par rôle, journal d’audit, respect de la loi camerounaise sur les données personnelles.
- **Fiabilité fiscale :** règles configurables et versionnées, sources officielles et date de référence visibles ; les taux doivent être validés avant publication.
- **Disponibilité :** sauvegardes régulières de la base et des fichiers.

## Architecture et technologies prévues

La cible est une PWA Next.js : le navigateur communique en HTTPS avec les Route Handlers/Server Actions. PostgreSQL persiste les données, Prisma gère le schéma et les migrations, Redis/BullMQ planifie rappels et SMS, et un stockage compatible S3 conserve les justificatifs. Service Worker et IndexedDB assurent cache et mode hors ligne. Les intégrations SMS, push et, dans les phases ultérieures, WhatsApp/Mobile Money sont isolées derrière des services adaptés.

- **Interface :** React, Next.js App Router, TypeScript, Tailwind CSS, shadcn/ui (Radix), React Hook Form, Zod, TanStack Query, next-intl, Recharts, date-fns.
- **PWA :** Serwist (ou next-pwa), Dexie.js/IndexedDB, Web Push API.
- **Serveur et données :** Next.js Route Handlers/Server Actions, PostgreSQL, Prisma, Auth.js ou OTP dédié, Redis, BullMQ, stockage S3 (Cloudflare R2, MinIO ou Supabase Storage).
- **Services externes envisagés :** passerelle SMS (Africa’s Talking, Twilio ou opérateur local), WhatsApp Business API (P2), APIs MTN MoMo/Orange Money (P2), Tesseract.js ou service OCR (P3).
- **Documents et qualité :** @react-pdf/renderer, ExcelJS, ESLint, Prettier, Vitest, Testing Library, Playwright, Docker/Compose, GitHub Actions, Sentry ; hébergement Vercel ou VPS Docker/Nginx.

## Données principales

Le modèle à préciser lors de la conception comprend : `User` (téléphone, rôle, langue, préférences), `Merchant` (profil et régime), `TaxRegime` et `TaxType` (règles versionnées), `Obligation` (période, échéance, estimation, statut), `Payment` (montant, date, mode, référence, preuve), `Document` (type, fichier et expiration), `Reminder` (obligation/document, palier, canal, état), `AuditLog` (acteur et changements) et `Sale`/`Expense` (opérations, catégorie, déductibilité, date).

## Répartition du travail et consignes

Les quatre personnes contribuant au projet sont :

- **FOKAM Wilfried**
- **SEUMEGNE Arielle**
- **TSEFACK Klein**
- **NZEUTEM Eunice**

Les responsabilités ci-dessous couvrent **toutes les fonctionnalités BF-01 à BF-44**, leurs interfaces, la logique métier et leurs tests. La répartition est organisée en lots cohérents et équilibrée selon la complexité, les intégrations et les livrables — pas uniquement selon le nombre d’identifiants. Chaque contributeur réalise le lot de bout en bout (interface, validation, persistance/API, états d’erreur et tests), documente les choix et signale rapidement les dépendances. Les trois lots doivent être estimés ensemble au démarrage puis rééquilibrés si l’effort réel diffère.

### FOKAM Wilfried — comptes, obligations, échéances et communications

**Périmètre :** BF-01 à BF-15, BF-42 et BF-43.

- Construire les parcours d’inscription/connexion OTP, profil, NIU, régime, établissements et rôles ; appliquer les autorisations et sécuriser la déconnexion.
- Réaliser les écrans de profil, liste/détail des obligations, calendrier, filtres, fiches fiscales, préférences de rappel et états vides/erreurs.
- Implémenter la génération et le statut des obligations, l’ajout manuel, les échéances, les rappels J-7/J-3/J-1, l’alerte de retard, les préférences de canal et l’estimation des pénalités.
- Réaliser dans le back-office la consultation/suspension des utilisateurs (BF-42) et l’interface de rédaction/ciblage/diffusion des messages (BF-43).
- Prévoir les tests du domaine (OTP, accès par rôle, génération et statut, non-duplication des rappels, messages et journalisation) et documenter les fournisseurs/configurations nécessaires.

### SEUMEGNE Arielle — paiements, documents et rapports

**Périmètre :** BF-16 à BF-26 et BF-35 à BF-37.

- Construire le tableau de bord commerçant, le formulaire de paiement, la fiche paiement, l’historique avec recherche/filtres, les confirmations et les vues de solde.
- Implémenter l’enregistrement, preuves, modification/annulation auditée, recalcul des soldes et cohérence des chiffres du tableau de bord.
- Réaliser le coffre-fort documentaire : ajout, classement, consultation, métadonnées/expiration, alerte et téléchargement PDF.
- Produire les rapports mensuels/trimestriels/annuels et exports PDF/Excel ; mettre en œuvre les liens de partage sécurisés, expirables et révocables (P3).
- Préparer les interfaces et l’intégration des paiements directs MTN MoMo/Orange Money (P2) et du rapprochement des relevés (P3), avec vérification des confirmations et traitement des erreurs. Le paiement direct ne doit pas être confondu avec l’enregistrement manuel du MVP.
- Tester calcul des soldes, preuves, audit, filtres, exports, partage et scénarios d’échec des intégrations.

### TSEFACK Klein — calcul fiscal, opérations, accompagnement et règles

**Périmètre :** BF-27 à BF-34, BF-38 à BF-41 et BF-44.

- Construire les écrans de simulation, résultats et préparation de déclaration ; implémenter les calculs paramétrés, date/source de référence, avertissement obligatoire et comparaison de régimes.
- Réaliser les interfaces et traitements de saisie des ventes/dépenses, catégories, déductibilité, consultation hors ligne et synchronisation (P2), puis factures/reçus avec TVA et scan OCR (P3).
- Créer les pages FAQ/guides, l’interface de l’assistant relié à une base validée et l’annuaire consultable par ville (P2/P3).
- Réaliser dans le back-office la gestion/versionnement des régimes, impôts, taux, seuils et échéances (BF-41), ainsi que les statistiques et le tableau de supervision (BF-44).
- Tester les calculs avec cas de référence validés, l’affichage du caractère indicatif, la synchronisation, les documents produits, les permissions administrateur et l’exactitude des statistiques. Ne publier aucun taux non validé.

### NZEUTEM Eunice — clôture finale du projet uniquement

**Aucun lot fonctionnel de développement n’est attribué à Eunice : son rôle est exclusivement le travail final qui clôture le projet.** Une fois les trois lots livrés, elle prend en charge :

1. vérifier que tous les identifiants BF-01 à BF-44 et les interfaces prévues sont couverts par des tests et que les critères d’acceptation sont satisfaits ;
2. intégrer les lots, résoudre les incompatibilités d’API, de données, de navigation et de traduction, puis faire exécuter les tests unitaires, d’intégration et de bout en bout ;
3. effectuer la vérification finale de sécurité, droits d’accès, accessibilité, responsive, français/anglais, fonctionnement hors ligne et gestion des erreurs ;
4. vérifier la configuration de production, migrations, sauvegardes, secrets, supervision et procédure de retour arrière ;
5. finaliser la documentation de lancement et d’exploitation, faire la recette globale et préparer le déploiement/la livraison finale.

**Consignes communes aux trois responsables de lot :** garder les identifiants BF dans les commits/PR, livrer une fonctionnalité testée et documentée, ne jamais mettre de secret dans le dépôt, fournir les migrations et variables d’environnement nécessaires, et demander une revue croisée à un autre responsable avant de déclarer le lot terminé. Les décisions fiscales nécessitant validation officielle doivent rester identifiées comme telles.

## Feuille de route

| Phase | Contenu |
|---|---|
| **Phase 1 — MVP** | Tous les besoins P1 : compte/profil, obligations, calendrier/rappels, suivi des paiements, coffre-fort, simulateur simple et back-office de base. |
| **Phase 2** | Besoins P2 : établissements, fiches fiscales, WhatsApp/pénalités, graphiques, document PDF, déclaration, ventes/dépenses hors ligne, rapports/exports, Mobile Money, FAQ/guides et statistiques. |
| **Phase 3** | Besoins P3 : multi-utilisateurs, rapprochement automatique, changement de régime, factures/reçus, OCR, partage sécurisé, assistant conversationnel et annuaire. |
| **Clôture** | Intégration de tous les lots, recette complète, contrôles qualité, documentation et livraison finale par NZEUTEM Eunice. |

## Initialisation prévue

Ces commandes sont un point de départ à adapter dès que le squelette applicatif, les fichiers de configuration et les services auront été ajoutés au dépôt :

```bash
git clone https://github.com/eunicefelixtine/FiscCam.git
cd FiscCam
npm install
cp .env.example .env
docker compose up -d
npx prisma migrate dev
npm run dev
```

Variables prévues selon les services activés : `DATABASE_URL`, `REDIS_URL`, `AUTH_SECRET`, `SMS_API_KEY`, `STORAGE_BUCKET`, `STORAGE_ACCESS_KEY`, `STORAGE_SECRET_KEY`, `VAPID_PUBLIC_KEY`, `VAPID_PRIVATE_KEY`. Ne renseigner les identifiants réels que dans un gestionnaire de secrets ou un fichier local non versionné.
