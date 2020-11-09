# Token
- smart contracts that follows some common rules -> it implements a standard set of functions that all other token contracts share
- e.g. transferFrom(address _from, address _to, uint256 _tokenId) and balanceOf(address _owner).
- Internally, smart contract usually has a mapping, **mapping(address => uint256) balances**, that keeps track of how much balance each address has.
- ERC20 - tokens that work like currencies
- token standard much better fit for crypto-collectibles -> **ERC721 tokens** 
  - are not interchangeable - since each one is assumed to be unique, and not divisible.
  
# ERC271 tokens 
- Implementing a token contract: 
  - 1. copy the interface to its own Solidity file and import it, import "./erc721.sol".
  - 2. have our contract inherit from it, and override each method with a function definition.
  - (in Solidity, contract can inherit from multiple contracts)
  - ```
    contract ZombieOwnership is ZombieAttack {
    }
    ```

# Transfer tokens in ERC271
- ```function transferFrom(address _from, address _to, uint256 _tokenId) external payable;```
- or ```
  function approve(address _approved, uint256 _tokenId) external payable;
  function transferFrom(address _from, address _to, uint256 _tokenId) external payable;
  ```
- First: token's owner calls transferFrom 
- Second:
  - token's owner calls **approve** with the address he wants to transfer to
  - contract then stores who is approved to take a token (usually in a mapping)
  - owner of the approved address call transferFrom
- Note: to fire an event: ```emit Transfer(.., .., ..);``` -> there's an event of transfer in ERC271 so we need to fire it at the end of the function

# Implementing **approve**
- step 1: owner calls approve and give it the _approved address of the new owner + the _tokenId you want to give.
- step 2: new owner calls transferFrom with _tokenId -> contract checks to make sure new owner has been approved + transfer him the token.

# Contract security enhancements - prevent underflow & overflow
- e.g. overflow/underflow a uint8
- use OpenZeppelin's library, SafeMath
  - ```using SafeMath for uint256;```
  - the SafeMath library has 4 functions - add, sub, mul, div
  - e.g.: ```uint256 b = a.add(3);```
- **library** keyword - similar to contract but:
  1. allow use of **using** keyword, which automatically tacks on all of the library's methods to another data type.
  2. SafeMath's add works just like +, but contains an **assert** to make sure no overflow.
- difference between **assert** and **require**:
  - both throw an error when false, 
  - require will refund the user the rest of their gas when a function fails, whereas assert will not.
  - therefore, only use assert when something has gone terribly wrong (e.g. overflow)
- ```myUint++; -> myUint = myUint.add(1);```

