# Architecture technique — ESMPS

## 1. Présentation générale

**ESMPS** est une application web de gestion scolaire développée pour l'École Sœur Marie Paul SAGNA (ÉSMPS), relevant du Diocèse de Ziguinchor.

L'application a pour objectif de centraliser les principales opérations administratives et pédagogiques d'un établissement scolaire, notamment :

* La gestion des élèves et des inscriptions.
* La gestion des années scolaires.
* La gestion des utilisateurs et de leurs rôles.
* Le suivi des activités pédagogiques.
* La gestion des notes et des bulletins scolaires.
* La gestion financière et le suivi des paiements.
* La gestion du transport scolaire, selon les fonctionnalités effectivement activées.

**Application en ligne :** https://ecolesmps.com
**Dépôt du code source :** https://github.com/malado04/ESMPS
**Dépôt de documentation :** https://github.com/malado04/ESMPS_Docs

> Cette documentation décrit l'organisation fonctionnelle connue du projet. Les détails techniques qui nécessitent une vérification du code source sont indiqués comme étant à confirmer.

## 2. Architecture générale

L'application repose sur une architecture web organisée autour de plusieurs composants.

```text
                    UTILISATEURS
                         |
                         v
                  Navigateur web
                         |
                         v
                  Application Laravel
                         |
             +-----------+-----------+
             |           |           |
             v           v           v
         Routes       Contrôleurs   Middleware
             |           |           |
             +-----------+-----------+
                         |
                         v
                     Modèles
                         |
                         v
                   Base de données

             Vues Blade / Interface AdminLTE
```

Ce schéma est une représentation conceptuelle. L'organisation exacte des dossiers, les relations entre les composants et les éventuels services supplémentaires doivent être confirmés à partir du dépôt source.

### 2.1. Interface utilisateur

L'interface web permet aux utilisateurs d'accéder aux fonctionnalités selon leurs rôles et leurs autorisations.

Le projet utilise les technologies suivantes pour la présentation :

* **Blade** : moteur de templates intégré à Laravel.
* **AdminLTE** : base d'interface d'administration.
* **HTML et CSS** : structure et présentation des pages.
* **JavaScript** : interactions côté navigateur.

Les versions exactes des bibliothèques doivent être vérifiées dans les fichiers de dépendances du projet.

### 2.2. Routes

Les routes définissent les URL accessibles dans l'application et associent les requêtes aux traitements correspondants.

Dans un projet Laravel, les routes web sont généralement déclarées dans :

```text
routes/
└── web.php
```

Elles peuvent notamment permettre d'accéder aux pages de gestion des élèves, des inscriptions, des années scolaires, des paiements et des autres modules.

Les routes sensibles doivent être protégées par les mécanismes d'authentification et d'autorisation appropriés.

### 2.3. Contrôleurs

Les contrôleurs reçoivent les requêtes, coordonnent les traitements et retournent les réponses destinées à l'utilisateur.

Dans Laravel, ils sont généralement organisés dans :

```text
app/
└── Http/
    └── Controllers/
```

Les contrôleurs doivent rester cohérents avec les responsabilités des modules et éviter de concentrer toute la logique métier dans une seule classe.

### 2.4. Modèles et données

Les modèles représentent les principales entités métier et permettent à l'application d'interagir avec la base de données.

Dans Laravel, ils sont généralement placés dans :

```text
app/
└── Models/
```

Les entités métier peuvent notamment concerner les élèves, les inscriptions, les utilisateurs, les années scolaires, les notes et les opérations financières.

La liste exhaustive des modèles, leurs attributs et leurs relations devra être établie à partir du code réel.

### 2.5. Vues

Les vues génèrent les pages HTML présentées aux utilisateurs.

Dans Laravel, elles se trouvent généralement dans :

```text
resources/
└── views/
```

Les vues Blade peuvent être organisées par module fonctionnel et utiliser des layouts communs afin de faciliter la maintenance de l'interface.

## 3. Architecture fonctionnelle

L'application est organisée autour de plusieurs domaines fonctionnels.

### 3.1. Gestion des utilisateurs

Le système distingue plusieurs profils d'utilisateurs.

Les rôles connus dans le projet sont :

| Identifiant indicatif | Rôle       |
| --------------------: | ---------- |
|                     0 | ADMIN      |
|                     1 | PARENT     |
|                     2 | TUTEUR     |
|                     3 | ENSEIGNANT |
|                     4 | ELEVE      |
|                     5 | CAISSE     |
|                     6 | DIRECTRICE |
|                     7 | ECONOMAT   |

Ces identifiants sont à confirmer dans les constantes, les énumérations ou les données réellement utilisées par la version déployée.

Les droits effectifs de chaque profil doivent être documentés à partir des règles d'autorisation du code. Un rôle ne garantit pas, à lui seul, qu'un utilisateur est correctement limité à ses responsabilités.

### 3.2. Gestion des années scolaires

L'année scolaire sert de référence temporelle pour organiser les activités de l'établissement.

Elle permet notamment de structurer les inscriptions et les données pédagogiques associées à une période scolaire.

Les opérations de sélection de l'année active doivent être cohérentes avec les règles métier de l'application. Il est recommandé d'éviter de dépendre uniquement de dates calculées si le système dispose d'un indicateur explicite d'année active.

### 3.3. Gestion des élèves et des inscriptions

Ce domaine regroupe les informations relatives aux élèves et à leur inscription dans l'établissement.

Les données d'inscription connues comprennent notamment :

* Le type d'inscription.
* Le statut de l'inscription.
* La décision du conseil.
* L'indication de redoublement.
* La moyenne annuelle.
* La date d'inscription.

