

# YourName

1. 自我介绍

2. 你认为你会完成本次残酷学习吗？
   
## Notes

<!-- Content_START -->

### 2024.09.23

今天學了function, event, static variables, 還有好像是qualifier一樣的東西，constant, internal, private, public。 
function還有modifier，可以設置啟動某function的條件。
還學到了boollean和一些運算式，很多都是比以前學過的東西。
現在就算小小複習一下，明天會更認真學！

### 2024.09.24

**constant**
constant变量必须在声明的时候初始化，之后再也不能改变。尝试改变的话，编译不通过。
**immutable**
immutable变量可以在声明时或构造函数中初始化，因此更加灵活。在Solidity v8.0.21以后，immutable变量不需要显式初始化，未显式初始化的immutable变量将使用数值类型的初始值（见 8. 变量初始值）。反之，则需要显式初始化。 若immutable变量既在声明时初始化，又在constructor中初始化，会使用constructor初始化的值。

### 2024.09.25
今天試著做一個很簡單的計算器還有一個很簡單的Twitter工具，然後deploy，view on etherscan.
另外還去了Ethereum Sepolia Faucet 拿了一點testnet用的Eth。

```
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;
contract Calculator {
    uint256 result = 0;
    function add( uint256 num) external {
        result += num;
    }
    function subtract(uint256 num) public {
        result -= num;
    }
    function multiply(uint256 num) public {
        result *= num;
    }
    function get() public view  returns (uint256) {
        return result;
    }
}
```
另外學了Mapping和array。
Mapping，就相當於給每個value 一個 key
每個Array都有對應的從0開始的Index. 
以上。

### 2024.09.27
昨天忘記更新，今天補上，今天嘗試理解了以下的代碼:
```
// SPDX-License-Identifier: GPL-3.0

pragma solidity >=0.8.2 <0.9.0;

contract Twitter {

    //mapping address to string and call it tweets
    mapping(address => string[]) public tweets;

    function createTwitter (string memory _tweet) public {
    // memory means store as temporary memory
        tweets[msg.sender].push(_tweet);
    }
    // mapping is both key and value, could be addresses to names. So you can find name by address 

    function getTweet(address _owner, uint _i) public view returns (string memory){
        return tweets[_owner][_i];
        // view is more gas efficient
    }

    function getAllTweets(address _owner) public view returns (string[] memory){
        return tweets[_owner];
    }

}

```

試圖了解create function 時整個代碼的結構，這樣寫起來更知道，下一個單詞是為什麼存在的:
總共有九個部分:
1. function keyword
2. function name
3. parameters
4. visibility specifier
5. state mutability keyword
6. return type
7. function body
8. local variable declaration
9. return statement

然後了解了struct，知道怎麼寫，什麼時候用:

When to Use a Struct
- When You Have Multiple Related Data Fields
- When You Want to Improve Code Readability
 -When You Need to Reuse a Logical Grouping of Data
 -When You Need a Data Model in a Mapping or Array

When Not to Use a Struct:

- For Simple Data: If your data is very simple (e.g., just a `uint` or a `string`), using a struct may be overkill. Structs are useful when there are multiple related fields.
- When Performance Is a Concern: Structs in Solidity are stored in memory or storage, which might consume more gas compared to simpler types, especially if the struct is large and complex. Use them wisely, considering the gas cost.

了解MAPPING的結構:
This line of code declares a public mapping in Solidity:

```solidity
mapping(address => string[]) public tweets;

```

Here's what it does:

- It creates a mapping called "tweets"
- The key to this mapping is an Ethereum address
- The value associated with each address is an array of strings
- Each address can have multiple tweets (stored as strings in the array)
- The 'public' keyword automatically creates a getter function for this mapping

In the context of a Twitter-like contract, this mapping allows each Ethereum address (user) to have an array of tweets associated with it. Users can add tweets to their array, and anyone can retrieve tweets for a specific address.

以上。

### 2024.09.29
Ethereum Client

安裝Parity, Open Ethereum, but were all deprecated. 

Installed Go Ethereum

https://geth.ethereum.org/downloads

 Node and 

