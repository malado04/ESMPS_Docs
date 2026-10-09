# Modules de l'application ESMPS

## 1. Introduction

ESMPS (École Sœur Marie Paul SAGNA) est une application web de gestion scolaire destinée à centraliser les activités administratives, pédagogiques et financières d'un établissement scolaire.

L'application est organisée en plusieurs modules fonctionnels. Chaque module répond à un besoin métier et peut interagir avec d'autres modules.

* **Application en production :** https://ecolesmps.com
* **Dépôt du code source :** https://github.com/malado04/ESMPS
* **Dépôt de la documentation :** https://github.com/malado04/ESMPS_Docs

Ce document présente les modules identifiés dans le projet. Les fonctionnalités exactes et les dépendances techniques doivent être confirmées à partir du code source.

## 2. Vue d'ensemble des modules

Les principaux domaines fonctionnels sont les suivants :

| Module                | Responsabilité principale                                  |
| --------------------- | ---------------------------------------------------------- |
| Années scolaires      | Organiser les données par année scolaire                   |
| Élèves                | Centraliser les dossiers des élèves                        |
| Inscriptions          | Gérer les inscriptions et les informations administratives |
| Pédagogie             | Suivre les notes, les moyennes et les résultats            |
| Bulletins             | Préparer et consulter les bulletins scolaires              |
| Finances              | Suivre les paiements et les soldes                         |
| Transport scolaire    | Organiser les routes, arrêts, bus, chauffeurs et tarifs    |
| Utilisateurs et rôles | Gérer les comptes et les autorisations                     |
| Tableaux de bord      | Présenter des indicateurs selon le profil connecté         |

La présence et le niveau de maturité de chaque fonctionnalité doivent être vérifiés dans la version de l'application concernée.

## 3. Module des années scolaires

### Objectif

Organiser les activités de l'établissement selon les années scolaires.

### Responsabilités

* Enregistrer les années scolaires.
* Identifier l'année scolaire active.
* Associer les inscriptions et les données concernées à une année.
* Consulter les données des années précédentes.
* Préparer le passage à une nouvelle année scolaire.

### Relations avec les autres modules

Le module des années scolaires peut être utilisé par les inscriptions, les résultats scolaires, les bulletins et les opérations financières lorsque celles-ci sont rattachées à une année donnée.

### Points de contrôle

* Une seule année scolaire active, si cette règle est imposée par l'application.
* Cohérence des dates de début et de fin.
* Préservation de l'historique des années clôturées.
* Contrôle des opérations autorisées sur une année clôturée.

## 4. Module des élèves

### Objectif

Centraliser les informations individuelles des élèves.

### Responsabilités

* Créer et consulter les dossiers.
* Modifier les informations autorisées.
* Rechercher un élève.
* Relier un élève à ses inscriptions.
* Faciliter la consultation de son historique scolaire.

### Relations avec les autres modules

Le dossier d'un élève peut être utilisé par les inscriptions, la pédagogie, les bulletins, les finances et le transport scolaire.

### Points de contrôle

* Éviter les doublons d'élèves.
* Vérifier les champs obligatoires.
* Protéger les données personnelles.
* Limiter les consultations et les modifications aux utilisateurs autorisés.

## 5. Module des inscriptions

### Objectif

Assurer le suivi administratif des inscriptions des élèves.

### Responsabilités

* Enregistrer une inscription.
* Associer l'inscription à une année scolaire.
* Gérer le type d'inscription.
* Suivre le statut administratif.
* Enregistrer les décisions du conseil.
* Conserver l'information relative au redoublement.
* Enregistrer la date d'inscription et, lorsque cela s'applique, la moyenne annuelle.

### Champs métier identifiés

Les champs suivants ont été identifiés dans le projet :

* `type_inscription`
* `status`
* `decision_conseil`
* `is_redoublant`
* `moyenne_annuelle`
* `date_inscription`

Leur signification exacte, leurs valeurs autorisées et leurs règles de validation doivent être vérifiées dans les modèles, migrations et contrôleurs.

### Relations avec les autres modules

Une inscription relie généralement un élève à une année scolaire. Elle peut également servir de référence au suivi pédagogique et financier.

## 6. Module pédagogique

### Objectif

Assurer le suivi des apprentissages et des résultats des élèves.

### Responsabilités

Selon les fonctionnalités réellement implémentées, ce module peut couvrir :

* La gestion des notes.
* La consultation des évaluations.
* Le calcul des moyennes.
* La consultation des résultats.
* Le suivi des décisions du conseil.
* La préparation des données nécessaires aux bulletins.

### Relations avec les autres modules

Le module pédagogique dépend des données des élèves et, selon les règles de gestion, de leurs inscriptions et de leur année scolaire.

### Points de contrôle

* Cohérence des notes et des coefficients.
* Exactitude des calculs de moyenne.
* Respect des règles de validation.
* Contrôle des droits de saisie et de consultation.

## 7. Module des bulletins scolaires

### Objectif

Présenter les résultats scolaires d'un élève dans un document structuré.

### Responsabilités

* Rassembler les résultats disponibles.
* Présenter les notes et les moyennes.
* Afficher les informations scolaires nécessaires.
* Présenter les appréciations ou décisions lorsqu'elles sont disponibles.
* Permettre la consultation et l'impression des bulletins, selon les fonctionnalités existantes.

### Relations avec les autres modules

Le bulletin exploite les informations des élèves, des inscriptions et du suivi pédagogique.

### Points de contrôle

* Correspondance entre le bulletin et les résultats enregistrés.
* Respect du format d'impression défini par l'établissement.
* Vérification des informations affichées.
* Limitation de l'accès aux utilisateurs autorisés.

## 8. Module financier

### Objectif

