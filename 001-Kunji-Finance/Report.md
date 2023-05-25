# Overview
## About P2PAudits
 P2P Audit, we source top talent from across the web3 ecosystem to put together the best team possible for your project, unlike traditional auditing organizations that employ teams of full-time security experts.

We understand that your project may have unique requirements and timelines, and we are flexible in accommodating your needs. Our goal is to work with you to ensure the security and reliability of your smart contracts, so you can focus on the growth and success of your project.

### P2PAuditKatana
The Auditkatana bot is an advanced AI-integrated tool that surpasses all other automated tools in the market, boasting an exceptional track record of zero false positives issues. Its primary objective is to assist in the identification of over 100 smart contract bugs and gas optimization issues. Additionally, the bot offers valuable suggestions to enhance the security of the code, ensuring its robustness and reliability. With its cutting-edge capabilities, the Auditkatana bot is revolutionizing the field of smart contract auditing, providing unparalleled accuracy and efficiency.

## Summary
This report has been prepared for Kunji Finance using P2PAuditkatana to scan and discover vulnerabilities and safe coding practices in their smart contract including the libraries used by the contract that are not officially recognized. The P2PAuditkatana tool runs a comprehensive static analysis on the Solidity code and finds vulnerabilities ranging from minor gas optimizations to major vulnerabilities leading to the loss of funds. The coverage scope pays attention to all the informational and critical vulnerabilities with over (100+) modules. The scanning and auditing process covers the following areas:

- Various common and uncommon attack vectors will be investigated to ensure that the smart contracts are secure from malicious actors.
- The scanner modules find and flag issues related to Gas optimizations that help in reducing the overall Gas cost.
- It scans and evaluates the codebase against industry best practices and standards to ensure compliance.
- It makes sure that the officially recognized libraries used in the code are secure and up to date.

The P2PAuditkatana Team recommends performing a manual audit to identify any vulnerabilities that are introduced after Kunji Finance introduces new features or refactors the code.

