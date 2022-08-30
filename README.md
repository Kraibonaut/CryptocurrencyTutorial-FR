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

We are using [Remix IDE](https://remix.ethereum.org/) for writing the smart contract.

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

This line specifies the compiler version of Solidity to be used. `^0.8.0` means any version greater than `0.8.0`. Usually, you would want to use the latest Solidity compiler version, as a new version usually implies either new features or optimizations.



---


```solidity
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol";
```

This line imports the `ERC-20` token standard from [OpenZeppelin](https://openzeppelin.com/) (OZ). OZ is an Ethereum security company. Among other things, OZ develops reference contracts for popular smart contract standards which are thoroughly tested and secure. Whenever implementing a smart contract which needs to comply with a standard, try to find an OZ reference implementation rather than rewriting the entire standard from scratch.

You can look at the implementation of `ERC-20` standard contract if you want by following the link - [https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol)

> Note: In the Sophomore track, we will take a deeper dive into the ERC-20 standard contract to understand everything that is going on within that contract.

---

```solidity
contract LW3Token is ERC20 {
    ...
}
```

This specifies a new contract, named LW3Token, in our Solidity file. Also, it says that this contract `is` an instance of `ERC20`. `ERC20` in this case refers to the standard contract we imported from OpenZeppelin. 

Essentially, we are extending the `ERC20` standard contract we imported from OpenZeppelin. So all the functions and logic that is built into `ERC20` is available for us to use, and we can add our own custom logic on top of it. 

If you are familiar with Object Oriented Programming principles, you can think of this as a class extending another class.


---

```solidity
constructor(string memory _name, string memory _symbol) ERC20(_name, _symbol) {
     ...
}
```

This bit has slightly weird syntax that you might not have seen before. `Kotlin` actually has some similar syntax, but I digress.

Essentially, we created `constructor` function that is called when the smart contract is first deployed. Within the constructor, we want two arguments from the user - `_name` and `_symbol` which specify the name and symbol of our cryptocurrency. E.g. name = Ethereum, symbol = ETH.

What happens after it is more interesting. Immediately after specifying the constructor function, we call `ERC20(_name, _symbol)`.

The `ERC20` contract we imported from OpenZeppelin has its own constructor, which requires the `name` and `symbol` parameters. Since we are extending the ERC20 contract, we need to initialize the ERC20 contract when we deploy ours. So, as part of our constructor, we also need to call the constructor on the `ERC20` contract.

Therefore, we are providing `_name` and `_symbol` variables to our contract, which we immediately pass on to the `ERC20` constructor, thereby initializing the `ERC20` smart contract.


---

```solidity
_mint(msg.sender, 10 * 10 ** 18);
```

`_mint` is an `internal` function within the `ERC20` standard contract, which means that it can only be called by the contract itself. External users cannot call this function. 

Since you as the developer want to receive some tokens when you deploy this contract, we call the `_mint` function to mint some tokens to `msg.sender`.

`_mint` takes two arguments - an address to mint to, and the amount of tokens to mint

`msg.sender` is a global variable injected by the Ethereum Virtual Machine, which is the address which made this transaction. Since you will be the one deploying this contract, your address will be there in `msg.sender`. 

`10 * 10 ** 18` specifies that you want 10 full tokens to be minted to your address.


---

> Note: You might be wondering why we did not just write `10` in the amount, instead of `10 ** 18` (which is actually 10 ^ 18).

Essentially, Solidity does not support floating point numbers - that is decimals. Also, since ERC20 tokens deal with money, using floating point numbers is a bad idea.

As an example, consider the simple calculation `(1/3) * 3` in a language that supports floating point numbers. What do you think this returns? 

If you thought it would return 1, you are wrong.

Due to inaccuracies in floating point calculations, since computers cannot represent an infinite number of digits, `(1/3) * 3` actually yields something like `0.999999999`.

As such, when representing financial currencies, decimals are not used due to calculation errors. As an alternative, we represent every currency as an amount relative to the smallest indivisible part of that currency. For example, $1 is represented as 100 cents, since you can't get smaller than 1 cent when dealing with USD. In that numbering system, 1 cent is just 1, not 0.01. $0.33 is represented as 33, not (1/3). 

`ERC20` tokens by default work with 18 decimal places. So 1 full `LW3Token` in this case, is actually represented as `10 ^ 18`. Therefore, to get 10 full `LW3Tokens`, we use `10 * 10 ** 18`.


---

## Compiling

Compile your contract by either pressing Save (CTRL + S on Windows, Command + S on Mac), or by going over to the `Compiler` tab in Remix, selecting `LW3Token.sol`, and hitting `Compile`.

![](https://i.imgur.com/grMZBw5.png)

## Deploying

Head over to the `Deployer` tab in Remix.

Select the `Injected Web3` environment (ensure you are on the Rinkeby Test Network), and connect your Metamask wallet.

Select the `LW3Token.sol` contract, and enter values for the constructor arguments `_name` and `_symbol`.

![](https://i.imgur.com/HvWLfZq.png)

Click `Transact` and approve the transaction from Metamask to deploy your contract!

![](https://i.imgur.com/rvBbYl4.png)

When deployed, the contract should show up under the `Deployed Contracts` section. Click the `Copy Address` button to copy the contract address.

![](https://i.imgur.com/19haj2L.png)

Go to [Rinkeby Etherscan](https://rinkeby.etherscan.io/) and search for your contract address and you should see it there!

Take a screenshot of it and share it on Discord to show off your newly created token :D

## Viewing Tokens in Metamask
You may notice that even though you minted tokens to your address, they don't show up in Metamask.

This is because Metamask cannot detect random ERC20 token balances (since there are literally hundreds of thousands of them). They have a list of the most well known ERC20 tokens that they can show automatically, but apart from that, for your own tokens, you will usually need to tell Metamask to add it to your wallet manually.

To do so:
- Copy your contract address
- Open Metamask and click `Import Tokens` in the `Assets` tab
- ![](https://i.imgur.com/XxKFtU0.png)
- Enter your Token Contract Address, and it should detect the name and number of decimals automatically
- Click Add, and you will see your balance in Metamask!
- ![](https://i.imgur.com/Wwxe0kg.png)

Share a screenshot in the Discord!

---

Congratulations! You've successfully deployed and minted your own ERC20 token! Onwards and upwards from here!
