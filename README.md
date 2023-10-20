# Advanced Solidity Homework: Martian Token Crowdsale

## Task
1. Create the KaseiCoin Token Contract
2. Create the KaseiCoin Crowdsale Contract
3. Create the KaseiCoin Deployer Contract
4. Deploy and Test the Crowdsale on a Local Blockchain
5. Optional: Extend the Crowdsale Contract by Using OpenZeppelin

## Instructions

### Step 1: Create the KaseiCoin Token Contract

1. Import the provided KaseiCoin.sol starter file into the Remix IDE.
2. Import contracts from the OpenZeppelin library
3. Define a contract for the KaseiCoin token, and name it KaseiCoin. Have the contract inherit the three contracts that you just imported from OpenZeppelin.
4. Inside your KaseiCoin contract, add a constructor with the following parameters: name, symbol, and initial_supply.
5. As part of your constructor definition, add a call to the constructor of the ERC20Detailed contract, passing the parameters name, symbol, and 18. (Recall that 18 is the value for the decimals parameter.)
6. Compile the contract by using compiler version 0.5.5.
7. Check for any errors, and debug them as needed.

End Result:
pragma solidity ^0.5.5;

import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/token/ERC20/ERC20.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/token/ERC20/ERC20Detailed.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/token/ERC20/ERC20Mintable.sol";

contract KaseiCoin is ERC20, ERC20Detailed, ERC20Mintable {
    constructor (
        string memory name, 
        string memory symbol, 
        uint initial_supply
        )
    ERC20Detailed (name, symbol, 18) 
    public 
    {
        // constructor can stay empty
    }
}

### Step 2: Create the KaseiCoin Crowdsale Contract

1. Import the provided KaseiCoinCrowdsale.sol starter code into the Remix IDE.
2. Have this contract inherit specific OpenZeppelin contracts:
3. In the KaisenCoinCrowdsale constructor, provide parameters for all the features of your crowdsale, such as rate, wallet (where to deposit the funds that the token raises), and token. Configure these parameters as you want for your KaseiCoin token.
4. Compile the contract by using compiler version 0.5.5.
5. Check for any errors, and debug them as needed.

End Result:
pragma solidity ^0.5.5;

import "./KaseiCoin.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/crowdsale/Crowdsale.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/crowdsale/emission/MintedCrowdsale.sol";

contract KaseiCoinCrowdsale is Crowdsale, MintedCrowdsale {

    constructor(
        uint256 rate,
        address payable wallet,
        KaseiCoin token
    ) public
        Crowdsale(rate, wallet, token)
    {
        // constructor can stay empty
    }
}

### Step 3: Create the KaseiCoin Deployer Contract

1. Create an address public variable named kasei_token_address, which will store the KaseiCoin address once that contract has been deployed.
2. Create an address public variable named kasei_crowdsale_address, which will store the KaseiCoinCrowdsale address once that contract has been deployed.
3. Add the following parameters to the constructor for the KaseiCoinCrowdsaleDeployer contract: name, symbol, and wallet.
4. Inside of the constructor body (that is, between the braces), complete the following steps:

  Create a new instance of the KaseiCoinToken contract.
  Assign the address of the KaseiCoin token contract to the kasei_token_address variable. (This will allow you to easily fetch the token's address later.)
  Create a new instance of the KaseiCoinCrowdsale contract by using the following parameters:
  The rate parameter: Set rate equal to 1 to maintain parity with ether.
  The wallet parameter: Pass in wallet from the main constructor. This is the wallet that will get paid all the ether that the crowdsale contract raises.
  The token parameter: Make this the token variable where KaseiCoin is stored.
  Assign the address of the KaseiCoin crowdsale contract to the kasei_crowdsale_address variable. (This will allow you to easily fetch the crowdsale’s address later.)
  Set the KaseiCoinCrowdsale contract as a minter.
  Have the KaseiCoinCrowdsaleDeployer renounce its minter role.

5. Compile the contract by using compiler version 0.5.0.
6. Check for any errors, and debug them as needed.

End Result:
contract KaseiCoinCrowdsaleDeployer {
    address public kasei_token_address;
    address public kasei_crowdsale_address;

    constructor(
        string memory name,
        string memory symbol,
        address payable wallet  
    ) public {
        KaseiCoin token = new KaseiCoin(name, symbol, 0);
        
        kasei_token_address = address(token);

        KaseiCoinCrowdsale kasei_crowdsale = new KaseiCoinCrowdsale(1, wallet, token);
            
        kasei_crowdsale_address = address(kasei_crowdsale);

        token.addMinter(kasei_crowdsale_address);
        
        token.renounceMinter();
    }
}

### Step 4: Deploy and Test the Crowdsale on a Local Blockchain

1. Deploy the crowdsale to a local blockchain by using Remix, MetaMask, and Ganache.
2. Test the functionality of the crowdsale by using test accounts to buy new tokens and then checking the balances of those accounts.
3. Review the total supply of minted tokens and the amount of wei that the crowdsale contract has raised.

(Look in images for results)
