Similar to GateKeeper One and the first gate is the same.

GateTwo

```solidity
    modifier gateTwo() {
        uint256 x;
        assembly {
            x := extcodesize(caller())
        }
        require(x == 0);
        _;
    }
```

This gate requires the code in the contract caller to be zero
But our contract gets some codesize once it is constructed.
Here the fact that the size of a contract remains 0 till the execution of its constructor is completed will help us pass this gate.
We can call the function in the constructor itself to pass this gate.

GateThree

```solidity
    modifier gateThree(bytes8 _gateKey) {
        require(uint64(bytes8(keccak256(abi.encodePacked(msg.sender)))) ^ uint64(_gateKey) == type(uint64).max);
        _;
    }
```

Gate3 requires some reduction of the value and providing it as `gateKey` to pass the gate.
The value can be reduced as follows

```solidity
    constructor(address _gateKeeper2){
        gateKeeper2 = Gatekeeper2(_gateKeeper2);
        bytes8 gateKey = bytes8(uint64(bytes8(keccak256(abi.encodePacked(address(this))))) ^ type(uint64).max);
        (bool success,) = _gateKeeper2.call(abi.encodeWithSignature("enter(bytes8)", gateKey));
        require(success, "Call to Gatekeeper 2 Failed");
    }
```
