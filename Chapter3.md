# Contract - immutable
- If there is flaw in contract code -> have to tell client to use different contract address that has the fix.

# Ownable 
- **function modifier** - looks just like function, but uses keyword **modifier** instead.
- can't be called directly like a function - instead we can attach the modifier's name at the end of a function definition to change that function's behavior.
- e.g.: 
```
function renounceOwnership() public onlyOwner {
    emit OwnershipTransferred(_owner, address(0));
    _owner = address(0);
}
```
> - when renounceOwnership is called, code inside onlyOwner runs first.
> - when it hits the _; statement in onlyOwner, it goes back and executes the code inside renounceOwnership.
- one of the most common use-cases is to add a quick **require check** before a function executes.

# Function modifiers can also take arguments
```
modifier olderThan(uint _age, uint _userId) {
  require(age[_userId] >= _age);
  _;
}
function driveCar(uint _userId) public olderThan(16, _userId) {
  // Some function logic
}
```
- driveCar passes its arguments to the modifier.

# When should you use subtypes of uints (uint8, uint16, uint32)? 
- normally no diff because Solidity reserves 256 bits regardless of uint size.
- exception: inside **structs** 
- If you have multiple uints inside a struct, using a smaller-sized uint when possible will allow Solidity to pack these variables together to take up less storage.
- e.g.: uint c; uint32 a; uint32 b; uses less gas than uint32 a; uint c; uint32 b; -> want to cluster identical data types together.

# Times
- contains the time units seconds, minutes, hours, days, weeks and years. (all with /'s)
- variable **now** contains the current unix timestamp of the latest block -> returns uint256 by default.

# Passing structs as arguments 
- can pass a **storage pointer** to a struct as an argument to a **private/internal** function - useful for passing around structs between functions
```
function _doStuff(Zombie storage _zombie) internal {
  // do stuff with _zombie
}
```
> - passed a reference to zombie instead of a zombie id

# Note: calldata is somehow similar to memory, but it's only available to external functions.

# Saving Gas with 'View' functions
- **view** functions don't cost any gas when they're called **externally** by a user.
- because **view** functions don't change anything in the blockchain.
- marking a function with view tells web3.js that it only needs to query your local Ethereum node to run the function, 
- and it doesn't actually have to create a transaction on the blockchain (which would need to be run on every single node, and cost gas).
> Note: If a view function is called internally from another function in the same contract that is not a view function, it will still cost gas. 
> This is because the other function creates a transaction on Ethereum, 
> and will still need to be verified from every node. So view functions are only free when they're called externally.

# **storage** - particularly **writes** are costly.
- avoid writing data to storage unless necessary.
- can use **memory** to create a new array inside a function w/o writing to storage.
- the array will only exist until the end of the function call. 
- this is a lot cheaper gas-wise than updating an array in storage — free if it's a view function called externally.
> Note: memory arrays must be created with a length argument (in this example, 3). 
> They currently cannot be resized like storage arrays can with array.push().

# for-loop in solidity (similar to Javascript)
```
  for (uint i = 1; i <= 10; i++) {...}
```
