# Web3.js - Javascript Library from Ethereum Foundation
- Depending on workflow, can do: 
  - npm install web3
  - yarn add web3
  
# 1. Web3 Provider
- Setting a Web3 Provider in Web3.js tells our code which node we should be talking to handle our reads and writes. 
- It's kind of like setting the URL of the remote web server for your API calls in a traditional web app.

# Infura 
- service that maintains a set of Ethereum nodes with a caching layer for fast reads, which you can access for free through their API. 
- Using Infura as a provider, you can reliably send and receive messages to/from the Ethereum blockchain without needing to set up and maintain your own node.
``` 
var web3 = new Web3(new Web3.providers.WebsocketProvider("wss://mainnet.infura.io/ws"));
```
> Note: Ethereum (and blockchains in general) use a public / private key pair to digitally sign transactions. 
> Think of it like an extremely secure password for a digital signature. 
> That way if I change some data on the blockchain, I can prove via my public key that I was the one who signed it 
> — but since no one knows my private key, no one can forge a transaction for me.

# Metamask - browser extension for Chrome and Firefox
- lets users securely manage their Ethereum accounts and private keys, and use these accounts to interact with websites that are using Web3.js. 
- as a developer, if you want users to interact with your DApp through a website in their web browser, you'll definitely want to make it Metamask-compatible.
> Note: Metamask uses Infura's servers under the hood as a web3 provider
> but it also gives the user the option to choose their own web3 provider.
> So by using Metamask's web3 provider, you're giving the user a choice, and it's one less thing you have to worry about in your app.
```
window.addEventListener('load', function() {

  // Checking if Web3 has been injected by the browser (Mist/MetaMask)
  if (typeof web3 !== 'undefined') {
    // Use Mist/MetaMask's provider
    web3js = new Web3(web3.currentProvider);
  } else {
    // Handle the case where the user doesn't have web3. Probably
    // show them a message telling them to install Metamask in
    // order to use our app.
  }

  // Now you can start your app & access web3js freely:
  startApp()

})
```

# To talk to contract, Web3 needs: **its address** and **its ABI**
- After deploying, it gets a fixed address on Ethereum where it will live forever.
- Contract ABI - Application Binary Interface. 
- It's a representation of your **contracts' methods** in JSON format that **tells Web3.js how to format function calls** in a way your contract will understand.

# Instantiate a Web3.js Contract
```
// Instantiate myContract
var myContract = new web3js.eth.Contract(myABI, myContractAddress);
```

# Calling Contract Functions - **call** and **send**
- call 
  - is used for **view** and **pure** functions.
  - only runs on the local node, and won't create a transaction on the blockchain.
  - myContract.methods.myMethod(123).call()
- send
  - create a transaction and change data on the blockchain.
  - needed to use send for any functions that aren't view or pure.
  - myContract.methods.myMethod(123).send()

# Solidity contract expects **owner** to be a Solidity **address** 
- MetaMask allows user to manage multiple accounts in their extension.
- to see which account is active: var userAccount = web3.eth.accounts[0]
- setInterval loop: 
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
- check every 100ms to see if userAccount still equals to web3.eth.accounts[0] 

# Sending Transactions
- sending a transaction requires a **from** address of who's calling the function (msg.semder)
- sending a transaction costs gas
- there will be a significant delay from when the users **send** a transaction and when that transaction actually takes effect on the blockchain.

# payable function
- We specify how much to send in **wei**, not Ether.
- A wei is the smallest sub-unit of Ether, there are 10^16 wei in one ether.
```
// This will convert 1 ETH to Wei
web3js.utils.toWei("1");
```
```
cryptoZombies.methods.levelUp(zombieId)
.send({ from: userAccount, value: web3js.utils.toWei("0.001", "ether") })
```

# listening to event
```
cryptoZombies.events.NewZombie()
.on("data", function(event) {
  let zombie = event.returnValues;
  // We can access this event's 3 return values on the `event.returnValues` object:
  console.log("A new zombie was born!", zombie.zombieId, zombie.name, zombie.dna);
}).on("error", console.error);
```
- trigger an alert everytime a zombie is created
- to only listen to events related to current user: use **indexed** keyword
```
// Use `filter` to only fire this code when `_to` equals `userAccount`
cryptoZombies.events.Transfer({ filter: { _to: userAccount } })
.on("data", function(event) {
  let data = event.returnValues;
  // The current user just received a zombie!
  // Do something here to update the UI to show it
}).on("error", console.error);
```
- filter when _to equals to current user
- to query past events: 
  use **getPastEvents**, 
  use the filters fromBlock and toBlock to give Solidity a time range for the event logs 
  > ("block" in this case referring to the Ethereum block number)
```
cryptoZombies.getPastEvents("NewZombie", { fromBlock: 0, toBlock: "latest" })
.then(function(events) {
  // `events` is an array of `event` objects that we can iterate, like we did above
  // This code will get us a list of every zombie that was ever created
});
```
- Since we can use this to query event logs since beginning of time -> an interesting use case: **Using events as a cheaper form of storage**.
- The tradeoff here is that events are not readable from inside the smart contract itself. 
- But it's an important use-case to keep in mind if you have some data you want to be historically recorded on the blockchain so you can read it from your app's front-end.
