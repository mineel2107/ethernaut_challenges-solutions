Our goal here is to take control of NaughtCoins, we can wait for the lockdown period to complete to transfer tokens to us, but the lockdown period is 10 years.
I wouldn't wait if i were you.

Let's try to get them now
There are several functions used by `ERC20 tokens`
One of them is `transfer()` which has been overriden in the contract , which lets the player transfer only after the lockdown period.

But they forgot to override the `transferFrom()` function.

Below is how we can build upon it to get our hands on those tokens, Well who doesn't like some free tokens ?

```solidity
    function attack() public {
        bool success = naughtCoin.transferFrom(sender,receiver,value);
        require(success,"Transfer failed");
    }
```
