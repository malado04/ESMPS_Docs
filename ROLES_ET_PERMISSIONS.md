# Rôles et permissions — ESMPS

## 1. Présentation

L'application ESMPS utilise un système de rôles pour organiser les accès aux fonctionnalités de gestion scolaire.

Chaque utilisateur possède un profil correspondant à ses responsabilités dans l'établissement. Les permissions doivent permettre de limiter l'accès aux informations et aux opérations sensibles, notamment les données personnelles des élèves, les résultats scolaires et les informations financières.

**Application :** https://ecolesmps.com

**Dépôt du code source :** https://github.com/malado04/ESMPS

Ce document décrit les rôles métier identifiés dans le projet. Les permissions exactes doivent être confirmées dans le code source avant d'être considérées comme la configuration officielle.

## 2. Rôles identifiés

Les identifiants métier connus sont les suivants :

| Code | Rôle       | Description                                            |
| ---: | ---------- | ------------------------------------------------------ |
|    0 | ADMIN      | Administrateur de l'application                        |
|    1 | PARENT     | Parent d'élève                                         |
|    2 | TUTEUR     | Tuteur d'élève                                         |
|    3 | ENSEIGNANT | Personnel enseignant                                   |
|    4 | ELEVE      | Élève                                                  |
|    5 | CAISSE     | Personnel chargé des opérations de caisse              |
|    6 | DIRECTRICE | Direction de l'établissement                           |
|    7 | ECONOMAT   | Personnel chargé des opérations relevant de l'économat |

Ces valeurs doivent être vérifiées dans l'énumération, le modèle utilisateur ou la configuration effectivement utilisée par l'application.

**Important :** les numéros ci-dessus représentent les codes métier connus. Ils ne constituent pas, à eux seuls, une preuve des autorisations réellement accordées à chaque rôle.

## 3. Description des rôles

### 3.1. ADMIN — Administrateur

L'administrateur assure la gestion technique et fonctionnelle de l'application.

Responsabilités possibles :

* Administrer les comptes utilisateurs.
* Gérer les rôles et les accès.
* Configurer les paramètres généraux.
* Administrer les données de référence.
* Accéder aux modules autorisés par la configuration.

Les droits réels du rôle ADMIN doivent être vérifiés. Un compte administrateur ne doit pas être partagé entre plusieurs personnes.

### 3.2. PARENT — Parent d'élève

Le parent consulte les informations scolaires et administratives de ses enfants, selon les fonctionnalités disponibles.

Accès métier envisagés :

* Consulter les informations des enfants associés à son compte.
* Consulter les résultats scolaires autorisés.
* Consulter les bulletins disponibles.
* Consulter les informations financières autorisées.
* Consulter les informations relatives au transport scolaire, si cette fonctionnalité est activée.

Un parent ne doit pas pouvoir consulter les dossiers d'enfants qui ne lui sont pas associés.

### 3.3. TUTEUR — Tuteur d'élève

Le tuteur accède aux informations des élèves dont il assure le suivi, conformément aux associations enregistrées dans l'application.

Accès métier envisagés :

* Consulter les dossiers des élèves suivis.
* Consulter les informations scolaires autorisées.
* Consulter les bulletins disponibles.
* Consulter les informations financières ou de transport lorsque cela est autorisé.

Les droits du tuteur doivent être distingués de ceux du parent si l'application prévoit des règles différentes.

### 3.4. ENSEIGNANT — Enseignant

L'enseignant intervient dans le suivi pédagogique des élèves.

Accès métier envisagés :

* Consulter les informations nécessaires à ses activités pédagogiques.
* Saisir ou modifier les notes dans les limites autorisées.
* Consulter les résultats des élèves relevant de ses responsabilités.
* Participer aux opérations pédagogiques autorisées.

L'enseignant ne doit pas obtenir automatiquement l'accès à la caisse, à l'administration des comptes ou à l'ensemble des données personnelles et financières.