Node.js Truffle (CLI Ganache)  through the terminal. 

https://medium.com/@1chooo/如何在-mac-安裝-node-js-npm-3d7101d998f4

https://nodejs.org/zh-tw/download/package-manager

https://archive.trufflesuite.com/docs/truffle/how-to/install/

- **Step 1 (`mkdir greeter`)**: Creates a new folder called `greeter` to organize your project files.
- **Step 2 (`cd greeter`)**: Moves into the `greeter` folder so that you can start working on your project.
- **Step 3 (`truffle init`)**: Initializes a new Truffle project inside the `greeter` folder, generating the basic structure needed for Ethereum smart contract development.

Since you're inside the `greeter` directory, you can simply run:

```bash
bash
複製程式碼
ls -l
```

### CLI commands:
Installing `brew` for directory `tree`: `brew install tree`, but brew must be installed first. 

Show all files within every folder:`tree -a`

Notice that the commands specified in the output line up well with the directory structure generated when initializing our application. truffle compile will compile all the contracts in the contracts directory, truffle migrate will deploy our compiled contracts by running the scripts in our migrations directory, and lastly, truffle test will run the tests in our test directory.


### 2024.09.30
Reference Type (要聲明數據儲存位置)

1. Array
2. Struct

數據儲存位置有三類: 

1. Storage: default on-chain
2. Memory: temporary variable, 通常這些數據都不需要上鏈。編譯器不知道在何處儲存這些變量內容。
3. Calldata: similar to memory but immutable. 

變長 (Variable-length):
String, bytes, dynamic array(`uint[]`, `address[]`). 

### 状态变量 State Variable

`gas`消耗高，因為是存在鏈上的數據變量，所有合約函數都可以訪問。

### 局部变量 Local Variable

`gas`低，僅在函數執行過程中有效的變量

### 全局变量 Global Variable

https://learnblockchain.cn/docs/solidity/units-and-global-variables.html#special-variables-and-functions

```solidity
function global() external view returns(address, uint, bytes memory){
    address sender = msg.sender;
    uint blockNum = block.number;
    bytes memory data = msg.data;
    return(sender, blockNum, data);
}
```

常見全局變量:

- `blockhash(uint blockNumber)`: (`bytes32`) 给定区块的哈希值 – 只适用于256最近区块, 不包含当前区块。
- `block.coinbase`: (`address payable`) 当前区块矿工的地址
- `block.gaslimit`: (`uint`) 当前区块的gaslimit
- `block.number`: (`uint`) 当前区块的number
- `block.timestamp`: (`uint`) 当前区块的时间戳，为unix纪元以来的秒
- `gasleft()`: (`uint256`) 剩余 gas
- `msg.data`: (`bytes calldata`) 完整call data
- `msg.sender`: (`address payable`) 消息发送者 (当前 caller)
- `msg.sig`: (`bytes4`) calldata的前四个字节 (function identifier)
- `msg.value`: (`uint`) 当前交易发送的 `wei` 值
- `block.blobbasefee`: (`uint`) 当前区块的blob基础费用。这是Cancun升级新增的全局变量。
- `blobhash(uint index)`: (`bytes32`) 返回跟当前交易关联的第 `index` 个blob的版本化哈希（第一个字节为版本号，当前为`0x01`，后面接KZG承诺的SHA256哈希的最后31个字节）。若当前交易不包含blob，则返回空字节。这是Cancun升级新增的全局变量。

### **4. 全局变量-以太单位与时间单位**

**以太单位**

`Solidity`中不存在小数点，以`0`代替为小数点，来确保交易的精确度，并且防止精度的损失，利用以太单位可以避免误算的问题，方便程序员在合约中处理货币交易。

- `wei`: 1
- `gwei`: 1e9 = 1000000000
- `ether`: 1e18 = 1000000000000000000

```
function weiUnit() external pure returns(uint) {
    assert(1 wei == 1e0);
    assert(1 wei == 1);
    return 1 wei;
}

function gweiUnit() external pure returns(uint) {
    assert(1 gwei == 1e9);
    assert(1 gwei == 1000000000);
    return 1 gwei;
}

function etherUnit() external pure returns(uint) {
    assert(1 ether == 1e18);
    assert(1 ether == 1000000000000000000);
    return 1 ether;
}
```

