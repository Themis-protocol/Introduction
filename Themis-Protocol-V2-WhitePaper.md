# Themis Protocol V2 WhitePaper
### Unlocking Liquidity For LP Position

Version-2 Published on Oct·28th, 2021

 Author：@Vincent Lee

# 1. Overview

Themis Protocol V2 is a decentralized p2p lending protocol built on the Ethernet blockchain. Themis Protocol has ERC-721/ERC1155 asset compatible features compared to other pooling model p2p lending protocols, allowing users to create pools of funds to lend against valuable NFTs (e.g. Uniswap V3 Position) for anonymous lending. Market makers can continue to receive transaction fee revenue from market-making after collateralizing the assets and they can lend a certain amount of tokens from the protocol for other purposes.

Compared to collateralized lending for ERC20 assets, which is extremely dependent on market liquidity, the assets collateralized in Themis Protocol have better value stability and favorable growth expectations. Themis provides market makers with an increased in fund utilization rate and creates a new money market to provide a decentralized demand savings service.

Themis Protocol V2 continues the design principle of V0.1 with minor adjustments to the existing business. For example, the economic model has been adjusted to add incentives for Borrowers allowing for compound business growth, direct incentives for Liquidity Providers have been removed to reduce the extreme volatility that governance tokens may encounter, and the liquidation model has been supplemented to make Themis Protocol more riskaverse.

