# Sommaire

- Déclarer une clé API
- Obtenir un jeton d'accès
- Interagir avec l'API Arena
- Comprendre l'API : Jeu / Actions / Réponses
- Documentation de référence des méthodes
- Documentation de référence des objets

# Déclarer une clé API

Avant de commencer à créer votre client et intégrer The First Spine à votre plateforme vous devez au préalable obtenir une clé API. Vous pouvez nous contacter pour en obtenir une. Nous n'en générons qu'à la demande et à l'étude de votre projet (on ne va pas vous générer une clé API pour vous permettre d'augmenter votre ratio victoires / défaites).

Une clé api contient plusieurs éléments :
- L'identifiant publique appelé `public`
- L'identifiant secret appelé `secret`
- Le domain autorisé sur la clé API appelé `allowed_domain`

# Obtenir un jeton d'accès

Vous ne pouvez interagir avec l'API sans obtenir de jeton d'accès. Ces jetons sont un moyen d'identifier les utilisateurs qui doivent donner leur accord explicitement.

Afin d'obtenir un jeton d'accès vous devez diriger l'utilisateur vers l'adresse `https://www.thefirstspine.fr/<fr|en>/access?key=<public>[&redirect=<redirect>]`. 

Une son accord donné, il sera redirigé vers votre le domaine autorisé de votre application comme suite : `<allowed_domain>[<redirect>]?access_token=<access_token>`

Le jeton d'accès à l'utilisateur est contenu dans le paramètre GET `<access_token>` après retour sur votre application.

# Interagir avec l'API Arena

L'API Arena est accessible depuis une URL unique : `https://www.thefirstspine.fr/api/arena`.

Chaque requête vers l'API a la forme suivante :

```curl
curl -X POST \
  http://localhost/api/arena \
  -H 'X-API-Credentials: <public>:<secret>' \
  -H 'Content-Type: application/json' \
  -d '{
  "method": "<method>",
  "parameters": <parameters>
}'
```

- `<public>` & `<secret>` font référence à votre clé API
- `<method>` est la méthode à appeler de l'API. L'API est organisée en méthodes que vous pouvez appeler comme `createGame` ou `getCards`.
- `<parameters>` est un objet qui change en fonction de la méthode appelée.

# Internationnalisation & mise en forme

L'API peut être à la fois en anglais et en français. Toutes les ressources seront traduites et disponibles dans la langue choisie (par défaut en Français). Vous pouvez changer la langue de l'API de deux manières :
- en ajoutant un header `Accept-Language`
- en ajoutant un paramètre `GET locale` à votre requête

De plus, certains textes d'objets (comme la description des cartes) sont mis en forme en HTML. Voici une liste des styles que nous utilisons pour mettre en forme le texte :

