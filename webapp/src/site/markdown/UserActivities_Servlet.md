# UserActivities Servlet

Le fichier `UserActivities.java` contient une servlet Java qui gère les requêtes HTTP GET pour récupérer des données concernant les utilisateurs et leurs activités dans une application web. Cette servlet utilise Jackson pour la sérialisation des données en format JSON.

## Structure du Fichier

Le fichier est structuré comme suit :

- Importation des bibliothèques.
- Déclaration de la classe `UserActivities` qui étend `HttpServlet`.
- Injection de dépendance d'un `PlayerManager`.
- Déclaration d'un logger pour le suivi des événements.
- Implémentation de la méthode `doGet` pour gérer les requêtes GET.

## Principales Fonctionnalités

- **Récupération des Utilisateurs Authentifiés :** Utilisation de `playerManager` pour récupérer la liste des utilisateurs authentifiés.
- **Récupération des Utilisateurs en Attente de Jeu :** Utilisation de `playerManager` pour récupérer la liste des utilisateurs en attente de jeu.
- **Récupération des Parties en Cours :** Utilisation de `playerManager` pour récupérer la liste des parties en cours.
- **Sérialisation des Données en JSON :** Utilisation de Jackson pour convertir les données en format JSON.
- **Gestion des Erreurs :** Gestion des exceptions et envoi d'une réponse d'erreur en cas de problème lors de la sérialisation des données.

## Dépendances

- **Jackson :** Utilisé pour la sérialisation des données en format JSON.
- **Jakarta Servlet API :** Pour la gestion des requêtes HTTP et des réponses.
- **Log4j :** Utilisé pour le logging des événements.

## Méthode `doGet`

La méthode `doGet` est appelée lorsqu'une requête HTTP GET est reçue par la servlet. Voici un aperçu de ce qu'elle fait :

1. **Configuration de la Réponse :** Définit le type de contenu de la réponse comme JSON et l'encodage comme UTF-8.
2. **Récupération des Données :** Utilise `playerManager` pour récupérer les utilisateurs authentifiés, les utilisateurs en attente de jeu et les parties en cours.
3. **Conversion des Données en JSON :** Utilise Jackson pour convertir les données en format JSON.
4. **Envoi de la Réponse :** Envoie la réponse JSON au client.
5. **Gestion des Erreurs :** Capture et journalise les erreurs éventuelles lors de la sérialisation des données.