[Themis Protocol WhitePaper V0.1](https://github.com/Themis-protocol/Introduction/blob/main/WhitePaper-V0.1-En.md)

## Contents
- [Overview](#1-Overview)
- [Introduction](#2-Introduction)
- [Themis Protocol V2](#3-Themis-Protocol-V2)
   - [Supply Market](#31-Supply-Market)
   - [Borrow Market](#32-Borrow-Market)
   - [Interest Rate Models & Market Dynamics](#33-Interest-Rate-Models-&-Market-Dynamics)
   - [Liquidation & Auction](#34-Liquidation-&-Auction)
   - [NFT Avatar](#35-NFT-Avatar)
- [Governance](#4-Governance)
   - [Themis Governance Token](#41-Themis-Governance-Token)
   - [Voting Module](#42-Voting-Module)
   - [Governance Process](#43-Governance-Process)
   - [Treasury](#44-Treasury)
- [Risks & Precautions](#5-Risks-&-Precautions)
   - [Risk Bearing](#51-Risk-Bearing)
   - [Risk Precautions](#52-Risk-Precautions)
   - [Extreme Risks](#53-Extreme-Risks)
- [Conclusion](#6-Conclusion)
- [Parameters & Rate](#7-Parameters-Rate)

# 2. Introduction

Money markets and AMMs are key components of DeFi. They're typically used to enable permissionless token lending and trading via smart contracts.

The introduction of concentrated liquidity allows market makers to flexibly distribute assets across price ranges, however, there is still a large demand for capital to deploy liquidity across price ranges. Further, when the focused liquidity is outside the price range, market makers need to repeatedly adjust their positions to maximize funding efficiency. These two needs give rise to an incentive to increase leverage on AMM positions.

Themis Protocol: Is a money market protocol for AMMs. It allows market makers to increase their leverage at a lower cost while having better control over the price range of their funds. This article will explain how Themis Protocol creates an asset bridge between money markets and AMMs through algorithms, functionality, and governance.

# 3. Themis Protocol V2

## 3.1 Supply Market

Themis Protocol's supply market currently allows for the supply of ERC20 tokens with an algorithm-agreed interest rate. It allocates interest based on the weight of the supply.

### 3.1.1 Removing the property of Supply Token as collateral

Previously, users supplying tokens in a money market would receive corresponding collateral tokens (e.g. cToken, aToken) as collateral in the agreement. These collateral tokens are destroyed after the user withdraws the assets. In the past, this model was accompanied by incentives that would allow some users to have an arbitrage opportunity and create a demand for Flash loans.

Themis Protocol also assigns a corresponding SP-token (Supply Provide Token) as a credential to the user at 1:1 after the user supplies an asset. But the asset is not directly used as collateral in the protocol because the SP-token has the property of transfer and settlement.

- Deposit 1 token to acquire 1 SP-token
- Transfer X number of SP-tokens from address A to address B, which will be able to take out x number of tokens in the Supply Pool.
- When SP-token is transferred out of address A, interest and other incentives will be settled to address A.

This allows SP-tokens to be transferred between addresses as claims and the incentives to be settled to the transferor.

*Figure 1：The exponential relationship among Deposit, Utilization, TVL and Current withdrawals

![Preview index](https://user-images.githubusercontent.com/75613147/143675271-da0232f4-6972-4711-9cc4-444ffe16d69c.jpg)

### 3.1.2 Flexible Access & Vaults

The Supply Market has a maximum funding utilization rate of 95% and even in times of market overheating, the Supply Market generally does not exceed 80%. With a diversified deposit base, the larger the total supply, the more flexible the access to the funds. In order to maintain a high total supply of funds over time, Themis has added the Vaults contracts to form agreements with a long-term supply of funds.

### 3.1.3 Cases

The basic scenarios to which the supply of tokens (including the acquisition of SP-tokens) applies include

- Receiving interest accrued from the completion of the agreement by the borrower
- The transfer attribute of SP-token. It will form a new debt trading market, e.g. an AMM market offering a 1:1 swap of SP-token/token that can settle fees and interest per transaction.
- SP-tokens can be used as collateral to borrow other assets.

*Figure 2：SP-token can be used as a new pass with a 1:1 deposit
![Untitled 1](https://user-images.githubusercontent.com/75613147/139277375-4fa61a00-2732-4316-9699-2b6d67bb719a.png)

## 3.2 Borrow Market

Themis Protocol's lending market allows ERC-721/ERC-1155 to be used as collateral to borrow assets and having the algorithm agree on the interest rate. The user redeems the collateral by returning all principal and interest. 

*Figure 3: Use TWAPs Oracle to obtain more accurate real-time prices for collateral
![TWAPs](https://user-images.githubusercontent.com/75613147/143675313-78c69252-3c7c-4f98-9c27-cd4abe71ca3d.jpg)

### 3.2.1 Collateral value

Users must borrow the asset to complete collateralization. The Oracle feed price will be obtained before being collateralized by using the Uniswap V3 position as collateral to borrow USDC. As an example, the process is as follows:
- Authorization in reading token 0 and token 1 information in UNI-V3-POS.
- Calculating the sum of the number of token 0 to USDC and the number of Token 1 to USDC through Uniswap's Oracle feed price. This results in the current equivalent number of USDC in that UNI-V3-POS.
- Users are able to select a collateral rate from 0-0.8, thus completing the borrowing of USDC and collateralizing the reNFT into the Protocol.
- The number of token 0 and token 1 collateralized in the Protocol is continuously monitored.

### 3.2.2 Collateral ratio

Themis Protocol offers two types of collateral rates. The first is the General Collateral Rate, which takes values in the range 0-0.65. And the second is the Certified Collateral Rate, which takes values in the range 0-0.8. Details of the Certified Collateral Rate are explained in Section 3.5 - NFT Avatar.

### 3.2.3 Reconciling positions and redeeming collateral

Calculating the outstanding principal and interest value/collateral value gives a liquidation factor. In order to avoid liquidation of the asset, users can adjust the liquidation risk by making partial repayments or increase the collateral. The collateral is redeemed when the borrowing account is fully repaid. Interest and incentives are settled once per reconciliation.

### 3.2.4 Cases

Lending for AMMs is achieved by allowing AMMs to adjust exposures without changing the collateral position.

- AMMs can quickly adjust positions spread across different liquidity bands to improve capital efficiency.
- Adding leverage to efficient liquidity positions.
- Collateralizing existing positions to short borrowed tokens.

## 3.3 Interest Rate Models & Market Dynamics

Unlike individual suppliers of funds or borrowers who must negotiate terms and interest rates, Themis Protocol and Compound Protocol use the same interest rate model to achieve interest rate equilibrium based on supply and demand in each money market. According to economic theory, the interest rate (the "price" of money) rises as a function of demand; when demand is low, the interest rate falls. And vice versa. The utilization rate of each market unifies supply and demand into one variable.

- Variable function.
    
    **U_a=Borrows_a/(Cash_a + Borrows_a)**
    
- The demand curve is prepared through governance is expressed as a function of the utilization rate.
- The interest rate earned by the supplier of funds is implicit and is equal to the borrowing rate multiplied by the utilization rate. 2.5% is a parameter, which is modified by a community vote.
    
    **BorrowingInterestsRate_a=2.5%+U_a*20%**
    
- For each transaction that occurs, the interest rate index for the asset is updated to compound the interest rate since the previous index, for the utilization period, denominated in Tokens of lending units, using the interest rate per block calculated as.
    
    **Index_a,n=Index_a,(n-1) * (1+r*t)**
    
- Total market outstanding borrowings were updated to include interest payments outstanding since the last index.
    
    **totalBorrowBalanced_a,n=totalBorrowBalanced_a,(n-1) * (1+r*t)**
    
- A portion of the accumulated interest is retained (set aside) as a reserve, determined by the reserveFactor (reserve factor), ranging from 0 to 1
    
    **reserves_a=reserves_a,(n-1)+totalBorrowBalance_a,(n-1) * (r * t * reserveFactor)**    

![Pooling Model](https://user-images.githubusercontent.com/75613147/141614598-ac04b9d8-113f-417f-aeec-e78cdbe6d664.png)

## 3.4 Liquidation & Auction

If the value of outstanding borrowings/collateral on an account reaches a certain threshold, Themis Protocol executes a liquidation process to eliminate the risk of the agreement. This creates an auction market with a spread between the liquidated asset and the outstanding asset. If the price of the collateral continues to fall during the auction, a forced liquidation process is triggered.

[Liquidation & Auction of Themis Protocol](https://github.com/Themis-protocol/Introduction/blob/main/Liquidation.md)

### 3.4.1 Liquidation Factor Trigger

When the outstanding borrowing/collateral value ≥ 0.9, the user will not be able to redeem the collateral. The asset will be liquidated by the liquidator to the auction contract, which will start at the current asset price.
![Untitled 3](https://user-images.githubusercontent.com/75613147/139277779-34cd5820-c052-4a5e-8d4a-187f725a78cf.png)

### 3.4.2 Liquidator Incentives

The protocol will incentivize 130% of the gas limit equivalent of governance tokens to the liquidator for the transfer of the assets to be liquidated to the auction contract.

### 3.4.3 Auction of liquidated assets

The liquidated assets will have 4 hours of bidding time after the auction starts. If no one bids for four hours, then the starting price adjusts to 0.95*previous round price and reset the 4 hours of bidding time. The auction process is as follows:
- Bidders bidding will initiate a transaction to the contract and lock in the funds and refund the last locked-in funds.
- Bidders can remove assets from the contract at the end of the round.
- A one-bid bidder will receive the lot directly at CurBidprice/0.95
- A successful auction will return the designated outstanding assets to the Lending Pool, and the excess will go to the Treasury.


### 3.4.4 Forced Liquidation

If the collateral price / outstanding principal and interest ≤ the risk factor (currently the risk factor is 1.03), then the liquidator is allowed to enforce the liquidation of the contract against the DEX agreement.

*Figure 6: Liquidation & Auction Mechanism
![liquidation](https://user-images.githubusercontent.com/75613147/143813658-0e9411c0-9869-4092-beaa-ef0c0b9cd6d1.png)

*Figure 7: Liquidation trend
![Untitled 4](https://user-images.githubusercontent.com/75613147/139277912-61b52251-3e98-40a0-9457-3eff6e288834.png)

## 3.5 NFT Avatar

Unlike the ERC20-to-ERC20 lending protocol that has Borrow 1 assets creating 1.3 supply, the collateral in the Themis protocol is not created by the funds deposited into the protocol. But it originates from a third party. The collateralization rate cannot exceed the threshold of the liquidation factor. As a result, Themis Protocol has an initial maximum collateralization rate of 0.65, i.e. an NFT with a collateralization value of 1 can borrow a maximum of 0.65 assets.

### 3.5.1 NFT Sign Certified Collateral Rate

If the address of the borrowed token is signed with a special NFT before collateralizing the asset, it can receive a higher collateral rate. 

Authentication method:

- The address is signed using an NFT with any tokenID in ERC721 to obtain authorization.
- The Token ID of this signed NFT is still detectable in the address when the address borrows a token.
- The percentage of all signatures of this type of NFT series reaches 10%, 20%, 50%, or more.
- Corresponding borrowing rate increases up to 0.7, 0.75, 0.8.

It should be noted that while more funds can be lent using the NFT Avatar feature, there will be a higher risk of triggering liquidation.

### 3.5.2 Casting/Reading ERC-721 Properties

ERC721 tokens have special properties that will potentially serve as important identifiers to enhance the credit of NFT holders in the future.

```python
def themisNFT(token_id):
    token_id = int(token_id)
    num_first_names = len(FIRST_NAMES)
    num_last_names = len(LAST_NAMES)
    nft_name = '%s %s' % (FIRST_NAMES[token_id % num_first_names], LAST_NAMES[token_id % num_last_names])

    ratio = RATIO[token_id % len(RATIO)]
    credit = CREDIT[token_id % len(CREDIT)]
    community = COMMUNITY[token_id % len(COMMUNITY)]
    image_url = _compose_image(['images/ratio/ratio-%s.png' % ratio,
                                'images/credit/credit-%s.png' % credit,
                                'images/Community/Community-%s.png' % community],
                               token_id)

    attributes = []
    _add_attribute(attributes, 'Ratio', ratio, token_id)
    _add_attribute(attributes, 'Credit', credit, token_id)
    _add_attribute(attributes, 'Community', community, token_id)

    return jsonify({
        'name': nft_name,
        'description': 'A THEMIS NFT.',
        'image': image_url,
        'external_url': 'https://themis.exchange/%s' % token_id,
        'attributes': attributes
    })
```

# 4. Governance

Themisused rudimentary economic model in our test network to encourage users to use our test network. This resulted in receiving important input from various eco-partners and releasing a new governance model. This new governance model is a combination of new incentives for the functions and contributions of various governance roles such as Lenders, Borrowers, Liquidators, Voters, Liquidity Providers, Market Makers, Eco-Partners, etc.

## 4.1 Themis Governance Token

### 4.1.1 Tokenomic Basis

Token symbol：$TMS

Total Supply：800,000,000

Total Supply of Incentive：466,000,000

Block Time：15s

### 4.1.2 Allocation

- Mining (Lender, Borrower, Liquidator Incentive): 35%
- Early Mining (Vaults early Incentive): 8%
- Stake, Liquidity, MM Incentive: 15%

### 4.1.3 Incentive Method

Themis Protocol settles the incentives through smart contracts. According to the data recorded in the block, the users of the protocol can freely choose to claim these incentives after meeting the conditions.

## 4.2 Voting Module

In order for Themis Protocol to move faster from administrator mode to community autonomy mode, a voting module will be introduced. It calculates votes by taking into account the number of staked governance tokens and the number of staked votes. 

## 4.3 Governance Process

### 4.3.1 Proposal

Any Themis-related proposals can be submitted in our Forum. For executable events, polling can be initiated with the assistance of relevant personnel.

### 4.3.2 Initiate a poll

Technical Committees, VC Whitelists, Foundations, can initiate new governance events by holding 10,000,000 votes.

General addresses holding 32,000,000 votes can also initiate governance events.

The voting round for an event cannot exceed 4 weeks.

Only one voting event can exist on the entire network at a time.

### 4.3.3 Vote Validation

Anyone holding a vote can vote on the event and can choose Y/N when voting. when the number of votes exceeds 51% of the whole network goes to the next process.

All votes cast will be destroyed.

### 4.3.4 Rejection

The technical committee has veto right on a proposal. 

When a proposal is rejected by the Technical Committee, all votes in the proposal are returned to the voters.

### 4.3.5 Governance event archiving

Completed polling events are marked as: Passed/Failed/In Progress/Completed and are displayed in the governance interface.

*Figure 8: Governance procedure reference
![Untitled 5](https://user-images.githubusercontent.com/75613147/139277979-d8d58976-b7ec-4476-b05a-41e6054604fe.png)

## 4.4 Treasury

All proceeds from the agreement will go to a Treasury address. Governance will vote on how it is used, such as buying back TMS, as a pledge reward, or investing in community development efforts.

## 5. Risks & Precautions

## 5.1 Risk Bearing

Themis Protocol's risk control concept is based on risk diversification and targeted governance. It distinguishes between the roles involved in the chain, which bear the following risks:

- Lender: Under-utilization of funds leads to lower returns and over-utilization of funds leads to lower liquidity.
- Borrower: Difficulty in accurately predicting borrowing costs due to floating interest rates and liquidation risk due to asset volatility.
- Liquidator: The fee subsidy received is insufficient to cover actual expenses.
- Bidders: Greater scope for arbitrage with non-bite size bids, but will also be exposed to asset float risk.
- Voters & Validators: Risk of asset volatility before and after pledging.
- Market Maker & Liquidity Provider: The risk of impermanent loss in providing liquidity.

## 5.2 Risk Precautions

### 5.2.1 SP-Token transfer property

When the deposit pool experiences excessive capital usage, The SP-token swap mechanism, and repurchase mechanism are set up to transfer the risk.

- When the SP-token cannot be temporarily taken out of the deposit, the Token can be exchanged 1:1 in the trading market of SP-token/token.
- When the SP-token cannot be taken out, a treasury or governance token will buy back and destroy the SP-token.

### 5.2.2 Partial repayment & additional collateral

Allowing partial repayment when the borrower is temporarily unable to repay in full, or adding collateral assets in the form of collateral to prevent liquidation.

### 5.2.3 Minting tax revolving mechanism

When all users claim their earnings, a mint tax of 0-10% is levied on the balance of Governance as a reserve to be recycled into the incentive pool.

## 5.3 Extreme Risks

### 5.3.1 Decentralized collateralization attack

As Borrower's assets are collateralized, their corresponding values are read based on the total number in the assets and then lent at a uniform collateralization rate. Therefore the actual collateral rate may generate slippage due to accuracy and liquidity pool depth. For example, LP collateral with the same assets will have a slightly larger actual collateral rate for 100Token0 and 100Token1 than for 1000Token0 and 1000Token1 collateral. This is due to the price slippage that occurs when trading large values on AMM. This mechanism will reduce the incentive for arbitrageurs to attack the contract through the Mega Whale account.

### 5.3.2 Extreme market risk

Since the collateral is an LP asset, the overall float ratio is closer to that of an asset with a lower float. Although the collateral position may become a one-sided asset under a liquidity focused mechanism. However, the quoting mechanism can change from time to time due to the depth of the liquidity pool and liquidation may be triggered early when there is a large liquidity movement, thus protecting depositors' interests.

# 6. Conclusion

This upgrade to Themis Protocol sets the tone for growth. Abandoning the architecture for Collection class NFT lending was a tough decision. However, Themis Protocol will be more focused on its exploration of DeFi and is expected to increase the percentage of retail users from Ether and L2, and Themis Protocol, which is focused on Organic Yields, will gradually become less dependent on the price of governance tokens.

# 7. Parameters & Rate
![image](https://user-images.githubusercontent.com/75613147/139278320-69a8644e-37e6-43c0-82ff-29203d7615c1.png)
*The above parameters can be modified through governance.
