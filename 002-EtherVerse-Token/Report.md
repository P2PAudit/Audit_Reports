# Overview
## About P2PAudits
 P2P Audit, we source top talent from across the web3 ecosystem to put together the best team possible for your project, unlike traditional auditing organizations that employ teams of full-time security experts.

We understand that your project may have unique requirements and timelines, and we are flexible in accommodating your needs. Our goal is to work with you to ensure the security and reliability of your smart contracts, so you can focus on the growth and success of your project.

## Scope
The code under review can be found within the [Readme.md](https://www.p2paudit.xyz/reports/002-EtherVerse-Token/Details), and is composed of multiple smart contract and interfaces written in the Solidity programming language and includes 149 nSLOC- normalized source lines of code (only source-code lines; no comments, no blank lines).


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
| **Medium:** | **1 issues** |
| **Low:** | **3 issues** |
| **Informational:** | **2 issues** |
| **Gas Optimizations:** | **4 issues** |

# Detailed Summary of Findings:

| Sl. No. | Name | Severity |
| --- | --- | --- |
| M-01 | <a href="#" >Transfer ETH by using transfer() may cause this transaction to fail </a>| Medium |
| L-01 | <a href="#" >Avoiding the use of floating Pragma.</a> | Low |
| L-02 | <a href="#" >Lack of 2-step transfer of ownership</a> | Low |
| L-03 | <a href="#" >Lack of 0-address check.</a> | Low |
| I-01 | <a href="#" >Use the latest version of solidity</a> | Informational |
| I-02 | <a href="#" >Contracts should have full test coverage</a> | Informational |
| G-01 | <a href="#" >Using > 0 costs more gas than != 0 when used on a uint in a require() statement</a> | Gas Optimization |
| G-02 | <a href="#" >Splitting require() statements that use && saves gas</a> | Gas Optimization |
| G-03 | <a href="#" >Custom Errors instead of Revert Strings to save Gas</a> | Gas Optimization |
| G-04 | <a href="#" >Strict inequalities (>) are more expensive than non-strict ones (>=)</a> | Gas Optimization |

<br />

# High Severity Issue:
**No high-severity issues were found.**

<br />

# Medium Severity Findings:

## M-01. **Transfer ETH by using transfer() may cause this transaction to fail**
Transferring ETH by using transfer() may cause this transaction to fail. Due to the fact that .transfer() and .send() forward exactly 2,300 gas to the recipient. This hardcoded gas stipend aimed to prevent reentrancy vulnerabilities, but this only makes sense under the assumption that gas costs are constant. Recently EIP 1884 was included in the Istanbul hard fork. One of the changes included in EIP 1884 is an increase in the gas cost of the SLOAD operation, causing a contract's fallback function to cost more than 2300 gas.

### Instances:
File: MDUToken.sol:301:
```solidity
function Investingtoken() payable public returns(bool success) {
			     uint256 tokens =  msg.value / tokenPrice;
          _balances[_msgSender()] += tokens;
          _balances[_creator] -= tokens;
          recipient.transfer(msg.value); //@audit Use call for transferring eth instead of transfer.
          receivedfund += msg.value;
          return true;

      }
```

### Recommended Mitigation Steps:
Use `call{value: msg.value}()` instead of `transfer` to send Ether.

```solidity
(bool success, ) = payable(recipient).call{value: msg.value}("");
require(success, "call failed");
```

<br />


# Low Severity Findings:

## L-01. **Avoiding the use of floating Pragma.**
Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

### Instances:
File: MDUToken.sol:3:

```solidity
pragma solidity ^0.8.15;
```

### Recommended Mitigation Steps:
Avoid Floating pragma and use fixed solidity version

<br />

## L-02. **Lack of 2-step transfer of ownership**
Ownable2Step is safer than Ownable for smart contracts because the owner cannot accidentally transfer smart contract ownership to a mistyped address. Rather than directly transferring to the new owner, the transfer only completes when the new owner accepts ownership. Also, If the nominated EOA account is not a valid account, it is entirely possible the owner may accidentally transfer ownership to an uncontrolled account, breaking all functions with the onlyOwner() modifier.

### Instances:
File: MDUToken.sol:81:
```solidity
function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        _transferOwnership(newOwner);
 }

function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
```

### Recommended Mitigation Steps:
Recommend considering implementing a two step process where the owner nominates an account and the nominated account needs to call an acceptOwnership() function for the transfer of ownership to fully succeed. This ensures the nominated EOA account is a valid and active account.

<br />

## L-03. **Lack of 0-address check.**
Checking addresses against zero-address during initialization or during setting is a security best practice. However, such checks are missing in the constructor of the Presale contract during recipient address initializations.

### Instances:
File: MDUToken.sol:288:

```solidity
constructor(address payable _recipient)  {
          admin = _msgSender();
          recipient = _recipient; //@audit No check for Zero-address for recipient.
      }
```

### Recommended Mitigation Steps:
Recommend adding zero-address checks for all initializations of address state variables.

<br />

# Informational Findings:

## I-01. **Use the latest version of solidity**
When deploying contracts, you should use the latest released version of Solidity. Apart from exceptional cases, only the latest version receives security fixes. Furthermore, breaking changes, as well as new features, are introduced regularly.

### Instances:
File: MDUToken.sol:3:
```solidity
pragma solidity ^0.8.15;
```

### Recommended Mitigation Steps:
Use the latest version of solidity i.e. 0.8.19 or 0.8.20.

<br />

## I-02. **Contracts should have full test coverage**
While 100% code coverage does not guarantee that there are no bugs, it often will catch easy-to-find bugs, and will ensure that there are fewer regressions when the code invariably has to be modified. Furthermore, in order to get full coverage, code authors will often have to re-organize their code so that it is more modular, so that each component can be tested separately, which reduces interdependencies between modules and layers, and makes for code that is easier to reason about and audit. Contracts should have 90%+ code coverage.

### References:
https://gist.github.com/CloudEllie/6639dbfd7dc1809a3baa28bb2895e1d9#n31-contracts-should-have-full-test-coverage

<br />


# Gas Optimization Findings:

## G-01. **Using > 0 costs more gas than != 0 when used on a uint in a require() statement**
0 is less efficient than != 0 for unsigned integers (with proof) != 0 costs less gas compared to > 0 for unsigned integers in require statements with the optimizer enabled (6 gas) Proof: While it may seem that > 0 is cheaper than !=, this is only true without the optimizer enabled and outside a require statement. If you enable the optimizer at 10k AND you’re in a require statement, this will save gas.

### Instances:
```solidity
MDUToken.sol:206:        require(_value > 0 && _balances[_msgSender()] >= _value);
MDUToken.sol:219:        require(_value > 0 && _balances[_msgSender()] >= _value);
```

### Recommended Mitigation Steps:
I suggest changing > 0 with != 0. Also, please enable the Optimizer.

<br />

## G-02. **Splitting require() statements that use && saves gas**
Require statements including conditions with the && operator can be broken down in multiple require statements to save gas.

### Instances:
```solidity
MDUToken.sol:206:        require(_value > 0 && _balances[_msgSender()] >= _value);
MDUToken.sol:219:        require(_value > 0 && _balances[_msgSender()] >= _value);
```

### Recommended Mitigation Steps:
Breakdown each condition in a separate require statement (though require statements should be replaced with custom errors)

<br />

## G-03. **Custom Errors instead of Revert Strings to save Gas**
Custom errors from Solidity 0.8.4 are cheaper than revert strings (cheaper deployment cost and runtime cost when the revert condition is met). Starting from Solidity v0.8.4,there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., revert("Insufficient funds.");),but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them. Custom errors are defined using the error statement, which can be used inside and outside of contracts (including interfaces and libraries).

