# Level 8 - Cryptocurrency Tutorial (ERC20)

Dans ce tutoriel étape par étape, vous apprendrez à créer et à déployer un jeton `ERC-20` sur Ethereum. 

Nous utiliserons [Metamask](https://metamask.io/) et [Remix IDE](https://remix.ethereum.org/) pour ce tutoriel.

## Vous préférez une vidéo ?
Si vous préférez apprendre à partir d'une vidéo, nous avons un enregistrement disponible de ce tutoriel sur notre chaîne YouTube. Regardez la vidéo en cliquant sur la capture d'écran ci-dessous, ou lisez le tutoriel !

[![Cryptocurrency Tutorial](https://i.imgur.com/RVzkVsZ.png)](https://www.youtube.com/watch?v=5yM5bojHbmQ "Cryptocurrency Tutorial")

## Qu'est-ce que l'ERC-20 ?
`ERC` signifie `Ethereum Request for Comment`. Il s'agit essentiellement de normes qui ont été approuvées par la communauté et qui sont utilisées pour transmettre des exigences et des spécifications techniques pour certains cas d'utilisation.

`ERC-20`  spécifiquement est une norme qui décrit les spécifications techniques d'un jeton fongible. 

> Un jeton fongible est un jeton dont toutes les "parties" sont identiques. Échanger 1 ETH contre un autre 1 ETH ne change rien. Vous avez toujours 1 ETH. Par conséquent, l'ETH est un jeton fongible. Toute monnaie fiduciaire est également fongible. 

> Les NFT sont des exemples de jetons non fongibles (nous y reviendrons plus tard) où chaque jeton est différent d'un autre jeton.

La plupart des jetons sur Ethereum sont conformes à la spécification `ERC-20`. Suivre une norme comme le `ERC-20` permet aux développeurs d'applications qui utilisent les jetons `ERC-20` de supporter facilement *tous* les jetons `ERC-20` sans avoir à écrire du code spécialisé pour chacun d'entre eux.

Par exemple, les échanges décentralisés comme [Uniswap](https://uniswap.org/) vous permettent d'échanger n'importe quel jeton contre n'importe quel autre jeton. Ceci n'est possible que parce que presque tous les jetons suivent la norme `ERC-20`, donc Uniswap pourrait écrire un code qui fonctionne avec tous les jetons suivant la norme.

## Conditions préalables

- Assurez-vous que vous avez téléchargé et installé [Metamask](https://metamask.io/). 
- Sélectionnez le `Rinkeby Testnet` réseau avec lequel travailler
- Demandez de l'éther de testnet sur Rinkeby par l'un des robinets suivants :
    - [Metamask Faucet](https://faucet.metamask.io/)
    - [Chainlink Faucet](https://faucets.chain.link/rinkeby)
    - [Paradigm Faucet](https://faucet.paradigm.xyz/)

Une fois que vous avez mis en place tous ces éléments, commençons !

## Écrire le code

Nous utilisons [Remix IDE](https://remix.ethereum.org/) pour écrire le contrat intelligent.

Dans Remix, créez un nouveau contrat, j'ai nommé le mien `LW3Token.sol` - vous pouvez lui donner le nom que vous voulez !

Dans le contrat, écrivez le code suivant :

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol";

contract LW3Token is ERC20 {
    constructor(string memory _name, string memory _symbol) ERC20(_name, _symbol) {
        _mint(msg.sender, 10 * 10 ** 18);
    }
}
```

Décortiquons-le ligne par ligne pour comprendre ce qui se passe :

```solidity
pragma solidity ^0.8.0;
```

Cette ligne spécifie la version du compilateur de Solidity à utiliser. `^0.8.0` signifie toute version supérieure à `0.8.0`. Habituellement, vous voudrez utiliser la dernière version du compilateur Solidity, car une nouvelle version implique généralement de nouvelles fonctionnalités ou des optimisations.



---


```solidity
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol";
```

Cette ligne importe le standard de jeton `ERC-20` à partir de [OpenZeppelin](https://openzeppelin.com/) (OZ). OZ est une société de sécurité Ethereum. Entre autres choses, OZ développe des contrats de référence pour les normes populaires de contrats intelligents qui sont testés en profondeur et sécurisés. Lorsque vous mettez en œuvre un contrat intelligent qui doit être conforme à une norme, essayez de trouver une implémentation de référence d'OZ plutôt que de réécrire toute la norme à partir de zéro.

Vous pouvez regarder l'implémentation du contrat standard `ERC-20` si vous le souhaitez en suivant ce lien - [https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol)

> Note : Dans le cadre du cursus Sophomore, nous allons plonger plus profondément dans le contrat standard ERC-20 afin de comprendre tout ce qui se passe dans ce contrat.

---

```solidity
contract LW3Token is ERC20 {
    ...
}
```

Ceci spécifie un nouveau contrat, nommé LW3Token, dans notre fichier Solidity. Elle indique également que ce contrat `est` une instance de `ERC20`. Dans ce cas, `ERC20` fait référence au contrat standard que nous avons importé d'OpenZeppelin. 

Essentiellement, nous étendons le contrat standard `ERC20` que nous avons importé d'OpenZeppelin. Ainsi, toutes les fonctions et la logique qui sont intégrées dans `ERC20` sont disponibles pour nous, et nous pouvons ajouter notre propre logique personnalisée par-dessus. 

Si vous êtes familier avec les principes de la programmation orientée objet, vous pouvez voir cela comme une classe étendant une autre classe.


---

```solidity
constructor(string memory _name, string memory _symbol) ERC20(_name, _symbol) {
     ...
}
```

Cette partie a une syntaxe légèrement bizarre que vous n'avez peut-être jamais vue auparavant. En fait, `Kotlin` a une syntaxe similaire, mais je m'égare.

Essentiellement, nous avons créé la fonction `constructor` qui est appelée lorsque le contrat intelligent est déployé pour la première fois. Dans le constructor, nous voulons deux arguments de l'utilisateur - `_name` et `_symbol` qui spécifient le nom et le symbole de notre crypto-monnaie. Par exemple, nom = Ethereum, symbole = ETH.

Ce qui se passe ensuite est plus intéressant. Immédiatement après avoir spécifié la fonction constructeur, nous appelons `ERC20(_name, _symbol)`.

Le contrat `ERC20` que nous avons importé d'OpenZeppelin a son propre constructor, qui nécessite les paramètres `name` et `symbol`. Puisque nous étendons le contrat ERC20, nous devons initialiser le contrat ERC20 lorsque nous déployons le nôtre. Ainsi, dans le cadre de notre constructor, nous devons également appeler le constructor du contrat `ERC20`.

Nous fournissons donc les variables `_name` et `_symbol` à notre contrat, que nous transmettons immédiatement au constructeur `ERC20`, initialisant ainsi le contrat intelligent `ERC20`.


---

```solidity
_mint(msg.sender, 10 * 10 ** 18);
```

`_mint` est une fonction `interne` du contrat standard `ERC20`, ce qui signifie qu'elle ne peut être appelée que par le contrat lui-même. Les utilisateurs externes ne peuvent pas appeler cette fonction.

Puisque vous, en tant que développeur, voulez recevoir des jetons lorsque vous déployez ce contrat, nous appelons la fonction `_mint` pour monnayer des jetons à `msg.sender`.

`_mint` prend deux arguments - une adresse vers laquelle frapper, et la quantité de jetons à frapper.

`msg.sender` est une variable globale injectée par la machine virtuelle d'Ethereum, qui est l'adresse qui a fait cette transaction. Puisque vous serez celui qui déploie ce contrat, votre adresse sera là dans `msg.sender`.

`10 * 10 ** 18` indique que vous voulez que 10 jetons complets soient frappés à votre adresse.


---

> Note : Vous vous demandez peut-être pourquoi nous n'avons pas simplement écrit " 10 " dans le montant, au lieu de " 10 ** 18 " (qui est en fait 10 ^ 18).

Essentiellement, Solidity ne prend pas en charge les nombres à virgule flottante - c'est-à-dire les décimales. De plus, comme les jetons ERC20 traitent de l'argent, l'utilisation de nombres à virgule flottante est une mauvaise idée.

A titre d'exemple, considérez le calcul simple `(1/3) * 3` dans un langage qui supporte les nombres à virgule flottante. A votre avis, qu'est-ce que cela donne ?

Si vous pensiez qu'il retournerait 1, vous avez tort.

En raison des imprécisions des calculs en virgule flottante, les ordinateurs ne pouvant pas représenter un nombre infini de chiffres, " (1/3) * 3 " donne en fait quelque chose comme " 0,999999999 ".

Ainsi, lors de la représentation des monnaies financières, les décimales ne sont pas utilisées en raison des erreurs de calcul. Comme alternative, nous représentons chaque devise comme un montant relatif à la plus petite partie indivisible de cette devise. Par exemple, 1 dollar est représenté par 100 cents, car il est impossible de descendre en dessous de 1 cent lorsqu'il s'agit de dollars américains. Dans ce système de numération, 1 cent est simplement 1, et non 0,01. 0,33 $ est représenté par 33, et non par (1/3).

Les jetons `ERC20` fonctionnent par défaut avec 18 décimales. Ainsi, 1 `LW3Token` complet dans ce cas, est en fait représenté par `10 ^ 18`. Donc, pour obtenir 10 `LW3Tokens` complets, on utilise `10 * 10 ** 18`.


---

## Compiling

Compilez votre contrat en appuyant soit sur Enregistrer (CTRL + S on Windows, Command + S on Mac), ou en allant sur l'onglet `Compiler` dans Remix, en sélectionnant `LW3Token.sol`, et en appuyant sur `Compile`.

![](https://i.imgur.com/grMZBw5.png)

## Déploiement 

Allez sur l'onglet "Déployer" dans Remix..

Sélectionnez l'environnement `Injected Web3` (assurez-vous d'être sur le Rinkeby Test Network), et connectez votre porte-monnaie Metamask.

Sélectionnez le contrat `LW3Token.sol`, et entrez les valeurs pour les arguments du constructeur `_name` et `_symbol`.

![](https://i.imgur.com/HvWLfZq.png)

Cliquez sur "Transact" et approuvez la transaction de Metamask pour déployer votre contrat !

![](https://i.imgur.com/rvBbYl4.png)

Une fois déployé, le contrat doit apparaître dans la section "Deployed Contracts". Cliquez sur le bouton "Copier l'adresse" pour copier l'adresse du contrat.

![](https://i.imgur.com/19haj2L.png)

Aller à [Rinkeby Etherscan](https://rinkeby.etherscan.io/) et recherchez l'adresse de votre contrat et vous devriez le voir !

Faites une capture d'écran et partagez-la sur Discord pour montrer votre jeton nouvellement créé :D

## Visualisation des jetons dans Metamask
Vous pouvez remarquer que même si vous avez frappé des jetons à votre adresse, ils n'apparaissent pas dans Metamask.

Cela est dû au fait que Metamask ne peut pas détecter les soldes aléatoires des jetons ERC20 (puisqu'il en existe littéralement des centaines de milliers). Ils ont une liste des jetons ERC20 les plus connus qu'ils peuvent afficher automatiquement, mais à part cela, pour vos propres jetons, vous devrez généralement demander à Metamask de les ajouter à votre portefeuille manuellement.

Pour ce faire:
- Copie de l'adresse de votre contrat
- Ouvrez Metamask et cliquez sur "Import Tokens" dans l'onglet "Assets".
- ![](https://i.imgur.com/XxKFtU0.png)
- Entrez l'adresse de votre contrat de jeton, et il devrait détecter automatiquement le nom et le nombre de décimales.
- Cliquez sur Ajouter, et vous verrez votre solde dans Metamask !
- ![](https://i.imgur.com/Wwxe0kg.png)

Share a screenshot in the Discord!

---

Félicitations ! Vous avez réussi à déployer et à frapper votre propre jeton ERC20 ! En avant et vers le haut à partir de maintenant !
