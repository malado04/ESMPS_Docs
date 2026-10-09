# Fonctionnalités de l'application ESMPS

## 1. Présentation générale

**ESMPS** est une application web de gestion scolaire développée pour l'École Sœur Marie Paul SAGNA (ÉSMPS), relevant du Diocèse de Ziguinchor.

Elle vise à centraliser les informations de l'établissement, à faciliter le suivi des élèves et à simplifier les tâches administratives, pédagogiques et financières.

L'application est accessible à l'adresse suivante :

* **Application :** https://ecolesmps.com
* **Dépôt du projet :** https://github.com/malado04/ESMPS
* **Dépôt de la documentation :** https://github.com/malado04/ESMPS_Docs

ESMPS est conçue comme une solution personnalisée. Chaque établissement client peut disposer de son propre déploiement, avec une configuration et une base de données distinctes selon l'installation.

## 2. Gestion des années scolaires

Ce module permet d'organiser les données de l'établissement par année scolaire.

Fonctionnalités :

* Gestion des années scolaires.
* Identification de l'année scolaire active.
* Association des inscriptions et des données scolaires à une année donnée.
* Consultation de l'historique des années précédentes.
* Préparation du passage à une nouvelle année scolaire.

**Point de vigilance :** les règles d'activation d'une année scolaire et les contrôles empêchant les incohérences doivent être confirmés dans le code de l'application.

## 3. Gestion des élèves

Ce module centralise les informations relatives aux élèves de l'établissement.

Fonctionnalités prévues ou à confirmer :

* Enregistrement des élèves.
* Consultation des informations individuelles.
* Modification des informations administratives.
* Recherche et consultation des dossiers.
* Association des élèves à leurs inscriptions scolaires.
* Accès à l'historique scolaire, selon les données disponibles.

Les informations personnelles doivent être accessibles uniquement aux utilisateurs autorisés.

## 4. Gestion des inscriptions

Le module des inscriptions assure le suivi administratif des élèves au sein de l'établissement.

Fonctionnalités :

* Enregistrement des inscriptions.
* Association d'une inscription à une année scolaire.
* Gestion du type d'inscription.
* Suivi du statut de l'inscription.
* Prise en compte de la décision du conseil.
* Gestion de l'information relative au redoublement.
* Enregistrement de la date d'inscription.
* Conservation des informations de moyenne annuelle, lorsque cela s'applique.

Les champs métier identifiés comprennent notamment `type_inscription`, `status`, `decision_conseil`, `is_redoublant`, `moyenne_annuelle` et `date_inscription`.

Les règles précises de validation et les différents statuts autorisés doivent être documentés à partir du code source.

## 5. Gestion pédagogique

Le suivi pédagogique permet de centraliser les informations utiles à l'évaluation des élèves.

Fonctionnalités à documenter selon l'implémentation :

* Gestion des notes.
* Consultation des résultats scolaires.
* Calcul des moyennes.
* Préparation des bulletins.
* Consultation des résultats par année scolaire.
* Prise en compte des décisions du conseil de classe.

### Bulletins scolaires

L'application prévoit la production de bulletins scolaires adaptés à l'établissement.

Les points à vérifier comprennent :

* Le format d'impression.
* Les informations affichées sur le bulletin.
* Les règles de calcul des moyennes.
* Les appréciations et décisions du conseil.
* Les contrôles d'accès à la consultation et à l'impression.

## 6. Gestion financière

Le module financier contribue au suivi des paiements liés à la scolarité.

Fonctionnalités à confirmer dans le code :

* Enregistrement des paiements.
* Consultation de l'historique des règlements.
* Suivi des montants dus et des montants encaissés.
* Calcul du reste à payer.
* Consultation de la situation financière d'un élève.
* Accès aux informations financières selon le rôle de l'utilisateur.

Les montants affichés doivent être cohérents avec les paiements réellement enregistrés. Les règles d'annulation, de correction et de traçabilité des opérations doivent également être vérifiées.

## 7. Gestion du transport scolaire

Le module de transport scolaire est destiné à organiser les moyens de transport mis à disposition des élèves.

### 7.1. Gestion des lignes ou routes

* Création et gestion des routes scolaires.
* Association des routes à une année scolaire, si cette règle est retenue dans l'application.
* Organisation des arrêts desservis.
* Consultation des informations relatives aux routes.

### 7.2. Gestion des arrêts

* Enregistrement des arrêts.
* Association des arrêts aux routes.
* Organisation de l'ordre de passage, selon les fonctionnalités disponibles.

### 7.3. Gestion des bus

* Enregistrement des bus.
* Consultation des informations relatives aux véhicules.
* Association d'un bus à une route, selon l'organisation retenue.

