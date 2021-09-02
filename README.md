# security-checklist

**Security checklist for smart contracts written in Solidity.** Modified from BoringCrypto's [BentoBox checklist](https://github.com/sushiswap/bentobox/blob/master/documentation/checks.txt).

## Variables

- `V1` - Can it be private?
- `V2` - Can it be constant?
- `V3` - Can it be immutable/constant?
- `V4` - Is visibility set (SWC-108)

## Structs

- `S1` - Are the members split on 256 boundaries?
- `S2` - Can any members be a smaller type?

## Functions

- `F1` - Set visibility: Change external to public to support batching. Should it be private? (SWC-100)
- `F2` - Should it be payable?
- `F3` - Can it be combined with another similar function?
- `F4` - Check behavior for all function arguments when wrong or extreme
- `F5` - Checks-Effects-Interactions pattern followed? (SWC-107)
- `F6` - Check for front-running possibilities, such as the approve function (SWC-114)
- `F7` - Avoid Insufficient gas grieving (SWC-126)
- `F8` - Are the correct modifiers applied, such as onlyOwner?
- `F9` - Return arguments are always assigned?

## Modifiers

- `M1` - No storage/memory changes (except for a reentrancy lock)
- `M2` - No external calls.
- `M4` - Are any unbounded loops/arrays used that can cause DoS? (SWC-128)
- `M5` - Check behavior for all function arguments when wrong or extreme.

## Code

- `C1` - All math done via SafeMath? (SWC-101)
- `C2` - Are any storage slots read multiple times?
- `C3` - Are any unbounded loops/arrays used that can cause DoS? (SWC-128)
- `C4` - Use block.timestamp only for long intervals (SWC-116)
- `C5` - Don't use block.number for elapsed time (SWC-116)
- `C6` - Don't use assert, tx.origin, address.transfer(), address.send() (SWC-115 SWC-134 SWC-110)
- `C7` - delegatecall only used within the same contract, never external even if trusted (SWC-112)
- `C8` - Don't use function types.
- `C9` - Don't use blockhash, etc for randomness (SWC-120)
- `C10` - Protect signatures against replay, use nonce and chainId (SWC-121)
- `C11` - All signatures strictly EIP-712 (SWC-117 SWC-122)
- `C12` - Output of abi.encodePacked shouldn't be hashed if using two or more dynamic types (SWC-133)
- `C13` - Careful with assembly, don't allow any arbitrary use data (SWC-127)
- `C14` - Don't assume a specific ETH balance (and token) (SWC-132)
- `C15` - Avoid insufficient gas grieving (SWC-126)?
- `C16` - Private data ISN'T private (SWC-136)
- `C17` - Don't use deprecated functions, they should turn red anyway (SWC-111)
  (suicide, block.blockhash, sha3, callcode, throw, msg.gas, constant for view, var)
- `C18` - Never shadow state variables (SWC-119)
- `C19` - No unused variables (SWC-131)
- `C20` - Is calculation on the fly cheaper than storing the value?
- `C21` - Are all state variables read from the correct contract: master vs. clone?
- `C22` - Is > or < or >= or <= correct?
- `C23` - Are logical operators correct ==, !=, &&, ||, !
- `C24` - Always mul before div, unless mul could overflow.
- `C25` - Magic numbers are replaced by a constant with a useful name.
- `C26` - Prefer using WETH over ETH when possible.
- `C27` - Use SafeERC20 or check return values safely.
- `C28` - Don't use `msg.value` in a loop or where reentrant delegatecalls are possible (like if using Multicall)
- `C29` - Updating a struct/array in memory won't modify it in storage.

## Calls in Functions

- `X1` - Is the result checked and errors dealt with? (SWC-104)
- `X2` - If there is an error, could it cause a DoS. Like balanceOf causing revert (SWC-113)
- `X3` - What if it uses all gas?
- `X4` - Is an external contract call needed?
- `X5` - Is a lock used? If so are the external calls protected?

## Static Calls

- `S1` - Is it actually marked as view in the interface?
- `S2` - If there is an error, could it cause a DoS? Like balanceOf causing revert (SWC-113)
- `S3` - What if it uses all gas?
- `S4` - Is an external contract call needed?

## Events

- `E1` - Should any arguments be indexed?
- `E2` - Is the creator of the relevant action logged and indexed?
- `E3` - Do not index strings or bytes.

## Contract

- `T1` - Are all events there?
- `T2` - No SELFDESTRUCT (SWC-106)
- `T3` - Check for correct inheritance, keep it simple and linear (SWC-125)

## Project

- `P1` - Use the right license (you must use GPL if you depend on GPL code, etc)
- `P2` - Use SPDX headers.
- `P3` - Unit test everything.
- `P4` - Fuzz test as much as possible.
- `P5` - Use the SMTChecker to prove invariants.

## DeFi

- `D1` - Check your assumptions about what other contracts do and return.
- `D2` - Don't mix internal accounting with actual balances.
- `D3` - Becareful of relying on the raw token balance of a contract to determine earnings.
- `D4` - Watch out for ERC-777 tokens. Even a token you trust could preform reentrancy if it's an ERC-777.
