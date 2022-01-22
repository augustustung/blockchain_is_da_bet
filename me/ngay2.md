# Tự học BLOCKCHAIN 🚂🚂🚂 Ngày 2

## 2. Mappings, Addresses, Inheritance (Kế thừa)

* Adresses:

> Chuỗi `Ethereum` được tạo thành từ các tài khoản, có thể coi address giống như tài khoản ngân hàng. 
`0x0cE446255506E92DF41614C46F1d6df9Cc969183`

* Mapping:

> Ánh xạ là một cách khác để lưu trữ dữ liệu có tổ chức trong Solidity.

```
// Đối với ứng dụng tài chính, lưu trữ `uint` là số dư tài khoản của người dùng:
mapping (address => uint) public accountBalance;
// hoặc có thể dùng để tìm kiến tài khoản
mapping (uint => string) userIdToName;
```

* msg.sender

> Trong Solidity có những biến toàn cục, trong đó có `msg.sender`. Nó là `address` của người gửi.

```
mapping (address => uint) favoriteNumber;

function setMyNumber(uint _myNumber) public {
  // Cập nhật `favoriteNumber` của bạn và ánh xạ lưu vào `_myNumber` tại `msg.sender`
  favoriteNumber[msg.sender] = _myNumber;
  // ^ Cú pháp này lưu dữ liệu giống array hoặc object
}

function whatIsMyNumber() public view returns (uint) {
  // Lấy ra giá trị đã lưu trong `msg.sender`
  // Sẽ = `0` nếu người dùng chưa gọi `setMyNumber`.
  return favoriteNumber[msg.sender];
}
```

* require();

> require giống với câu lệnh if nếu false thì sẽ return luôn.

```
function sayHi(string memory _name) public returns (string memory) {
  // so sánh nếu _name bằng "Tung". Trả ra lỗi nếu sai.
  // (Side note: Solidity không có so sánh string, nên phải so sánh keccak256 hashes để xem string có bằng không)
  require(keccak256(abi.encodePacked(_name)) == keccak256(abi.encodePacked("Tung")));
  // Nếu đúng, thì sẽ chạy tiếp:
  return "Hi!";
}
```

* Inheritance(kế thừa):

> Một trong những tính năng của Solidity giúp `contract` dễ quản lý

```
contract Doge {
  function catchphrase() public returns (string memory) {
    return "So Wow CryptoDoge";
  }
}

contract BabyDoge is Doge {
  function anotherCatchphrase() public returns (string memory) {
    return "Such Moon BabyDoge";
  }
}

BabyDoge kế thừa từ Doge. Nó có nghĩa là nếu có dịch hay deploy BabyDoge, thì nó vẫn có quyền truy cập vào cả catchphrase() và anotherCatchphrase() (và tất cả public functions đã khai báo trong Doge).

```

* Import:

```
import "./someothercontract.sol";

contract newContract is SomeOtherContract {

}
```

* Storage vs Memory (Data location):

> `Storage`: Lưu trữ biến vĩnh viễn.

> `Memory`: Lưu tạm rời, sẽ bị xoá bên ngoài function

```
contract SandwichFactory {
  struct Sandwich {
    string name;
    string status;
  }

  Sandwich[] sandwiches;

  function eatSandwich(uint _index) public {
    // Sandwich mySandwich = sandwiches[_index];

    // ^ Trông rất ổn nhưng Solidity sẽ cảnh báo nên 
    // nên khai báo rõ ràng `storage` hoặc `memory` ở đây.

    // khai báo với từ khoá `storage`:
    Sandwich storage mySandwich = sandwiches[_index];
    // ...trong trường hợp nào thì `mySandwich` là con trỏ đến `sandwiches[_index]`
    // trong storage, và...
    mySandwich.status = "Eaten!";
    // ...cái này sẽ thay đổi vĩnh viễn `sandwiches[_index]` trên blockchain.

    // Nếu chỉ muốn bản sao chép, có thể dùng `memory`:
    Sandwich memory anotherSandwich = sandwiches[_index + 1];
    // ...trong trường hợp này `anotherSandwich` nó sẽ chỉ copy dữ liệu trong memory và
    anotherSandwich.status = "Eaten!";
    // ...sẽ chỉ thay đổi biến tạm thời và khônng có tác dụng `sandwiches[_index + 1]`. Giải pháp:
    sandwiches[_index + 1] = anotherSandwich;
    // ...nếu muốn lưu vào blockchain storage.
  }
}
```


* Internal and External

> In addition to public and private, Solidity has two more types of visibility for functions: internal and external.
> Internal is the same as private
> External is similar to public

```
contract Sandwich {
  uint private sandwichesEaten = 0;

  function eat() internal {
    sandwichesEaten++;
  }
}

contract BLT is Sandwich {
  uint private baconSandwichesEaten = 0;

  function eatWithBacon() public returns (string memory) {
    baconSandwichesEaten++;
    // We can call this here because it's internal
    eat();
  }
}
```

* Interface

```
contract NumberInterface {
  function getNum(address _myAddress) public view returns (uint);
}

contract MyContract {
  address NumberInterfaceAddress = 0xab38... 
  // ^ The address of the FavoriteNumber contract on Ethereum
  NumberInterface numberContract = NumberInterface(NumberInterfaceAddress);
  // Now `numberContract` is pointing to the other contract

  function someFunction() public {
    // Now we can call `getNum` from that contract:
    uint num = numberContract.getNum(msg.sender);
    // ...and do something with `num` here
  }
}
```

* Handling Multiple Return Values

```
function multipleReturns() internal returns(uint a, uint b, uint c) {
  return (1, 2, 3);
}

function processMultipleReturns() external {
  uint a;
  uint b;
  uint c;
  // This is how you do multiple assignment:
  (a, b, c) = multipleReturns();
}

// Or if we only cared about one of the values:
function getLastReturnValue() external {
  uint c;
  // We can just leave the other fields blank:
  (,,c) = multipleReturns();
}
```

* If statements

> If statements in Solidity look just like javascript

```
function eatBLT(string memory sandwich) public {
  // Remember with strings, we have to compare their keccak256 hashes
  // to check equality
  if (keccak256(abi.encodePacked(sandwich)) == keccak256(abi.encodePacked("BLT"))) {
    eat();
  }
}
```