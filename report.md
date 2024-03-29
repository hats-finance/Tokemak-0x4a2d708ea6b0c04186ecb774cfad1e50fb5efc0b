# **Tokemak Audit Competition on Hats.finance** 


## Introduction to Hats.finance


Hats.finance builds autonomous security infrastructure for integration with major DeFi protocols to secure users' assets. 
It aims to be the decentralized choice for Web3 security, offering proactive security mechanisms like decentralized audit competitions and bug bounties. 
The protocol facilitates audit competitions to quickly secure smart contracts by having auditors compete, thereby reducing auditing costs and accelerating submissions. 
This aligns with their mission of fostering a robust, secure, and scalable Web3 ecosystem through decentralized security solutions​.

## About Hats Audit Competition


Hats Audit Competitions offer a unique and decentralized approach to enhancing the security of web3 projects. Leveraging the large collective expertise of hundreds of skilled auditors, these competitions foster a proactive bug hunting environment to fortify projects before their launch. Unlike traditional security assessments, Hats Audit Competitions operate on a time-based and results-driven model, ensuring that only successful auditors are rewarded for their contributions. This pay-for-results ethos not only allocates budgets more efficiently by paying exclusively for identified vulnerabilities but also retains funds if no issues are discovered. With a streamlined evaluation process, Hats prioritizes quality over quantity by rewarding the first submitter of a vulnerability, thus eliminating duplicate efforts and attracting top talent in web3 auditing. The process embodies Hats Finance's commitment to reducing fees, maintaining project control, and promoting high-quality security assessments, setting a new standard for decentralized security in the web3 space​​.

## Tokemak Overview

Autopilot enables a new way to LP by automatically rebalancing liquidity across DEXs and pools for the best yield

## Competition Details


- Type: A public audit competition hosted by Tokemak
- Duration: 2 weeks
- Maximum Reward: $22,223.64
- Submissions: 25
- Total Payout: $20,001.27 distributed among 20 participants.

## Scope of Audit

This audit will focus on the Strategy portion of our code base and the logic related to the evaluation of rebalances. This includes the following the contracts:

- `src/strategy/LMPStrategy.sol`
- `src/strategy/LMPStrategyConfig.sol`
- `src/strategy/NavTracking.sol`
- `src/strategy/ViolationTracking.sol`



## Low severity issues


