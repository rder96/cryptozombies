## Background
Solidity looks alot like C/C++/Javascript and less like python - i.e. ";" as statement terminator + statically typed. 
- Uses lowerCamelCase + UpperCamelCase for contract(class)
- [https://stackoverflow.com/questions/33839154/in-ethereum-solidity-what-is-the-purpose-of-the-memory-keyword]

# Introduction 
1. All source code starts with a **version pragma** - declaration of Solidity compiler this code should use.
   - E.g. range of 0.5.0 (inclusive) and 0.6.0 (exclusive): 
     ```
     pragma solidity >=0.5.0 <0.6.0;
     ```
2. A **contract** is the fundamental building block of Ethereum applications(think Class) - all variables and functions belong to a contract.
   ```
   contract HelloWorld {
   
   }
   ```
3. **State variables** - permanently stored in contract storage.\
i.e. they are written to the Ethereum blockchain (think of them being written to a DB) 
   - **uint** (Unsigned Integer) - its value must be non-negative 
     ``` 
     uint myUnsignedInteger = 100;
     ```
   > Note: In Solidity, uint is actually an alias for uint256, a 256-bit unsigned integer.\
   > You can declare uints with less bits — uint8, uint16, uint32, etc..\
   > But in general you want to simply use uint except in specific cases
   - **int** (Signed Integer) - value can be non-negative 
   - **string** - for arbitrary-length UTF-8 data
   
4. **Math Operations** - similar to Python. 
i.e. Addition(x + y),..., Modulus/Remainder(x % y), Exponential(x ** y)

5. **structs** - equivalent to C++ structs: To create more complicated data types that have multiple properties.
   ``` 
   struct Person {
    uint age; // variable decl 
    string name;
   }
   ```
6. **Array** - *fixed* array and *dynamic* array.
- Can declare an array as **public** - Solidity will automatically create a **getter** method for it.
- i.e. Other contracts can read from, but not write to, this array.
- This is a useful pattern for storing public data in your contract.
```
Person[] public people;
```
   a. *Fixed* array - fixed size + type
   ``` 
   uint[2] fixedArray; 
   string[5] stringArray; 
   ```
   b. *Dynamic* array - has no fixed size, can keep growing + fixed type 
   ``` 
   uint[] dynamicArray; 
   ```
- can also create *struct* array
``` 
struct Person; 
Person[] people; 
```
- array.push() - add something to the **end** of the array\
e.g.: 
``` 
people.push(Person(16, "Vitalik"));
```

7. **Function**
```
function eatHamburgers(string memory _name, uint _amount) public {

}
```
- Note:
  - We set the function visibility - public
  - We provide instruction where the _name variable should be stored - in **memory** 
    - This is required for all reference types such as arrays, structs, mappings, and strings.
  > - In Javascript, string is primitive type but in Solidity, string is reference type!
  > - It's convention (but not required) to start function parameter variable names with an underscore(_)
  >         in order to differentiate them from global variables.
  > - Pass by value: function modifying value would not affect initial parameter value.
  >         Pass by reference: function changing value of the variable it receives, value of original variable gets changed.

8. Function is public by default!
- Thus, it is good practice to make function as **private** by default.
- Only make **public** functions that you want to expose to others.
  ```
  uint[] numbers;
  function _addToArray(uint _number) private {
      numbers.push(_number);
  }
  ```
- Use keyword **private** after function name
- It is also conventional to start private function with underscore(_)

9. Returning values from **functions** 
```
string greeting = "What's up dog";

function sayHello() public returns (string memory) {
  return greeting;
}
``` 
> In Solidity, the **function declaration** contains the **type of the return value** (in this case string).
- Function modifiers 
  - If function only views data and not modify -> declare as **view function** 
  ```
  function sayHello() public view returns (string memory) {
  ```
  - If function not access any data in the app -> declare as **pure function**\
    (If it doesn't read from the state of the app — only return value depends on its function parameters)
  ```
  function _multiply(uint a, uint b) private pure returns (uint) {
      return a * b;
  }
  ```
  > It may be hard to remember when to mark functions as pure/view.\
  > Luckily the Solidity compiler is good about issuing warnings to let you know when you should use one of these modifiers.

10.  Keccak256 and Typecasting 
- Keccak256(a version of SHA-3) - built-in hash function: maps an input into a random 256-bit hexidecimal number.
- ** keccak256 expects a single parameter of type bytes -> so need to pack an param(): 
  ```
  keccak256(abi.encodePacked("aaaab"));
  ```
- Typecasting in Solidity: 
  ```
  uint8 a = 5;
  uint b = 6;
  // throws an error because a * b returns a uint, not uint8:
  uint8 c = a * b; 
  // we have to typecast b as a uint8 to make it work:
  uint8 c = a * uint8(b); 
  ```
  - Since we want to store it as uint8, we typecast b as a uint8. 

11. Events (From Javascript):  a way for your contract to communicate that something happened on the blockchain -> your app front-end,\
which can be 'listening' for certain events and take action when they happen.
```
// declare the event
event IntegersAdded(uint x, uint y, uint result);

function add(uint _x, uint _y) public returns (uint) {
  uint result = _x + _y;
  // fire an event to let the app know the function was called:
  emit IntegersAdded(_x, _y, result);
  return result;
}
```
- event UpperCaseCamel(..,); 
- To fire event (let the app know function was called) - emit UpperCaseCamel(...);
