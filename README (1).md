# TaxSuivi : Gestion fiscale pour commerçants camerounais

Application web progressive (PWA) permettant aux commerçants du Cameroun de suivre leurs obligations fiscales, leurs échéances et l'ensemble de leurs paiements d'impôts et taxes.

> **Avertissement** : les montants et simulations fournis par l'application sont des estimations non officielles. Les règles fiscales doivent être validées par rapport à la loi de finances en vigueur et aux textes de la Direction Générale des Impôts (DGI).

---

## Sommaire

1. [Contexte et objectifs](#1-contexte-et-objectifs)
2. [Hypothèses et périmètre](#2-hypothèses-et-périmètre)
3. [Utilisateurs cibles](#3-utilisateurs-cibles)
4. [Besoins fonctionnels](#4-besoins-fonctionnels)
5. [Besoins non fonctionnels](#5-besoins-non-fonctionnels)
6. [Technologies utilisées](#6-technologies-utilisées)
7. [Architecture générale](#7-architecture-générale)
8. [Modèle de données (aperçu)](#8-modèle-de-données-aperçu)
9. [Structure du projet](#9-structure-du-projet)
10. [Installation et lancement](#10-installation-et-lancement)
11. [Feuille de route](#11-feuille-de-route)
12. [Questions ouvertes](#12-questions-ouvertes)

---

## 1. Contexte et objectifs

De nombreux commerçants camerounais connaissent mal leurs obligations fiscales, oublient des échéances ou perdent leurs quittances, ce qui entraîne des pénalités. L'application vise à :

- centraliser toutes les obligations fiscales d'un commerçant ;
- le prévenir avant chaque échéance ;
- conserver l'historique complet de ses paiements avec les preuves ;
- lui donner une estimation simple de ce qu'il doit payer.

## 2. Hypothèses et périmètre

- L'application **suit** les paiements, elle ne les **effectue pas** (le paiement se fait auprès de la banque, du guichet ou des canaux officiels, puis il est enregistré dans l'application). Le paiement intégré par Mobile Money est une évolution ultérieure.
- Cible prioritaire : petits et moyens commerçants (Impôt Général Synthétique en priorité, puis régime du réel simplifié).
- Plateforme : application web responsive installable (PWA) pour les commerçants, et interface d'administration web.

**Légende des priorités** : **P1** = indispensable (MVP) · **P2** = important · **P3** = évolution.

## 3. Utilisateurs cibles

| Acteur | Description |
|---|---|
| **Commerçant** | Utilisateur principal : enregistre son profil, ses paiements et ses documents |
| **Comptable** (P3) | Accès délégué aux données d'un ou plusieurs commerçants |
| **Administrateur** | Gère les règles fiscales, les utilisateurs et les communications |

## 4. Besoins fonctionnels

### 4.1 Compte et profil fiscal

| ID | Besoin | Priorité | Critère d'acceptation |
|---|---|---|---|
| BF-01 | Inscription et connexion par numéro de téléphone avec code OTP reçu par SMS | P1 | Un utilisateur crée son compte avec un numéro camerounais valide et un code reçu par SMS |
| BF-02 | Création du profil commerçant : nom ou raison sociale, NIU, activité, ville, centre des impôts de rattachement | P1 | Le format du NIU est contrôlé et les champs obligatoires sont signalés |
| BF-03 | Choix du régime fiscal (IGS, réel simplifié, réel) | P1 | Le régime choisi détermine la liste des obligations |
| BF-04 | Modification du profil et déconnexion sécurisée | P1 | Les modifications sont historisées |
| BF-05 | Gestion de plusieurs établissements ou points de vente par commerçant | P2 | Chaque établissement a ses propres obligations et paiements |
| BF-06 | Comptes multi-utilisateurs avec rôles (propriétaire, comptable, caissier) | P3 | Les droits d'accès dépendent du rôle |

### 4.2 Obligations fiscales

| ID | Besoin | Priorité | Critère d'acceptation |
|---|---|---|---|
| BF-07 | Génération automatique des obligations de l'année selon le régime (impôt, période, échéance, montant estimé) | P1 | À la fin de l'inscription, l'utilisateur voit sa liste d'obligations |
| BF-08 | Ajout manuel d'une obligation (taxe communale, par exemple) | P1 | L'obligation apparaît dans le calendrier et le suivi |
| BF-09 | Statuts : à venir, à payer, payé, partiellement payé, en retard | P1 | Le statut se met à jour automatiquement selon les paiements et la date du jour |
| BF-10 | Fiches explicatives en langage simple sur chaque impôt (qui, quand, combien, comment payer) en français et en anglais | P2 | Chaque obligation renvoie vers sa fiche |

### 4.3 Calendrier et rappels

| ID | Besoin | Priorité | Critère d'acceptation |
|---|---|---|---|
| BF-11 | Vue calendrier et vue liste des échéances | P1 | Filtrage par mois et par impôt |
| BF-12 | Rappels automatiques par SMS et notification push à J-7, J-3 et J-1 | P1 | Un rappel est envoyé une seule fois par échéance et par palier |
| BF-13 | Alerte de retard | P1 | Un message part le lendemain d'une échéance non soldée |
| BF-14 | Rappels par WhatsApp | P2 | Canal activable dans les préférences de notification |
| BF-15 | Estimation des pénalités encourues en cas de retard | P2 | Montant estimé affiché sur l'obligation en retard |

### 4.4 Suivi des paiements (cœur du système)

| ID | Besoin | Priorité | Critère d'acceptation |
|---|---|---|---|
| BF-16 | Enregistrer un paiement : obligation, montant, date, mode (espèces, banque, Mobile Money), référence | P1 | Le paiement est lié à une obligation et le solde restant est recalculé |
| BF-17 | Joindre une preuve (photo ou PDF de la quittance) | P1 | Le fichier est consultable depuis la fiche du paiement |
| BF-18 | Modifier ou annuler un paiement avec trace | P1 | L'historique indique qui a modifié quoi et quand |
| BF-19 | Historique avec recherche et filtres (impôt, période, statut) | P1 | Résultats affichés en moins de 2 secondes |
| BF-20 | Tableau de bord : total payé, reste à payer, prochaine échéance, nombre d'obligations en retard | P1 | Les chiffres correspondent à l'historique |
| BF-21 | Évolution des paiements par année et par impôt (graphiques) | P2 | Graphiques consultables sur le tableau de bord |
| BF-22 | Paiement direct via Mobile Money (MTN MoMo, Orange Money) | P2 | Paiement confirmé et enregistré automatiquement |
| BF-23 | Rapprochement automatique avec les relevés Mobile Money | P3 | Les paiements détectés sont proposés à l'association |

### 4.5 Coffre-fort de documents

| ID | Besoin | Priorité | Critère d'acceptation |
|---|---|---|---|
| BF-24 | Ajouter, classer et consulter des documents (attestation, patente, déclaration, quittance) | P1 | Chaque document a un type, une date et éventuellement une date d'expiration |
| BF-25 | Alerte avant expiration d'un document | P1 | Rappel 30 jours avant l'expiration |
| BF-26 | Export des documents en PDF | P2 | Téléchargement depuis la fiche du document |

### 4.6 Calcul et simulation

| ID | Besoin | Priorité | Critère d'acceptation |
|---|---|---|---|
| BF-27 | Estimer l'impôt à partir du chiffre d'affaires saisi, selon le régime | P1 | Le résultat s'appuie sur des taux paramétrés et affiche la date de référence de la loi de finances |
| BF-28 | Mention « estimation non officielle » sur chaque résultat | P1 | Mention visible à chaque simulation |
| BF-29 | Aide à la préparation des déclarations avec pré-remplissage des montants | P2 | Récapitulatif prêt à reporter sur la déclaration officielle |
| BF-30 | Simulation d'un changement de régime fiscal | P3 | Comparaison chiffrée entre deux régimes |

### 4.7 Ventes et dépenses (base de calcul)

| ID | Besoin | Priorité | Critère d'acceptation |
|---|---|---|---|
| BF-31 | Saisie simplifiée des ventes et des dépenses, y compris hors ligne | P2 | Les opérations saisies hors ligne sont synchronisées au retour de la connexion |
| BF-32 | Catégorisation des opérations et distinction des dépenses déductibles | P2 | Total des dépenses déductibles calculé par période |
| BF-33 | Génération de factures et de reçus avec calcul automatique de la TVA | P3 | Facture exportable en PDF |
| BF-34 | Scan de factures par OCR | P3 | Champs principaux pré-remplis à partir de la photo |

### 4.8 Rapports et exports

| ID | Besoin | Priorité | Critère d'acceptation |
|---|---|---|---|
| BF-35 | Rapport récapitulatif des paiements par période (mensuel, trimestriel, annuel) | P2 | Rapport cohérent avec l'historique |
| BF-36 | Export PDF et Excel | P2 | Fichiers téléchargeables |
| BF-37 | Partage sécurisé avec un comptable ou un agent des impôts | P3 | Lien à durée limitée et révocable |

### 4.9 Assistance et accompagnement

| ID | Besoin | Priorité | Critère d'acceptation |
|---|---|---|---|
| BF-38 | FAQ et guides pas à pas (obtenir un NIU, faire une déclaration, payer une patente) | P2 | Contenu accessible depuis le menu et depuis les fiches d'impôt |
| BF-39 | Assistant conversationnel pour les questions fiscales courantes | P3 | Réponses basées sur une base de connaissances validée |
| BF-40 | Annuaire des centres des impôts et des centres de gestion agréés | P3 | Recherche par ville |

### 4.10 Administration (back-office)

| ID | Besoin | Priorité | Critère d'acceptation |
|---|---|---|---|
| BF-41 | Gérer les régimes, types d'impôts, taux, seuils et échéances | P1 | Modification sans toucher au code, avec historique des versions |
| BF-42 | Gérer les comptes utilisateurs (consultation, suspension) | P1 | Action journalisée |
| BF-43 | Envoyer un message à tous les utilisateurs ou à un groupe | P1 | Message diffusé par notification ou SMS |
| BF-44 | Statistiques d'usage et tableau de bord de supervision | P2 | Nombre d'utilisateurs, de paiements enregistrés, de rappels envoyés |

## 5. Besoins non fonctionnels

| Domaine | Exigence |
|---|---|
| **Hors ligne** | Consultation de l'historique et enregistrement d'un paiement sans connexion, avec synchronisation ensuite |
| **Performance** | Interface fluide sur un Android d'entrée de gamme avec connexion lente ; poids des pages réduit |
| **Simplicité** | Interface épurée, icônes explicites, vocabulaire courant plutôt que jargon fiscal |
| **Langues** | Français et anglais |
| **Sécurité** | Chiffrement en transit (HTTPS) et au repos, authentification, contrôle d'accès par rôle, journal d'audit |
| **Confidentialité** | Conformité à la loi camerounaise sur la protection des données personnelles |
| **Disponibilité** | Sauvegardes régulières de la base et des fichiers |
| **Fiabilité fiscale** | Règles paramétrables et versionnées, validées par rapport à la loi de finances en vigueur |
| **Accessibilité** | Contrastes suffisants, tailles de texte lisibles, navigation au clavier |

## 6. Technologies utilisées

### Front-end
| Technologie | Rôle |
|---|---|
| **React** | Bibliothèque d'interface |
| **Next.js** (App Router) | Framework full-stack : rendu serveur, routage, API Routes / Server Actions |
| **TypeScript** | Typage statique |
| **Tailwind CSS** | Mise en forme |
| **shadcn/ui** (Radix UI) | Composants accessibles |
| **React Hook Form** + **Zod** | Formulaires et validation partagée client/serveur |
| **TanStack Query** | Gestion des données serveur et du cache |
| **next-intl** | Internationalisation (français / anglais) |
| **Recharts** | Graphiques du tableau de bord |
| **date-fns** | Manipulation des dates et du calendrier |

### PWA et mode hors ligne
| Technologie | Rôle |
|---|---|
| **Serwist** (ou next-pwa) | Service Worker et mise en cache |
| **Dexie.js** (IndexedDB) | Stockage local des données hors ligne |
| **Web Push API** | Notifications push |

### Back-end
| Technologie | Rôle |
|---|---|
| **Next.js Route Handlers / Server Actions** | API applicative |
| **PostgreSQL** | Base de données relationnelle |
| **Prisma** | ORM et migrations |
| **Auth.js** (ou implémentation OTP dédiée) | Sessions et authentification |
| **Redis** + **BullMQ** | File de tâches : rappels planifiés, envois de SMS |
| **Stockage compatible S3** (Cloudflare R2, MinIO ou Supabase Storage) | Quittances et documents |

### Services externes
| Service | Rôle |
|---|---|
| **Passerelle SMS** (Africa's Talking, Twilio ou opérateur local) | Codes OTP et rappels |
| **WhatsApp Business API** (P2) | Rappels WhatsApp |
| **API Mobile Money** : MTN MoMo, Orange Money (P2) | Paiement et rapprochement |
| **Tesseract.js** ou service OCR (P3) | Lecture des factures |

### Export de documents
| Technologie | Rôle |
|---|---|
| **@react-pdf/renderer** | Génération de PDF |
| **ExcelJS** | Export Excel |

### Qualité, tests et DevOps
| Outil | Rôle |
|---|---|
| **ESLint** + **Prettier** | Qualité et style du code |
| **Vitest** + **Testing Library** | Tests unitaires et composants |
| **Playwright** | Tests de bout en bout |
| **Docker** / Docker Compose | Environnement reproductible |
| **GitHub Actions** | Intégration et déploiement continus |
| **Sentry** | Suivi des erreurs |
| **Vercel** ou **VPS** (Docker + Nginx) | Hébergement |

## 7. Architecture générale

```
┌──────────────────────────┐
│  Navigateur / PWA        │
│  React + Next.js         │
│  Service Worker + Dexie  │◄── mode hors ligne
└────────────┬─────────────┘
             │ HTTPS
┌────────────▼─────────────┐        ┌────────────────────┐
│  Next.js (serveur)       │───────►│  Passerelle SMS    │
│  Server Actions / API    │        │  Push / WhatsApp   │
└───┬────────┬─────────┬───┘        └────────────────────┘
    │        │         │
┌───▼───┐ ┌──▼────┐ ┌──▼───────────┐
│Postgre│ │ Redis │ │ Stockage S3  │
│  SQL  │ │BullMQ │ │ (documents)  │
└───────┘ └───────┘ └──────────────┘
```

## 8. Modèle de données (aperçu)

Entités principales (à affiner lors de la conception) :

- **User** : id, téléphone, rôle, langue, préférences de notification
- **Merchant** : id, userId, raison sociale, NIU, activité, ville, centre des impôts, régime fiscal
- **TaxRegime** : id, nom, règles et seuils (versionnés)
- **TaxType** : id, nom, périodicité, taux, règle d'échéance (versionnés)
- **Obligation** : id, merchantId, taxTypeId, période, échéance, montant estimé, statut
- **Payment** : id, obligationId, montant, date, mode, référence, preuve
- **Document** : id, merchantId, type, fichier, date, date d'expiration
- **Reminder** : id, obligationId ou documentId, palier, canal, statut d'envoi
- **AuditLog** : id, acteur, action, entité, date, anciennes et nouvelles valeurs
- **Sale / Expense** (P2) : id, merchantId, montant, catégorie, déductible, date

## 9. Structure du projet

```
taxsuivi/
├── prisma/                 # Schéma et migrations
├── public/                 # Icônes, manifest PWA
├── src/
│   ├── app/                # Routes Next.js (App Router)
│   │   ├── (auth)/         # Connexion, inscription OTP
│   │   ├── (app)/          # Tableau de bord, obligations, paiements, documents
│   │   ├── admin/          # Back-office
│   │   └── api/            # Route Handlers (webhooks, cron)
│   ├── components/         # Composants UI
│   ├── lib/                # Utilitaires, client Prisma, validations Zod
│   ├── server/             # Logique métier : fiscalité, rappels, paiements
│   ├── offline/            # Dexie, synchronisation
│   ├── messages/           # Traductions fr / en
│   └── tests/
├── docker-compose.yml
└── README.md
```

## 10. Installation et lancement

> Section à compléter une fois le projet initialisé.

```bash
# Cloner le dépôt
git clone <url-du-depot>
cd taxsuivi

# Installer les dépendances
npm install

# Configurer l'environnement
cp .env.example .env

# Lancer PostgreSQL et Redis
docker compose up -d

# Appliquer les migrations
npx prisma migrate dev

# Démarrer en développement
npm run dev
```

Variables d'environnement prévues : `DATABASE_URL`, `REDIS_URL`, `AUTH_SECRET`, `SMS_API_KEY`, `STORAGE_BUCKET`, `STORAGE_ACCESS_KEY`, `STORAGE_SECRET_KEY`, `VAPID_PUBLIC_KEY`, `VAPID_PRIVATE_KEY`.

## 11. Feuille de route

| Phase | Contenu |
|---|---|
| **Phase 1 : MVP** | Tous les besoins P1 : compte et profil, obligations, calendrier et rappels, suivi des paiements, coffre-fort, simulateur simple, back-office de base |
| **Phase 2** | Besoins P2 : ventes et dépenses, rapports et exports, Mobile Money, WhatsApp, fiches et FAQ, statistiques |
| **Phase 3** | Besoins P3 : multi-utilisateurs, factures, OCR, assistant conversationnel, partage sécurisé, rapprochement automatique |

## 12. Questions ouvertes

1. L'application doit-elle seulement **suivre** les paiements ou aussi les **effectuer** ?
2. Quelle est la cible exacte : petits commerçants informels (IGS), PME au réel, ou les deux ?
3. Existe-t-il un partenaire institutionnel (DGI, mairie, banque) avec lequel s'intégrer ?
4. Quel fournisseur SMS utiliser pour le Cameroun (coût, fiabilité, délivrabilité) ?
5. Quelles sources officielles feront foi pour les taux et les échéances (loi de finances de l'année, circulaires de la DGI) ?
