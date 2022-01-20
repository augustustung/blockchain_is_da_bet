# Teaching myself BLOCKCHAIN 🚂🚂🚂 Day 3

## 3. Advanced Solidity Concepts

* Immutability of Contracts

> After deploy a contract to Ethereum, it's immutable. If there's a flaw in contract code, there's no way to patch it later.
> Instead of hard-coded  `ckAddress = ...`, we can create a function `setKittyContractAddress ` to set a new address for the variable.

```
function setKittyContractAddress(address _address) external {
    kittyContract = KittyInterface(_address);
  }	
```

* Ownable Contracts

> Because of `setKittyContract` is external so anyone can call it. To handle cases like this, one common practice that has emerged is to make contracts `Ownable`.


* onlyOwner Function Modifier

> Function Modifiers looks like function but uses key word `modifier` instead of the keyword `function`.

> onlyOwner function:
```
  modifier onlyOwner() {
    require(isOwner());
    _;
  }

  function renounceOwnership() public onlyOwner {
    emit OwnershipTransferred(_owner, address(0));
    _owner = address(0);
  }
```


* Gas — the fuel Ethereum DApps run on

> In Solidity, users have to pay every time they execute a function on DApp using a currency called `gas`.

> How much gas is required to execute a function depends on how complex that function's logic is.

> Struct packing to save gas
```
struct   {
  uint a;
  uint b;
  uint c;
}

struct MiniMe {
  uint32 a;
  uint32 b;
  uint c;
}

// `mini` will cost less gas than `normal` because of struct packing
NormalStruct normal = NormalStruct(10, 20, 30);
MiniMe mini = MiniMe(10, 20, 30); 
```

* Time Units
```
uint lastUpdated;

// Set `lastUpdated` to `now`
function updateTimestamp() public {
  lastUpdated = now;
}

// Will return `true` if 5 minutes have passed since `updateTimestamp` was 
// called, `false` if 5 minutes have not passed
function fiveMinutesHavePassed() public view returns (bool) {
  return (now >= (lastUpdated + 5 minutes));
}
```

* Passing structs as arguments
```
function _doStuff(Zombie storage _zombie) internal {
  // do stuff with _zombie
}
```
 
> Remember to have the last line of the modifier call the rest of the function with `_;`.