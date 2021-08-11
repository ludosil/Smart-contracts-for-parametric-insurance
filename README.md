# DAML model for parametric insurance market

- Market for parametric insurance contracts consists of buyers and sellers

- An admin tracks the balance of each participant; funds are either free to use or locked in case they are pledged on an existing contract

- Anyone can propose an insurable event on which contracts can be written

- Admin approves proposals; since an event requires a trusted data source to evaluate the payoff, not every proposal is valid

- Given the approved insurable events, anyone in the market can propose to another party an insurance contract based on that event, as either the buyer or seller

- If the proposal is accepted, it is approved by the admin, contingent on both parties having sufficient funds to pay the premium / put up the notional

- Once the expiry date is reached, the admin evaluates the contract using a trusted data source and updates the balances accordingly

More info on parametric insurnace here: [What is parametric insurance? | Swiss Re](https://corporatesolutions.swissre.com/insights/knowledge/what_is_parametric_insurance.html) 

