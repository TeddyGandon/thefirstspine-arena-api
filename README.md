# 📄 Sommaire

- Déclarer une clé API
- Obtenir un jeton d'accès
- Interagir avec l'API Arena
- Comprendre l'API : Jeu / Actions / Réponses
- Documentation de référence des méthodes
- Documentation de référence des objets

# 🔑 Déclarer une clé API

Avant de commencer à créer votre client et intégrer The First Spine à votre plateforme vous devez au préalable obtenir une clé API. Vous pouvez nous contacter pour en obtenir une. Nous n'en générons qu'à la demande et à l'étude de votre projet (on ne va pas vous générer une clé API pour vous permettre d'augmenter votre ratio victoires / défaites).

Une clé api contient plusieurs éléments :
- L'identifiant publique appelé `public`
- L'identifiant secret appelé `secret`
- Le domain autorisé sur la clé API appelé `allowed_domain`

# 🎫 Obtenir un jeton d'accès

Vous ne pouvez interagir avec l'API sans obtenir de jeton d'accès. Ces jetons sont un moyen d'identifier les utilisateurs qui doivent donner leur accord explicitement.

Afin d'obtenir un jeton d'accès vous devez diriger l'utilisateur vers l'adresse `https://www.thefirstspine.fr/<fr|en>/access?key=<public>[&redirect=<redirect>]`. 

Une fois l'utilisateur ayant donné son accord, il sera redirigé vers votre le domaine autorisé de votre application comme suite : `<allowed_domain>[<redirect>]?access_token=<access_token>`

Le jeton d'accès à l'utilisateur est contenu dans le paramètre GET `<access_token>` après retour sur votre application.

# ↔ Interagir avec l'API Arena

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

# 🎲 Comprendre l'API : Jeu / Actions / Réponses

Avant de lire cette partie, nous vous invitons à consulter cet article qui regroupe les réflexions que nous avons eu sur le système d'Arena : https://github.com/TeddyGandon/thefirstspine-arena-throughts/blob/master/core.md

Lorsque vous créez un jeu avec Arena une pile d'actions potientielle est créée. Arena attend alors une réponse de la part de l'utilisateur concerné par la pile d'actions disponible.

# 💗 Documentation de référence des méthodes

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

### Input

### Output

### Example

## getGameActions

### Input

### Output

### Example

## getMessages

### Input

### Output

### Example

## respondToGameAction

### Input

### Output

### Example

## zombifyUser

### Input

### Output

### Example

# 💗 Documentation de référence des objets

## Card

A card on the game.

**Fields**

| Name                 | Description                                     |
| -------------------- | ----------------------------------------------- |
| int `card_id`        | The ID of the resource                          |
| string `name`        | TODO                                            |
| string `description` | TODO                                            |
| string `image`       | TODO                                            |
| int `top_str`        | TODO                                            |
| int `top_def`        | TODO                                            |
| string `top_cpt`     | TODO                                            |
| int `right_str`      | TODO                                            |
| int `right_def`      | TODO                                            |
| string `right_cpt`   | TODO                                            |
| int `bottom_str`     | TODO                                            |
| int `bottom_def`     | TODO                                            |
| string `bottom_cpt`  | TODO                                            |
| int `left_str`       | TODO                                            |
| int `left_def`       | TODO                                            |
| string `left_cpt`    | TODO                                            |
| int `life`           | TODO                                            |
| string `cpt`         | TODO                                            |
| string `type`        | TODO                                            |

## Deck

A Deck resource is a pre-built deck in the game.

**Fields**

| Name                 | Description                                     |
| -------------------- | ----------------------------------------------- |
| int `deck_id`        | The ID of the resource                          |
| string `color`       | TODO                                            |
| string `name`        | TODO                                            |
| string `image`       | TODO                                            |
| string `description` | TODO                                            |
| Card[] `cards`       | TODO                                            |

## User

A user is a user registered on the website and on any services of The First Spine.

**Fields**

| Name                 | Description                                     |
| -------------------- | ----------------------------------------------- |
| int `user_id`        | The ID of the resource                          |
| string `name`        | The name of the user                            |
| object `points`      | The number of points the user has               |
| string `avatar`      | The image of the user                           |

## ArenaCard

TODO

**Fields**

| Name                 | Description                                     |
| -------------------- | ----------------------------------------------- |
| int `arena_card_id`  | The ID of the resource                          |
| int `user_id`        | The ID of the user which the card belongs to    |
| Card `card`          | The associated card                             |
| string `location`    | The location of the card                        |
| string `position`    | The position of the card on the board (X-Y)     |
| Object `options`     | The options of the card                         |

## ArenaGame

TODO

**Fields**

| Name                     | Description                                     |
| ------------------------ | ----------------------------------------------- |
| int `arena_game_id`      | The ID of the resource                          |
| string `game_type`       | TODO                                            |
| string `created_at`      | TODO                                            |
| User[] `users`           | The list of the users                           |
| Deck[] `decks`           | The list of the decks used by the users         |
| object `options`         | TODO                                            |
| ArenaGameType `gameType` | The game type associated with the game          |
| ArenaCard[] `board`      | The cards on the board                          |

## ArenaGameAction

An action is defined by a group of desired sub-actions called a script.

**Fields**

| Name                       | Description                                     |
| -------------------------- | ----------------------------------------------- |
| int `arena_game_action_id` | The ID of the resource                          |
| int `arena_game_id`        | The game ID concerned by the action             |
| int `user_id`              | The user ID concerned by the action             |
| int `reference`            | The reference of the action                     |
| string `title`             | The title of the action                         |
| Object `script`            | TODO                                            |

### Scripts syntaxe

Each action is defined by one or more prompts in the `script` property:
```
{
  (string neededProperty): {
    "type": (string type),
    "message": (string message),
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

Is it possible to have binded results in the parameters of an action script. Each binded result will be a string that look like this: `"$result"`.

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
| string[] `types`         | TODO                                            |
| int `min`                | TODO                                            |
| int `max`                | TODO                                            |

**choseSquare**

Chose a square on the board.

| Name                     | Description                                     |
| ------------------------ | ----------------------------------------------- |
| boolean `nearPlayer`     | TODO                                            |
| boolean `nearControlled` | TODO                                            |
| boolean `isEmpty`        | TODO                                            |
| string[] `nextTo`        | TODO                                            |
| string[] `types`         | TODO                                            |
| string[] `range`         | TODO                                            |

**skip**

Skip the action.

## ArenaGameType

TODO

**Fields**

| Name                     | Description                                     |
| ------------------------ | ----------------------------------------------- |
| string `backgroundImage` | TODO                                            |
| int `neededUsers`        | TODO                                            |
| bool `usePredefinedDeck` | TODO                                            |
| bool `pointsWin`         | TODO                                            |
| bool `pointsLose`        | TODO                                            |
| bool `timesPerWeek`      | TODO                                            |

## ArenaMessage

TODO

**Fields**

TODO
