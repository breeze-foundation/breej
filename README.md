# Breej
Javascript library for breeze blockchain

## Installation

Install with `npm install --save breej` inside your project. Then just
```
const breej = require('breej')
```
#### CDN style
If you are working in the browser and want to load breej from a CDN:
```
<script src="https://unpkg.com/breej/bin/breej.min.js"></script>
```

```
breej.init({api: 'http://localhost:3001'})
```

## GET API

### GET single account
```
breej.getAccount('alice', (err, account) => {
    console.log(err, account)
})
```

### GET many accounts
Just pass an array of usernames instead
```
breej.getAccounts(['alice', 'bob'], (err, accounts) => {
    console.log(err, accounts)
})
```

### GET account transaction history
For the history, you also need to specify a block number. The api will return all blocks lower than the specified block where the user was involved in a transaction
```
breej.getAccountHistory('alice', 0, (err, blocks) => {
    console.log(err, blocks)
})
```
### GET single content
```
breej.getContent('alice', 'pocNl2YhZdM', (err, content) => {
    console.log(err, content)
})
```

### GET followers
```
breej.getFollowers('alice', (err, followers) => {
    console.log(err, followers)
})
```

### GET following
```
breej.getFollowers('alice', (err, followers) => {
    console.log(err, followers)
})
```

### GET contents by author
You can pass a username and permlink (identifying a content) in the 2nd and 3rd argument to 'get more'.
```
breej.getDiscussionsByAuthor('alice', null, null, (err, contents) => {
    console.log(err, contents)
})
```

### GET contents by creation time
You can pass a username and a permlink to 'get more'.
```
breej.getNewDiscussions('alice', null, null, (err, contents) => {
    console.log(err, contents)
})
```


### GET notifications
```
breej.getNotifications('alice', (err, contents) => {
    console.log(err, contents)
})
```

### GET all votes by account
```
breej.getVotesByAccount('alice', 0, (err, votes) => {
    console.log(err, votes)
})
```



## POST API

To send a transaction to the network, you will need multiple steps. First you need to define your transaction and sign it.

```
var newTx = {
    type: breej.TransactionType.FOLLOW,
    data: {
        target: 'bob'
    }
}

newTx = breej.sign(alice_key, 'alice', newTx)
```
After this step, the transaction is forged with a timestamp, hash, and signature. This transaction needs to be sent in the next 60 secs or will be forever invalid.

You can send it like so
```
breej.sendTransaction(newTx, function(err, res) {
    cb(err, res)
})
```
The callback will return once your transaction has been included in a new block.

Alternatively, you can just want the callback as soon as the receiving node has it, you can do:
```
breej.sendRawTransaction(newTx, function(err, res) {
    cb(err, res)
})
```

## Convenience

### Generate a keypair
```
console.log(breej.keypair())
```

### Growing variables
Voting Power and Bandwidth are growing in time but the API will only return the latest update in the `vp` and `bw` fields of the accounts. To get the actual value, use votingPower() and bandwidth()
```
breej.getAccount('alice', (err, account) => {
    console.log(breej.votingPower(account))
    console.log(breej.bandwidth(account)) 
})
```