La structure exacte des champs, leurs types et leurs règles de validation doivent être vérifiés dans les migrations et les modèles.

### 3.4. Gestion pédagogique

Le domaine pédagogique concerne le suivi scolaire et les résultats des élèves.

Il peut inclure la gestion des notes, le calcul des moyennes et la génération des bulletins.

Les bulletins du projet sont prévus au format A4 portrait. Les règles de calcul, les périodes d'évaluation et les modalités de validation doivent être documentées séparément à partir de l'implémentation effective.

### 3.5. Gestion financière

Le domaine financier concerne le suivi des montants dus, des encaissements et des éventuels soldes restant à payer.

Les règles de calcul doivent être documentées de façon à distinguer clairement :

* Le montant attendu.
* Le montant effectivement encaissé.
* Le montant restant à payer.
* La période ou l'échéance concernée.

Les modalités exactes de facturation et d'encaissement sont à confirmer dans le code source.

### 3.6. Gestion du transport scolaire

Le module de transport scolaire s'organise conceptuellement autour de l'année scolaire et des routes.

Une route peut être associée aux éléments suivants :

* Des arrêts.
* Un bus.
* Un chauffeur.
* Une tarification.

Les périodicités de tarification envisagées sont mensuelle, trimestrielle et annuelle.

La structure exacte des relations entre les routes, les bus, les chauffeurs, les arrêts et les échéances doit être vérifiée dans les modèles et les migrations.

## 4. Architecture de la base de données

L'application utilise une base de données relationnelle, dont le moteur et la version doivent être confirmés dans la configuration du projet.

Les migrations Laravel constituent une source importante pour comprendre la structure de la base.

Elles sont généralement stockées dans :

```text
database/
└── migrations/
```

Elles permettent notamment de documenter :

* Les tables et leurs colonnes.
* Les clés primaires et étrangères.
* Les contraintes d'intégrité.
* Les index.
* Les relations entre les entités.

Il est recommandé de maintenir une documentation du schéma de données à jour lorsque de nouvelles fonctionnalités sont ajoutées.

**Important :** les identifiants, mots de passe et autres secrets de connexion ne doivent jamais être publiés dans la documentation.

## 5. Authentification et autorisations

L'accès à l'application doit être réservé aux utilisateurs autorisés.

Les mécanismes à documenter et à vérifier comprennent :

* L'authentification des utilisateurs.
* La gestion des sessions.
* La protection des routes privées.
* Le contrôle des autorisations selon le rôle.
* La validation des données reçues.
* La protection contre les accès non autorisés aux données d'autres utilisateurs.

Les mécanismes réellement utilisés doivent être identifiés à partir de la configuration Laravel et du code source.

## 6. Organisation du code source

L'organisation conventionnelle d'un projet Laravel est la suivante :

```text
ESMPS/
├── app/
│   ├── Http/
│   │   └── Controllers/
│   └── Models/
├── bootstrap/
├── config/
├── database/
│   ├── migrations/
│   └── seeders/
├── public/
├── resources/
│   └── views/
├── routes/
├── storage/
├── tests/
├── .env.example
├── artisan
├── composer.json
└── README.md
```

Cette arborescence est indicative. Elle devra être adaptée si le dépôt ESMPS présente des dossiers supplémentaires, des noms différents ou une organisation personnalisée.

Le fichier `composer.json` permet notamment d'identifier les dépendances PHP et les scripts disponibles.

Le fichier `.env.example` doit décrire les variables nécessaires à la configuration, sans contenir de véritables secrets.

## 7. Déploiement et isolation des établissements

ESMPS est conçu comme une solution personnalisée pouvant être déployée séparément pour chaque établissement client.

Dans ce modèle, chaque déploiement possède sa propre instance applicative et doit disposer d'une configuration de données adaptée à cet établissement.

Cette organisation facilite la séparation opérationnelle des établissements, mais ne suffit pas à garantir une sécurité complète.

La sécurité dépend également :

* De la configuration du serveur et de la base de données.
* Des permissions sur les fichiers.
* De la gestion des comptes et des mots de passe.
* De la mise à jour des dépendances.
* De la protection des sauvegardes.
* De la configuration HTTPS.
* De la surveillance des accès et des erreurs.

Les paramètres propres à chaque établissement doivent rester distincts et ne pas être publiés dans le dépôt public.

## 8. Maintenance et évolution

Pour faciliter la maintenance, il est recommandé de :

1. Documenter chaque nouveau module.
2. Versionner les modifications avec Git.
3. Vérifier les migrations avant leur exécution en production.
4. Tester les fonctionnalités sensibles avant déploiement.
5. Sauvegarder la base de données et les fichiers nécessaires.
6. Conserver les secrets dans la configuration du serveur.
7. Documenter les procédures de restauration.
8. Maintenir une trace des changements importants dans `CHANGELOG.md`.

## 9. Points techniques à confirmer

Avant de considérer cette documentation comme une description technique définitive, il reste à vérifier dans le dépôt :

* La version exacte de Laravel et de PHP.
* Le moteur et la version de la base de données.
* Les dépendances installées.
* La méthode d'authentification.
* Les middlewares et les règles d'autorisation.
* La liste réelle des modèles, contrôleurs et migrations.
* Les mécanismes de génération des bulletins.
* Les règles de gestion des paiements.
* L'implémentation exacte du module de transport.
* La procédure de déploiement et de sauvegarde.

## 10. Conclusion

L'architecture d'ESMPS repose sur une application web Laravel structurée autour de composants de présentation, de traitement métier et d'accès aux données.

Cette documentation constitue une base de référence pour comprendre l'organisation générale du projet. Elle doit être complétée et validée à partir du code source réel afin de fournir une documentation technique fidèle à la version déployée.
