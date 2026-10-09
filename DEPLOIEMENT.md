# Guide de déploiement — ESMPS

## 1. Objectif

Ce document décrit les étapes à suivre pour déployer ou mettre à jour l'application ESMPS sur un serveur de production.

**Application :** ÉSMPS — École Sœur Marie Paul SAGNA
**URL :** https://ecolesmps.com
**Code source :** https://github.com/malado04/ESMPS
**Documentation :** https://github.com/malado04/ESMPS_Docs

ESMPS est une application web développée avec Laravel. La procédure exacte doit être adaptée à la version de Laravel, de PHP et au moteur de base de données utilisés par le projet.

## 2. Prérequis

Avant le déploiement, vérifier les éléments suivants :

* Accès au serveur d'hébergement.
* Accès SSH si disponible.
* Accès au gestionnaire de fichiers ou au terminal cPanel.
* Nom de domaine configuré.
* Version de PHP compatible avec le projet.
* Extensions PHP requises par Laravel et ses dépendances.
* Base de données créée et accessible.
* Certificat SSL actif.
* Dépendances du projet disponibles via Composer.
* Sauvegarde récente en cas de mise à jour.

Les versions requises doivent être vérifiées dans `composer.json` et dans la documentation correspondant à la version de Laravel utilisée.

## 3. Préparation du projet

### 3.1. Récupérer le code source

Sur un environnement disposant de Git :

```bash
git clone https://github.com/malado04/ESMPS.git
cd ESMPS
```

Pour un serveur déjà configuré avec le dépôt, se placer dans le répertoire de l'application et vérifier la branche utilisée :

```bash
git status
git branch --show-current
```

Ne pas remplacer une installation existante sans avoir vérifié les modifications locales et effectué une sauvegarde.

### 3.2. Installer les dépendances PHP

Si Composer est disponible :

```bash
composer install --no-dev --optimize-autoloader
```

Cette commande installe les dépendances déclarées dans `composer.lock` lorsqu'il est présent.

Si le projet utilise un fichier de verrouillage, il est préférable de le conserver dans Git afin de reproduire un environnement cohérent.

Ne pas exécuter cette commande avant d'avoir vérifié que la version de PHP du serveur est compatible avec le projet.

### 3.3. Préparer les variables d'environnement

Créer le fichier `.env` à partir du modèle fourni :

```bash
cp .env.example .env
```

Adapter ensuite les paramètres à l'environnement de production.

Exemple de configuration à compléter :

```dotenv
APP_NAME=ESMPS
APP_ENV=production
APP_DEBUG=false
APP_URL=https://ecolesmps.com

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=nom_base_de_donnees
DB_USERNAME=utilisateur_base
DB_PASSWORD=mot_de_passe_a_configurer
```

**Important :** cet exemple suppose une base MySQL ou MariaDB. Le moteur réellement utilisé doit être confirmé avant de renseigner `DB_CONNECTION` et les autres paramètres.

Ne jamais publier le fichier `.env` dans un dépôt Git public. Utiliser des identifiants propres à la production et limiter les droits du compte de base de données.

### 3.4. Générer la clé de l'application

Lors d'une nouvelle installation, générer la clé Laravel :

```bash
php artisan key:generate
```

Cette opération doit être effectuée avec précaution sur une installation existante : remplacer la clé peut invalider certaines données chiffrées ou sessions. Ne pas régénérer la clé à chaque déploiement.

## 4. Configuration de la base de données

Créer la base de données et son utilisateur depuis cPanel ou le panneau d'administration du serveur.

Accorder uniquement les droits nécessaires à l'application.

Vérifier les paramètres de connexion dans `.env`.

Avant d'exécuter les migrations, effectuer une sauvegarde si la base contient déjà des données.

Pour une nouvelle installation :

```bash
php artisan migrate --force
```

Cette commande applique les migrations disponibles. Elle ne doit être exécutée que lorsque le schéma attendu et les migrations du projet ont été vérifiés.

**Attention :** ne pas utiliser `migrate:fresh` sur la production. Cette commande supprime les tables existantes et peut entraîner une perte de données.

Les seeders ne doivent être exécutés que si leur contenu a été vérifié et que leur exécution est adaptée à l'environnement concerné.

## 5. Configuration du serveur web

### 5.1. Répertoire public

Laravel doit être servi depuis le répertoire `public` de l'application, et non depuis la racine contenant les fichiers internes du framework.

Exemple d'organisation recommandée :

```text
/home/UTILISATEUR/
└── applications/
    └── ESMPS/
        ├── app/
        ├── bootstrap/
        ├── config/
        ├── database/
        ├── resources/
        ├── routes/
        ├── storage/
        ├── vendor/
        └── public/
```

Le domaine `ecolesmps.com` doit pointer vers le répertoire `public` de cette installation.

L'emplacement réel dépend de la configuration de l'hébergement. Ne pas exposer directement les dossiers `app`, `config`, `database`, `storage` ou les fichiers `.env`.

Si l'hébergement impose une racine web différente, mettre en place une configuration adaptée avec les précautions nécessaires pour garder les fichiers sensibles hors de l'espace public.

### 5.2. HTTPS

Vérifier que le certificat SSL est actif et que l'application est accessible via :

https://ecolesmps.com

Contrôler également les redirections HTTP vers HTTPS et les éventuels contenus chargés sans HTTPS.

## 6. Permissions des fichiers

Laravel doit pouvoir écrire dans certains répertoires.

Les principaux répertoires concernés sont :

```text
storage/
bootstrap/cache/
```

Vérifier que l'utilisateur exécutant PHP dispose des droits nécessaires.

Éviter d'accorder des permissions générales trop larges. En particulier, ne pas appliquer systématiquement `chmod -R 777` à l'application.

## 7. Finalisation du déploiement