| Classe CSS             | Explication                                                   | Couleur | Icône    |
| ---------------------- | ------------------------------------------------------------- | ------- | -------- |
| wizard                 | Met en surbrillance le mot "sorcier"                          | gris    | Non      |
| spell                  | Met en surbrillance le mot "sortilège"                        | jaune   | Non      |
| creature               | Met en surbrillance le mot "créature"                         | rouge   | Non      |
| artefact               | Met en surbrillance le mot "artefact"                         | bleu    | Non      |
| stat life              | Une statistique de vie                                        | N/A     | Non      |
| stat str               | Une statistique de force                                      | N/A     | Non      |
| stat def               | Une statistique de défense                                    | N/A     | Non      |
| stats capacity         | Une capacité                                                  | N/A     | Non      |
| icon icon-trahison     | Une icône de capacité (entouré d'un élément `stats capacity`) | N/A     | ❓       |
| icon terre-brulee      | Une icône de capacité (entouré d'un élément `stats capacity`) | N/A     | 🔥       |

# Comprendre l'API : Jeu / Actions / Réponses

⚠️ Avant de lire cette partie, nous vous invitons à consulter cet article qui regroupe les réflexions que nous avons eu sur le système d'Arena : https://github.com/TeddyGandon/thefirstspine-arena-throughts/blob/master/core.md

Lorsque vous créez un jeu avec Arena une pile d'actions potientielle est créée. Arena attend alors une réponse de la part de l'utilisateur concerné par la pile d'actions disponible.

# Documentation de référence des méthodes

## createGame

Créé une partie dans Arena.

### Input

- `String(quickGame|bga|fireMountain|beltain|lostGraveyard|pumpking) gameType`: le type de jeu que vous souhaitez créer.
- `Object({token: String, destiny: String(conjurer|summoner|sorcerer|hunter), origin: String(healer|surgeon|ignorant|architect)})[] players`: les joueurs qui vont participer à l'activité

### Output

Retourne une instance de type `ArenaGame`

### Example

```curl
curl -X POST \
  https://www.thefirstspine.fr/api/arena \
  -H 'Content-Type: application/json' \
  -H 'X-Api-Credentials: ptest:stest' \
  -d '{
    "method": "createGame",
    "parameters": {
        "gameType": "quickGame",
        "players": [
            {
                "token": "Nefo4ny3pVhJtoOHYIHz4crpfvJNQtYPrdIoxpaZVVjTfwiyjT0xTR12sWvdYBlRLgyYRVPOOZwBB8X6xXF0KG5RZxskzc27Qa25",
                "destiny": "conjurer",
                "origin": "healer"
            },
            {
                "token": "timXiiA98hZYnfzGAXrS18GKaRyh0H1CwkcK6EoqiGumRGA1Af8NIBI8FSYTf43ygbzLPqzJO3GnOP8u45xpBxT5pTAjz9AQTfvh",
                "destiny": "summoner",
                "origin": "surgeon"
            }
        ]
    }
}'
```

## getCards

Retourne toutes les cartes dans l'instance de jeu créé.

### Input

- `int arena_game_id`: l'identifiant de l'instance.

### Output

Retourne un tableau d'instances de type `ArenaCard`

### Example

```curl
curl -X POST \
  https://www.thefirstspine.fr/api/arena \
  -H 'Content-Type: application/json' \
  -H 'X-Api-Credentials: ptest:stest' \
  -d '{
    "method": "getCards",
    "parameters": {
        "arena_game_id": 2
    }
}'
```

## getGame

### Input

- `int arena_game_id`: l'identifiant de l'instance.

### Output

Retourne une instance de type `ArenaGame`

### Example

```curl
curl -X POST \
  https://www.thefirstspine.fr/api/arena \
  -H 'Content-Type: application/json' \
  -H 'X-Api-Credentials: ptest:stest' \
  -d '{
    "method": "getGame",
    "parameters": {
        "arena_game_id": 2
    }
}'
```

## getGameAction

Retourne une action de jeu.

### Input

- `int arena_game_action_id`: l'identifiant de l'action à retourner

### Output

Retourne une instance de type `ArenaGameAction`

### Example

```curl
curl -X POST \
  https://www.thefirstspine.fr/api/arena \
  -H 'Content-Type: application/json' \
  -H 'X-Api-Credentials: ptest:stest' \
  -d '{
    "method": "getGameAction",
    "parameters": {
        "arena_game_action_id": 12
    }
}'
```

## getGameActions

Retourne les actions disponibles et qui sont en attente de réponse.

### Input

- `int arena_game_id`: l'identifiant de l'instance de jeu.

### Output

Retourne un tableau d'instances de type `ArenaGameAction`

### Example

```curl
curl -X POST \
  https://www.thefirstspine.fr/api/arena \
  -H 'Content-Type: application/json' \
  -H 'X-Api-Credentials: ptest:stest' \
  -d '{
    "method": "getGameActions",
    "parameters": {
        "arena_game_id": 2
    }
}'
```

## getMessages

Retourne les messages créés dans l'instance de jeu.

### Input

- `int arena_game_id`: l'identifiant de l'instance de jeu.

### Output

Retourne un tableau d'instances de type `ArenaMessage`

### Example

```curl
curl -X POST \
  https://www.thefirstspine.fr/api/arena \
  -H 'Content-Type: application/json' \
  -H 'X-Api-Credentials: ptest:stest' \
  -d '{
    "method": "getMessages",
    "parameters": {
        "arena_game_id": 2
    }
}'
```

## respondToGameAction

Répond à une action

### Input

- `String token`: le jeton d'accès correspondant à l'utilisateur devant répondre
- `int arena_game_id`: l'identifiant de la partie en cours
- `int arena_game_action_id`: l'identifiant de l'action
- `Object response`: la réponse à l'action - voir "Documentation de référence des objets" plus bas pour plus d'informations sur comment répondre à une action

### Output

Retourne un résultat équivalent à `getGameActions`.

### Example

```curl
curl -X POST \
  https://www.thefirstspine.fr/api/arena \
  -H 'Content-Type: application/json' \
  -H 'X-Api-Credentials: ptest:stest' \
  -d '{
    "method": "getMessages",
    "parameters": {
        "token": "Nefo4ny3pVhJtoOHYIHz4crpfvJNQtYPrdIoxpaZVVjTfwiyjT0xTR12sWvdYBlRLgyYRVPOOZwBB8X6xXF0KG5RZxskzc27Qa25",
        "arena_game_id": 2,
        "arena_game_action_id": 6,
        "response": {
            "from": "3-3",
            "to": "3-4"
        }
    }
}'
```

## zombifyUser

"Zombifie" un joueur, c'est à dire que le joueur ne pourra plus interagir et passera son tour à chaque fois. Vous pouvez utiliser ceci lorsqu'un joueur devient inactif trop longtemps ou lorsqu'il déclare forfait. En effet, il n'existe pas de moyen de concéder une victoire, une partie doit être terminée.

### Input

- `String token`: le jeton d'accès correspondant à l'utilisateur devant être zombifié
- `int arena_game_id`: l'identifiant de la partie en cours

### Output

Retourne un résultat équivalent à `getGame`.

### Example

```curl
curl -X POST \
  https://www.thefirstspine.fr/api/arena \
  -H 'Content-Type: application/json' \
  -H 'X-Api-Credentials: ptest:stest' \
  -d '{
    "method": "zombifyUser",
    "parameters": {
        "token": "Nefo4ny3pVhJtoOHYIHz4crpfvJNQtYPrdIoxpaZVVjTfwiyjT0xTR12sWvdYBlRLgyYRVPOOZwBB8X6xXF0KG5RZxskzc27Qa25",
        "arena_game_id": 2
    }
}'
```

# Documentation de référence des objets

## Card

Représente une carte dans le jeu.

**Champs**

| Nom                  | Description                                                    |
| -------------------- | -------------------------------------------------------------- |
| int `card_id`        | L'identifiant de la ressource                                  |
| String `name`        | Nom de la carte                                                |
| String `description` | Description de la carte                                        |
| String `image`       | L'image liée à la carte                                        |
| int `top_str`        | Force du côté supérieur                                        |
| int `top_def`        | Défense du côté supérieur                                      |
| String `top_cpt`     | Capacité du côté supérieur                                     |
| int `right_str`      | Force du côté droit                                            |
| int `right_def`      | Défense du côté droit                                          |
| String `right_cpt`   | Capacité du côté droit                                         |
| int `bottom_str`     | Force du côté inférieur                                        |
| int `bottom_def`     | Défense du côté inférieur                                      |
| String `bottom_cpt`  | Capacité du côté inférieur                                     |
| int `left_str`       | Force du côté gauche                                           |
| int `left_def`       | Défense du côté gauche                                         |
| String `left_cpt`    | Capacité du côté gauche                                        |
| int `life`           | Vie de la carte                                                |
| String `cpt`         | Capacité de la carte                                           |
| String `type`        | Type de la carte (`artefact`, `creature`, `spell` ou `system`) |

## Deck

Un Deck est un paquet de cartes pré-construit dans le jeu.

**Champs**

| Nom                  | Description                                     |
| -------------------- | ----------------------------------------------- |
| int `deck_id`        | L'identifiant de la ressource                   |
| String `color`       | Couleur du deck                                 |
| String `name`        | Nom du deck                                     |
| String `image`       | Image du deck                                   |
| String `description` | Description du deck                             |
| Card[] `cards`       | Cartes présentes dans le deck                   |

## User

Un utilisateur enregistré sur les services de The First Spine.

**Champs**

| Nom                  | Description                                     |
| -------------------- | ----------------------------------------------- |
| int `user_id`        | L'identifiant de la ressource                   |
| String `name`        | Nom d'utilisateur                               |
| String `avatar`      | The image of the user                           |
| Object `loots`       | Les récompenses de l'utilisateur                |
| Object `arena`       | Les préférences sur Arena de l'utilisateur      |
| User[] `friends`     | Les amis de l'utilisateur                       |

## ArenaCard

Un objet `ArenaCard` est une carte qui a été générée pour une partie sur Arena. Elle est toujours associée à une instance `ArenaGame`.

**Champs**

| Nom                  | Description                                                        |
| -------------------- | ------------------------------------------------------------------ |
| int `arena_card_id`  | L'identifiant de la ressource                                      |
| int `user_id`        | L'identifiant de l'utilisateur propriétaire de la carte            |
| Card `card`          | La carte associée                                                  |
| String `location`    | La location de la carte (`board`, `deck`, `hand`, `throw`)         |
| String `position`    | La position de la carte sur le tableau                   (X-Y)     |
| Object `options`     | Les options de la carte (comme ses marqueurs par exemple)          |

## ArenaGame

Représente une instance de jeu

**Champs**

| Name                     | Description                                     |
| ------------------------ | ----------------------------------------------- |
| int `arena_game_id`      | L'identifiant de la ressource                   |
| String `game_type`       | Le type de jeu                                  |
| String `created_at`      | Date de création de la partie                   |
| User[] `users`           | La liste des joueurs                            |
| Deck[] `decks`           | La liste des decks utilisés par les joueurs     |
| object `options`         | Options de jeu                                  |
| ArenaGameType `gameType` | Le type de jeu                                  |
| ArenaCard[] `board`      | Les cartes sur le plateau de jeu                |

## ArenaGameAction

Une instance de `ArenaGameAction` est définie par un group de sous-actions qui définissent les interations attendues. On appelle ces sous-actions le script.

**Champs**

| Name                       | Description                                                    |
| -------------------------- | -------------------------------------------------------------- |
| int `arena_game_action_id` | L'identifiant de la ressource                                  |
| int `arena_game_id`        | Identifiant de la partie dont fait partie l'action             |
| int `user_id`              | L'utilisateur concerné par cette action                        |
| int `reference`            | La référence de l'action                                       |
| String `title`             | Le titre de l'action                                           |
| Object `script`            | Le script de l'action                                          |

### Syntaxe du script

Chaque action est définie par une ou plusieurs interactions. Les résultats de ces interactions sont à retourner dans la réponse de l'action.
```
{
  <String neededProperty>: {
    "type": <String type>,
    "message": <String message>,
    "params": <Object parameters>
  }
}
```

Par exemple :
```
{
  "card": {
    "type": "choseCard",
    "message": "Choisissez une créature ou un artefact",
    "params": {
      "types": ["creature", "artefact"]
    }
  },
  "square": {
    "type": "choseSquare",
    "message": "Choisissez une case sur laquelle poser votre carte",
    "params": {
      "nearPlayer": false,
      "nearControlled": true,
      "isEmpty": true
    }
  }
}
```

### Paramètres dynamiques

Parfois, les scripts d'Arena ont des paramètres dynamiques, c'est à dire que le résultat d'un précédent script sera mis dans les options d'un paramètre. Chaque résultat attendu est précédé d'un `$`.

Exemple :

```
{
  "from": {
    "type": "choseSquare",
    "message": "Choisissez une carte sur le plateau",
    "params": {
      "types": ["creature"]
    }
  },
  "to": {
    "type": "choseSquare",
    "message": "Choisissez une case à côté non vide",
    "params": {
      "nextTo": "$from",
      "isEmpty": true
    }
  }
}
```

Le paramètre `nextTo` aura la valeur du champ `from` lors de l'execution du script.

### Documentation de référence des scripts

**choseCards**

Chose a card in the hand.

| Name                     | Description                                     |
| ------------------------ | ----------------------------------------------- |
| String[] `types`         | TODO                                            |
| int `min`                | TODO                                            |
| int `max`                | TODO                                            |

**choseSquare**

Chose a square on the board.

| Name                     | Description                                     |
| ------------------------ | ----------------------------------------------- |
| Boolean `nearPlayer`     | TODO                                            |
| Boolean `nearControlled` | TODO                                            |
| Boolean `isEmpty`        | TODO                                            |
| String[] `nextTo`        | TODO                                            |
| String[] `types`         | TODO                                            |
| String[] `range`         | TODO                                            |

**skip**

Skip the action.

## ArenaGameType

TODO

**Champs**

| Name                     | Description                                     |
| ------------------------ | ----------------------------------------------- |
| String `backgroundImage` | TODO                                            |
| int `neededUsers`        | TODO                                            |
| bool `usePredefinedDeck` | TODO                                            |
| bool `pointsWin`         | TODO                                            |
| bool `pointsLose`        | TODO                                            |
| bool `timesPerWeek`      | TODO                                            |

## ArenaMessage

TODO

**Champs**

TODO
