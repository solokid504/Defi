# Defi
Decentralized finance guide 
<!DOCTYPE html>
<html>
<head>
	<title>DeFi Terminology Dictionary</title>
</head>
<body>
	<h1>Decentralized Finance (DeFi) Terminology Dictionary</h1>
	<table>
		<tr>
			<th>Term</th>
			<th>Definition</th>
		</tr>
		<tr>
			<td>Decentralized Finance (DeFi)</td>
			<td>A financial system built on decentralized blockchain technology.</td>
		</tr>
		<tr>
			<td>Cryptocurrency</td>
			<td>A digital or virtual currency secured by cryptography.</td>
		</tr>
		<tr>
			<td>Blockchain</td>
			<td>A digital ledger of transactions that is decentralized and distributed across a network of computers.</td>
		</tr>
		<tr>
			<td>Smart Contract</td>
			<td>A self-executing contract with the terms of the agreement between buyer and seller being directly written into lines of code.</td>
		</tr>
		<tr>
			<td>Ethereum</td>
			<td>A decentralized blockchain platform that enables the creation of smart contracts and decentralized applications (dApps).</td>
		</tr>
		<tr>
			<td>Bitcoin</td>
			<td>The first and largest cryptocurrency by market capitalization.</td>
		</tr>
		<tr>
			<td>Altcoin</td>
			<td>Any cryptocurrency other than Bitcoin.</td>
		</tr>
		<tr>
			<td>Token</td>
			<td>A digital representation of an asset or utility that is issued and managed on a blockchain.</td>
		</tr>
		<tr>
			<td>Stablecoin</td>
			<td>A type of cryptocurrency designed to maintain a stable value relative to a specific asset or basket of assets.</td>
		</tr>
		<tr>
			<td>Liquidity</td>
			<td>The ease with which an asset can be bought or sold in the market without affecting its price.</td>
		</tr>
		<tr>
			<td>Yield Farming</td>
			<td>A process of earning rewards by staking or lending cryptocurrencies in DeFi protocols.</td>
		</tr>
		<tr>
			<td>Automated Market Maker (AMM)</td>
			<td>A type of decentralized exchange that uses an algorithm to determine the price of assets based on supply and demand.</td>
		</tr>
		<tr>
			<td>Governance Token</td>
			<td>A token that grants its holder the right to vote on proposals and decisions in a decentralized autonomous organization (DAO).</td>
		</tr>
		<tr>
			<td>Decentralized Autonomous Organization (DAO)</td>
			<td>An organization that operates based on rules encoded as computer programs on a blockchain.</td>
		</tr>
		<tr>
			<td>Flash Loan</td>
			<td>A type of DeFi loan that allows borrowers to take out loans without collateral for a very short period of time.</td>
		</tr>
	</table>
</body>
</html>

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
