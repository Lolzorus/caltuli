# Spécification de classe : `Home`

## Package
`online.caltuli.webapp.servlet`

## Description
Home.java : Cette servlet gère les requêtes HTTP GET et POST pour la 
page d'accueil de l'application web. Elle affiche des informations sur 
les utilisateurs actuellement authentifiés, ceux en attente de jeux, 
et les jeux en cours. Les informations sont mises à jour en temps réel 
via des requêtes GET vers la servlet UserActivities. Cette servlet 
fait partie de la couche de présentation dans l'architecture 
multi-tiers de l'application, interagissant directement avec les 
utilisateurs finaux.


## Fonctionnalités au 30 Mai 2024
- Affichage en Temps Réel : La page home.jsp affiche des informations 
sur les utilisateurs authentifiés, ceux en attente de jeux, et les jeux 
en cours, mises à jour toutes les X secondes via des requêtes GET vers 
la servlet UserActivities.
- Liens de navigation vers d'autres pages de l'application (création de 
compte, authentification, déconnexion et documentation/code source).
- Interactions par WebSocket : Utilise GameWebSocket pour gérer les 
interactions de jeu en temps réel et les mises à jour de statut des 
utilisateurs via des sessions WebSocket liées à chaque ID de jeu.
- Support de React pour le Gameplay : Intègration d'une application React 
pour permettre une expérience de jeu interactive.



## Dépendances

### Dépendances Internes

- `online.caltuli.business.exception.BusinessException` : Classe d'exception
  personnalisée du module `business`, utilisée pour gérer les exceptions liées à
  la logique métier de l'application.
- `online.caltuli.business.UserManager` : Classe du module `business` pour gérer
  les opérations liées aux utilisateurs ; utilisée par Home pour récupérer la liste
  des utilisateurs actuellement connectés.
- `online.caltuli.model.UserConnection` : Classe du module `model` représentant
  la connexion d'un utilisateur. Ce module contient les classes de modèle qui
  définissent la structure des données manipulées par l'application.

### Dépendances Tierces

- `jakarta.inject.Inject` : Utilisé pour l'injection de dépendances.
- `jakarta.servlet.ServletException`, `jakarta.servlet.http.HttpServlet`,
  `HttpServletRequest`, `HttpServletResponse`, `HttpSession` : Classes et
  interfaces de Jakarta EE pour gérer les requêtes et réponses HTTP, ainsi que
  les sessions utilisateurs.
- `org.apache.logging.log4j.LogManager`, `Logger` : Classes fournies par Apache
  Logging pour configurer et effectuer le logging des activités et des erreurs
  de l'application.
- `com.fasterxml.jackson.core.type.TypeReference`, `com.fasterxml.jackson.databind.ObjectMapper`, 
`com.fasterxml.jackson.databind.module.SimpleModule` : Utilisés pour la sérialisation des objets Java en JSON.
- `jakarta.json.Json`, `jakarta.json.JsonObject` : Utilisés pour créer et manipuler des objets JSON.
- `jakarta.websocket.Session` : Utilisé pour gérer les sessions WebSocket.

## Attributs

- `userManager` : Cette instance de `UserManager`, obtenue par injection CDI, est
  responsable de la gestion des opérations liées aux utilisateurs, comme l'enregistrement
  de nouveaux utilisateurs, la mise à jour des informations utilisateur, et la validation
  des identifiants utilisateur. L'annotation `@Inject` facilite cette injection,
  démontrant l'application des principes de CDI pour encourager un couplage faible,
  augmenter la modularité et améliorer la testabilité par l'externalisation de la
  création des instances. Avec l'annotation `@ApplicationScoped`, `UserManager` est
  déclaré comme un bean à portée d'application, ce qui signifie qu'une seule instance
  est créée et utilisée partout dans l'application. Cette unique instance partagée aide
  à optimiser l'utilisation des ressources et à assurer l'uniformité des opérations
  utilisateurs à travers l'application. L'adoption de `@ApplicationScoped` reflète
  l'objectif d'avoir un ensemble de services utilisateur cohérents et réutilisables
  sans nécessiter la création répétée d'instances. La considération pour la
  synchronisation devient pertinente lors de l'accès à ces services partagés en
  environnement multithread pour éviter les conflits d'accès concurrents.
- `logger` : Instance de `Logger` utilisée pour enregistrer les activités de la
  servlet.