Faciliter le suivi des paiements et des situations financières liés à la scolarité.

### Responsabilités

* Enregistrer les règlements.
* Consulter l'historique des paiements.
* Suivre les montants dus et encaissés.
* Calculer ou consulter les soldes restants.
* Afficher la situation financière selon les droits de l'utilisateur.

### Relations avec les autres modules

Le module financier utilise les informations des élèves et peut s'appuyer sur leurs inscriptions pour rattacher les opérations à une année scolaire.

### Points de contrôle

* Exactitude des montants.
* Cohérence entre paiements, montants dus et soldes.
* Traçabilité des corrections et annulations.
* Restriction de l'accès aux données financières.

## 9. Module de transport scolaire

### Objectif

Organiser les moyens de transport scolaire et les informations associées.

### 9.1. Routes scolaires

Responsabilités :

* Créer et consulter les routes.
* Organiser les arrêts desservis.
* Associer les ressources de transport aux routes selon les règles définies.

### 9.2. Arrêts

Responsabilités :

* Enregistrer les arrêts.
* Les associer aux routes.
* Définir leur ordre de passage si cette fonctionnalité est disponible.

### 9.3. Bus

Responsabilités :

* Enregistrer les véhicules.
* Consulter leurs informations.
* Organiser leur affectation aux routes.

### 9.4. Chauffeurs

Responsabilités :

* Enregistrer les chauffeurs.
* Consulter leurs informations.
* Associer les chauffeurs aux véhicules ou aux routes selon les règles de gestion.

### 9.5. Tarification

Le modèle fonctionnel envisagé prévoit les périodicités suivantes :

* Mensuelle.
* Trimestrielle.
* Annuelle.

Les règles de calcul, les dates d'échéance et les éventuels paiements liés au transport doivent être documentés après vérification de l'implémentation.

### Relations entre les entités

Le modèle métier envisagé est le suivant :

```text
Année scolaire
    |
    v
Route scolaire
    |
    +---- Arrêts
    |
    +---- Bus
    |      |
    |      +---- Chauffeur (selon les règles d'affectation)
    |
    +---- Tarification
           |
           +---- Mensuelle
           +---- Trimestrielle
           +---- Annuelle
```

Ce schéma représente une organisation fonctionnelle cible. Il ne garantit pas que toutes ces relations soient déjà présentes dans la base de données.

## 10. Module des utilisateurs et des rôles

### Objectif

Contrôler l'accès aux fonctionnalités et aux données de l'application.

### Rôles métier identifiés

| Rôle       | Responsabilité générale                                    |
| ---------- | ---------------------------------------------------------- |
| ADMIN      | Administration de l'application                            |
| PARENT     | Consultation des informations autorisées des enfants       |
| TUTEUR     | Consultation des informations autorisées des élèves suivis |
| ENSEIGNANT | Accès aux fonctionnalités pédagogiques autorisées          |
| ELEVE      | Consultation des informations scolaires autorisées         |
| CAISSE     | Accès aux opérations financières autorisées                |
| DIRECTRICE | Suivi et supervision de l'établissement                    |
| ECONOMAT   | Gestion des opérations relevant de l'économat              |

Les permissions exactes doivent être vérifiées dans les mécanismes d'autorisation de l'application.

### Points de contrôle

* Protection des routes.
* Vérification des autorisations côté serveur.
* Séparation des responsabilités.
* Protection des comptes et des sessions.
* Limitation des accès aux données sensibles.

## 11. Module des tableaux de bord

### Objectif

Présenter une synthèse des informations utiles à chaque profil.

### Responsabilités possibles

* Afficher les effectifs.
* Présenter des indicateurs sur les inscriptions.
* Résumer les paiements et les soldes.
* Faciliter l'accès aux modules autorisés.
* Présenter les informations pédagogiques pertinentes.

Les indicateurs disponibles dépendent du code et du rôle de l'utilisateur connecté.

## 12. Relations générales entre les modules

Le schéma ci-dessous présente les dépendances métier générales :

```text
                    ANNÉES SCOLAIRES
                           |
                           v
                         ÉLÈVES
                           |
                           v
                      INSCRIPTIONS
                      /     |      \
                     v      v       v
              PÉDAGOGIE  FINANCES  TRANSPORT
                     |
                     v
                  BULLETINS

           UTILISATEURS ET RÔLES
                    |
                    v
         Contrôle des accès aux modules

             TABLEAUX DE BORD
                    |
                    v
        Synthèse des données autorisées
```

Ce schéma est conceptuel : les relations techniques réelles doivent être confirmées dans les modèles, les migrations et les contrôleurs Laravel.

## 13. Principes communs aux modules

Les modules doivent respecter les principes suivants :

1. **Cohérence des données :** les opérations doivent préserver l'intégrité des informations.
2. **Contrôle des accès :** chaque utilisateur ne doit accéder qu'aux fonctionnalités autorisées.
3. **Traçabilité :** les opérations sensibles doivent pouvoir être contrôlées selon les mécanismes disponibles.
4. **Maintenabilité :** les évolutions doivent rester cohérentes avec l'architecture Laravel.
5. **Préservation de l'historique :** les données des années précédentes doivent être conservées selon les règles métier.
6. **Documentation :** les changements importants doivent être répercutés dans les fichiers de documentation.

## 14. Vérification technique

Pour finaliser ce document, il convient de rapprocher chaque module des éléments correspondants dans le dépôt :

* Routes Laravel.
* Contrôleurs.
* Modèles Eloquent.
* Migrations et relations de base de données.
* Vues Blade.
* Middlewares et règles d'autorisation.
* Tests automatisés, lorsqu'ils existent.

Les modules ou fonctionnalités non confirmés doivent rester identifiés comme tels jusqu'à leur vérification.
