# ORM / API - Ynov B2 - TP1

Vous allez réaliser une API pour un système d'actualités, avec des articles.

Critères d'évaluation :

- Création d'une API
- Utilisation du Serializer de Symfony
- Bonne implémentation des méthodes HTTP dans les contrôleurs
- Utilisation des groupes de sérialisation
- Relations entre entités
- Formulaires et CRUD

## Mode de rendu

Lien vers votre dépôt Git, que je pourrai clôner.

## Dépôt Git

Votre dépôt Git contiendra un fichier `README.md` dans lequel vous indiquerez :

- Quel est l'intérêt de créer une API plutôt qu'une application classique ?
- Résumez les étapes du mécanisme de sérialisation implémenté dans Symfony
- Qu'est-ce qu'un groupe de sérialisation ? A quoi sert-il ?
- Quelle est la différence entre la méthode PUT et la méthode PATCH ?
- Quels sont les différents types de relation entre entités pouvant être mis en place avec Doctrine ?
- Qu'est-ce qu'un `Trait` en PHP et à quoi peut-il servir ?

## Exercice

Votre système permettra d'enregistrer des articles d'information dans une base de données.

### Structure de la base de données

> Entité : Article

| Champ | Type | Commentaires |
|---|---|---|
| id | integer | Clé primaire |
| title | string | Titre de l'article |
| content | text | Contenu de l'article |
| status | integer | Valeurs possibles : 0 si l'article n'est pas publié, 1 si l'article est en rédaction (sorte de 'brouillon'), 2 si l'article est publié |
| trending | boolean | TRUE si l'article est populaire et doit être mis en avant, FALSE sinon |
| published | date | Date de publication (peut être NULL) |
| created | date | Date de création |
| category_id | integer | clé étrangère vers la table category |

> Entité : Category

| Champ | Type | Commentaires |
|---|---|---|
| id | integer | Clé primaire |
| name | string | nom de la catégorie |
| created | date | Date de création |

### Fixtures

Vous créerez des fixtures avec des données générées de manière aléatoire pour disposer de données de test.

### Fonctionalités

- Réalisez un CRUD complet (POST, GET collection, GET élément, PATCH, PUT, DELETE) pour les 2 entités.

> Note pour la suppression d'une catégorie : si vous avez une erreur relative aux clés étrangères enregistrées dans les articles pour la catégorie que vous voulez supprimer, ignorez-la, ou bien renseignez-vous sur [cette page](https://www.doctrine-project.org/projects/doctrine-orm/en/2.7/reference/working-with-associations.html) de la documentation de Doctrine.

- L'API disposera, en plus du CRUD, des endpoints suivants :
  - sur les articles, un endpoint permettant de récupérer seulement les articles populaires ("trending"). Utilisez le repository des articles pour filtrer votre requête
  - un endpoint permettant de récupérer les articles d'une catégorie uniquement. Idem, utiliser le repository des articles pour filtrer votre requête

- Le champ `created`, présent dans les 2 entités, devra contenir la date du jour et être renseigné automatiquement à la création d'une entité. Utilisez un `EventSubscriber` pour automatiser ce traitement.

- A sa création, un article devra être placé par défaut en statut "brouillon". Vous êtes libres sur le choix de l'implémentation de ce mécanisme. Il doit être automatisé.

- Il n'est pas nécessaire de créer une entité pour le statut d'un article. Vous pouvez créer une classe à part et reproduire un système d'énumérations avec des constantes, comme vu en cours.

- Le champ "published" d'un article devra automatiquement recevoir la date du jour lorsque le statut d'un article passera en "publié" (valeur 2). Automatisez ce traitement à l'aide d'un `EventSubscriber` qui écoutera la mise à jour d'une entité

### Groupes de sérialisation

> Article

Lorsqu'un article sera renvoyé au client (seul ou dans une collection), les champs suivants devront être sérialisés :

- id
- title
- content
- trending
- published
- category
  - id
  - name

> Category

Une catégorie, elle, présentera ces champs :

- id
- name