Convert Gwei to Ether:

Convert Ether to USD by multiply its price in USD. 

**时间单位**

可以在合约中规定一个操作必须在一周内完成，或者某个事件在一个月后发生。这样就能让合约的执行可以更加精确，不会因为技术上的误差而影响合约的结果。因此，时间单位在`Solidity`中是一个重要的概念，有助于提高合约的可读性和可维护性。

- `seconds`: 1
- `minutes`: 60 seconds = 60
- `hours`: 60 minutes = 3600
- `days`: 24 hours = 86400
- `weeks`: 7 days = 604800

### Mappings in Solidity

Mappings are a data structure in Solidity used to store **key-value pairs**. You can think of them as **hash tables**. Mappings allow you to retrieve a value by providing a key, like looking up a person's wallet address using their ID.

### Declaration

Mappings are declared using the format `mapping(_KeyType => _ValueType)`, where `_KeyType` is the type of the key and `_ValueType` is the type of the value.

Example:

```solidity
solidity
複製程式碼
mapping(uint => address) public idToAddress; // ID mapped to address
mapping(address => address) public swapPair; // Address mapped to another address

```

### Rules for Mappings

1. **Key Type Restrictions**:
    - The key type (`_KeyType`) must be a built-in Solidity type like `uint` or `address`. You **cannot** use custom structs as the key.
    - The value type (`_ValueType`) can be any type, including custom structs.
    
    Example of incorrect usage (this will cause an error):
    
    ```solidity
    struct Student {
        uint256 id;
        uint256 score;
    }
    mapping(Student => uint) public testVar; // Error: Custom struct as key
    
    ```
    
2. **Storage Requirement**:
    
    Mappings must be stored in **storage**, meaning they can be used as **state variables** within contracts, but cannot be used as parameters or return types in public functions because mappings represent relationships (key-value pairs) and not direct data.
    
3. **Automatic Getter**:
    
    If a mapping is declared as `public`, Solidity automatically creates a getter function, allowing you to look up values by providing the key.
    
4. **Adding Key-Value Pairs**:
    
    To add key-value pairs, use the following syntax:
    
    ```solidity
    solidity
    複製程式碼
    _Var[_Key] = _Value;
    
    ```
    
    Example:
    
    ```solidity
    function writeMap(uint _Key, address _Value) public {
        idToAddress[_Key] = _Value;
    }
    
    ```
    

### How Mappings Work

1. **No Key Storage**:
    
    Mappings **do not store** any information about the keys and do not have a `length` property.
    
2. **Data Access**:
    
    The value in the mapping is accessed using a hash (`keccak256(abi.encodePacked(key, slot))`), where `slot` is the storage slot where the mapping is declared.
    
3. **Default Values**:
    
    If a key has no associated value, the default value for that type is returned. For example, for `uint`, the default value is `0`.
    

---

### Simplified Summary:

- **Mapping**: A data structure that stores key-value pairs, like a hash table.
- **Key**: Must be a basic type (e.g., `uint`, `address`).
- **Value**: Can be any type, including custom types.
- Mappings are stored in **storage** and cannot be passed as parameters or returned from public functions.
- **No length or key storage**: Mappings don't store the keys or have a `length` property.
- **Default value**: Unused keys return default values (e.g., `0` for `uint`).

---

**Example 4-6. Testing the greeting can be made dynamic**

```jsx
const GreeterContract = artifacts.require("Greeter");
contract("Greeter", () => {
	it("has been deployed successfully", async () => {
		const greeter = await GreeterContract.deployed();
		assert(greeter, "contract failed to deploy");
});

describe("greet()", () => {
	it("returns 'Hello, World!'", async () => {
		const greeter = await GreeterContract.deployed();
		const expected = "Hello, World!";
		const actual = await greeter.greet();

    assert.equal(actual, expected, "greeted with 'Hello, World!'");
    });
  });
});

contract("Greeter: update greeting", () => {
  describe("setGreeting(string)", () => {

it("sets greeting to passed in string", async () => {
    const greeter = await GreeterContract.deployed()
    const expected = "Hi there!";

    await greeter.setGreeting(expected);
    const actual = await greeter.greet();

    assert.equal(actual, expected, "greeting was not updated");
   });
 });
});
```

