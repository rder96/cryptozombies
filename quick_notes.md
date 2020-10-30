## Background
Solidity looks alot like C/C++/Javascript and less like python - i.e. ";" as statement terminator + statically typed. 
- Uses lowerCamelCase + UpperCamelCase for contract(class)  

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
>  - Can declare an array as **public** - Solidity will automatically create a **getter** method for it.
>  - i.e. Other contracts can read from, but not write to, this array.
>  - This is a useful pattern for storing public data in your contract.
> ```
> Person[] public people;
> ```
   - *Fixed* array - fixed size + type
   ``` 
   uint[2] fixedArray; 
   string[5] stringArray; 
   ```
   - *Dynamic* array - has no fixed size, can keep growing + fixed type 
   ``` 
   uint[] dynamicArray; 
   ```
   - can also create *struct* array
   ``` 
   struct Person; 
   Person[] people; 
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
  > Note 1: In Javascript, string is primitive type but in Solidity, string is reference type!\
  > Note 2: It's convention (but not required) to start function parameter variable names with an underscore(_)
  >         in order to differentiate them from global variables.\
  > Note 3: Pass by value: function modifying value would not affect initial parameter value.\
  >         Pass by reference: function changing value of the variable it receives, value of original variable gets changed.
