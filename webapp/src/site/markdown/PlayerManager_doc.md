# Documentation de la Classe PlayerManager

La classe `PlayerManager` gère les opérations liées aux joueurs et aux jeux dans l'application en ligne de jeu.

## Table des matières

1. [Aperçu](#aperçu)
2. [Méthodes](#méthodes)
3. [Utilisation](#utilisation)
4. [Exemples](#exemples)
5. [Contributions](#contributions)
6. [Licence](#licence)

## Aperçu

La classe `PlayerManager` étend la classe `UserManager` et fournit des fonctionnalités spécifiques liées à la gestion des joueurs et des jeux. Elle permet de créer de nouveaux jeux, de rejoindre des jeux existants et d'accéder aux informations sur les joueurs et les jeux.

## Méthodes

### `makeUserProposeGame(User user_who_proposed_the_game): GameManager`

Crée un nouveau jeu proposé par un utilisateur donné.

- **Paramètres**:
  - `user_who_proposed_the_game`: L'utilisateur qui propose le jeu.
- **Retourne**:
  - `GameManager`: Le gestionnaire de jeu associé au nouveau jeu.

### `makeUserPlayWithUser(int id_of_user_who_proposed_the_game, User user_who_joins_the_game): GameManager`

Permet à un utilisateur de rejoindre un jeu proposé par un autre utilisateur.

- **Paramètres**:
  - `id_of_user_who_proposed_the_game`: L'identifiant de l'utilisateur qui a proposé le jeu.
  - `user_who_joins_the_game`: L'utilisateur qui rejoint le jeu.
- **Retourne**:
  - `GameManager`: Le gestionnaire de jeu associé au jeu auquel l'utilisateur a rejoint.

### `getGameProposedByUser(User user_who_proposed_game): Game`

Récupère le jeu proposé par un utilisateur donné.

- **Paramètres**:
  - `user_who_proposed_game`: L'utilisateur qui a proposé le jeu.
- **Retourne**:
  - `Game`: Le jeu proposé par l'utilisateur, ou `null` s'il n'y a aucun jeu associé à cet utilisateur.

### `usersToPlayer(Map<Integer, User> users): Map<Integer, Player>`

Convertit une map d'utilisateurs en une map de joueurs.

- **Paramètres**:
  - `users`: La map d'utilisateurs à convertir.
- **Retourne**:
  - `Map<Integer, Player>`: La map de joueurs convertie.

### `gamesToGameSummaries(Map<Integer, Game> games): Map<Integer, GameSummary>`

Convertit une map de jeux en une map de résumés de jeux.

- **Paramètres**:
  - `games`: La map de jeux à convertir.
- **Retourne**:
  - `Map<Integer, GameSummary>`: La map de résumés de jeux convertie.

### `getWaitingToPlayUser(): Map<Integer, User>`

Récupère la map des utilisateurs en attente de jouer.

- **Retourne**:
  - `Map<Integer, User>`: La map des utilisateurs en attente de jouer.

### `getGames(): Map<Integer, Game>`

Récupère la map des jeux.

- **Retourne**:
  - `Map<Integer, Game>`: La map des jeux.

## Utilisation

Pour utiliser la classe `PlayerManager`, instanciez-la dans votre application et appelez les méthodes appropriées selon vos besoins. Assurez-vous d'avoir une instance de `UserManager` correctement configurée avant d'utiliser `PlayerManager`.

## Exemples

```java
// Création d'un nouveau jeu proposé par un utilisateur
GameManager gameManager = playerManager.makeUserProposeGame(user);

// Rejoindre un jeu proposé par un autre utilisateur
GameManager gameManager = playerManager.makeUserPlayWithUser(userId, user);

// Récupérer le jeu proposé par un utilisateur
Game game = playerManager.getGameProposedByUser(user);

// Convertir une map d'utilisateurs en une map de joueurs
Map<Integer, User> users = ...
Map<Integer, Player> players = playerManager.usersToPlayer(users);