- `authenticatedUsers` : Liste des utilisateurs authentifiés formatée pour le client.
- `waitingToPlayUsers` : Liste des utilisateurs en attente de jeux formatée pour le client.
- `games` : Informations sommaires des jeux en cours.
- `game` : Détails du jeu sérialisés en JSON.
- `gameId` : ID du jeu utilisé pour initialiser les composants React.
- `playerId` : ID du joueur utilisé pour initialiser les composants React.

## Constructeurs

- `Home()` : Constructeur par défaut.

## Méthodes

### doGet(HttpServletRequest request, HttpServletResponse response)

- **Description** : 
  - Traite les requêtes GET en récupérant la liste des utilisateurs connectés 
  et en transférant la requête vers la page JSP de la page d'accueil. 
  - Prépare les attributs nécessaires pour le rendu de l'interface utilisateur 
  de la page d'accueil en React et verifie si l'utilisateur est en jeu ou non.
  - Sérialise les détails du jeu en JSON et configure les variables nécessaires 
  pour la gestion de l'état du jeu et les connexions WebSocket.
- **Paramètres** :
    - `HttpServletRequest request` : La requête HTTP entrante.
    - `HttpServletResponse response` : La réponse HTTP à envoyer.
- **Exceptions** : `ServletException`, `IOException`

### doPost(HttpServletRequest request, HttpServletResponse response)

- **Description** :
  - Traite les requêtes POST en les redirigeant vers la méthode
  `doGet` pour réutiliser la logique de récupération et d'affichage des
  utilisateurs connectés.
  - Met à jour les états des jeux et communique ces changements via WebSocket aux clients concernés.
- **Paramètres** :
    - `HttpServletRequest request` : La requête HTTP entrante.
    - `HttpServletResponse response` : La réponse HTTP à envoyer.
    - `action` : Détermine le type d'opération à exécuter (new_game ou play_with).
    - `user_id` : Spécifie l'ID de l'utilisateur à rejoindre dans un jeu (utilisé avec play_with).
- **Exceptions** : `ServletException`, `IOException`

## Diagrammes

[TO DO]

## Exemples d'Utilisation

### Déploiement de la Servlet

La servlet `Home` peut être déployée et configurée pour répondre aux requêtes HTTP via le fichier `web.xml` ou par annotations directement dans le code.

#### Via `web.xml`

Dans le fichier `web.xml`, ajoutez la configuration suivante pour enregistrer la servlet `Home` :

    <servlet>
        <servlet-name>Home</servlet-name>
        <servlet-class>online.caltuli.webapp.servlet.gui.Home</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>Home</servlet-name>
        <url-pattern>/home</url-pattern>
    </servlet-mapping>

Cette configuration spécifie que la servlet `Home` doit répondre aux requêtes sur le chemin `/home`.

#### Via Annotations

Dans le code de la classe `Home`, vous pouvez spécifier le mappage de la servlet en utilisant l'annotation `@WebServlet` :

    @WebServlet("/home")
    public class Home extends HttpServlet {
        // Reste de la classe
    }

### Traitement des Requêtes GET

Lorsqu'une requête GET est reçue pour `/home`, la servlet traite la requête comme suit :

    protected void doGet(HttpServletRequest request, HttpServletResponse response)
        throws ServletException, IOException {
    request.setAttribute("authenticatedUsers", playerManager.usersToPlayer(playerManager.getAuthenticatedUsers()));
    request.setAttribute("waitingToPlayUsers", playerManager.usersToPlayer(playerManager.getWaitingToPlayUser()));
    request.setAttribute("games", playerManager.gamesToGameSummaries(playerManager.getGames()));

    HttpSession session = request.getSession(false);
    User user = (User) session.getAttribute("user");

    if (user != null) {
        GameManager gameManager = (GameManager) session.getAttribute("gameManager");
        if (gameManager != null) {
            Game game = gameManager.getGame();
            ObjectMapper objectMapper = new ObjectMapper();
            String rawJson = objectMapper.writeValueAsString(game);
            String safeJson = rawJson.replace("\"", "\\\"").replace("'", "\\'");
            request.setAttribute("game", safeJson);
            request.setAttribute("gameId", game.getId());
            request.setAttribute("playerId", user.getId());
        }
    }

    this.getServletContext().getRequestDispatcher("/WEB-INF/home.jsp").forward(request, response);
}

Cette méthode récupère la liste des utilisateurs connectés et la passe à la page JSP `home.jsp` pour l'affichage.


### Traitement des Requêtes POST