### 3.5. ELEVE — Élève

L'élève consulte les informations scolaires auxquelles son compte lui donne accès.

Accès métier envisagés :

* Consulter ses propres informations autorisées.
* Consulter ses résultats scolaires.
* Consulter ses bulletins disponibles.
* Accéder aux informations utiles à sa scolarité.

Un élève ne doit pas pouvoir consulter ou modifier le dossier d'un autre élève.

### 3.6. CAISSE — Personnel de caisse

Le rôle CAISSE concerne les opérations financières relevant de la caisse.

Accès métier envisagés :

* Consulter les informations nécessaires à l'encaissement.
* Enregistrer les paiements autorisés.
* Consulter les historiques de règlements nécessaires au travail.
* Consulter les montants dus et les soldes.
* Produire les documents de paiement disponibles.

Les corrections, annulations et suppressions de paiements doivent être encadrées. Les opérations financières sensibles devraient être traçables.

### 3.7. DIRECTRICE — Direction

La direction assure le suivi et la supervision des activités de l'établissement.

Accès métier envisagés :

* Consulter les indicateurs de gestion.
* Suivre les effectifs et les inscriptions.
* Consulter les informations pédagogiques nécessaires à la supervision.
* Consulter les données financières autorisées.
* Superviser les activités de l'établissement.

Les droits de la direction doivent être définis selon les responsabilités réelles et ne doivent pas être déduits uniquement de l'intitulé du rôle.

### 3.8. ECONOMAT — Économat

Le rôle ECONOMAT concerne les activités de gestion relevant de l'économat.

Les accès métier envisageables dépendent des fonctionnalités réellement présentes :

* Consulter les données nécessaires aux opérations de l'économat.
* Gérer les informations de son périmètre.
* Consulter les états et rapports autorisés.

Les permissions précises doivent être établies à partir des écrans, contrôleurs et règles métier du projet.

## 4. Matrice indicative des permissions

La matrice suivante constitue une proposition de référence pour l'analyse des accès. Elle ne représente pas une extraction des permissions réellement configurées.

| Fonctionnalité                    | ADMIN      | PARENT                       | TUTEUR                       | ENSEIGNANT          | ELEVE                | CAISSE                         | DIRECTRICE | ECONOMAT     |
| --------------------------------- | ---------- | ---------------------------- | ---------------------------- | ------------------- | -------------------- | ------------------------------ | ---------- | ------------ |
| Administration des utilisateurs   | À vérifier | Non prévu                    | Non prévu                    | Non prévu           | Non prévu            | Non prévu                      | À vérifier | À vérifier   |
| Gestion des années scolaires      | À vérifier | Non prévu                    | Non prévu                    | À vérifier          | Non prévu            | Non prévu                      | À vérifier | À vérifier   |
| Gestion des dossiers élèves       | À vérifier | Lecture limitée              | Lecture limitée              | Lecture pédagogique | Lecture personnelle  | Lecture nécessaire à la caisse | À vérifier | Selon besoin |
| Gestion des inscriptions          | À vérifier | Lecture limitée              | Lecture limitée              | Selon besoin        | Lecture personnelle  | Selon besoin                   | À vérifier | Selon besoin |
| Saisie des notes                  | À vérifier | Non prévu                    | Non prévu                    | Selon affectation   | Non prévu            | Non prévu                      | À vérifier | Non prévu    |
| Consultation des bulletins        | À vérifier | Enfants associés             | Élèves suivis                | Selon besoin        | Bulletin personnel   | Non prévu                      | À vérifier | Non prévu    |
| Enregistrement des paiements      | À vérifier | Non prévu                    | Non prévu                    | Non prévu           | Non prévu            | À vérifier                     | À vérifier | À vérifier   |
| Consultation des états financiers | À vérifier | Lecture limitée si autorisée | Lecture limitée si autorisée | Non prévu           | Non prévu            | Selon responsabilité           | À vérifier | À vérifier   |
| Gestion du transport scolaire     | À vérifier | Lecture si autorisée         | Lecture si autorisée         | Selon besoin        | Lecture si autorisée | Selon besoin                   | À vérifier | À vérifier   |

