# Cryptomafias
Online Multiplayer Mafia party game built on Ethereum. Stake coins! Win the Game! Get the reward!

## Inspiration
We found that most of the projects in this space heavily are collectible based and nothing like the Web2 games we are used to. This makes the gaming experience very isolated and static. While you can collect NFTs and in some cases do something with it but they rarely have meaningful interactions. We wanted to change this and make a game that in its very core feels like your regular Web2 games that you can enjoy with your friends but with the added benefits and security of blockchain.

## What it does
We have built a blockchain incentivized online multiplayer mafia party Game.
1. We maintains game history of a player as an NFT.
2. When you sign up, your profile is minted as an NFT and stored as a textile bucket with IPNS link and you will also get some CMC(Crypto Mafia Coin) which is our in-game currency as a registration bonus.
3. When you sign in, your profile will be retrieved from the IPFS and it will be used to generate your unique avatar from the hash of your tokenId.
4. There is 2 game modes: 1) casual game - which is just a normal gameplay, 2) competitive game - In this you will be staking your coin and can win/lose the coins.
5. You can create a new room or join an existing room.
6. In the Game we have Mafia, Doctor, Detective and Villagers, the aim is for either Mafia to kill everyone else or for Villagers to find all Mafia and eject them from the game during voting phase. Doctor can choose to save a player from being killed during everynight while Detective can inspect role of a player during night.
7. You can chat in-game with fellow players and decide who to vote on!
8. The winning party will get all staked coins and distribute equally among themselves with a small fee deduced from the stack so that we can maintain game servers smoothly

## How we built it
Our project has three major repositories:
1. [GameContracts](https://github.com/cryptomafias/GameContracts) - It contains smart contracts we made for the game
2. [mafia-backend](https://github.com/cryptomafias/mafia-backend) - It contains REST API server which manages the gameplay
3. [mafia-game](https://github.com/cryptomafias/mafia-game) - It contains frontend for the game.

We are using react.js to build the frontend of the game. We are using textile threaddb to maintain game data and enable P2P real-time chatting. We have used express.js to create REST API service that provides endpoints that can be used to take game actions. We have used smart contracts for maintaining user profile as an NFT, staking initial fund and distributing rewards in the end.

> You can read more about the functionalities of a specific service in the ReadME of that project.
 
## Challenges we ran into
One of the first challenge we ran into was due to the nature of the game. We can't use blockchain to make gameplay fully decentralized because in mafia roles of the player should be secret. Also we want to make the role distribution verifiable at the end of the game so normal centralized way of shuffling role didn't sound good. That's why we decided to use chainlink VRF to get a random number and use the number we get from signing the random number we got from the VRF and use it as a seed for shuffling roles. So, In the end we can disclose the signature and players can verify that. We also want to make role distribution process verifiable. So, we come up with the idea of encrypting a secret AES key with public key of the player and broadcasting it over all the players. We will then encrypt the role with that AES key and broadcast it. So, player who is recepeint can get the role imediatly and at the end of the game player can disclose the AES key and everyone can verify the role. 

Another major challenge we faced was how to change the phase(Ex: Night, Discussion and Voting) of the game periodically. We first think about using chainlink alarmclock but that feature was still in the beta and didn't available over matic testnet. It would also be less effecient in terms of gas and blockchain storage to maintain this details on chain. So, we searched different alternatives and finally decided to use `bree.js` which is a cron job scheduler for nodejs. 

## Accomplishments that we're proud of
We have created a game where people can play to earn real money. We have also created NFT of player's profile which will be used to maintain player's history and owned avatars. This will let people show of their NFTs as an in-game avatar rather than just holding it.

## What we learned
We learned to use Chainlink VRF. We also learned to use Textile threadDB and Chakra UI in React for creating UI for the game. I have only used FastAPI until now for creating Rest API services but since textile didn't have client for Python, I learned express.js and created backend using it.

## What's next for Crypto Mafia
Currently, we haven't fully integrated all the game functionality with the frontend so we are going to complete it first and after that we are planing to expand player profile and add friends feature. Also we are going to sell different addons like hat, hoodies for the in-game avatars as an NFT. We have also divised an algorithm for giving increasing returns to regular players.
