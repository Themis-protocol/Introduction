# Liquidation & Auction of Themis-Protocol
Date: 2021/09/16

Published by: Vincent

There are three indispensable functions in a lending protocol: lending, borrowing, and liquidating. This article focuses on the design logic of Themis' liquidation and auction.
## Liquidation of the borrower
In Themis-Protocol V1, the borrower obtains a loan by collateralizing the valued assets created in Uniswap V3, which constitutes a lending relationship with the Lending Pool. When the protocol determines that the borrower is unable to repay the lending pool, the assets collateralized by the borrower will be liquidated by the protocol. Liquidation is therefore judged primarily on the following terms.
- Is the total current price of the collateral still higher than the outstanding principal amount?
- Is the collateral sufficiently liquid to compensate for the liquidation?
- Will the liquidation of the assets result in slippage and losses to the Lending Pool?
### The Ratio of the current price of collateral to the outstanding principal amount
First, we define the liquidation factor (F_liquidation) as the outstanding principal and interest/collateral price
- Consider the price of collateral at the time of borrowing by the borrower as C.
- The Collateral Factor is a constant, denoted as F_collateral.
- the funds lent by the user from the Lending Pool are F_collateral*C.

As the lending proceeds, the borrower incurs interest over the lending time and the value as R. The collateral changes with the market as ∆C.

We can derive F_liquidation=(F_collateral*C+R)/(C+∆C)

#### F_liquidation ∈ (0 , 1)
The closer the price of the principal outstanding is to the price of the collateral, the higher the risk, so that a liquidation trigger condition applies: (F_collateral*C+R)/(C+∆C) ≥ F_liquidation.
![image](https://user-images.githubusercontent.com/75613147/133575737-ef5419c9-558e-4294-b9ee-0fdaa6c85533.png)
### Ensure sufficient liquidity compensation in case of liquidation
Since there are multiple trading markets for cryptocurrencies, resulting in liquidity that is fragmented. Therefore, we introduce the role of a liquidator to provide a liquidity buffer. When an asset is liquidated, the liquidator will pay a sum of money to buy the liquidated asset. The paid funds go into the Lending Pool. Then the act of liquidation must be profitable.

Profits can be earned from 2 places：
- Cross-Market Arbitrage
- Additional Allowance
### The liquidator intervenes in the liquidation in the form of an auction
Consider the auction bid price as P. When (C + ∆C) > P, Then the liquidation to be profitable. When an asset is liquidated, the starting price of the asset is the current price of the asset. When calculating the current price of the asset, the Uncollected Fee in UNI-V3 NFT is not calculated. The liquidator's bid will be lower than the price of the liquidated asset in the vast majority of cases.
### Continuous Price Reduction Auctions
Since the liquidated assets are still subject to volatility after being liquidated. Therefore Themis plans to set up a mechanism similar to a Dutch-style auction. In other words, when all liquidators believe that the assets being auctioned will continue to decline. The current round of liquidated assets will be auctioned at a reduced starting price. And the next round will be held.
### The Information available to the liquidator
- Current token types that need to be liquidated to the lending pool
- Current number of tokens in the V3 position
- The estimated value of current liquidated assets
- Latest bid
### Avoid losses due to slippage in trading
The actual liquidation will incur fees and slippage in the trade. Therefore we need to introduce a risk factor to compensate for this loss. To ensure that the value of the liquidated assets is higher than the outstanding principal and interest. 

Define the risk factor as F_risk=(C+∆C)/(F_collateral*C+R)
![image](https://user-images.githubusercontent.com/75613147/133576407-19b42464-1430-44dd-a8e3-c9f0acbef335.png)
#### F_risk ∈ (1,1/F_liquidation)
## Oracle
In UNI-V3 collateralized lending, the prices of the collateral and lending assets are all taken from Uniswap. the frequency of obtaining prices is the same as Uniswap. Therefore when the contract reaches the liquidation condition. The underlying asset will be marked as pending_liquidation and display in the Auction Pad until liquidation is completed.
## Themis' liquidation preference for the highest bidder
In previous lending protocols for cryptocurrencies, the liquidation mechanism was first-come, first-served. This is because volatility is in sync with the market when a single asset is used as collateral. However, in Themis-Protocol V1, the borrower is collateralizing LP assets. This asset is subject to mutual replacement equilibrium within it and translates into impermanent losses when the market fluctuates. Higher fee income is generated when the assets fluctuate significantly. Therefore we do not need to liquidate the mortgagee's assets in the fastest way possible.
### Themis will generate more protocol revenue
Since (C+∆C)>P>(F_collateral*C+R), Themis receives protocol income as it is liquidated. These agreement revenues are transferred to Treasury, which decides how to utilizes them through governance.
Precisely because of the relatively high protocol income. Therefore we can give the liquidator an additional incentive of 130% of the current Gas-limit from the Reserving Pool to encourage the liquidator.
### Let's take some examples
eg: Assuming that in Compound the user is collateralizing ETH and borrowing DAI. If ETH happens to fluctuate by more than 50%. Then the collateral undergoes a 50% price fluctuation.
Whereas in Themis, assume the user is collateralizing an LP of ETH/DAI. when the volatility of ETH exceeds 50%, the value of LP may fluctuate less than 25%.
eg: Assuming the user is collateralizing an ETH/WBTC LP and lending DAI when the market drops 50% for ETH and 30% for WBTC, the overall LP value drop will be closer to the one with the lower drop.
In the case of Compound providing lending to the market, if the market falls radically, a large number of liquidated assets will squeeze market liquidity and may lead to further market declines. But in the case of Themis providing lending to the market, if the market falls radically. Liquidity may still be retained.
## Conclude
Themis' liquidation is another innovation that distinguishes it from classic money market liquidation. Not only does it give more people the opportunity to participate in the disposal of distressed assets for a return. At the same time, Themis is also working to provide a new approach to native asset governance. Several of the parameter factors currently developed for our product are
- Collateral factor: 0.5
- Liquidation factor: 0.75
- Risk Factor: 1.05
- Liquidation fee compensation: 1.3* Current_Gas-limit
We will adjust all initial parameters a few more times before governance is officially announced until all parameters are open for community governance.