```
  assert.equal(actual, expected, "greeted with 'Hello, World!'");
});

```

});
});

contract("Greeter: update greeting", () => {
describe("setGreeting(string)", () => {

**Example 4-7. Adding setGreeting() to Greeter**

```solidity

function setGreeting(string calldata greeting) external {
}
```

**Example 4-8. Adding state variable to Greeter contract**

```solidity
pragma solidity >= 0.4.0 < 0.7.0;

contract Greeter {
    string private _greeting;

function greet() external pure returns(string memory) {
        return "Hello, World!";
    }

function setGreeting(string calldata greeting) external {

    }

}
```

**Example 4-10. Reading from our state variable**

```solidity
pragma solidity >= 0.4.0 < 0.7.0;

contract Greeter {
    string private _greeting = "Hello, World!";

function greet() external view returns(string memory) {
        return _greeting;
    }

function setGreeting(string calldata greeting) external {
        _greeting = greeting;
}

}
```
### 2024.10.01
#### Constant and Immutable

In this lesson, we introduce two keywords in Solidity related to constants: `constant` and `immutable`. Once a state variable is declared with either of these keywords, its value cannot be changed after initialization. Using constants in this way enhances the contract’s security and reduces gas costs.

Additionally, only numeric types can be declared with both `constant` and `immutable`. Strings and bytes can be declared as `constant`, but not as `immutable`.

### `constant` and `immutable`

### `constant`

A `constant` variable must be initialized at the time of declaration, and its value cannot be changed afterward. If you try to modify it, the code won’t compile.

```solidity

// A constant variable must be initialized at the time of declaration and cannot be changed
uint256 constant CONSTANT_NUM = 10;
string constant CONSTANT_STRING = "0xAA";
bytes constant CONSTANT_BYTES = "WTF";
address constant CONSTANT_ADDRESS = 0x0000000000000000000000000000000000000000;

```

### `immutable`

An `immutable` variable offers more flexibility because it can be initialized either at the time of declaration or within the constructor. In Solidity version 8.0.21 and later, `immutable` variables don’t need to be explicitly initialized. If not explicitly initialized, they will default to their initial values (based on their type, as discussed in the variable initialization section). If you initialize an `immutable` variable both at the time of declaration and in the constructor, the value from the constructor will be used.

```solidity

// An immutable variable can be initialized in the constructor and cannot be changed afterward
uint256 public immutable IMMUTABLE_NUM = 9999999999;

// In Solidity 8.0.21 and later, the following variables default to their initial values
address public immutable IMMUTABLE_ADDRESS;
uint256 public immutable IMMUTABLE_BLOCK;
uint256 public immutable IMMUTABLE_TEST;

```

You can use global variables (like `address(this)` or `block.number`) or custom functions to initialize `immutable` variables. In the following example, we use the `test()` function to initialize `IMMUTABLE_TEST` to 9:

```solidity

// The constructor is used to initialize immutable variables
constructor() {
    IMMUTABLE_ADDRESS = address(this);
    IMMUTABLE_NUM = 1118;
    IMMUTABLE_TEST = test();
}

function test() public pure returns(uint256) {
    uint256 value = 9;
    return value;
}

```

This makes `immutable` variables more flexible compared to `constant`, as they can be set at runtime in the constructor, while still offering the same benefits once initialized.

A `constructor` in Solidity is a special function that is automatically executed once when a contract is deployed. It is used to initialize the contract's state variables or perform any setup tasks that are necessary at the time of contract deployment.

### Initial Values of Variables

In Solidity, any variable that is declared but not assigned a value is automatically given an initial or default value. In this lesson, we'll cover the default values for commonly used variables.

### Default Values for Value Types:

- **Boolean**: `false`
- **String**: `""` (empty string)
- **Integer (`int`)**: `0`
- **Unsigned Integer (`uint`)**: `0`
- **Enum**: The first element in the enum list
- **Address**: `0x0000000000000000000000000000000000000000` (or `address(0)`)
- **Functions**:
    - **Internal**: Empty function
    - **External**: Empty function

You can verify these initial values by using the getter functions of public variables:

```solidity
solidity
複製程式碼
bool public _bool; // false
string public _string; // ""
int public _int; // 0
uint public _uint; // 0
address public _address; // 0x0000000000000000000000000000000000000000

enum ActionSet { Buy, Hold, Sell }
ActionSet public _enum; // The first element (Buy) with an index of 0

function fi() internal {} // Empty internal function
function fe() external {} // Empty external function

```

### Default Values for Reference Types:

- **Mappings**: All elements in the mapping are set to their default values.
- **Structs**: All members of the struct are initialized to their default values.
- **Arrays**:
    - **Dynamic Arrays**: `[]` (empty array)
    - **Static Arrays** (fixed-length): All elements are initialized to their default values.

You can also use public variables to verify the default values of reference types:

```solidity

// Reference Types
uint[8] public _staticArray; // Static array where all elements are initialized to 0: [0,0,0,0,0,0,0,0]
uint[] public _dynamicArray; // Dynamic array initialized as an empty array: []
mapping(uint => address) public _mapping; // Mapping where all elements default to 0x0000000000000000000000000000000000000000

// Struct with all members initialized to default values (0, 0)
struct Student {
    uint256 id;
    uint256 score;
}
Student public student;

```

`delete` Operator:

The `delete` operator resets a variable to its initial or default value.

```solidity

// delete operator
bool public _bool2 = true;
function d() external {
    delete _bool2; // Using delete will reset _bool2 to its default value, which is false
}

```

By using the `delete` operator, you can revert variables back to their original default state.

### Control Flow

Solidity's control flow is similar to other programming languages and mainly includes the following:

### `if-else`

```solidity
solidity
複製程式碼
function ifElseTest(uint256 _number) public pure returns(bool) {
    if(_number == 0) {
        return true;
    } else {
        return false;
    }
}

```

### `for` Loop

```solidity
solidity
複製程式碼
function forLoopTest() public pure returns(uint256) {
    uint sum = 0;
    for(uint i = 0; i < 10; i++) {
        sum += i;
    }
    return sum;
}

```

### `while` Loop

```solidity
solidity
複製程式碼
function whileTest() public pure returns(uint256) {
    uint sum = 0;
    uint i = 0;
    while(i < 10) {
        sum += i;
        i++;
    }
    return sum;
}

```

### `do-while` Loop

```solidity
solidity
複製程式碼
function doWhileTest() public pure returns(uint256) {
    uint sum = 0;
    uint i = 0;
    do {
        sum += i;
        i++;
    } while(i < 10);
    return sum;
}

```

### Ternary Operator

The ternary operator is the only operator in Solidity that takes three operands. The syntax is: `condition ? expression if true : expression if false`. It is often used as a shortcut for `if` statements.

```solidity

// Ternary operator
function ternaryTest(uint256 x, uint256 y) public pure returns(uint256) {
    // Return the larger of x and y
    return x >= y ? x : y;
}

```

Additionally, the keywords `continue` (which skips to the next iteration of a loop) and `break` (which exits the current loop) can be used in Solidity.

### Insertion Sort in Solidity

Before we begin: Over 90% of people writing insertion sort in Solidity make mistakes.

### Insertion Sort

Sorting algorithms are used to arrange an unordered set of numbers, such as `[2, 5, 3, 1]`, in ascending order. Insertion sort is one of the simplest sorting algorithms, and it's often the first algorithm many people learn. The idea is straightforward: from left to right, compare each number with the ones before it. If the current number is smaller, swap its position with the previous one. Here's an illustration:

### Python Code

Let’s first look at the Python code for insertion sort:

```python

# Python program for implementation of Insertion Sort
def insertionSort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i-1
        while j >= 0 and key < arr[j]:
            arr[j+1] = arr[j]
            j -= 1
        arr[j+1] = key
    return arr

```

### Rewriting it in Solidity (with a bug)

It takes only 8 lines of Python to implement insertion sort. It seems simple. Now, after converting it to Solidity, with corresponding functions, variables, and loops, it takes just 9 lines of code:

```solidity

// Incorrect Insertion Sort
function insertionSortWrong(uint[] memory a) public pure returns(uint[] memory) {
    for (uint i = 1; i < a.length; i++) {
        uint temp = a[i];
        uint j = i - 1;
        while((j >= 0) && (temp < a[j])) {
            a[j+1] = a[j];
            j--;
        }
        a[j+1] = temp;
    }
    return a;
}

```

But when I ran it in Remix with input `[2, 5, 3, 1]`, BOOM! There was a bug! I tried to fix it for a long time but couldn’t find the issue. I then googled "Solidity insertion sort" and discovered that most tutorials online had mistakes, for example, "Sorting in Solidity without Comparison."

### The Correct Solidity Insertion Sort

After hours of debugging and with help from a friend in the Dapp-Learning community, we finally found the bug. In Solidity, the `uint` type (unsigned integer) cannot hold negative values. In the insertion sort algorithm, the variable `j` might become `-1`, causing an underflow error.

To fix this, we need to adjust the code so that `j` cannot become negative. Here is the correct code:

```solidity
solidity
複製程式碼
// Correct Insertion Sort
function insertionSort(uint[] memory a) public pure returns(uint[] memory) {
    // Note that uint cannot take a negative value
    for (uint i = 1; i < a.length; i++) {
        uint temp = a[i];
        uint j = i;
        while((j >= 1) && (temp < a[j-1])) {
            a[j] = a[j-1];
            j--;
        }
        a[j] = temp;
    }
    return a;
}

```

### Result:

Input `[2, 5, 3, 1]`, output `[1, 2, 3, 5]`.

Now it works correctly!

---

### Constructor and Modifier in Solidity: Ownable Example

In this lesson, we'll use the **Ownable** contract as an example to explain the **constructor** and **modifiers** in Solidity.

### Constructor

The **constructor** is a special function in Solidity. Each contract can define one constructor, and it runs automatically **once** when the contract is deployed. It is often used to initialize contract parameters, such as setting the `owner` address of the contract.

For example:

```solidity
solidity
複製程式碼
address owner; // Define an owner variable

// Constructor function
constructor(address initialOwner) {
    owner = initialOwner; // Set the owner to the provided initialOwner address during contract deployment
}

```

**Note**: The syntax for constructors has changed in different versions of Solidity. Before Solidity version 0.4.22, constructors used the same name as the contract. This older method could lead to errors (e.g., a contract named `Parents` might accidentally have a constructor named `parents`, turning the constructor into a regular function). From Solidity 0.4.22 onwards, the constructor keyword was introduced to avoid these issues.

Here’s an example of the old constructor syntax:

```solidity
solidity
複製程式碼
pragma solidity =0.4.21;
contract Parents {
    // The function with the same name as the contract acts as the constructor
    function Parents () public {
    }
}

```

### Modifier

A **modifier** in Solidity is a unique syntax feature, similar to a **decorator** in object-oriented programming. It adds specific characteristics to functions and helps reduce code redundancy. A modifier is like Iron Man's armor: functions that use it get special behavior. Modifiers are commonly used for checks that need to be performed before executing the function body, such as verifying addresses, variables, or balances.

### Example: `onlyOwner` Modifier

We define a modifier called `onlyOwner` to restrict access to certain functions to the contract owner:

```solidity

// Define the modifier
modifier onlyOwner {
    require(msg.sender == owner); // Check if the caller is the owner
    _; // If true, execute the function body; otherwise, revert the transaction
}

```

Now, we can use this `onlyOwner` modifier to ensure that only the owner can execute the following function:

```solidity

function changeOwner(address _newOwner) external onlyOwner {
   owner = _newOwner; // Only the current owner can change the owner
}

```

In this `changeOwner` function, the owner of the contract can be changed, but only if the function is called by the current owner. If anyone else tries to call it, the function will revert and throw an error. This is a common way to control access and permissions in smart contracts.


```





<!-- Content_END -->
