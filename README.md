# Polygon-zkStake-Reward-Claim-Script
This script allows you to claim rewards from the Polygon zkStake protocol using the web3 library.
const Web3 = require("web3");

// Connect to the Polygon network
const providerUrl = "https://polygon-rpc.com";
const web3 = new Web3(new Web3.providers.HttpProvider(providerUrl));

// Specify the staking contract details
const stakingContractAddress = "0x1234567890abcdef1234567890abcdef12345678";
const stakingContractAbi = [
  // Staking contract ABI interface
  // ...
];

// Specify the user's staking information
const userAddress = "0xabcdef1234567890abcdef1234567890abcdef12"; // User's Ethereum address

// Claim rewards from the staking contract
async function claimRewards() {
  const stakingContract = new web3.eth.Contract(stakingContractAbi, stakingContractAddress);

  const claimMethod = stakingContract.methods.claimRewards();

  const gas = await claimMethod.estimateGas({ from: userAddress });
  const gasPrice = await web3.eth.getGasPrice();

  const signedTransaction = await web3.eth.accounts.signTransaction(
    {
      to: stakingContractAddress,
      data: claimMethod.encodeABI(),
      gas: gas,
      gasPrice: gasPrice,
      value: 0,
      chainId: 137, // Polygon network chain ID
    },
    "your-private-key"
  );

  const transactionReceipt = await web3.eth.sendSignedTransaction(signedTransaction.rawTransaction);

  console.log("Transaction Hash:", transactionReceipt.transactionHash);
  console.log("Rewards claimed successfully!");
}

claimRewards();
