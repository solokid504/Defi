# Defi
Decentralized finance guide 
const Web3 = require('web3');
const Compound = require('@compound-finance/compound-js');

const providerUrl = 'https://mainnet.infura.io/v3/YOUR_INFURA_PROJECT_ID';
const provider = new Web3.providers.HttpProvider(providerUrl);

const web3 = new Web3(provider);
const networkId = await web3.eth.net.getId();

const compound = new Compound(provider, { networkId });

const daiAddress = Compound.util.getAddress(Compound.DAI);
const cDaiAddress = Compound.util.getAddress(Compound.cDAI);

const amountToBorrow = Compound.ethMantissa(1);
const ethAddress = Compound.util.getAddress(Compound.ETH);

const accounts = await web3.eth.getAccounts();
const borrower = accounts[0];

const daiBalance = await compound.getBalance(daiAddress, borrower);
console.log(`DAI balance: ${daiBalance.toString()}`);

const ethBalance = await web3.eth.getBalance(borrower);
console.log(`ETH balance: ${ethBalance.toString()}`);

await compound.enableErc20Token(daiAddress, borrower);

const cDaiBalanceBefore = await compound.getBalance(cDaiAddress, borrower);
console.log(`cDAI balance before: ${cDaiBalanceBefore.toString()}`);

await compound.borrow(daiAddress, amountToBorrow, borrower);

const daiBalanceAfter = await compound.getBalance(daiAddress, borrower);
console.log(`DAI balance after: ${daiBalanceAfter.toString()}`);

const cDaiBalanceAfter = await compound.getBalance(cDaiAddress, borrower);
console.log(`cDAI balance after: ${cDaiBalanceAfter.toString()}`);
