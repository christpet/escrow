# Escrow smart contract

It's strange that in the crypto world, where "trustlessness" is such a core value, the only way to hire and pay people is essentially to trust them completely. I haven't found a lightweight way to hire people for services, so I'm working on this smart contract.

It will allow customers to ensure that payment only goes through to the recipient if they deliver a particular service. If they don't, the customer can lodge a dispute and the judge can return their money.

I hope to build a UI on this eventually too.

Forked from Flexiana's escrow smart contract. So in the below, "Flexiana" refers to the delivery user.

## How to deploy

### Deployment to mainnet

Probably the easiest way to compile and deploy the smart contract can be found here: https://ethereum.stackexchange.com/questions/33536/how-to-compile-and-deploy-a-smart-contract-without-running-a-full-node

### Setting addresses

After creating the contract, the owner must call `setAddressesOnce(judge, delivery, customer)` and setup the judge, delivery and customer addresses.
This method can be called just once. Every other call ends with exception.

## How to use as a judge

Please, backup your private key. Only delivery and customer cooperating and holding keys can recover funds on this address.

Judge can call exactly 2 methods:

1. `transferToCustomer`
2. `transferToFlexiana`

## How to use as a delivery

Please, backup your private key on secure location. Losing private key means that only judge can recover funds by transfering them back to the customer.

### Changing status (return to sender, transfer to you)

Flexiana can setup 3 statuses:

1. `changeFlexianaStatusToDoNothing` - calling this method, Flexiana signals that funds should stay in the escrow
2. `changeFlexianaStatusToShouldTransferToCustomer` - calling this method says that Flexiana wants to return escrow back to customer
3. `changeFlexianaStatusToShouldTransferToFlexiana` - calling this method says that Flexiana wants to withdraw funds

### Withdrawal

For withdrawals, Flexiana can call 2 methods:

1. `transferToFlexiana` - if Flexiana has status ShouldTransferToCustomer, anyone can call this method.
2. `withdraw` - if customer and you have status ShouldTransferToFlexiana, method automatically recognizes you are eligible recipient and will send funds to your address.

## How to use as a customer

Please keep private key backed up in secure location. If you'll loose private key, only judge can recover funds by calling transferToFlexiana by transfering funds to Flexiana.

### Changing status (return to sender, transfer to you)

Customer can setup 3 statuses:

1. `changeCustomerStatusToDoNothing` -  by calling this method, customer states, she doesn't want to do anything with funds in escrow
2. `changeCustomerStatusToShouldTransferToCustomer` - by calling this method, customer states that funds should return back to customer
3. `changeCustomerStatusToShouldTransferToFlexiana` - by calling this method, customer states that funds should be transferred to Flexiana

### Withdrawal

For withdrawals, customer can call 2 methods:

1. `transferToCustomer` - if Flexiana has status ShouldTransferToCustomer, anyone can call this method.
2. `withdraw` - if Flexiana and you have status ShouldTransferToCustomer, method automatically recognizes you are eligible recipient and will send funds to your address.

### Deposit

SmartContract has 2 deposit methods.

1. `deposit` - accepts any amount of Ethereum (note, only ETH can be withdrawn, any other token sent to the address will be lost!).
2. `customerDeposit` - slightly more secure. It allows transaction only from customer address. Important note: do not send any tokens to this address. This smart contract allows only Ethereum withdrawal.

Please note you can deposit multiple times.
Currently, withdrawal is possible only all at once. Partial withdrawals aren't implemented yet.

## How to check balance of ETH

There are multiple ways to check balance of given address.
Easiest way on mainnet are:

- https://www.etherchain.org
- https://etherscan.io 

  
## TODO

- Write Unit Tests #javascript
- Remove dependency on OpenZeppelin's ownable #easy #solidity
- Remove mentions of Flexiana in contract API #easy #solidity
- Partial withdrawals & authorizing partial withdrawal #solidity
- Some UI to create, inspect & manage escrows #maybe #javascript #golang 

## Support us

- Please, don't remove LICENSE file & when you will use it, let world know you are using Flexiana library
- If you need to write any smart contract, contact us on jiri@flexiana.com
- Feel free to donate Litecoin to address LWZXy7zp9Lj6aFYJi8XdDN96rEptL6c1te or ETH to address 0xa92f637e930395e81D3b6e78ACbAEA8DE735CB46 or BTC to address 1P4m6neNV1pm8WwGDex64tKn4zezg2xLG3
