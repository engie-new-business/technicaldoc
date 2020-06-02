# Summary‌

## Problem
Sometimes Ethereum transactions are stuck or lost.
To attempt to solve this problem, developers overpay transactions and must remain on-call to unblock them.
We designed a solution with a new approach to get rid of this situation

## Solution
Rockside is a transaction relayer for sending Blockchain transactions at a fixed and predetermined price and time.
Rockside ensure the validation of your transactions and bears any additional gas.
Our service is fully non-custodial. Rockside only relays pre-authorised transactions.

## How does it work ?

Rockside uses Meta transactions and wraps your signed message in a new  transaction.
Rockside monitors and bumps the fee of “stuck” transactions. They are automatically replayed until their inclusion on the Blockchain.
Under the hood, Rockside uses multiple signer accounts and on-chain replay protection mechanism.
You can decide whether or not your users pay the transaction fees thanks to our gasless feature.
You can also use Rockside to execute a batch of  transactions in a single on-chain transaction.
Lastly, while processing a batch of multiple transactions, you can send others with higher priority.

[**Replay and cancel transactions in depth**](replay/README.md)

## How to use it ?

To get rid of complex development and infrastructure, we built a simple and ultra-reliable REST API. 
Request a transaction at a maximum price and time, Rockside handles the transaction.

## [Glossary](GLOSSARY.md)