- **Event Not Emitted as Indicated in `NavTracking::insert` Code Commentary**

  The issue involves the 'NavTracking::insert' function in the Tokemak v2-core-hats project. An 'emit event' TODO is left out in the function and is not correctly executed. A proposed solution has been made to create an event and execute it at the end of the function.


  **Link**: [Issue #1](https://github.com/hats-finance/Tokemak-0x4a2d708ea6b0c04186ecb774cfad1e50fb5efc0b/issues/1)


- **Issue with Checking Past NAV in Pool Swaps Code Contradicting Documentation**

  The issue is about a discrepancy in the contracts behaviour versus the intended behaviour as per the documentation. The point of contention is that in case all three delta values are negative, pool swaps should be paused until the NAV reaches the highest recorded value or 90 days have passed since the test, as per the document. However, as per the posted codes, the first condition is never checked, causing a difference in the expected and actual behaviour.


  **Link**: [Issue #4](https://github.com/hats-finance/Tokemak-0x4a2d708ea6b0c04186ecb774cfad1e50fb5efc0b/issues/4)


- **Issue with Underlying Token Decimals in getRebalanceValueStats Function**

  The issue discusses an incorrect assumption in the 'getRebalanceValueStats' function where it's supposed that the underlying token for 'LMPVault' is in 18 decimals. This might not be the case as there's no enforcement in the constructor. A remedy proposed is to scale up/down 'params.amountIn/params.amountOut' based on the token decimals to achieve the right precision. A user comment suggests it's currently configured to only support WETH as the baseAsset.


  **Link**: [Issue #5](https://github.com/hats-finance/Tokemak-0x4a2d708ea6b0c04186ecb774cfad1e50fb5efc0b/issues/5)


- **Potential Precision Loss in LMPStrategy ReturnExPrice Calculation**

  The issue highlights a potential precision loss in a line of code within the 'Tokemak/v2-core-hats' repository. The problematic part involves separate division operations during a calculation with variables 'result.baseApr', 'weightBase', 'result.feeApr', 'weightFee', 'result.incentiveApr' and 'weightIncentive'. The proposed fix suggests changing the calculation order to avoid this precision loss.


  **Link**: [Issue #10](https://github.com/hats-finance/Tokemak-0x4a2d708ea6b0c04186ecb774cfad1e50fb5efc0b/issues/10)


- **Issue with EIP-2612 Deadline Compliance in LMPStrategy Solidity File**

  The issue highlights a discrepancy from the EIP-2612 specification that states signatures should be allowed on the exact deadline timestamp. The problem encountered in the LMPStrategy.sol file is all about timestamp validation where it's suggested to consider whether it makes more sense to include the expiration timestamp as a valid timestamp, as currently done for deadlines.


  **Link**: [Issue #12](https://github.com/hats-finance/Tokemak-0x4a2d708ea6b0c04186ecb774cfad1e50fb5efc0b/issues/12)


- **Inconsistency in Parameters and Absence of Constraints in Validate Function**

  The `validate` function lacks verification for certain parameters, leading to inconsistencies with the documentation. Parameters like `minInDays`, `maxInDays`, `relaxStepInDays`, `relaxThresholdInDays`, and `tightenStepInDays` as defined in the test files are not in line with their declarations. To maintain code consistency and stability, it's suggested to declare parameters with expected data values as constants.


  **Link**: [Issue #19](https://github.com/hats-finance/Tokemak-0x4a2d708ea6b0c04186ecb774cfad1e50fb5efc0b/issues/19)


- **LMPStrategyConfig.validate Inconsistently Checks for MAX_NAV_TRACKING**

  The issue lies in the '30-60-90 NAV Test or LookBack Test', which monitors swapping costs. It was found that the maximum number of tracked days is 91, contradicting the specified maximum of 90 days. This error occurs because 'config.navLookback.lookback3InDays' only considers values greater than 'MAX_NAV_TRACKING', thus including 91. It is recommended to replace ">" with ">=" to correctly check 'config.navLookback.lookback3InDays' against 'MAX_NAV_TRACKING'.


  **Link**: [Issue #22](https://github.com/hats-finance/Tokemak-0x4a2d708ea6b0c04186ecb774cfad1e50fb5efc0b/issues/22)


- **Precision Loss in `verifyLSTPriceGap()` Due to Integer Division in Tokemak**

  The function `verifyLSTPriceGap()` in the Tokemak-0x4a2d708ea6b0c04186ecb774cfad1e50fb5efc0b project is designed to calculate the largest difference between spot & safe prices for the underlying tokens. However, due to the rounding down nature of the Solidity, the function's output suffers from precision loss leading to an inaccurate result. The function allows the execution to proceed even when it exceeds the allowed tolerance limit due to the precision issues, which could potentially pose a vulnerability threat. A refactored formula was offered to eliminate this issue.


  **Link**: [Issue #23](https://github.com/hats-finance/Tokemak-0x4a2d708ea6b0c04186ecb774cfad1e50fb5efc0b/issues/23)



## Conclusion

The publicly hosted Tokemak audit competition on Hats.finance, a platform that advocates for decentralized security in the Web3 ecosystem, drew 25 submissions. A total of 20 participants successfully found bugs and shared remedies within a two-week timeframe, splitting the $20,001.27 prize pool among themselves. This pay-for-results approach encourages top-quality security audit submissions, making web3 projects more robust and secure. The audit focused on the Strategy module, uncovering several low severity issues mainly related to incorrect assumptions or calculations in the code and mismatched behaviours as per the documentation. Solutions proposed ranged from simple adjustments in calculations or operations to declarations of expected data values as constants for stability. This decentralized approach affirms the efficacy and economic efficiency of autonomous bug bounty competitions in the web3 space.

## Disclaimer


This report does not assert that the audited contracts are completely secure. Continuous review and comprehensive testing are advised before deploying critical smart contracts.


The Tokemak audit competition illustrates the collaborative effort in identifying and rectifying potential vulnerabilities, enhancing the overall security and functionality of the platform.


Hats.finance does not provide any guarantee or warranty regarding the security of this project. Smart contract software should be used at the sole risk and responsibility of users.

