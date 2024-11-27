To pass this level succesfully , we need to

1. Claim ownership of the contract.
2. Reduce the balance of the contract to 0.

---

1. ### Claiming ownership of the contract

   To change the owner of the contract we have 2 functions.
   contribute() and receive()
   contribute can't be used as we need to send `eth > contributions[owner]` ,which is 1000 ether but there is a require in the beginning requiring the `msg.value` to be less than `0.001 ether`.
   So,we will need to trigger receive function by sending any amount of eth.

2. ### Reducing balance to 0
   We have a straight forward withdraw function which requires the caller to be the owner.
   As we have claimed ownership in the last step , we can now `Drain the contract`.
