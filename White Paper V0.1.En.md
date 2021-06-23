# THEMIS PROTOCOL WHITEPAPER
The Premier NTF Collateral Lending Protocol

Version-0.1 Published on Jun·10th，2021
# Contents
- [Contents](#Contents)
- [Motivation](#Motivation)
- [Desin Principles](#Design-Principles)
  - [NFT Lending Basis](#NFT-Lending-Basis)
  - [Interest Caculation](#Intrest-Caculation)
  - [SP-Token Pledge](#SP-Token-Pledge)
  - [Borrowing Proceeds](#Borrowing-Proceeds)
  - [NFT Oracle](#NFT-Oracle)
  - [Collateral rate calculation for UNI-V3 NFT](#Collateral-rate-calculation-for-UNI-V3-NFT)
  - [Third-party platform provides NFT's collateral rate calculation](#Third-party-platform-provides-NFT's-collateral-rate-calculation)
  - [Liquidation](#Liquidation)
    - [Basic liqudation](#Basic-liqudation)
    - [Reserve liquidation](#Reserve-liquidation)
    - [Liquidators](#Liquidators)
- [Token Economics](#Token-Economics)
  - [Token Basic Information](#Token-Basic-Information)
  - [Liquidity Farming and Supply Farming](Liquidity-Farming-and-Supply-Farming)
    - [Block allocation principle](#Block-allocation-principle)
    - [Risk hedging between the liquidity pool and the lending pool](#Risk-hedging-between-the-liquidity-pool-and-the-lending-pool)
  - [Liquidator incentives](#Liquidator-incentives)
  - [Governance and Voting](#Governance-and-Voting)
    - [Distribution of Agreement Proceeds](#Distribution-of-Agreement-Proceeds)
- [Risk Management](#Risk-Management)
  - [Risk-taking role](#Risk-taking-role)
    - [Risk of the decentralization pool](#Risk-of-the-decentralization-pool)
  - [Impermanent loss of liquidity](#Impermanent-loss-of-liquidity)
  - [Extreme Quotes](#Extreme-Quotes)
- [Outlook](#Outlook)


# Motivation

Since the release of smart contracts on Ethereum, DApp development has matured, with developers creating a total of 10.7 million smart contracts in 2020, up 55% from 6.9 million in 2019. Superior DeFi protocols such as Uniswap, Compoud, and Yearn were born on Ethereum, and with Layer2 development bringing further improvements in performance and efficiency, the future will see the exploration and innovation of Ethereum in decentralized finance creating a huge derivatives market.

The imagination brought by the innovation of DeFi has made the concept of NFT hot again, and the market has gradually accepted the governance of various types of native assets, and as more NFT native assets are created. Providing governance for native assets such as trading, liquidity, lending, collateral, voting, etc. will be an important development trend.

**Themis protocol is a decentralized NFT lending protocol that will provide a collateralized lending scenario for NFTs.** 

We plan to improve the efficiency of lending by providing centralized lending instead of P2P lending. Thus, we can get a more efficient use of funds, while spreading the risk to different segments such as lending pools, auction, yielding farming, staking, and liquidation, and governing the native crypto assets in the protocol in principle through the three-point balance of yield, security, and liquidity. In the future, we will continue to explore the possibilities of DeFi to provide a more secure and easy-to-use product so that more people can benefit through crypto assets.

# Design Principles

Themis Protocol is deployed on Ethereum at its core and will eventually support multiple chains in parallel, and Themis will have business nodes at various points based on the characteristics of the asset flows. This means that the early operational framework of Themis is very important, and the ultimate vision of Themis protocol is to be the "Libra" for measuring the value of native crypto assets.

Themis is designed to follow the following principles: 

1. **Compatible with all assets on** Ethereum**:** The first practical and widely used smart contract platform is Ethereum, and each crypto community has its own mature native assets deployed on Ethereum, which provides a good environment for other communities to access. At the same time, compatibility with Ethereum means that we can realize code panning on the same type of public chains such as BSC, or parallelism on multiple public chains through cross-chain bridges, allowing more users to use our products.
2. **Simple risk management model**: Themis' risk management logic follows the principle of three-way assessment, assigning risk to the characteristics of asset safety, profitably and liquidity, and governing to resist systemic risk by assigning different risks and returns to different links.
3. **Lending based on marketable assets:** For collateralized lending, the core is to ensure that the value of collateral is higher than the lending funds. Access to prices and encashment of borrower collateral must therefore come from long-term unobstructed decentralized agreements.
4. **Business Expansibility:** The basic framework containing collateral lending, liquidate, asset auction, LP-token mining pool, risk reserve pool, and quote collector corresponds to the future business expansion possibilities of full-class collateral lending, issuing NFT, Decentralized Exchange, insurance, Oracle, and Vault.
5. **Governance of Native Crypto Assets:** Themis must ensure business compliance and therefore provides services only for native crypto assets. The newly generated Tokens are Utility Token or crypto credentials generated by the governance session. These credentials may have value, but do not promise Security Token properties as business dividends, equity, etc. 

## NFT Lending Basis

Peer-to-peer agreements directly facilitate collateralized and unsecured lending between market participants. When this format was initially applied to NFT lending, there was no information parity between the lender and the borrower, which directly resulted in significant costs for both parties. p2p lending proved to be inefficient in decentralized networks. Instead, decentralized lending protocols such as Compound and AAVE are currently used to create pools of funds with algorithmically calculated interest rates based on changes in the supply and demand of assets. The suppliers and borrowers of assets interact directly with the protocol to earn or pay variable interest rates, which we believe is more efficient. The challenge is how to solve the problem of pricing NFT assets and the criteria for lending funds.

This means that on the collateral end, we need to be able to obtain the price of the collateral, and there are only two basic ideas for obtaining the price; the

1. Obtaining prices through publicly available transaction records.
2. Obtaining the price by calculating the value of the NFT itself.

The feasibility of collateralized lending presupposes that the value of the collateral is always higher than the value of the lending funds, and obviously path 1 is unable to ensure the relationship between price and value, while path 2 has limited solutions, and these limited solutions are roughly to:

- Acquisition of financial attributes by splitting the NFT tenure period.
- Determining its value through rigid exchange with other agreements.
- compensating for the risky accidents that may occur with liquidity through governance tokens.

Splitting NFT tenures does not comply with our compliance design principles due to the fact that collateralized NFT tenures that are split must undergo an auction or other transactional act when the collateral is liquidated, at which point distributing the proceeds by tenure would be an act of profit-sharing. Even in the real world, splitting the title of a collection that is to be auctioned off may be illegal in most jurisdictions. The new Token created by splitting the ownership has potential security characteristics. and therefore are not considered.

Whereas rigid payments in other protocols, such as UNI-V3's LP vouchers whose prices are more easily tracked. Governance tokens are used to compensate for risky incidents that may occur and are also generally accepted today. Thus NFT lending is based on a Compound-like form of agreement for NFTs that can be rigidly redeemed, where the assets held in the agreement have a collateralization factor from 0 to 1. The liquidity and value of the underlying assets determine the size of the collateralization factor. The collateral sum multiplied by the collateral factor equals the amount available for borrowing by the subscriber. When it is determined that the user is unable to repay, a liquidation process is executed to redeem the lender's principal and interest through a rigid payment in a third party. When the third-party rigid payment cannot redeem the principal and interest, the additional governance tokens from the reserve pool are redeemed by the act of selling into the liquidity pool. 

So when reading the user-authorized NFT, Themis protocol scans at least 3 blocks (to prevent flash lending attack) to get an `ApproveBind` of the asset price in the third-party protocol platform, and once successful, `TokenHub` will mark the price relationship of that NFT to the bound asset to get an offer. Based on this quote the next lending process is executed.

## Interest calculation

Unlike individual suppliers of funds or borrowers who must negotiate terms and interest rates, Themis Protocol and Compound protocol use the same interest rate model to achieve interest rate equilibrium based on supply and demand in each money market. According to economic theory, the interest rate (the "price" of money) rises as a function of demand; when demand is low, the interest rate falls. And vice versa. The utilization rate of each market unifies supply and demand into one variable.

- Variable function.

    ### U_a=Borrows_a/(Cash_a + Borrows_a)

- The demand curve is prepared through governance and expressed as a function of the utilization rate. For example, the borrowing rate is similar to.
- The interest rate earned by the supplier of funds is implicit and is equal to the borrowing rate multiplied by the utilization rate. where 2.5% is a settable parameter, which is modified by a community vote.

    ### BorrowingInterestsRate_a=2.5%+U_a*20%

- For each transaction that occurs, the interest rate index for the asset is updated to compound the interest rate since the previous index, for the utilization period, denominated in Tokens of lending units, using the interest rate per block calculated as.

    ### Index_a,n=Index_a,(n-1) * (1+r*t)

- Total market outstanding borrowings were updated to include interest payments outstanding since the last index at.

    ### totalBorrowBalanced_a,n=totalBorrowBalanced_a,(n-1) * (1+r+t)

- A portion of the accumulated interest is retained (set aside) as a reserve, determined by the reserveFactor (reserve factor), ranging from 0 to 1

    ### reserves_a=reserves_a,(n-1)+totalBorrowBalance_a,(n-1) * (r * t * reserveFactor)

## SP-Token Pledge

When a lender deposits a Token, it receives an SP-Token (Ctoken_name) credential and receives an additional TMS incentive in the form of an SP-Token.

## Borrowing proceeds

The revenue distribution of the lending is as follows

Lender earnings: Lending interest + SP-Token earnings

Themis protocol dividend pool distribution: Lending interest - Borrowing interest

Interest = principal * interest rate * time

## NFT Oracle

Obtaining NFT quotes is a complex process, and depending on the source of data collection, corresponding to different risks, our sources of quote collection are divided into

- The total price of assets represented by credentials that can be rigidly redeemed in third-party agreements, such as UNI-V3 or certain financial attribute NFTs.
- Prices provided by third-party platforms and prophecy machines in historical transaction records, which we refer to as platform-provided NFTs.

After obtaining the offer price, the collateralization rate is then evaluated based on the cashability of the asset as a way to improve the efficiency of the use of funds.

## Collateral rate calculation for UNI-V3 NFT

uniswap in V3 will focus on the credentials of the liquidity making market as a NFT that can rigidly pay out all the funds of the liquidity provider, but the overall value of the Token (A) and Token (B) added into the liquidity and the reward (P) received may change along with the transactions that occur. The acceptable range of collateralization rates should be slightly less than the ratio of the peak asset price at the time of rigid payment to the sum price of the assets at the time of collateralization. When price scales ( Ticks ) and price ranges ( Ranges ) are introduced, focused liquidity market making further locks in the range of fluctuations in the value of assets in the liquidity pool.

In the calculation of x*y=k, where and are the quantities of the two tokens x and y, are constants. In other words, earlier versions of Uniswap were designed to provide liquidity over the entire range of prices. This facilitates implementation and allows for efficient aggregation of liquidity, but means that a large portion of the assets in the liquidity pool will never be available. This ratio is about 50%. That is, in the case of an ETH/USDT pair, no matter how volatile the price is, USDT will never fall below the initial 50%, even if there are infrequent losses incurred. The asset ETH on the other end of the comparison will also be the number of ETH corresponding to the USDT price. In the V3 version, aposition was introduced so that virtual positions can be depleted and thus no longer provide liquidity. The virtual position (aposition) corresponds to the real asset as follows

![image](https://user-images.githubusercontent.com/75613147/121540808-5c92c000-ca39-11eb-8100-78c64c9b0c0e.png)

- The sum of assets x and y is the final true sum of assets in that NFT, and with the prophecy machine, we can obtain the prices of these two assets to calculate a reasonable mortgage rate.
- If the borrower defaults at this point, we can then follow the regular liquidation process.
- We can estimate the last possible remaining assets in the NFT by reading the values that can be taken from Ticks and Ranges, and take the lowest value of the change as the basis for setting the collateralization rate

![image](https://user-images.githubusercontent.com/75613147/121540923-703e2680-ca39-11eb-86d3-a4f451ad59cc.png)

In summary, the collateralization rate we give to UNI-V3 NFT is approximately: 

**Collateral rate = [Final Price(x+y)/Origin Price(x+y)]**Unit_token**65%,**

which will be approximately equal to 65% of the total value of the liquid assets provided.

## Architecture
![image](https://user-images.githubusercontent.com/75613147/121540198-dd9d8780-ca38-11eb-992b-195454664dec.png)

## Third-party platform provides NFT's collateral rate calculation

Secured lending for NFTs provided by third-party platforms is subject to strict vetting and the tokens that can be lent initially will be limited, which will be decided by a community vote. Collectible NFTs do not have good liquidity. This creates difficulties for liquidation. Therefore Themis protocol is primarily liquidated by means of a compensation reserve, which means that when the reserve pool is not sufficient, collection NFTs will not be available for lending. When the reserve pool is sufficient, third party NFTs will have the possibility to obtain a 50% collateralization rate.

## Liquidation

If the value of outstanding debits on an account exceeds its repayment capacity, a portion of the debit can be repaid at the current market price minus a liquidation discount; this eliminates the risk of the agreement. In the event that a user's funds are in repayment crisis, the liquidation process may continue.

Any Ethernet address with borrowed assets can invoke the liquidation function to exchange their assets for the borrower's SP-Token collateral. Since both users, assets and prices are included in the Themis protocol, the liquidation is executed effortlessly and does not rely on any external systems or orders.

### Basic liqudation

In Themis' basic liquidation process, when a bad debt occurs, the bad debt goes to a liquidation list, the liquidator pays a fee to execute the liquidation, and the liquidator receives a certain TMS bonus from the reserve pool. The first step of liquidation causes the bad asset to flow to the auction floor for a 48-hour Dutch auction at the initial price until the NFT price drops to the collateral rate to execute a sell liquidation or reserve liquidation.

### Reserve liquidation

Themis protocol will continue to add TMS to the margin pool during its operation, and when the margin pool liquidation is required, TMS will be sold into the market for the lender's principal, at which point the liquidity provider may incur an unvarying loss. However, the liquidity provider can also obtain liquidity mining rewards by pledging the corresponding LP-Token.

### Liquidators

The liquidator is an important role to assist the platform in manual liquidation. Since the protocol cannot be automatically liquidated, a liquidation list will be generated for bad debts, and the liquidator can perform liquidation through the list while gaining TMS from it.

- Each reserve pool receives 10% of TMS per block, i.e. 2.6. Here is the amount that can be withdrawn and needs to be taken out by triggering the contract
- When the NFT is aborted, the corresponding order enters the reserve pool, and the reserve pool aggregates the order amount. When the NFT flow auction, the formation of a value to deal with bad debts, when the value < the repo pool balance, can be handled by the liquidator to initiate a contract to deal with bad debts. When the bad debt value is greater than the repo pool TMS balance, the order cannot be processed and needs to wait for replenishment.
- The bad debt order value can be accumulated, and the clearer can clear a single contract or a batch (and for one transaction), and needs to pay the gas fee by itself
- At this time the natural user can see the repurchase order on the interface, at this time you can click actuarial, pay the handling fee (BNB or ETH) and trigger the following process
    - The contract sells TMS equivalent to the user's default amount and interest on uniswap or pancake, and the resulting coins are credited back to the corresponding lending pool
    - The TMS received by the user at each liquidation is fixed, i.e. the TMS. so the liquidation should be 100% of the liquidated bad debt value, i.e. 100% of the liquidated value goes to DEX for repurchase, while the corresponding TMS is awarded to the liquidator. A fixed amount of TMS equivalent to 130% of the price of the Gas fee (to encourage batching of orders and avoid a massive squeeze on the liquidity pool), which is 130% of the TMS also comes out of the repo pool.
- When the total amount of bad debts exceeds 50% of the TMS of the repo pool. Liquidation cannot take place. (On day 30, supplier can vote with token like TUSDT to meet the split and liquidate all TMS of the liquidation pool)

# Token Economics

Themis Protocol will issue governance tokens TMS to be used for the governance of each segment.

## Token Basic Information

Token Name: TMS

Total Number of Issues: 600,000,000

Total Number of Farms: 330,000,000

Block Generate Time: 15s/block

Total block reward of TMS per block initially: 26 TMS

Block Reduction Height: 2,024,000 (approx. 1 year)

Reduction Range: 20%

## Liquidity Farming and Supply Farming

Depending on the governance of different roles, Themis protocol will award 26 tokens to different roles in each block, where lenders and liquidity providers who provide borrowing will be rewarded with TMS respectively, and a portion of TMS will be reserved for issuance to the reserve pool to maintain liquidation.

### Block allocation principle

TMS from each block will be placed into the lending reward pool, the liquidity mining reward pool and the reserve pool, respectively, on a pro rata basis.

- Lending pool: the total reward for each SP-Token in the lending pool is allocated based on the weight of each SP-Token (token_name), while each user is allocated based on the weight share in the SP-Token (token_name).
- Yielding Farming Pool: The Yielding Farming pool is allocated and configured based on the size of the funds lent out by the lending pool, and when the percentage of SP-Token (token_name) lent out increases is, the reward corresponding to platform-LP-Token TMS/(token_name) will increase.
- Reserve pool: each time a block is released a fixed amount of TMS is put into the reserve pool

### Risk hedging between the liquidity pool and the lending pool

When Tokens from the lending pool are lent out, the mining reward given to the Yielding Farming of the corresponding pairs is increased, attracting users to hedge against the risk by providing the corresponding liquidity size. Due to the premise that reserve pool liquidation may occur, the average risk of impermanent loss taken is less when the liquidity of that pool is thicker.

### Liquidator incentives

Liquidators will pay a Gas fee upon liquidation, and the assets liquidated will be returned to the lending pool, but will additionally receive a TMS incentive of 130% of the fee corresponding to the TMS, deducted from the reserve pool.

## Governance and Voting

TMS can be used to govern transactions in Themis Protocol to vote on proposals by pledging to obtain votes, a feature that will be available in V2.

[Protocol Parameter Governance](https://www.notion.so/6555f2d18d27440ba470ea18b922d43a)

### Distribution of Agreement Proceeds

Themis Protocol will distribute 100% of all proceeds to TMS holders, which users need to receive by pledging TMS, which include but are not limited to.

- % of Utility Fee: the spread and commissions that the platform receives from each segment.
- Revenue from the sale of the platform's original NFTs: the profit the platform receives from the sale of NFTs.
- Data Usage Fee: the revenue the platform receives from data services such as advertising, prophecy machines, and other data interface services.

# Risk Management

## Risk-taking role

Themis team actively explores the rationality and security of the NFT lending protocol. The Themis protocol is based on the concept of risk-dispersed, targeted governance and differentiates the roles involved in the chain, which primarily bear the following risks.

- Lender: the lender mainly bears the yield risk
- Lender & Borrower: The lender mainly bears the yield risk. Since Themis is a decentralized protocol and the borrower applies overcollateralization, the lender can always recover the principal and interest by liquidating the borrower's assets, but the interest yield is only slightly higher than the average of treasury bonds.
- Liquidity provider: mainly bears the security risk due to impermanent losses. Since the liquidity provider provides liquidity to a trading pair, coupled with the possible impact on the liquidity pool from having Farming increase and reserve liquidation, the impermanent losses will likely be amplified and there may be a risk of principal loss in terms of the principal amount invested in the liquidity pool. However, the returns from liquidity mining could be very high.
- Liquidator: The liquidator mainly bears the risk of profitability. As a beneficiary, it needs to pay the corresponding Gas fee to assist in liquidation, and although it will get an additional liquidation reward, there may be a relative price change between the TMS of the liquidation reward and the Gas fee paid.
- Dividend holders: Dividend holders mainly bear the liquidity risk and can participate in all Utility allocations of themis protocol by pledging TMS, but there may be price fluctuations in TMS when trading TMS.

### Risk of the decentralization pool

When the pool of funds is lent out in large quantities and is in lending status, the lender may not be able to redeem the principal amount they lent at any time. The platform will open up the third party platform for splitting in the future, and the lender can give up the interest return and force the principal amount to be redeemed in advance.

## Impermanent loss of liquidity

In providing liquidity is due to follow the principle of x * y = k or aggregated liquidity market making, when the price of one end exceeds the position range, the other end of the asset is bound to be depleted, of course the current AMM market making mechanism has avoided this. However, the impermanent loss still exists, i.e. the liquidity provider may have sold assets at one end at a lower price or depleted adding assets at the other end in large quantities. In Themis' process, in the case of TMS/USDT, if the liquidation of TMS may accelerate the depletion of USDT due to the reserve compensation mechanism. Leaving the liquidity provider to bear the loss of a one-way position. Even though the purpose of the liquidity provider is to gain the TMS in addition to earning fees. Thus the liquidity provider is forced to gain more TMS. this additional impermanent loss. We will give a voucher for this in V2.

### Impermanent loss voucher

The Impermanent Loss Voucher is an additional voucher granted to the liquidity provider by Themis protocol. since the rationale for reserve liquidation is to compensate for the additional tokens issued by the lending funds. the original collateral has been relegated to the committee. The committee therefore has the ability to dispose of these assets, and in V2, the impermanent Loss Voucher can be used as an insurance mechanism in exchange for non-performing assets that the committee plans to dispose of.

## Extreme Quotes

When there is a systemic risk that all assets decline and the excess borrowed assets cannot be liquidated in a timely manner, this may result in the protocol being forced to deactivate (due to certain restrictions initially set by the protocol to avoid extreme quotes, such as closing down collateralized lending on third-party platform categories when the reserve pool is insufficient. Or new borrowers may not be able to borrow additional funds when the lending pool is undersupplied, etc.) What is certain at this time is that in the event of a systemic liquidity crisis, to ensure that there is no stagnation of platform development due to the collapse of the economic system. The platform's auction function, lending function will remain. However, the rewards of liquidity mining will be reduced to avoid the unpredictable loss of large liquidity providers. By voting, it is decided that at a certain block height re, all out block rewards originally rewarded to liquidity mining go to the black hole address.

# Outlook

It is difficult to make a definitive statement about the future of Themis protocol, as it will continue to evolve, and our framework is designed to support the reasonable operation of all DeFi products. Themis vision is to become a value scales for native crypto assets. While we are actively developing and completing our NFT pricing and collateral lending business, we are constantly looking for opportunities to expand our business horizontally to bring more convenience and revenue to the community. We firmly believe that collateral is the "magic mirror" of all asset values. This, in turn, cannot be achieved without a core function like general Oracle. Our mission is to find better pricing models for native crypto assets and provide more equity to them. And this means that we may move in the following directions in the long term.

1. a full range of crypto lending operations and optimizing the efficiency of crypto lending.
2. a platform that provides a trading environment for all types of cryptos, such as DEX, NFT auctions, and native crypto offerings.
3. providing accurate quotation data and becoming a NFT Oracle.
4. issuing products for different financial strategies, such as Vault Pool.
5. providing derivatives services, such as futures, options, leveraged transactions and insurance

Themis will potentially be the ultimate platform for aggregating DeFi protocols.
