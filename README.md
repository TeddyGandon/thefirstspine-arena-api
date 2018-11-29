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

Une fois l'utilisateur ayant donné son accord, il sera redirigé vers votre le domaine autorisé de votre application comme suite : `<allowed_domain>[<redirect>]?access_token=<access_token>`

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
| icon icon-trahison     | Une icône de capacité (entouré d'un élément `stats capacity`) | N/A     | Oui      |
| icon terre-brulee      | Une icône de capacité (entouré d'un élément `stats capacity`) | N/A     | Oui      |

# Comprendre l'API : Jeu / Actions / Réponses

Avant de lire cette partie, nous vous invitons à consulter cet article qui regroupe les réflexions que nous avons eu sur le système d'Arena : https://github.com/TeddyGandon/thefirstspine-arena-throughts/blob/master/core.md

Lorsque vous créez un jeu avec Arena une pile d'actions potientielle est créée. Arena attend alors une réponse de la part de l'utilisateur concerné par la pile d'actions disponible.

# Documentation de référence des méthodes

## createGame

Créé une partie dans Arena.

### Input

- `String(quickGame|bga|fireMountain|beltain|lostGraveyard|pumpking) gameType`: le type de jeu que vous souhaitez créer.
- `Object({token: String, destiny: String(conjurer|summoner|sorcerer|hunter), origin: String(healer|surgeon|ignorant|architect)})[] players`: les joueurs qui vont participer à 

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

### Input

### Output

### Example

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

Une Deck est un paquet de cartes pré-construit dans le jeu.

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

## ArenaCard

TODO

**Champs**

| Nom                  | Description                                     |
| -------------------- | ----------------------------------------------- |
| int `arena_card_id`  | L'identifiant de la ressource                   |
| int `user_id`        | The ID of the user which the card belongs to    |
| Card `card`          | The associated card                             |
| String `location`    | The location of the card                        |
| String `position`    | The position of the card on the board (X-Y)     |
| Object `options`     | The options of the card                         |

## ArenaGame

TODO

**Champs**

| Name                     | Description                                     |
| ------------------------ | ----------------------------------------------- |
| int `arena_game_id`      | L'identifiant de la ressource                   |
| String `game_type`       | TODO                                            |
| String `created_at`      | TODO                                            |
| User[] `users`           | The list of the users                           |
| Deck[] `decks`           | The list of the decks used by the users         |
| object `options`         | TODO                                            |
| ArenaGameType `gameType` | The game type associated with the game          |
| ArenaCard[] `board`      | The cards on the board                          |

## ArenaGameAction

An action is defined by a group of desired sub-actions called a script.

**Champs**

| Name                       | Description                                     |
| -------------------------- | ----------------------------------------------- |
| int `arena_game_action_id` | L'identifiant de la ressource                   |
| int `arena_game_id`        | The game ID concerned by the action             |
| int `user_id`              | The user ID concerned by the action             |
| int `reference`            | The reference of the action                     |
| String `title`             | The title of the action                         |
| Object `script`            | TODO                                            |

### Scripts syntaxe

Each action is defined by one or more prompts in the `script` property:
```
{
  (String neededProperty): {
    "type": (String type),
    "message": (String message),
    "params": (object parameters)
  }
}
```

For instance:
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

### Result binding

Is it possible to have binded results in the parameters of an action script. Each binded result will be a String that look like this: `"$result"`.

For instance:

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

the parameter `nextTo` will have the value of the result of the `from` script.

### Scripts reference

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
| boolean `nearPlayer`     | TODO                                            |
| boolean `nearControlled` | TODO                                            |
| boolean `isEmpty`        | TODO                                            |
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