### Instances:
```solidity
MDUToken.sol:51:        require(owner() == _msgSender(), "Ownable: caller is not the owner");
```

### Recommended Mitigation Steps:
I suggest replacing revert strings with custom errors.

<br />

## G-04. **Strict inequalities (>) are more expensive than non-strict ones (>=)**
Strict inequalities (>) are more expensive than non-strict ones (>=). This is due to some supplementary checks (ISZERO, 3 gas). I suggest using >= instead of > to avoid some opcodes here:

### Instances:
```solidity
MDUToken.sol:234:            _value > 0 &&
```

### Recommended Mitigation Steps:
Change the code to 
```solidity
 // From:
 _value > 0 &&

 // To:
 _value >=1 &&
```

<br />

# Disclaimer:
A smart contract security review, as a comprehensive evaluation process, cannot provide absolute assurance regarding the absence of vulnerabilities. It necessitates a meticulous allocation of time, resources, and expertise to identify as many potential vulnerabilities as possible. Despite our diligent efforts, we, as a team, cannot guarantee 100% security after conducting the review, nor can we assure the discovery of all possible issues within your smart contracts. Consequently, we highly recommend implementing subsequent security reviews, bug bounty programs, and on-chain monitoring to reinforce the overall security posture of your smart contracts. By adopting these proactive measures, you can enhance the robustness and resilience of your smart contract ecosystem.
