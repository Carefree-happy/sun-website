---
sidebar_position: 3
---

> [Github Discussions](https://github.com/smartcontractkit/full-blockchain-solidity-course-js/discussions)  
- Ask questions and chat about the course here!  

> [Stack Exchange Ethereum](https://ethereum.stackexchange.com/)  
- Great place for asking technical questions about Ethereum  

> [StackOverflow](https://stackoverflow.com/)  
- Great place for asking technical questions overall

> vscode 安装 solidity + hardhat
specify it up above(make it to be something on words, )
## 1. base information

Web1: The permissionless open sourced web with static content.

Web2: The permissioned web, with dynamic content. Where companies run your agreements on their servers.

Web3: The permissionless web, with dynamic content. Where decentralized censorship resistant networks run your agreement and code. It generally is accompanied by the idea of user owned ecosystems, where the protocols you interact with you also own a portion of, instead of solely being the product.

[Ethereum](https://ethereum.org/en/)
[MetaMask](https://metamask.io/)
[网页查看钱包情况](https://goerli.etherscan.io/address/0x154c1c07bc9e344d9bfbde0a219c6a8854ede39b)
[钱包链接网络,等待发钱](https://faucets.chain.link/goerli)
Waiting for confirmation 可以看看链接地址在干啥
Transaction hash : 0xedfa6e09b79c359c4436536caf5887f93c1d264ca419a697cedb1421f94c297e
在[etherscan查看钱包情况时]能看到刚才的交易信息。

[区块链技术概念Demo](https://andersbrownworth.com/blockchain/)
[公私钥Demo](https://andersbrownworth.com/blockchain/public-private-keys/)

[1](https://ethereum.org/en/)
[2](https://goerli.etherscan.io/token/0x326c977e6efc84e512bb9c30f76e30c160ed06fb?a=0x154C1c07BC9E344D9BfBDE0a219C6a8854eDe39b)
[3](chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/home.html#)
[4](https://github.com/smartcontractkit/full-blockchain-solidity-course-js)

## 2. Solidity Basics

### basic

view 表示函数返回区块链上的数据信息

pure 表示存粹的逻辑运算

calldata & memory: the variable is only going to exist temporarily during the function is called, calldata can not change while memory can change

storage: variables exist, even outside of just the function executing.

[Chainlink](https://docs.chain.link/resources/link-token-contracts)
[get free ETH](https://goerlifaucet.com/)

首先见识一下各种类型，以及默认值

```sol
// SPDX-License-Identifier: MIT
pragma solidity 0.8.7;

contract SimpleStorage {
    bool hasFavorNumber = false;
    int32 favoriteNumber = -123;
    uint256 favoriteInt = 5;
    string favoriteNumberInText = "Five";
    bytes32 favBytes = "12121";
}

```

### 结论一： The more "stuff" in your function, the more gas it costs.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.7;

contract SimpleStorage {
    uint256 public favoriteNumber;

    function store(uint256 _favoriteNumber) public {
        favoriteNumber = _favoriteNumber;
        // favoriteNumber = favoriteNumber + 1;
    }
}
```
### deploy 结果
gas	137636 gas
transaction cost	119683 gas 
execution cost	119683 gas 

gas	137636 gas
transaction cost	119683 gas 
execution cost	119683 gas 



gas	181644 gas
transaction cost	157951 gas 
execution cost	157951 gas 


gas	181644 gas
transaction cost	157951 gas 
execution cost	157951 gas 

相同的逻辑中间添加注释代码不影响 gas 的值,注释取消 gas 变大

### store 1,2,3,10,100 编译完，第一次调用 store 产生的 gas 较大
gas	50283 gas
transaction cost	43724 gas 
execution cost	43724 gas 

gas	30618 gas
transaction cost	26624 gas 
execution cost	26624 gas 

gas	30618 gas
transaction cost	26624 gas 
execution cost	26624 gas 

gas	30618 gas
transaction cost	26624 gas 
execution cost	26624 gas 

gas	30618 gas
transaction cost	26624 gas 
execution cost	26624 gas

### 增加函数逻辑后，store 结论如上，且产生的 gas 变大

gas	50752 gas
transaction cost	44132 gas 
execution cost	44132 gas

gas	31087 gas
transaction cost	27032 gas 
execution cost	27032 gas 

gas	31087 gas
transaction cost	27032 gas 
execution cost	27032 gas 

gas	31087 gas
transaction cost	27032 gas 
execution cost	27032 gas 

gas	31087 gas
transaction cost	27032 gas 
execution cost	27032 gas 

Look inside the curly brackets

view & pure don't spend any gas

```solidity
    # pure function

    function add() public pure returns(uint8){
        return (1 + 1);
    }
```
### If a gas calling function calls a view or pure function - only then will it cost gas

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.7;

contract SimpleStorage {
    uint8 public favoriteNumber;

    function store(uint8 _favoriteNumber) public {
        favoriteNumber = _favoriteNumber;
        retrive();
    }

    function retrive() public view returns(uint8){
        return favoriteNumber;
    }
}

deploy
gas	169873 gas
transaction cost	147715 gas 
execution cost	147715 gas 

first store
gas	50528 gas
transaction cost	43937 gas 
execution cost	43937 gas 

second store
gas	30863 gas
transaction cost	26837 gas 
execution cost	26837 gas
```

### 结论三：view function is free, unless you're calling it inside of a function that costs gas, in which case it will cost gas.

basic solidity arrays & structs
```
People[] public person;
// 0: 2, Patrick, 1: 7,John
```

### Dynamic array vs fixed-sized array
```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.7;

contract SimpleStorage {
    uint8 public favoriteNumber;

    struct People {
        uint256 favoriteNumber;
        string name;
    }

    People[] public person;
    // 0: 2, Patrick, 1: 7,John

    function addPerson(string memory _name, uint256 _favoriteNumber) public {
        People memory newPerson = People({favoriteNumber: _favoriteNumber, name: _name});
        person.push(newPerson);
    }

    function store(uint8 _favoriteNumber) public {
        favoriteNumber = _favoriteNumber;
        retrive();
    }

    function retrive() public view returns(uint8){
        return favoriteNumber;
    }
}
```
### Basic Solidity Errors & Warnings

calldata is temporary variables that can't be modified.
memory is temporary variables that can be modified.
storage is permanent variables that can be modified.

a string is actually an array of bytes.

mapping 有点像类
声明 mapping(string => uint256) public nameToFavoriteNumber;
增加 nameToFavoriteNumber[_name] = _favoriteNumber;
[删除 ？？](https://me.tryblockchain.org/solidity-delete.html)

### deploy your first contract
https://www.youtube.com/watch?v=gyMwXuJrbJQ&t=12305s

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.7;

contract SimpleStorage {
    uint8 public favoriteNumber;

    struct People {
        uint256 favoriteNumber;
        string name;
    }

    mapping(string => uint256) public nameToFavoriteNumber;

    People[] public person;
    // 0: 2, Patrick, 1: 7,John

    function addPerson(string memory _name, uint256 _favoriteNumber) public {
        People memory newPerson = People({favoriteNumber: _favoriteNumber, name: _name});
        person.push(newPerson);
        nameToFavoriteNumber[_name] = _favoriteNumber;
    }

    function store(uint8 _favoriteNumber) public {
        favoriteNumber = _favoriteNumber;
        retrive();
    }

    function retrive() public view returns(uint8){
        return favoriteNumber;
    }
}
```

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.7;

contract SimpleStorage {
    uint public storeData;

    struct People {
        uint256 storeData;
        string name;
    }

    People[] public people;

    mapping(string => uint256) public nameToStoreData;

    function set(uint x) public {
        storeData = x;
    }

    function get() public view returns (uint) {
        return storeData;
    }

    function retrive() public view returns(uint256) {
        return storeData;
    }

    function addPerson(string memory _name, uint256 _favoriteNumber) public {
        people.push(People(_favoriteNumber, _name));
        nameToStoreData[_name] = _favoriteNumber;
    }
}
```

Testnet Faucets: 测试币发放网址

Main Faucet: https://faucets.chain.link 查看交易记录

Backup Faucet: https://goerlifaucet.com/

Chainlink 区块链链接
https://docs.chain.link/resources/link-token-contracts/#goerli-testnet

[Rinkeby Testnet Explorer](https://rinkeby.etherscan.io/)

## 3. Remix Storage Factory

### Importing Contracts into other Contracts
[code](https://github.com/smartcontractkit/full-blockchain-solidity-course-js#lesson-2-welcome-to-remix-simple-storage)

```
// SPDX-License-Identifier: MIT

pragma solidity 0.8.7;

import "./SimpleStorage.sol";

contract StorageFactory {
    SimpleStorage public simpleStorage;

    function createSimpleStorageContract() public {
        simpleStorage = new SimpleStorage();
    }
}
```

```
// SPDX-License-Identifier: MIT

pragma solidity 0.8.7;

import "./SimpleStorage.sol";

contract StorageFactory {
    SimpleStorage[] public simpleStorageArray;

    function createSimpleStorageContract() public {
        SimpleStorage simpleStorage = new SimpleStorage();
        simpleStorageArray.push(simpleStorage);
    }

    function sfStore(uint256 _simpleStorageIndex, uint256 _simpleStorageNumber) public {
        // address
        // ABI - Application Binary Interface
        SimpleStorage simpleStorage = simpleStorageArray[_simpleStorageIndex];

        simpleStorage.store(_simpleStorageNumber);
    }

    function sfGet(uint256 _simpleStorageIndex)
}
```
### Inheritance & Overrides
```solidity
// SPDX-License-Identifier: MIT

pragma solidity 0.8.7;

import "./SimpleStorag.sol";

contract ExtraStorage is SimpleStorage {
    // + 5
    // override 原函数方法添加virtual，继承方法添加override
    // virtual override

    function store(uint256 _favoriteNumber) public override{
        favoriteNumber = _favoriteNumber + 5;
    }
}
```
### Recap 概述

## 4. Remix Fund Me




## 5. Ethers.js Simple Storage

## 6. Hardhat Simple Storage

## 7. Hardhat Fund Me

## 8. HTML / Javascript Fund Me (Full Stack / Front End)

## 9. Hardhat Smart Contract Lottery

## 10. NextJS Smart Contract Lottery (Full Stack / Front End)

## 11. Hardhat Starter Kit

## 12. Hardhat ERC20s

## 13. Hardhat DeFi & Aave

## 14. Hardhat NFTs (EVERYTHING you need to know about NFTs)

## 15. NextJS NFT Marketplace (If you finish this lesson, you are a full-stack MONSTER!)

## 16. Hardhat Upgrades

## 17. Hardhat DAOs

## 18. Security & Auditing


FAQ:
TypeError: Data location must be "memory" or "calldata" for parameter in function, but none was given.

Potentially should be constant/view/pure but is not.


纸房子：韩国版  
电驭叛客