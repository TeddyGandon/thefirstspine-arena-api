# TFS Arena - API publique

## Sommaire

- Déclarer une clé API
- Obtenir un jeton d'accès
- Interagir avec l'API Arena
- Documentation de référence

## Déclarer une clé API

Avant de commencer à créer votre client et intégrer TFS à votre plateforme vous devez au préalable obtenir une clé API. Vous pouvez nous contacter pour en obtenir une. Nous n'en générons qu'à la demande et à l'étude de votre projet (on ne va pas vous générer une clé API pour vous permettre d'augmenter votre ratio victoires / défaites).

Une clé api contient plusieurs éléments :
- L'identifiant publique appelé `public`
- L'identifiant secret appelé `secret`
- Le domain autorisé sur la clé API appelé `allowed_domain`

## Obtenir un jeton d'accès

Vous ne pouvez interagir avec l'API sans obtenir de jeton d'accès. Ces jetons sont un moyen d'identifier les utilisateurs qui doivent donner leur accord explicitement.

Afin d'obtenir un jeton d'accès vous devez diriger l'utilisateur vers l'adresse `https://www.thefirstspine.fr/<fr|en>/access?key=<public>[&redirect=<redirect>]`. 

Une fois l'utilisateur ayant donné son accord, il sera redirigé vers votre le domaine autorisé de votre application comme suite : `<allowed_domain>[<redirect>]?access_token=<access_token>`

Le jeton d'accès à l'utilisateur est contenu dans le paramètre GET `<access_token>` après retour sur votre application.
