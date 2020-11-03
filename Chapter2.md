# 1. Addresses 
- Ethereum blockchain is made up of **accounts** (think bank account) 
- Account has a balance of **Ether** (currency used on Ethereum blockchain)
- Each account has an **address** (like a bank account number) 

# 2. Mappings 
- Way of storing organized data (like structs/ arrays) 
- A a key-value store for storing and looking up data. 
  - For a financial app, storing a uint that holds the user's account balance:
    - key (address), value (uint)
    ```
    mapping (address => uint) public accountBalance;
    ```
  - Or could be used to store / lookup usernames based on userId
    - key (uint), value (string)
    ```
    mapping (uint => string) userIdToName;
    ``` 
  - Note: address is a tyoe (refer above) 
- to set/change the value of mappings: 
  ```
  favoriteNumber[msg.sender] = _myNumber;
  ```
  - ^ The syntax for storing data in a mapping is just like with arrays
  
# 3. msg.sender (one of the **global variables** that are available to all functions) 
- refers to the address of the person (or smart contract) who called the current function.
> Note: In Solidity, function execution always needs to start with an external caller.
> A contract will just sit on the blockchain doing nothing until someone calls one of its functions.
> So there will always be a msg.sender.
- Using msg.sender gives you the security of the Ethereum blockchain
  — the only way someone can modify someone else's data would be to steal the **private key** associated with their **Ethereum address**.

# 4. require
- function will throw an error and stop executing if some condition is not true.
> Side note: Solidity doesn't have native string comparison, so we
> compare their keccak256 hashes to see if the strings are equal
```
require(keccak256(abi.encodePacked(_name)) == keccak256(abi.encodePacked("Vitalik")));
```

# 5. Contract Inheritance - like Java
- Can be used for logical inheritance (e.g. animal subsclass) or simply to organize code.
```
contract Doge {
  function 1 { 
  }
}

contract BabyDoge {
  function 2 {
  }
} 
```
- BabyDoge inherits from Doge - it will have access to any public functions we defined on Doge

# 6. import - to import a file (Javascript) 
```
import "./someothercontract.sol";
```

# 7. Storage vs Memory (Data location) 
- Storage - variables stored permanently on the blockchain
- Memory variables - temporary, erased between external function calls to your contract
- Need to use these keywords in **structs** and **arrays** within functions
```
Sandwich storage mySandwich = sandwiches[_index];
```
> mySandwich is a pointer to 'sandwiches[_index]' in storage and changing its value alter it on the blockchain permanently.
- If you just want a copy, can use: 
```
Sandwich memory anotherSandwich = sandwiches[_index + 1];
```
> anotherSandwich will a copy of the data in memory, modifying it will not affect the original data
> - However, to copy the changes back to blockchain storage:
>   ```
>   sandwiches[_index + 1] = anotherSandwich;
>   ```

