## Unlock the vault to pass the level!

We can see in the contract that the password is private

```solidity
    bytes32 private password;
```

But this doesn't stop us from accessing the password from Ethereum blockchain
All contract storage on Ethereum is publicly accessible at the storage level via tools like Etherscan or web3 provider calls like eth_getStorageAt.

The solution will be as follows

```solidity
    contract StorageReader {

        // Function to read a value from a specific storage slot in another contract
        function getStorageAt(address contractAddress, uint256 slot) public view returns (bytes32) {
            bytes32 result;
            assembly {
                // Create the input for the static call
                let ptr := mload(0x40) // Get free memory pointer
                mstore(ptr, slot)      // Store the storage slot number in memory

                // Make the static call to the target contract
                let success := staticcall(
                    gas(),             // Use all available gas
                    contractAddress,   // Address of the target contract
                    ptr,               // Pointer to the slot number input
                    0x20,              // Size of the input (32 bytes, since it's a single slot number)
                    ptr,               // Output pointer (where the result will be stored)
                    0x20               // Output size (32 bytes, since a single slot holds 32 bytes)
                )

                // Store the result in the variable `result`
                result := mload(ptr)
            }
            return result;
        }
    }

    contract hackVault{
        StorageReader storageReader = new StorageReader();
        function hackTarget(address contractAddress, uint256 slot) public view returns(bytes32){
            bytes32 value = storageReader.getStorageAt(contractAddress, slot);
            return value;
        }
    }
```
