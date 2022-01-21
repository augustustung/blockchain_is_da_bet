# Teaching myself BLOCKCHAIN 🚂🚂🚂 Day 4

## 4. `BATTLE` SYSTEM xDDDDDDDDDDD

* The `payable` Modifier

> `payable` are a special type of function that can receive Ether.
```
contract OnlineStore {
  function buySomething() external payable {
    // Check to make sure 0.001 ether was sent to the function call:
    require(msg.value == 0.001 ether);
    // If so, some logic to transfer the digital item to the caller of the function:
    transferThing(msg.sender);
  }
}
```

```
// Assuming `OnlineStore` points to your contract on Ethereum:
OnlineStore.buySomething({from: web3.eth.defaultAccount, value: web3.utils.toWei(0.001)})
```

* Withdraws

```
contract GetPaid is Ownable {
  function withdraw() external onlyOwner {
    address payable _owner = address(uint160(owner()));
    _owner.transfer(address(this).balance);
  }
}

uint itemFee = 0.001 ether;
msg.sender.transfer(msg.value - itemFee);
```

* Random Numbers

```
// Generate a random number between 1 and 100:
uint randNonce = 0;
uint random = uint(keccak256(abi.encodePacked(now, msg.sender, randNonce))) % 100;
randNonce++;
uint random2 = uint(keccak256(abi.encodePacked(now, msg.sender, randNonce))) % 100;
```

## 5. ERC721 & Crypto-Collectibles

* ERC721 Standard, Multiple Inheritance

> Multiple Inheritance
```
contract SatoshiNakamoto is NickSzabo, HalFinney {
  // Omg, the secrets of the universe revealed!
}
```

* Preventing Overflows

```
using SafeMath for uint256;

uint256 a = 5;
uint256 b = a.add(3); // 5 + 3 = 8
uint256 c = a.mul(2); // 5 * 2 = 10
```

* COMMENTSSSSSSSS
```
/// @title A contract for basic math operations
/// @author H4XF13LD MORRIS 💯💯😎💯💯
/// @notice For now, this contract just adds a multiply function
contract Math {
  /// @notice Multiplies 2 numbers together
  /// @param x the first uint.
  /// @param y the second uint.
  /// @return z the product of (x * y)
  /// @dev This function does not currently check for overflows
  function multiply(uint x, uint y) returns (uint z) {
    // This is just a normal comment, and won't get picked up by natspec
    z = x * y;
  }
}

@title and @author are straightforward.

@notice explains to a user what the contract / function does. @dev is for explaining extra details to developers.

@param and @return are for describing what each parameter and return value of a function are for.

Note that you don't always have to use all of these tags for every function — all tags are optional. But at the very least, leave a @dev note explaining what each function does.
```


## 6. App Front-ends & Web3.js

* Infura

> Infura is a service that maintains a set of Ethereum nodes with a caching layer for fast reads, which you can access for free through their API. Using Infura as a provider, you can reliably send and receive messages to/from the Ethereum blockchain without needing to set up and maintain your own node.

```
var web3 = new Web3(new Web3.providers.WebsocketProvider("wss://mainnet.infura.io/ws"));
```

* Metamask

> Metamask is a browser extension for Chrome and Firefox that lets users securely manage their Ethereum accounts and private keys, and use these accounts to interact with websites that are using Web3.js.

* Contract Address

> After you deploy your contract, it gets a fixed address on Ethereum where it will live forever. If you recall from Lesson 2, the address of the CryptoKitties contract on Ethereum mainnet is `0x06012c8cf97BEaD5deAe237070F9587f8E7A266d.`

* Contract ABI

> ABI stands for Application Binary Interface. Basically it's a representation of your contracts' methods in JSON format that tells Web3.js how to format function calls in a way your contract will understand.

* Calling Contract Functions

> `Call` is used for view and pure functions. It only runs on the local node, and won't create a transaction on the blockchain.
`myContract.methods.myMethod(123).call()`

> `Send` will create a transaction and change data on the blockchain. You'll need to use send for any functions that aren't `view` or `pure`.

`myContract.methods.myMethod(123).send()`

* Metamask & Accounts

> Getting the user's account in MetaMask
`var userAccount = web3.eth.accounts[0]`
 
 or

```
var accountInterval = setInterval(function() {
  // Check if account has changed
  if (web3.eth.accounts[0] !== userAccount) {
    userAccount = web3.eth.accounts[0];
    // Call some function to update the UI with the new account
    updateInterface();
  }
}, 100);
```