### 7.4. Gestion des chauffeurs

* Enregistrement des chauffeurs.
* Consultation de leurs informations.
* Association des chauffeurs aux bus ou aux routes, selon les règles de gestion définies.

### 7.5. Gestion des tarifs

Le modèle fonctionnel envisagé distingue plusieurs périodicités de paiement :

* Mensuelle.
* Trimestrielle.
* Annuelle.

Les règles de calcul des tarifs, d'affectation des élèves aux routes et de suivi des paiements du transport doivent être confirmées dans l'implémentation.

## 8. Gestion des utilisateurs et des rôles

L'application repose sur une gestion des utilisateurs et des droits d'accès.

Les rôles métier identifiés dans le projet sont :

| Rôle       | Responsabilité générale                                  |
| ---------- | -------------------------------------------------------- |
| ADMIN      | Administration générale de l'application                 |
| PARENT     | Accès aux informations autorisées concernant les enfants |
| TUTEUR     | Accès aux informations autorisées des élèves suivis      |
| ENSEIGNANT | Accès aux fonctionnalités pédagogiques autorisées        |
| ELEVE      | Accès aux informations scolaires autorisées              |
| CAISSE     | Opérations financières autorisées                        |
| DIRECTRICE | Suivi et supervision de l'établissement                  |
| ECONOMAT   | Gestion des opérations relevant de l'économat            |

Cette description est indicative. Les permissions réelles doivent être contrôlées dans le code, notamment dans les middlewares, les contrôleurs et les mécanismes d'autorisation.

## 9. Tableau de bord

Les tableaux de bord permettent aux utilisateurs de consulter des informations synthétiques adaptées à leurs responsabilités.

Les indicateurs peuvent notamment concerner :

* Les effectifs scolaires.
* Les inscriptions.
* Les paiements et les soldes.
* Les informations pédagogiques.
* Les données de transport scolaire.

La disponibilité des indicateurs varie selon le profil connecté et les modules effectivement activés.

## 10. Interface et navigation

L'application utilise une interface web construite avec Laravel Blade et AdminLTE.

Les objectifs de l'interface sont :

* Faciliter la navigation entre les modules.
* Présenter les informations sous forme de tableaux et de formulaires.
* Simplifier les opérations courantes.
* Adapter les menus et les accès aux responsabilités des utilisateurs.

Les éléments précis de l'interface dépendent de la version déployée.

## 11. Sécurité et isolation des données

Chaque déploiement destiné à un établissement doit être configuré pour séparer ses données de celles des autres établissements.

Les mesures de sécurité à vérifier et à maintenir comprennent :

* L'authentification des utilisateurs.
* Le contrôle des autorisations.
* La validation des données saisies.
* La protection des sessions.
* La sécurisation des fichiers téléversés, le cas échéant.
* La sauvegarde régulière de la base de données et des fichiers.
* La gestion sécurisée des identifiants et des variables d'environnement.
* La limitation de l'accès aux données sensibles.

**Important :** l'existence de déploiements séparés ne suffit pas, à elle seule, à garantir la sécurité. La configuration du serveur, des accès, des sauvegardes et de l'application doit également être contrôlée.

## 12. Évolutions possibles

Selon les besoins de l'établissement et l'état réel du projet, les évolutions envisageables peuvent comprendre :

* L'amélioration des rapports et statistiques.
* La génération de documents administratifs.
* Le renforcement de la traçabilité des opérations.
* L'amélioration des notifications.
* L'optimisation de la gestion du transport scolaire.
* L'amélioration des outils de suivi financier.
* La mise en place de procédures automatisées de sauvegarde et de déploiement.

Ces éléments constituent des pistes d'évolution et ne doivent pas être considérés comme des fonctionnalités déjà disponibles.

## 13. Maintenance et mise à jour

Toute évolution de l'application doit respecter les règles suivantes :

* Tester les changements avant leur mise en production.
* Vérifier les migrations de base de données.
* Sauvegarder les données avant les opérations sensibles.
* Contrôler les droits d'accès après modification.
* Documenter les changements importants dans `CHANGELOG.md`.
* Mettre à jour la documentation lorsque le comportement d'un module évolue.

## 14. Limites de cette documentation

Ce document décrit les principaux modules fonctionnels connus du projet. Il ne remplace pas une vérification complète du code source.

Avant de considérer cette liste comme exhaustive, il convient de contrôler :

* Les routes Laravel et les contrôleurs.
* Les modèles et les migrations.
* Les vues Blade et les formulaires.
* Les règles de validation.
* Les middlewares et les autorisations.
* Les fonctionnalités réellement accessibles en production.

La documentation doit être mise à jour à chaque évolution importante d'ESMPS.
