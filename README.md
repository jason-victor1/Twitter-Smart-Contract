# Twitter Smart Contract

This project is a simple Twitter smart contract that I created. It allows users to post tweets, like and unlike tweets, and retrieve tweets. The contract also includes functionality to change the maximum tweet length, which can only be done by the owner of the contract.

## Features

- Post tweets
- Like and unlike tweets
- Retrieve individual and all tweets of a user
- Change maximum tweet length (owner only)

## Getting Started

### Prerequisites

To deploy and interact with this smart contract, I used [Remix IDE](https://remix.ethereum.org/), a powerful web-based tool for developing and deploying smart contracts.

### Creating the Contract

1. **Open Remix IDE**  
   I started by going to [Remix IDE](https://remix.ethereum.org/).

2. **Create a New File**  
   In the Remix file explorer, I clicked on the `+` button to create a new file. I named it `Twitter.sol`.

3. **Add the Contract Code**  
   I copied and pasted the following code into `Twitter.sol`:

    ```solidity
    // SPDX-License-Identifier: MIT

    pragma solidity ^0.8.0;

    contract Twitter {

        uint16 public MAX_TWEET_LENGTH = 280;

        struct Tweet {
            uint256 id;
            address author;
            string content;
            uint256 timestamp;
            uint256 likes;
        }
        mapping(address => Tweet[] ) public tweets;
        address public owner;

        constructor() {
            owner = msg.sender;
        }

        modifier onlyOwner() {
            require(msg.sender == owner, "YOU ARE NOT THE OWNER!");
            _;
        }

        function changeTweetLength(uint16 newTweetLength) public onlyOwner {
            MAX_TWEET_LENGTH = newTweetLength;
        }

        function createTweet(string memory _tweet) public {
            require(bytes(_tweet).length <= MAX_TWEET_LENGTH, "Tweet is too long bro!" );

            Tweet memory newTweet = Tweet({
                id: tweets[msg.sender].length,
                author: msg.sender,
                content: _tweet,
                timestamp: block.timestamp,
                likes: 0
            });

            tweets[msg.sender].push(newTweet);
        }

        function likeTweet(address author, uint256 id) external {  
            require(tweets[author][id].id == id, "TWEET DOES NOT EXIST");

            tweets[author][id].likes++;
        }

        function unlikeTweet(address author, uint256 id) external {
            require(tweets[author][id].id == id, "TWEET DOES NOT EXIST");
            require(tweets[author][id].likes > 0, "TWEET HAS NO LIKES");

            tweets[author][id].likes--;
        }

        function getTweet(uint _i) public view returns (Tweet memory) {
            return tweets[msg.sender][_i];
        }

        function getAllTweets(address _owner) public view returns (Tweet[] memory) {
            return tweets[_owner];
        }

    }
    ```

### Compiling the Contract

1. **Compile the Contract**  
   In Remix, I navigated to the `Solidity Compiler` tab on the left sidebar. I made sure the compiler version was set to `0.8.x` (matching the version in the contract). I then clicked `Compile Twitter.sol`.

### Deploying the Contract

1. **Deploy the Contract**  
   I went to the `Deploy & Run Transactions` tab on the left sidebar. I selected `JavaScript VM` as the environment. Then, I clicked the `Deploy` button.

2. **Interact with the Contract**  
   Once deployed, the contract appeared under `Deployed Contracts` at the bottom of the `Deploy & Run Transactions` tab. I expanded the contract to see available functions.

### Using the Contract

1. **Change Tweet Length (Owner Only)**
    - I input a new maximum tweet length in the `changeTweetLength` field and clicked the `changeTweetLength` button.
    ```solidity
    function changeTweetLength(uint16 newTweetLength) public onlyOwner
    ```

2. **Create Tweet**
    - I input tweet content in the `createTweet` field and clicked the `createTweet` button.
    ```solidity
    function createTweet(string memory _tweet) public
    ```

3. **Like Tweet**
    - I input the author's address and tweet ID in the `likeTweet` fields and clicked the `likeTweet` button.
    ```solidity
    function likeTweet(address author, uint256 id) external
    ```

4. **Unlike Tweet**
    - I input the author's address and tweet ID in the `unlikeTweet` fields and clicked the `unlikeTweet` button.
    ```solidity
    function unlikeTweet(address author, uint256 id) external
    ```

5. **Get Tweet**
    - I input the tweet index in the `getTweet` field and clicked the `getTweet` button to retrieve a specific tweet.
    ```solidity
    function getTweet(uint _i) public view returns (Tweet memory)
    ```

6. **Get All Tweets**
    - I input the owner's address in the `getAllTweets` field and clicked the `getAllTweets` button to retrieve all tweets from a specific user.
    ```solidity
    function getAllTweets(address _owner) public view returns (Tweet[] memory)
    ```

## Deployed to Blockchain

I deployed the Twitter smart contract to the Sepolia test network. Here’s how I did it:

1. **Get Free ETH**: I used the Sepolia faucet to obtain free ETH for the deployment. The faucet provided the necessary test ETH to cover gas fees.

2. **Deploy using MetaMask**: I connected my MetaMask wallet to Remix IDE. After ensuring I had enough test ETH, I selected the "Injected Web3" environment in the `Deploy & Run Transactions` tab of Remix IDE, which connects to my MetaMask wallet. Then, I deployed the contract to the Sepolia test network. 

3. **Post a Tweet**: After deploying the contract, I created a tweet on the Sepolia test network using the `createTweet` function in the deployed contract.

4. **Sepolia test network Etherscan**
![Deployed Contract Screenshot](https://github.com/jason-victor1/Twitter-Smart-Contract/blob/main/twitter%20contract.png?raw=true)

## License

This project is licensed under the MIT License.
