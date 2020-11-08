# view and pure
view - by running the function, no data will be saved/changed.
pure - not only the function not save any data to the blockchain, but it also doesn't read any data from the blockchain
- both don't cost any gas to call if they're called externally from outside the contract.

# The payable Modifier
- they are special type of functions that can accept Ether
- because both the money (Ether), data (transaction payload) and the contract code itself all live on Ethereum,
it is possible to call a function and pay money to the contract at the same time.
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
- **msg.value** is a way to see how much Ether was sent to the contract
> Note: If a function is not marked payable and you try to send Ether to it as above, the function will reject your transaction.

# After you send Ether to a contract, it gets stored in the contract's Ethereum account
- it will be trapped there, unless add a function to withdraw it.
```
contract GetPaid is Ownable {
  function withdraw() external onlyOwner {
    address payable _owner = address(uint160(owner()));
    _owner.transfer(address(this).balance);
  }
}
```
- _owner variable has to be **address payable** type for doing a sending and transferring ether instruction.
- can transfer Ether to an adress using **transfer** function.
- address(this).balance will return total balance stored on the contract.
- can use **transfer** to send fund to any Ethereum address. 
- e.g.:
```
msg.sender.transfer(msg.value - itemFee);
``` 
> transfer extra back to msg.sender

# Random number generation using keccak256 
- vulnerable to attack by dishonest node.
- use an oracle to access a random number function from outside of the Ethereum blockchain.