## Scope
The code under review can be found within the [Readme.md](https://www.p2paudit.xyz/reports/001-Kunji-Finance/Details), and is composed of multiple smart contract and interfaces written in the Solidity programming language and includes 1856 nSLOC- normalized source lines of code (only source-code lines; no comments, no blank lines).


# Severity classification
| Severity | Impact: High | Impact: Medium | Impact: Low |
| --- | --- | --- | --- |
| Likelihood: High | Critical | High | Medium |
| Likelihood: Medium | High | Medium | Low |
| Likelihood: Low | Medium | Low | Low |

<br />


| **Findings Summary** | **Issues** |
| --- | --- |
| **High:** | **0 issues** |
| **Medium:** | **2 issues** |
| **Low:** | **3 issues** |
| **Informational:** | **6 issues** |
| **Gas Optimizations:** | **6 issues** |

# Detailed Summary of Findings:

| Sl. No. | Name | Severity | Status |
| --- | --- | --- | --- |
| M-01 | <a href="#m-01-use-safetransfersafetransferfrom-consistently-instead-of-transfertransferfrom" >Use safeTransfer/safeTransferFrom consistently instead of transfer/transferFrom</a> | Medium | Fixed |
| M-02 | <a href="#m-02-precision-loss-in-calculation-for-vaultprofit" >Precision loss in Calculation for vaultProfit</a> | Medium | Fixed |
| L-01 | <a href="#l-01-use-safeapprove-instead-of-approve" >Use safeApprove() instead of approve()</a> | Low | Acknowledged |
| L-02 | <a href="#l-02-avoid-using-floating-pragma" >Avoid using Floating Pragma</a> | Low | Acknowledged |
| L-03 | <a href="#l-03-lack-of-2-step-ownership-transfer" >Lack of 2-step Ownership transfer</a> | Low | Acknowledged |
| I-01 | <a href="#i-01-open-todos" >Open TODOs</a> | Info | Acknowledged |
| I-02 | <a href="#i-02-unused-receive-function-will-lock-ether-in-contract" >Unused receive() function will lock Ether in contract</a> | Info | Acknowledged |
| I-03 | <a href="#i-03-use-a-more-recent-version-of-solidity" >Use a more recent version of solidity</a> | Info | Acknowledged |
| I-04 | <a href="#i-04-contracts-should-have-full-test-coverage" >Contracts should have full test coverage</a> | Info | Acknowledged |
| I-05 | <a href="#i-05-file-is-missing-natspec" >File is missing NatSpec</a> | Info | Acknowledged |
| I-06 | <a href="#i-06-variable-names-dont-follow-the-solidity-style-guide" >Variable names don't follow the Solidity style guide</a> | Info | Acknowledged |
| G-01 | <a href="#g-01-pre-increment-costs-less-gas-as-compared-to-post-increment" >Pre-increment costs less gas as compared to Post-increment</a> | Gas Optimization | Acknowledged |
| G-02 | <a href="#g-02-custom-errors-instead-of-revert-strings-to-save-gas" >Custom Errors instead of Revert Strings to save Gas</a> | Gas Optimization | Acknowledged |
| G-03 | <a href="#g-03-strict-inequalities--are-more-expensive-than-non-strict-ones-" >Strict inequalities (>) are more expensive than non-strict ones (>=)</a> | Gas Optimization | Acknowledged |
| G-04 | <a href="#g-04-variables-no-need-to-explicitly-initialize-variables-with-default-values" >Variables: No need to explicitly initialize variables with default values</a> | Gas Optimization | Acknowledged |
| G-05 | <a href="#g-05-using-private-rather-than-public-for-constants-saves-gas" >Using private rather than public for constants, saves gas</a> | Gas Optimization | Acknowledged |
| G-06 | <a href="#g-06-functions-guaranteed-to-revert-when-called-by-normal-users-can-be-markedpayable" >Functions guaranteed to revert when called by normal users can be marked payable</a> | Gas Optimization | Acknowledged |


# Medium Severity Findings:

## M-01. **Use safeTransfer/safeTransferFrom consistently instead of transfer/transferFrom**
It is good to add a require() statement that checks the return value of token transfers or to use something like OpenZeppelin’s safeTransfer/safeTransferFrom unless one is sure the given token reverts in case of a failure. Failure to do so will cause silent failures of transfers and affect token accounting in contract.

### Instances:
```solidity
contracts/UsersVault.sol:525:                IERC20Upgradeable(underlyingTokenAddress).transfer(
```

### Recommended Mitigation Steps:
Consider using safeTransfer/safeTransferFrom or require() consistently.

<br />

## M-02. Precision loss in Calculation for vaultProfit
In the case of performing Integer division, Solidity may truncate the result. Hence we must multiply before dividing to prevent such loss in precision

### Instances:
```solidity
contracts/UsersVault.sol:364:            vaultProfit = overallProfit - ((overallProfit / 100) * kunjiFee);
```

### Recommended Mitigation:
By doing all our multiplications first, we mitigate rounding related issues as much as possible.

### References:
https://soliditydeveloper.com/solidity-design-patterns-multiply-before-dividing

<br />

# Low Severity Findings

## L-01. Use safeApprove() instead of approve():
This is probably an oversight since SafeERC20 was imported and safeTransfer() was used for ERC20 token transfers. Nevertheless, note that approve() will fail for certain token implementations that do not return a boolean value (). Hence it is recommend to use safeApprove().

### Instances:
```solidity
contracts/UsersVault.sol:201:        if (!IERC20Upgradeable(_tokenAddress).approve(adapterAddress, amount)) {
contracts/TraderWallet.sol:309:        if (!IERC20Upgradeable(_tokenAddress).approve(adapterAddress, amount)) {
contracts/adapters/gmx/GMXAdapter.sol:524:            IERC20(token).approve(spender, amount);
contracts/adapters/uniswap/UniswapV3Adapter.sol:219:            IERC20(token).approve(spender, type(uint256).max);
```

## Recommended Mitigation Steps:
It is recommend to use safeApprove().

<br />

## L-02. Avoid using Floating Pragma
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

### Instances:
```solidity
contracts/CustomReentrancyGuard.sol:2:pragma solidity ^0.8.9;
contracts/interfaces/IPlatformAdapter.sol:2:pragma solidity ^0.8.9;
contracts/interfaces/IContractsFactory.sol:2:pragma solidity ^0.8.9;
contracts/interfaces/ITraderWallet.sol:2:pragma solidity ^0.8.9;
contracts/interfaces/IAdapter.sol:2:pragma solidity ^0.8.9;
contracts/interfaces/IUsersVault.sol:3:pragma solidity ^0.8.9;
contracts/interfaces/IAdaptersRegistry.sol:2:pragma solidity ^0.8.9;
contracts/factoryLibraries/TraderWalletDeployer.sol:2:pragma solidity ^0.8.9;
contracts/factoryLibraries/UsersVaultDeployer.sol:2:pragma solidity ^0.8.9;
contracts/UsersVault.sol:2:pragma solidity ^0.8.9;
contracts/TraderWallet.sol:2:pragma solidity ^0.8.9;
contracts/adapters/gmx/interfaces/IGmxReader.sol:2:pragma solidity ^0.8.9;
contracts/adapters/gmx/interfaces/IGmxRouter.sol:2:pragma solidity ^0.8.9;
contracts/adapters/gmx/interfaces/IGmxVault.sol:2:pragma solidity ^0.8.9;
contracts/adapters/gmx/interfaces/IGmxAdapter.sol:2:pragma solidity ^0.8.9;
contracts/adapters/gmx/interfaces/IGmxOrderBook.sol:2:pragma solidity ^0.8.9;
contracts/adapters/gmx/interfaces/IGmxPositionManager.sol:2:pragma solidity ^0.8.9;
contracts/adapters/gmx/interfaces/IVaultPriceFeed.sol:2:pragma solidity ^0.8.9;
contracts/adapters/gmx/GMXAdapter.sol:2:pragma solidity ^0.8.9;
contracts/adapters/uniswap/UniswapV3Adapter.sol:3:pragma solidity ^0.8.9;
contracts/adapters/uniswap/interfaces/IUniswapV3Factory.sol:2:pragma solidity ^0.8.9;
contracts/adapters/uniswap/interfaces/IQuoterV2.sol:2:pragma solidity ^0.8.9;
contracts/adapters/uniswap/interfaces/IUniswapV3Adapter.sol:2:pragma solidity ^0.8.9;
contracts/adapters/uniswap/interfaces/IUniswapV3Pool.sol:2:pragma solidity ^0.8.9;
contracts/adapters/uniswap/interfaces/INonfungiblePositionManager.sol:2:pragma solidity ^0.8.9;
contracts/adapters/uniswap/interfaces/IUniswapV3Router.sol:2:pragma solidity ^0.8.9;
contracts/adapters/Lens.sol:3:pragma solidity ^0.8.9;
contracts/ContractsFactory.sol:2:pragma solidity ^0.8.9;
```

### Instances:
Recommend using fixed solidity version

<br />

## L-03. Lack of 2-step Ownership transfer
The current ownership transfer process on TraderWallet, UsersVaultV2, and UsersVault contract involves the current owner calling `transferOwnership()` function. This function checks that the new owner is not the zero address and proceeds to write the new owner’s address into the owner’s state variable. If the nominated EOA account is not a valid account, it is entirely possible the owner may accidentally transfer ownership to an uncontrolled account, breaking all functions with the `onlyOwner()` modifier.

### Instances:
```solidity
contracts/mocks/UsersVaultV2.sol:161:        transferOwnership(_ownerAddress);
contracts/TraderWallet.sol:129:        transferOwnership(_ownerAddress);
contracts/UsersVault.sol:135:        transferOwnership(_ownerAddress);
```

### Recommended Mitigation Steps:
Recommend considering implementing a two step process where the owner nominates an account and the nominated account needs to call an `acceptOwnership()` function for the transfer of ownership to fully succeed. This ensures the nominated EOA account is a valid and active account.

<br />

# Informational Findings:

## I-01. Open TODOs
Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment

### Instances:
```solidity
contracts/TraderWallet.sol:378:    // @todo rename '_traderOperation' to '_tradeOperation'
contracts/adapters/gmx/interfaces/IGmxAdapter.sol:63:    *// todo*
contracts/adapters/gmx/GMXAdapter.sol:117:            amountIn = amountIn * ratio / ratioDenominator;   *// @todo replace with safe mulDiv*
contracts/adapters/uniswap/UniswapV3Adapter.sol:93:            // @todo consider using 'slippage' as parameter of the _buy (or _sell) function
contracts/adapters/uniswap/UniswapV3Adapter.sol:97:            // @todo complex slippage regulation

```

<br />

## I-02. Unused receive() function will lock Ether in contract
If the intention is for the Ether to be used, the function should call another function, otherwise, it should revert.

### Instances:
```solidity
contracts/UsersVault.sol:152:    receive() external payable {}
contracts/TraderWallet.sol:152:    receive() external payable {}
```

### Reference:
https://code4rena.com/reports/2022-05-sturdy#l-06-unused-receive-function-will-lock-ether-in-contract

<br />

## I-03. Use a more recent version of solidity
When deploying contracts, you should use the latest released version of Solidity. Apart from exceptional cases, only the latest version receives security [fixes](https://github.com/ethereum/solidity/security/policy#supported-versions). Furthermore, breaking changes, as well as new features, are introduced regularly.

### Instances:
```solidity
contracts/CustomReentrancyGuard.sol:2:pragma solidity ^0.8.9;
contracts/interfaces/IPlatformAdapter.sol:2:pragma solidity ^0.8.9;
contracts/interfaces/IContractsFactory.sol:2:pragma solidity ^0.8.9;
contracts/interfaces/ITraderWallet.sol:2:pragma solidity ^0.8.9;
contracts/interfaces/IAdapter.sol:2:pragma solidity ^0.8.9;
contracts/interfaces/IUsersVault.sol:3:pragma solidity ^0.8.9;
contracts/interfaces/IAdaptersRegistry.sol:2:pragma solidity ^0.8.9;
contracts/factoryLibraries/TraderWalletDeployer.sol:2:pragma solidity ^0.8.9;
contracts/factoryLibraries/UsersVaultDeployer.sol:2:pragma solidity ^0.8.9;
contracts/UsersVault.sol:2:pragma solidity ^0.8.9;
contracts/TraderWallet.sol:2:pragma solidity ^0.8.9;
contracts/adapters/gmx/interfaces/IGmxReader.sol:2:pragma solidity ^0.8.9;
contracts/adapters/gmx/interfaces/IGmxRouter.sol:2:pragma solidity ^0.8.9;
contracts/adapters/gmx/interfaces/IGmxVault.sol:2:pragma solidity ^0.8.9;
contracts/adapters/gmx/interfaces/IGmxAdapter.sol:2:pragma solidity ^0.8.9;
contracts/adapters/gmx/interfaces/IGmxOrderBook.sol:2:pragma solidity ^0.8.9;
contracts/adapters/gmx/interfaces/IGmxPositionManager.sol:2:pragma solidity ^0.8.9;
contracts/adapters/gmx/interfaces/IVaultPriceFeed.sol:2:pragma solidity ^0.8.9;
contracts/adapters/gmx/GMXAdapter.sol:2:pragma solidity ^0.8.9;
contracts/adapters/uniswap/UniswapV3Adapter.sol:3:pragma solidity ^0.8.9;
contracts/adapters/uniswap/libraries/BytesLib.sol:9:pragma solidity >=0.8.0 <0.9.0;
contracts/adapters/uniswap/interfaces/IUniswapV3Factory.sol:2:pragma solidity ^0.8.9;
contracts/adapters/uniswap/interfaces/IQuoterV2.sol:2:pragma solidity ^0.8.9;
contracts/adapters/uniswap/interfaces/IUniswapV3Adapter.sol:2:pragma solidity ^0.8.9;
contracts/adapters/uniswap/interfaces/IUniswapV3Pool.sol:2:pragma solidity ^0.8.9;
contracts/adapters/uniswap/interfaces/INonfungiblePositionManager.sol:2:pragma solidity ^0.8.9;
contracts/adapters/uniswap/interfaces/IUniswapV3Router.sol:2:pragma solidity ^0.8.9;
contracts/adapters/Lens.sol:3:pragma solidity ^0.8.9;
contracts/ContractsFactory.sol:2:pragma solidity ^0.8.9;

```

## References:
https://code4rena.com/reports/2022-06-notional-coop/#10-use-a-more-recent-version-of-solidity

<br />

## I-04. Contracts should have full test coverage
While 100% code coverage does not guarantee that there are no bugs, it often will catch easy-to-find bugs, and will ensure that there are fewer regressions when the code invariably has to be modified. Furthermore, in order to get full coverage, code authors will often have to re-organize their code so that it is more modular, so that each component can be tested separately, which reduces interdependencies between modules and layers, and makes for code that is easier to reason about and audit. Contracts should have 90%+ code coverage.

### References:
https://gist.github.com/CloudEllie/6639dbfd7dc1809a3baa28bb2895e1d9#n31-contracts-should-have-full-test-coverage

<br />

## I-05. File is missing NatSpec
**NatSpec** is a documentation format created by the solidity team which was inspired by [Doxygen](https://en.wikipedia.org/wiki/Doxygen). it helps you describe the intent behind the code of your solidity smart contracts which could be usefully for the author themselves and other people who might be interested in your code like other developers, users and auditors. **NatSpec** does not only help describe the intent of your code, it also tells the readers how to interact with it too.

*There are multiple instances of this issue in the contracts:*

### References:
https://docs.soliditylang.org/en/v0.8.17/natspec-format.html

<br />

## I-06. Variable names don't follow the Solidity style guide
For `constant` variable names, each word should use all capital letters, with underscores separating each word (CONSTANT_CASE). But there are multiple instances in the contract where this pattern isn’t followed.

### Instances:
```solidity
contracts/adapters/gmx/GMXAdapter.sol:25:    address constant public gmxRouter = 0xaBBc5F99639c9B6bCb58544ddf04EFA6802F4064;
contracts/adapters/gmx/GMXAdapter.sol:26:    address constant public gmxPositionRouter = 0xb87a436B93fFE9D75c5cFA7bAcFff96430b09868;
contracts/adapters/gmx/GMXAdapter.sol:27:    IGmxVault constant public gmxVault = IGmxVault(0x489ee077994B6658eAfA855C308275EAd8097C4A);
contracts/adapters/gmx/GMXAdapter.sol:28:    address constant public gmxOrderBook = 0x09f77E8A13De9a35a7231028187e9fD5DB8a2ACB;
contracts/adapters/gmx/GMXAdapter.sol:29:    address constant public gmxOrderBookReader = 0xa27C20A7CF0e1C68C0460706bB674f98F362Bc21;
contracts/adapters/gmx/GMXAdapter.sol:30:    address constant public gmxReader = 0x22199a49A999c351eF7927602CFB187ec3cae489;
contracts/adapters/gmx/GMXAdapter.sol:32:    uint256 constant public ratioDenominator = 1e18;
contracts/adapters/uniswap/UniswapV3Adapter.sol:25:    IUniswapV3Router constant public uniswapV3Router = IUniswapV3Router(0xE592427A0AEce92De3Edee1F18E0157C05861564);
contracts/adapters/uniswap/UniswapV3Adapter.sol:26:    IQuoterV2 constant public quoter = IQuoterV2(0x61fFE014bA17989E743c5F6cB21bF9697530B21e);
contracts/adapters/uniswap/UniswapV3Adapter.sol:28:    uint256 constant public ratioDenominator = 1e18;
contracts/adapters/uniswap/UniswapV3Adapter.sol:31:    uint128 constant public slippageAllowanceMax = 3e17;  // 30%
contracts/adapters/uniswap/UniswapV3Adapter.sol:34:    uint128 constant public slippageAllowanceMin = 1e15;  // 0.1%
contracts/adapters/Lens.sol:23:    IQuoterV2 constant public quoter = IQuoterV2(0x61fFE014bA17989E743c5F6cB21bF9697530B21e);
contracts/adapters/Lens.sol:26:    IGmxPositionRouter constant public gmxPositionRouter = IGmxPositionRouter(0xb87a436B93fFE9D75c5cFA7bAcFff96430b09868);
contracts/adapters/Lens.sol:27:    IGmxOrderBookReader constant public gmxOrderBookReader = IGmxOrderBookReader(0xa27C20A7CF0e1C68C0460706bB674f98F362Bc21);
contracts/adapters/Lens.sol:28:    IGmxVault constant public gmxVault = IGmxVault(0x489ee077994B6658eAfA855C308275EAd8097C4A);
contracts/adapters/Lens.sol:29:    address constant public gmxOrderBook = 0x09f77E8A13De9a35a7231028187e9fD5DB8a2ACB;
contracts/adapters/Lens.sol:30:    address constant public gmxReader = 0x22199a49A999c351eF7927602CFB187ec3cae489;

```

### Recommendation:
Consider using all capital letters, with underscores separating each word for constants.

<br />

# Gas Optimization Findings:

## G-01. Pre-increment costs less gas as compared to Post-increment:
++i costs less gas as compared to i++ for unsigned integer, as per-increment is cheaper(its about 5 gas per iteration cheaper)

i++ increments i and returns initial value of i. Which means

`uint i =  1; i++; // ==1 but i ==2`

But ++i returns the actual incremented value:

`uint i = 1; ++i; *// ==2 and i ==2 , no need for temporary variable here*`

In the first case, the compiler has create a temporary variable (when used) for returning 1 instead of 2.

### Instances:
```solidity
contracts/UsersVault.sol:380:        currentRound++;
contracts/TraderWallet.sol:502:            for (i = 0; i < traderSelectedAdaptersArray.length; i++) {

```

### Recommendations:
Change post-increment to pre-increment.

<br />

## G-02. Custom Errors instead of Revert Strings to save Gas
Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met). Starting from Solidity v0.8.4,there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., revert("Insufficient funds.");),but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Custom errors are defined using the error statement, which can be used inside and outside of contracts (including interfaces and libraries).

### Instances:
```solidity
contracts/CustomReentrancyGuard.sol:62:        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");
contracts/adapters/uniswap/libraries/BytesLib.sol:22:        require(_length + 31 >= _length, "slice_overflow");
contracts/adapters/uniswap/libraries/BytesLib.sol:23:        require(_bytes.length >= _start + _length, "slice_outOfBounds");
contracts/adapters/uniswap/libraries/BytesLib.sol:83:        require(_bytes.length >= _start + 20, "toAddress_outOfBounds");
contracts/adapters/uniswap/libraries/BytesLib.sol:94:        require(_bytes.length >= _start + 3, "toUint24_outOfBounds");

```

### Recommendations:
We suggest replacing revert strings with custom errors.

<br />

## G-03. Strict inequalities (>) are more expensive than non-strict ones (>=)
Strict inequalities (>) are more expensive than non-strict ones (>=). This is due to some supplementary checks (ISZERO, 3 gas. I suggest using >= instead of > to avoid some opcodes here:

### Instances:
```solidity
contracts/UsersVault.sol:236:            userDeposits[_msgSender()].pendingAssets > 0
contracts/UsersVault.sol:273:            userWithdrawals[_msgSender()].pendingShares > 0
contracts/UsersVault.sol:430:            userDeposits[_receiver].pendingAssets > 0
contracts/UsersVault.sol:444:            userWithdrawals[_receiver].pendingShares > 0
contracts/UsersVault.sol:462:            userDeposits[_msgSender()].pendingAssets > 0
contracts/UsersVault.sol:498:            userWithdrawals[_msgSender()].pendingShares > 0
contracts/TraderWallet.sol:476:            initialTraderBalance > 0
contracts/adapters/uniswap/UniswapV3Adapter.sol:228:            _slippage > slippageAllowanceMax) revert InvalidSlippage();
```
<br />

## G-04. Variables: No need to explicitly initialize variables with default values
If a variable is not set/initialized, it is assumed to have the default value (0 for uint, false for bool, address(0) for address…). Explicitly initializing it with its default value is an anti-pattern and wastes gas.

We can use `uint number;` instead of `uint number = 0;`

### Instances:

```solidity
contracts/TraderWallet.sol:500:        uint256 i = 0;
contracts/UsersVault.sol:394:        bool success = false;
contracts/TraderWallet.sol:392:        bool success = false;
contracts/TraderWallet.sol:499:        bool found = false;

```

### Recommendations:
I suggest removing explicit initializations for default values.

<br />

## G-05. Using private rather than public for constants, saves gas
If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that [returns a tuple](https://github.com/code-423n4/2022-08-frax/blob/90f55a9ce4e25bceed3a74290b854341d8de6afa/src/contracts/FraxlendPair.sol#L156-L178) of the values of all currently-public constants. Saves **3406-3606 gas** in deployment gas due to the compiler not having to create non payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

### Instances:
```solidity
contracts/adapters/gmx/GMXAdapter.sol:25:    address constant public gmxRouter = 0xaBBc5F99639c9B6bCb58544ddf04EFA6802F4064;
contracts/adapters/gmx/GMXAdapter.sol:26:    address constant public gmxPositionRouter = 0xb87a436B93fFE9D75c5cFA7bAcFff96430b09868;
contracts/adapters/gmx/GMXAdapter.sol:27:    IGmxVault constant public gmxVault = IGmxVault(0x489ee077994B6658eAfA855C308275EAd8097C4A);
contracts/adapters/gmx/GMXAdapter.sol:28:    address constant public gmxOrderBook = 0x09f77E8A13De9a35a7231028187e9fD5DB8a2ACB;
contracts/adapters/gmx/GMXAdapter.sol:29:    address constant public gmxOrderBookReader = 0xa27C20A7CF0e1C68C0460706bB674f98F362Bc21;
contracts/adapters/gmx/GMXAdapter.sol:30:    address constant public gmxReader = 0x22199a49A999c351eF7927602CFB187ec3cae489;
contracts/adapters/gmx/GMXAdapter.sol:32:    uint256 constant public ratioDenominator = 1e18;
contracts/adapters/uniswap/UniswapV3Adapter.sol:25:    IUniswapV3Router constant public uniswapV3Router = IUniswapV3Router(0xE592427A0AEce92De3Edee1F18E0157C05861564);
contracts/adapters/uniswap/UniswapV3Adapter.sol:26:    IQuoterV2 constant public quoter = IQuoterV2(0x61fFE014bA17989E743c5F6cB21bF9697530B21e);
contracts/adapters/uniswap/UniswapV3Adapter.sol:28:    uint256 constant public ratioDenominator = 1e18;
contracts/adapters/uniswap/UniswapV3Adapter.sol:31:    uint128 constant public slippageAllowanceMax = 3e17;  // 30%
contracts/adapters/uniswap/UniswapV3Adapter.sol:34:    uint128 constant public slippageAllowanceMin = 1e15;  // 0.1%
contracts/adapters/Lens.sol:23:    IQuoterV2 constant public quoter = IQuoterV2(0x61fFE014bA17989E743c5F6cB21bF9697530B21e);
contracts/adapters/Lens.sol:26:    IGmxPositionRouter constant public gmxPositionRouter = IGmxPositionRouter(0xb87a436B93fFE9D75c5cFA7bAcFff96430b09868);
contracts/adapters/Lens.sol:27:    IGmxOrderBookReader constant public gmxOrderBookReader = IGmxOrderBookReader(0xa27C20A7CF0e1C68C0460706bB674f98F362Bc21);
contracts/adapters/Lens.sol:28:    IGmxVault constant public gmxVault = IGmxVault(0x489ee077994B6658eAfA855C308275EAd8097C4A);
contracts/adapters/Lens.sol:29:    address constant public gmxOrderBook = 0x09f77E8A13De9a35a7231028187e9fD5DB8a2ACB;
contracts/adapters/Lens.sol:30:    address constant public gmxReader = 0x22199a49A999c351eF7927602CFB187ec3cae489;
```

### Reference:
https://gist.github.com/CloudEllie/6639dbfd7dc1809a3baa28bb2895e1d9#g23-functions-guaranteed-to-revert-when-called-by-normal-users-can-be-marked-payable

<br />

## G-06. Functions guaranteed to revert when called by normal users can be marked payable
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.

### Instances:
```solidity
contracts/ContractsFactory.sol:
   67:     function addInvestor(address _investorAddress) external onlyOwner {

   73:     function removeInvestor(address _investorAddress) external onlyOwner {

   82:     function addTrader(address _traderAddress) external onlyOwner {

   88:     function removeTrader(address _traderAddress) external onlyOwner {

	 97:      function setAdaptersRegistryAddress(
   98          address _adaptersRegistryAddress
   99:     ) external onlyOwner {

	105:     function setFeeRate(uint256 _newFeeRate) external onlyOwner {

contracts/TraderWallet.sol:
	156      function setVaultAddress(
  157          address _vaultAddress
  158:     ) external onlyOwner notZeroAddress(_vaultAddress, "_vaultAddress") {

	212      function setTraderAddress(
  213          address _traderAddress
  214:     ) external onlyOwner notZeroAddress(_traderAddress, "_traderAddress") {
```

There are multiple instances of this issue in the contracts.

<br />

# Disclaimer:
A smart contract security review, as a comprehensive evaluation process, cannot provide absolute assurance regarding the absence of vulnerabilities. It necessitates a meticulous allocation of time, resources, and expertise to identify as many potential vulnerabilities as possible. Despite our diligent efforts, we, as a team, cannot guarantee 100% security after conducting the review, nor can we assure the discovery of all possible issues within your smart contracts. Consequently, we highly recommend implementing subsequent security reviews, bug bounty programs, and on-chain monitoring to reinforce the overall security posture of your smart contracts. By adopting these proactive measures, you can enhance the robustness and resilience of your smart contract ecosystem.