**Légende :**

* **À vérifier :** permission à confirmer dans le code.
* **Lecture limitée :** accès uniquement aux informations liées aux personnes concernées.
* **Selon besoin :** accès limité aux responsabilités professionnelles.
* **Non prévu :** accès normalement déconseillé pour ce rôle, sauf exigence métier explicitement définie.

Les mentions « Non prévu » et « Lecture limitée » sont des recommandations de conception, et non la preuve que l'application bloque déjà ces accès.

## 5. Principes de sécurité

### 5.1. Principe du moindre privilège

Chaque utilisateur doit disposer uniquement des permissions nécessaires à ses fonctions.

### 5.2. Vérification côté serveur

Masquer un bouton dans une vue Blade ne suffit pas à protéger une fonctionnalité.

Les contrôleurs, middlewares, politiques d'autorisation ou mécanismes équivalents doivent également empêcher les opérations non autorisées.

### 5.3. Protection des données individuelles

Les parents, tuteurs et élèves doivent être limités aux dossiers correspondant à leurs associations et à leurs droits.

Cette vérification doit être effectuée côté serveur, y compris lorsqu'un identifiant d'élève est transmis directement dans une URL ou une requête.

### 5.4. Protection des opérations financières

Les opérations de caisse doivent respecter les permissions définies. Les corrections, annulations et suppressions doivent être encadrées et traçables selon les mécanismes disponibles.

### 5.5. Protection des comptes

Les mots de passe ne doivent jamais être stockés en clair. Les comptes sensibles doivent être attribués individuellement et les accès inutilisés doivent être désactivés selon les procédures de l'établissement.

## 6. Vérifications à effectuer dans le code Laravel

Pour établir la matrice officielle, examiner les éléments suivants dans le dépôt ESMPS :

1. **Modèle utilisateur :** vérifier la manière dont le rôle est enregistré.
2. **Énumération ou constantes :** confirmer les identifiants 0 à 7.
3. **Routes :** identifier les routes protégées et leurs middlewares.
4. **Contrôleurs :** vérifier les contrôles d'accès avant chaque opération sensible.
5. **Vues Blade :** repérer les menus et boutons affichés selon les rôles.
6. **Policies et Gates :** contrôler les règles d'autorisation si elles sont utilisées.
7. **Tests :** vérifier que les accès autorisés et interdits sont testés.

### Exemple de contrôle à effectuer

Pour chaque module, répondre aux questions suivantes :

* Qui peut consulter les données ?
* Qui peut créer un enregistrement ?
* Qui peut modifier un enregistrement ?
* Qui peut supprimer ou annuler une opération ?
* Les restrictions sont-elles vérifiées côté serveur ?
* Les données sont-elles limitées au bon élève, à la bonne année scolaire et au bon établissement ?

## 7. Évolution des rôles et permissions

Toute modification des rôles doit être documentée et testée.

Lorsqu'un rôle est ajouté, renommé ou supprimé, vérifier :

* Les constantes et énumérations.
* Les comptes existants.
* Les menus de navigation.
* Les routes et contrôleurs.
* Les mécanismes d'autorisation.
* Les tests automatisés.
* Les éventuelles données de production concernées.

Les changements importants doivent être consignés dans `CHANGELOG.md`.

## 8. Conclusion

Les rôles d'ESMPS permettent de structurer les accès selon les responsabilités des utilisateurs. La sécurité effective dépend toutefois des contrôles réalisés dans l'application, et non du seul nom ou numéro attribué à un rôle.

Ce document doit être actualisé après vérification du code source afin de remplacer les permissions indicatives par les permissions réellement implémentées.
