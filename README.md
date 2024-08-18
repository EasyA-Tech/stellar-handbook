# The Stellar handbook.

Welcome. This is EasyA's legendary handbook. If you want to learn Stellar like a legend, then you're in the right place.

# Purpose

This handbook serves as a guide to the Stellar ecosystem, geared towards those just joining for the first time. It isn't just a beginners' handbook; it's a legendary handbook. Even if you've already immersed yourself in the ecosystem, you'll find tons of helpful tidbits around here!

## Table of Contents

- [Introduction](#introduction)
- [Getting Started](#getting-started)
- [Core Concepts](#core-concepts)
- [Development Tools](#development-tools)
- [Smart Contracts](#smart-contracts)
- [Stellar Network](#stellar-network)
- [Ecosystem Projects](#ecosystem-projects)
- [Resources](#resources)
- [Handy Code Snippets](#handy-code-snippets)
- [Contributing](#contributing)

## Introduction

What is Stellar:

- [Stellar Overview](https://www.stellar.org/learn/intro-to-stellar) - An introduction to Stellar's mission of creating an open and affordable financial system for all.

## Getting Started

The no-fluff starter:

- [Official Stellar documentation](https://developers.stellar.org/docs/) - Comprehensive documentation covering all aspects of building on Stellar, from basic concepts to advanced integrations.
- [Stellar 101](https://www.stellar.org/learn/intro-to-stellar) - A beginner-friendly guide to understanding Stellar's key features and use cases.

## Core Concepts

Explanation of fundamental concepts in the Stellar ecosystem:

- [Stellar Consensus Protocol (SCP)](https://www.stellar.org/papers/stellar-consensus-protocol) - The original whitepaper detailing Stellar's unique approach to consensus.
- [Stellar Assets and Anchors](https://youtu.be/Cf9CdFVse-w) - A video explaining how assets are created and managed on the Stellar network, and the role of anchors in the ecosystem.

## Development Tools

Key tools and environments for Stellar:

- [Stellar Laboratory](https://www.stellar.org/laboratory/) - An interactive webapp for creating and testing Stellar transactions and operations.
- [Stellar SDKs](https://developers.stellar.org/docs/tools-and-sdks) - Official tools/SDKs for various programming languages to interact with the Stellar network.
- [Horizon API](https://horizon.stellar.org/) - The REST API that serves as the interface between Stellar Core and applications that want to access the Stellar network.

## Smart Contracts

How to write and deploy smart contracts on Stellar:

- [Stellar Smart Contracts](https://www.stellar.org/developers-blog/smart-contracts-on-stellar) - An overview of Stellar's approach to smart contracts, focusing on simplicity and safety.
- [Stellar Smart Contract Tutorial](https://developers.stellar.org/docs/issuing-assets/anatomy-of-an-asset#example-smart-contract) - A practical guide to implementing simple smart contracts on Stellar.

## Stellar Network

Going into the network level:

- [Stellar Dashboard](https://dashboard.stellar.org/) - A real-time view of Stellar network statistics, including ledger closures, fee stats, and lumen distribution.
- [StellarExpert](https://stellar.expert/explorer/public/) - A comprehensive block explorer and analytics platform for the Stellar network.
- [Stellar Status Updates](https://status.stellar.org/) - Current operational status of various Stellar services and APIs.

## Ecosystem Projects

Cool projects built on Stellar:

- [SatoshiPay](https://satoshipay.io/) - A platform enabling content monetization and micropayments using Stellar.

## Resources

Extra stuff:

- [Stellar Community Blog](https://medium.com/stellar-community) - A collection of articles from community members covering various aspects of the Stellar ecosystem.
- [Stellar Developers Blog](https://www.stellar.org/developers-blog) - Official blog posts from the Stellar Development Foundation, focusing on technical updates and developer resources.

## Handy Code Snippets

Get a taste of Stellar development with these code snippets:

### Creating a Stellar Account

```javascript
const StellarSdk = require('stellar-sdk');
const server = new StellarSdk.Server('https://horizon-testnet.stellar.org');

// Create a completely new and unique pair of keys
const pair = StellarSdk.Keypair.random();

console.log('Secret key:', pair.secret());
console.log('Public key:', pair.publicKey());

(async function createAccount() {
  try {
    const response = await fetch(
      `https://friendbot.stellar.org?addr=${encodeURIComponent(pair.publicKey())}`
    );
    const responseJSON = await response.json();
    console.log("SUCCESS! You have a new account :)\n", responseJSON);
  } catch (e) {
    console.error("ERROR!", e);
  }
})();
```

### Sending a Payment

```javascript
const StellarSdk = require('stellar-sdk');
const server = new StellarSdk.Server('https://horizon-testnet.stellar.org');

// Keys for accounts to issue and receive the payment
const sourceSecretKey = 'SCZANGBA5YHTNYVVV4C3U252E2B6P6F5T3U6MM63WBSBZATAQI3EBTQ4';
const destinationId = 'GA2C5RFPE6GCKMY3US5PAB6UZLKIGSPIUKSLRB6Q723BM2OARMDUYEJ5';

// Transaction will hold a built transaction we can resubmit if the result is unknown.
let transaction;

(async function main() {
  // Transactions require a valid sequence number that is specific to this account.
  // We can fetch the current sequence number for the source account from Horizon.
  const account = await server.loadAccount(sourceSecretKey);

  // Right now, there's one function that fetches the base fee.
  // In the future, we'll have functions that are smarter about suggesting fees,
  // e.g.: `fetchBaseFee` or `recommendFee`.
  const fee = await server.fetchBaseFee();

  transaction = new StellarSdk.TransactionBuilder(account, { 
    fee,
    networkPassphrase: StellarSdk.Networks.TESTNET
  })
    .addOperation(StellarSdk.Operation.payment({
      destination: destinationId,
      // Make a payment in XLM
      asset: StellarSdk.Asset.native(),
      amount: "10"
    }))
    // Wait a maximum of three minutes for the transaction
    .setTimeout(180)
    .build();

  // Sign the transaction to prove you are actually the person sending it.
  transaction.sign(StellarSdk.Keypair.fromSecret(sourceSecretKey));

  try {
    const transactionResult = await server.submitTransaction(transaction);
    console.log(JSON.stringify(transactionResult, null, 2));
    console.log('\nSuccess! View the transaction at: ');
    console.log(transactionResult._links.transaction.href);
  } catch (e) {
    console.log('An error has occurred:');
    console.log(e);
  }
})();
```

These examples showcase:
1. How to create a new Stellar account on the testnet.
2. How to send a payment on the Stellar network.

## Contributing

We welcome contributions to make this handbook even more legendary! Here's how you can contribute:

1. Fork the repository
2. Create a new branch (`git checkout -b feature/amazing-addition`)
3. Make your changes
4. Commit your changes (`git commit -am 'Add some amazing feature'`)
5. Push to the branch (`git push origin feature/amazing-addition`)
6. Create a new Pull Request

Please ensure your contributions align with our code of conduct and contribution guidelines.

## Credit/Inspiration

This handbook was inspired by the famous awesome lists by sindresorhus and the Awesome Stellar list. We need awesome lists for Web3 ecosystems, with more of a hacker's guide to how they work. This is the answer to that need.