Pour les requêtes POST à `/home`, la servlet redirige simplement vers `doGet` pour réutiliser la logique de récupération et d'affichage des utilisateurs connectés :

    protected void doPost(HttpServletRequest request, HttpServletResponse response)
        throws ServletException, IOException {
    HttpSession session = request.getSession(false);
    User user = (User) session.getAttribute("user");

    if (user == null) {
        return;
    }

    String action = request.getParameter("action");
    GameManager gameManager = null;

    if ("new_game".equals(action)) {
        try {
            gameManager = playerManager.makeUserProposeGame(user);
            session.setAttribute("gameManager", gameManager);
        } catch (BusinessException e) {
            // Handle exception
        }
    } else if ("play_with".equals(action)) {
        String userIdString = request.getParameter("user_id");
        if (userIdString != null && !userIdString.isEmpty()) {
            try {
                int userId = Integer.parseInt(userIdString);
                gameManager = playerManager.makeUserPlayWithUser(userId, user);
                session.setAttribute("gameManager", gameManager);
            } catch (NumberFormatException | BusinessException e) {
                // Handle exceptions
            }
        }
    }

    if (gameManager != null) {
        // Notify relevant users via WebSocket
        JsonObject newSecondPlayerUpdateJsonObject = Json.createObjectBuilder()
                .add("update", "secondPlayer")
                .add("newValue", JsonUtil.convertToJson(user))
                .build();
        JsonObject newGameStateUpdateJsonObject = Json.createObjectBuilder()
                .add("update", "gameState")
                .add("newValue", JsonUtil.convertToJson(GameState.WAIT_FIRST_PLAYER_MOVE))
                .build();
        HashSet<Session> webSocketSessions = GameWebSocket.getSessionsRelatedToGameId(gameManager.getGame().getId());
        for (Session webSocketSession : webSocketSessions) {
            if (webSocketSession != null && webSocketSession.isOpen()) {
                try {
                    webSocketSession.getBasicRemote().sendText(newSecondPlayerUpdateJsonObject.toString());
                    webSocketSession.getBasicRemote().sendText(newGameStateUpdateJsonObject.toString());
                } catch (Exception e) {
                    // Handle exception
                }
            }
        }
    }

    doGet(request, response);
}


### Description de `home.jsp`

Cette JSP (`home.jsp`) sert de page d'accueil pour l'application web `Onlineplay`.
Elle est configurée pour utiliser l'encodage "UTF-8" et les balises core de Jakarta
à travers le préfixe "c".
Elle permet de voir les utilisateurs connectés, les utilisateurs en attente de partie, 
et les jeux en cours. Elle est liee a une application react permettant une plus 
grande interactivité.

#### Fonctionnalités Clés

- **Affichage Conditionnel** : La page utilise `<c:if>` pour afficher des
  informations basées sur la présence d'objets dans `sessionScope`, tel que
  `userConnection` et `user`. Cela inclut l'ID de connexion, l'adresse IP,
  l'horodatage de connexion, l'ID de l'utilisateur, l'autorisation d'accès,
  l'identifiant de l'utilisateur, le nom d'utilisateur, le hash du mot de
  passe, l'email et les messages de l'utilisateur.

- **Liens de Navigation** : Des liens sont fournis pour l'inscription (`Sign up`),
  la connexion (`log in`), la déconnexion (`log out`), l'accès au site du projet
  généré par `maven-site-plugin`, et les sources du projet sur GitHub.

- **Liste des Utilisateurs Connectés** : Un `<c:forEach>` itère sur
  `connectedUserList` pour afficher la liste des noms d'utilisateurs connectés.

- **Action d'Utilisateurs** : Créer ou rejoindre une partie (utilisation de form).


#### Sécurité

La page utilise `<c:out>` pour afficher le contenu des variables, ce qui aide à
prévenir les attaques XSS en échappant automatiquement les caractères spéciaux.

#### Points d'Intégration

- **Variables de Session** : `home.jsp` dépend des variables de session
  `userConnection` et `user` pour afficher les informations de l'utilisateur
  et de sa connexion.

- **Liste des Utilisateurs Connectés** : La servlet `Home` doit fournir
  `connectedUserList` à la JSP via `request.setAttribute`.

- **Integration de React** : utilise `react.js` et `react.chunk.js` pour integrer 
l'application react.

#### Commentaires pour le Débogage

La JSP inclut des sections marquées `<!-- begin to debug -->` et `<!-- end to 
debug -->` qui affichent des détails sur les objets `sessionScope.userConnection`
et `sessionScope.user` pour le débogage. Ces sections peuvent être supprimées ou
commentées en production pour des raisons de sécurité et de performance.

