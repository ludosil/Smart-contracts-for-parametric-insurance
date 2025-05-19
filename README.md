# DAML model for parametric insurance market

[TOC]

## Design v1

__Note: please see PowerPoint slides for an outline of the design__

Workflows in model:

- Market for parametric insurance contracts consists of buyers and sellers
- An admin tracks the balance of each participant; funds are either free to use or locked in case they are pledged on an existing contract
- Anyone can propose an insurable event on which contracts can be written
- Admin approves proposals; since an event requires a trusted data source to evaluate the payoff, not every proposal is valid
- Given the approved insurable events, anyone in the market can propose to another party an insurance contract based on that event, as either the buyer or seller
- If the proposal is accepted, it is approved by the admin, contingent on both parties having sufficient funds to pay the premium / put up the notional
- Once the expiry date is reached, the admin evaluates the contract using a trusted data source and updates the balances accordingly

More info on parametric insurnace here: [What is parametric insurance? | Swiss Re](https://corporatesolutions.swissre.com/insights/knowledge/what_is_parametric_insurance.html) 

## Design v2

Consider 5 stages in the model:

- Setup
- Adding insurable events
- Adding/maintaining quotes
- Trading
- Settlement

Consider the workflow of each stage

### Setup

- Admin invites buyers/sellers to join the market
- Buyers/sellers accept/decline the invitation
- Admin creates market with buyers/sellers as participants

### Adding insurable events

- Buyers/sellers propose insurable events on which to write insurance contracts
- Admin accepts/rejects the proposal; criteria for a valid insurable event include a valid location, an unambiguous metric and the availability of a trusted data source to evaluate the contract on expiry

### Adding/maintaining quotes

- Buyers/sellers can quote to trade on an insurance contract. Minimal required features of the quote are the size of the trade and whether they're willing to complete partial trades
- Can also add a time limit on which the quote is valid. The admin monitors this vs. the current time and removes quotes that are no longer valid
- Admin validates the quote vs. the balance that the buyer/seller maintains
- Buyers/sellers can remove a quote at any time via a propose-accept pattern with the admin
- When a contract payout is determined, the admin monitors quotes from the seller to determine if they are still valid. The buyer pays the premium in advance, so they are guaranteed not to be in a worse position after the trade settles, but a seller's payout can impact their ability to write business (note that in the current setup, where funds are escrowed to cover payouts this is not necessary)
- If a quote crosses with an existing quote - buys/sells against an existing sell/buy order with a price acceptable to both parties - then a trade is triggered

### Trading

- Buyers/sellers can trade against an existing quote. If the trade is deemed valid, an insurance contract is created
- A trade is valid if:
  - the counterpart to the quoter has sufficient funds
  - the size is filled in full, or partially with the quote accepting partial fulfilment
- Note that the presence of an active quote implies that the quoter is in a position to trade, hence limited/no further validation required

### Settlement

- Settlement is triggered when the expiry date of the contract is reached. It may also be triggered on certain contract types e.g. for a max or min type, the contract can be settled the value is triggered at any time

- Settlement updates the balance of the buyer and seller; the locked funds in the seller's account are transferred to either to the free funds of the seller (no exercise) or the buyer (exercise)

  



Comments

- Admin above is quite generic and covers a lot of roles; in practice this might be split into different roles e.g. a supervisor to accept/reject participants via KYC process and a market bot to monitor the day-to-day tasks such as validating whether a seller can offer a contract given their current balance

- Can buyers/sellers be added after the market has been created? e.g. via a addParticipant function that admin applies

  