Après configuration de l'environnement et de la base de données, exécuter les commandes nécessaires.

```bash
php artisan config:cache
php artisan route:cache
php artisan view:cache
```

La mise en cache des routes doit être validée avec le projet : certaines déclarations de routes ou fonctionnalités peuvent ne pas être compatibles avec cette mise en cache.

En cas de problème, consulter les journaux Laravel et ceux du serveur web.

Vérifier ensuite :

* L'accès à la page d'accueil.
* La connexion et la déconnexion.
* L'accès aux fonctionnalités selon les rôles.
* L'affichage des élèves et des inscriptions.
* La sélection de l'année scolaire.
* Les opérations pédagogiques.
* Les opérations financières.
* Les fichiers et documents générés.
* Les éventuels modules supplémentaires activés.

## 8. Mise à jour d'une installation existante

Une mise à jour doit suivre un processus contrôlé.

### Étape 1 — Sauvegarder

Sauvegarder la base de données, les fichiers téléversés et les fichiers de configuration nécessaires.

### Étape 2 — Vérifier les modifications

Examiner les changements du dépôt et identifier les migrations ou les dépendances modifiées.

```bash
git status
git log -5 --oneline
```

### Étape 3 — Récupérer le code

Après vérification de la branche de production et des modifications locales :

```bash
git pull
```

La commande exacte dépend de la stratégie de déploiement et de la branche utilisée. Ne pas l'exécuter aveuglément sur un serveur contenant des modifications non versionnées.

### Étape 4 — Mettre à jour les dépendances

Si les dépendances ont changé :

```bash
composer install --no-dev --optimize-autoloader
```

### Étape 5 — Appliquer les migrations

Après examen des migrations et sauvegarde de la base :

```bash
php artisan migrate --force
```

### Étape 6 — Reconstruire les caches

```bash
php artisan optimize:clear
php artisan config:cache
php artisan view:cache
```

Ajouter la mise en cache des routes uniquement après vérification de sa compatibilité.

### Étape 7 — Tester

Effectuer des tests fonctionnels ciblés avant de considérer la mise à jour comme terminée.

## 9. Sauvegardes et restauration

Les sauvegardes doivent inclure les éléments nécessaires à la reprise du service :

* Base de données.
* Fichiers téléversés par les utilisateurs.
* Fichiers de configuration conservés séparément.
* Éléments spécifiques à l'installation.

Les sauvegardes doivent être stockées dans un emplacement protégé, distinct de l'espace web public. L'accès doit être limité et les restaurations doivent être testées régulièrement.

La fréquence de sauvegarde doit être adaptée à l'activité de l'établissement et au volume de données.

## 10. Sécurité en production

Les points suivants doivent être vérifiés :

* `APP_ENV=production`.
* `APP_DEBUG=false`.
* Accès HTTPS actif.
* Fichier `.env` inaccessible publiquement.
* Identifiants de base de données protégés.
* Permissions des fichiers limitées.
* Dépendances mises à jour après vérification de compatibilité.
* Comptes utilisateurs et rôles contrôlés.
* Sauvegardes protégées.
* Journaux d'erreurs accessibles uniquement aux personnes autorisées.

Les erreurs détaillées ne doivent pas être affichées aux utilisateurs en production.

## 11. Résolution des problèmes courants

### Erreur 500

Vérifier les journaux de Laravel :

```text
storage/logs/
```

Contrôler les paramètres `.env`, les permissions et la compatibilité PHP.

### Erreur de connexion à la base de données

Vérifier le nom de la base, l'utilisateur, le mot de passe, l'hôte et le moteur configuré.

Après une modification de configuration, vider puis reconstruire le cache de configuration.

### Page blanche ou erreur après mise à jour

Vérifier les journaux PHP et Laravel, la version PHP et les dépendances installées.

### Fichiers téléversés inaccessibles

Vérifier les permissions, le chemin de stockage et, si l'application utilise le stockage public Laravel, la présence du lien symbolique approprié.

Ne créer ce lien qu'après avoir confirmé que le projet utilise ce mécanisme.

## 12. Retour arrière

Si une mise à jour provoque une panne :

1. Identifier la cause et évaluer les conséquences.
2. Suspendre les opérations risquant d'aggraver le problème.
3. Restaurer la version précédente du code si nécessaire.
4. Restaurer la base de données uniquement si cela est nécessaire et après évaluation des données créées depuis la mise à jour.
5. Vérifier la configuration et les fichiers téléversés.
6. Tester les fonctions essentielles avant de rétablir le service.

Une restauration du code ne suffit pas toujours si une migration a modifié la base de données. Les migrations doivent être conçues et déployées avec un plan de retour arrière.

## 13. Informations à compléter pour l'environnement ESMPS

Pour rendre cette procédure spécifique au serveur de production, compléter les informations suivantes dans une copie interne et sécurisée du document :

| Élément                          | Valeur à renseigner                  |
| -------------------------------- | ------------------------------------ |
| Hébergeur                        | À confirmer                          |
| Version PHP                      | À confirmer                          |
| Version Laravel                  | À confirmer                          |
| Moteur de base de données        | À confirmer                          |
| Version de la base               | À confirmer                          |
| Branche Git de production        | À confirmer                          |
| Répertoire de l'application      | À confirmer                          |
| Répertoire public du domaine     | À confirmer                          |
| Méthode de déploiement           | SSH, Git ou gestionnaire de fichiers |
| Méthode de sauvegarde            | À confirmer                          |
| Procédure de restauration testée | À confirmer                          |

Ne pas inscrire de mot de passe, de clé privée ou de secret dans ce document.

---

**Fin du document.** Cette procédure doit être validée sur l'environnement de préproduction, ou sur une copie de test, avant toute intervention importante sur la production.
