# Etherum_camen_nojnica
```
//ERC20
// SPDX-License-Identifier: MIT
pragma solidity 0.8.20;
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable.sol";

contract Lexa is ERC20("LexaTocken", "TT"),Ownable(msg.sender){
    mapping (address => int) private room; 
    mapping (address => string) private history;
    int private length;
    address[] private clients;
    uint256 private bet = 50;
    address public Owner;
    constructor(){
        Owner = msg.sender;
    }
    function getFifty() public onlyOwner {
        _mint(msg.sender, 50 * 10**18);
    }

    function setChoice(int value) public returns (string memory){
        if (balanceOf(msg.sender) < bet){
            return "Balance less 50 tokens!";
        }
        if (length == 2){
            return "Two participinets!";
        }
        if (value == 1 || value == 2 || value == 3){
            history[msg.sender] = "You bet! Waiting opponent";
            transfer(Owner, bet);
            length++;
            clients.push(msg.sender);
            room[msg.sender] = value;
            
            return "Good";
        } else {
            return "Incorrect choice!";
        }
        
        //return "123";
    }
    function checkResult() public view returns (string memory){
        return history[msg.sender];
    }
    function reset() public {
        clear();
    }

    function play() public onlyOwner {
        int result1 = room[clients[0]];
        
        int result2 = room[clients[1]];
        int index;
        //transferFrom(clients[0], clients[1], bet);

        if (result1 == result2){
            index = -1;
        } else if ((result1 % 3) + 1 == result2){
            index = 0;
        } else {
            index = 1;
        }
        rewardClient(index);
        clear();

    }

    function rewardClient(int index) private{
        if (index == -1){
            history[clients[0]] = "Draw!";
            history[clients[1]] = "Draw!";
            transfer(clients[0], bet);
            transfer(clients[1], bet);
            return;
        }
        if (index == 0){
            history[clients[0]] = "You win!";
            history[clients[1]] = "You lose!";
            transfer(clients[0], 2*bet);
            return;
        } 
        if (index == 1){
            history[clients[1]] = "You win!";
            history[clients[0]] = "You lose!";
            transfer(clients[1], 2*bet);
            return;
        }
    }

    function clear() private{
        length = 0;
        clients.pop();
        clients.pop();
        
    }
}
```
