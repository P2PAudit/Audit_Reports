## About Kunji Finance 
Kunji Finance is a decentralized crypto asset management platform that offers three investment strategies for users to choose from. The platform is currently integrated with the Uniswap and GMX decentralized exchanges, which allows users to easily access a wide range of crypto assets. Kunji Finance is a non-custodial platform, which means that users retain full control of their funds at all times. This makes Kunji Finance a secure and reliable option for crypto investors of all levels of experience.

## In-Scope Contracts:

| File  | Lines  | nLines  | nSLOC  | Complex. Score  |
| --- | --- | --- | --- | --- |
| contracts/interfaces/IContractsFactory.sol  | 14  | 5  | 3  | 11  |
| contracts/interfaces/IPlatformAdapter.sol  | 18  | 13  | 9  | 5  |
| contracts/interfaces/IAdaptersRegistry.sol  | 6  | 5  | 3  | 3  |
| contracts/interfaces/IAdapter.sol  | 31  | 30  | 13  | 3  |
| contracts/interfaces/ITraderWallet.sol  | 56  | 7  | 4  | 47  |
| contracts/interfaces/IUsersVault.sol  | 57  | 8  | 4  | 41  |
| contracts/UsersVault.sol  | 611  | 577  | 441  | 269  |
| contracts/factoryLibraries/UsersVaultDeployer.sol  | 54  | 45  | 37  | 32  |
| contracts/factoryLibraries/TraderWalletDeployer.sol  | 48  | 41  | 34  | 32  |
| contracts/CustomReentrancyGuard.sol  | 73  | 73  | 25  | 12  |
| contracts/TraderWallet.sol  | 534  | 478  | 369  | 245  |
| contracts/ContractsFactory.sol  | 208  | 187  | 147  | 95  |
| contracts/adapters/uniswap/UniswapV3Adapter.sol  | 233  | 210  | 117  | 80  |
| contracts/adapters/Lens.sol  | 229  | 201  | 94  | 89  |
| contracts/adapters/uniswap/interfaces/IUniswapV3Adapter.sol  | 39  | 7  | 3  | 9  |
| contracts/adapters/uniswap/interfaces/INonfungiblePositionManager.sol  | 163  | 115  | 40  | 28  |
| contracts/adapters/uniswap/interfaces/IUniswapV3Pool.sol  | 215  | 15  | 4  | 37  |
| contracts/adapters/uniswap/interfaces/IQuoterV2.sol  | 92  | 50  | 17  | 9  |
| contracts/adapters/uniswap/interfaces/IUniswapV3Router.sol  | 64  | 56  | 37  | 21  |
| contracts/adapters/uniswap/interfaces/IUniswapV3Factory.sol  | 78  | 34  | 12  | 13  |
| contracts/adapters/gmx/interfaces/IGmxAdapter.sol  | 85  | 20  | 7  | 21  |
| contracts/adapters/gmx/interfaces/IGmxRouter.sol  | 139  | 42  | 33  | 38  |
| contracts/adapters/gmx/interfaces/IVaultPriceFeed.sol  | 26  | 5  | 3  | 33  |
| contracts/adapters/gmx/interfaces/IGmxOrderBook.sol  | 139  | 32  | 27  | 49  |
| contracts/adapters/gmx/interfaces/IGmxVault.sol  | 38  | 6  | 3  | 11  |
| contracts/adapters/gmx/interfaces/IGmxReader.sol  | 30  | 5  | 3  | 9  |
| contracts/adapters/gmx/interfaces/IGmxPositionManager.sol  | 17  | 6  | 3  | 5  |
| contracts/adapters/gmx/GMXAdapter.sol  | 539  | 508  | 315  | 129  |
| contracts/adapters/uniswap/libraries/BytesLib.sol  | 103  | 95  | 49  | 137  |
| Totals | 3939 | 2876 | 1856 | 1513 |

---

### Scope Details:

| Lines | 3939 |
| --- | --- |
| nLines | 2876 |
| nLOC | 1856 |

**Complexity Score:** 1513

- **Lines**: total lines of the source unit
- **nLines**: normalized lines of the source unit (e.g. normalizes functions spanning multiple lines)
- **nSLOC**: normalized source lines of code (only source-code lines; no comments, no blank lines)
- **Complexity Score**: a custom complexity score derived from code statements that are known to introduce code complexity (branches, loops, calls, external interfaces, ...)

---

### Contracts Excluded from Scope:

Files:

| contracts/mocks/UsersVaultMock.sol |
| --- |
| contracts/mocks/AdapterMock.sol |
| contracts/mocks/ERC20Mock.sol |
| contracts/mocks/TraderWalletV2.sol |
| contracts/mocks/UsersVaultV2.sol |
| contracts/mocks/ContractsFactoryMock.sol |
| contracts/mocks/AdaptersRegistryMock.sol |
| contracts/mocks/ERC20CustomInherits.sol |
| contracts/mocks/GmxVaultPriceFeedMock.sol |
| contracts/mocks/TraderWalletMock.sol |

---

****Dependencies / External Imports****

| Dependency / Import Path  | Count  |
| --- | --- |
| @openzeppelin/contracts-upgradeable/access/OwnableUpgradeable.sol  | 5  |
| @openzeppelin/contracts-upgradeable/interfaces/IERC20Upgradeable.sol  | 5  |
| @openzeppelin/contracts-upgradeable/proxy/utils/Initializable.sol  | 1  |
| @openzeppelin/contracts-upgradeable/security/ReentrancyGuardUpgradeable.sol  | 1  |
| @openzeppelin/contracts-upgradeable/token/ERC20/ERC20Upgradeable.sol  | 1  |
| @openzeppelin/contracts-upgradeable/token/ERC20/utils/SafeERC20Upgradeable.sol  | 1  |
| @openzeppelin/contracts/proxy/transparent/TransparentUpgradeableProxy.sol  | 2 |