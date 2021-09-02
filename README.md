# security-checklist

**Security checklist for smart contracts written in Solidity.** Modified from BoringCrypto's [BentoBox checklist](https://github.com/sushiswap/bentobox/blob/master/documentation/checks.txt).

## Variables

- `V1` - Can it be private?
- `V2` - Can it be constant?
- `V3` - Can it be immutable/constant?
- `V4` - Is visibility set? (SWC-108)

## Structs

- `S1` - Are the members split on 256 boundaries?
- `S2` - Can any members be a smaller type?

## Functions

- `F1` - Can it be external?
- `F2` - Should it be private? (SWC-100)
- `F3` - Should it be payable?
- `F4` - Can it be combined with another similar function?
- `F5` - Check behavior for all function arguments when wrong or extreme.
- `F6` - Checks before effects pattern followed? (SWC-107)
- `F7` - Check for front-running possibilities, such as the approve function. (SWC-114)
- `F8` - Avoid insufficient gas griefing. (SWC-126)
- `F9` - Are the correct modifiers applied, such as `onlyOwner`?
- `F10` - Return arguments are always assigned?
- `F11` - Write down and test invariants about state before a function can run correctly.
- `F12` - Write down and test invariants about the return or any changes to state after a function has run.
- `F13` - Take care when naming functions, because people will assume behavior based on the name.
- `F14` - If a function is intentionally unsafe (to save gas, etc), use an unwieldy name to force users to think twice.

## Modifiers

- `M1` - Do not make storage/memory changes except for a reentrancy lock.
- `M2` - Do not make external calls if possible.

## Code

- `C1` - Using SafeMath or 0.8 checked math? (SWC-101)
- `C2` - Are any storage slots read multiple times?
- `C3` - Are any unbounded loops/arrays used that can cause DoS? (SWC-128)
- `C4` - Use block.timestamp only for long intervals. (SWC-116)
- `C5` - Don't use block.number for elapsed time. (SWC-116)
- `C7` - Avoid delegatecall wherever possible, especially to external (even if trusted) contracts. (SWC-112)
- `C8` - Don't use function types.
- `C9` - Don't use blockhash, etc for randomness. (SWC-120)
- `C10` - Protect signatures against replay, use nonce and `block.chainid`. (SWC-121)
- `C11` - All signatures strictly EIP-712. (SWC-117 SWC-122)
- `C12` - Output of abi.encodePacked shouldn't be hashed if using two or more dynamic types. (SWC-133)
- `C13` - Careful with assembly, don't allow any arbitrary use data. (SWC-127)
- `C14` - Don't assume a specific ETH balance. (SWC-132)
- `C15` - Avoid insufficient gas grieving. (SWC-126)
- `C16` - Private data isn't private. (SWC-136)
- `C17` - Updating a struct/array in memory won't modify it in storage.
- `C18` - Never shadow state variables. (SWC-119)
- `C19` - No unused variables. (SWC-131)
- `C20` - Is calculation on the fly cheaper than storing the value?
- `C21` - Are all state variables read from the correct contract: master vs. clone?
- `C22` - Is all usage of `>` or `<` or `>=` or `<=` correct?
- `C23` - Are logical operators correct `==`, `!=`, `&&`, `||`, `!`
- `C24` - Always mul before div, unless mul could overflow.
- `C25` - Are magic numbers are replaced by a constant with a useful name?
- `C26` - Prefer using WETH over ETH when possible.
- `C27` - Use SafeERC20 or check return values safely.
- `C28` - Don't use `msg.value` in a loop or where reentrant delegatecalls are possible (like if it inherits Multicall/Batchable).
- `C29` - Don't assume `msg.sender` is always a relevant user.
- `C30` - Don't use `assert` unless for fuzzing or formal verification. (SWC-110)
- `C31` - Don't use `tx.origin`. (SWC-115)
- `C32` - Don't use `address.transfer()` or `address.send()`. (SWC-134)
- `C33` - When using low-level calls, ensure the contract exists before calling.
- `C34` - When calling a function with many parameters, use the named argument syntax.
- `C35` - Do not use assembly for create2. Prefer the modern salted contract creation syntax.
- `C36` - Do not use assembly to access chainId or contract code/size/hash. Prefer the modern Solidity syntax.

## External Calls

- `X1` - Is the result checked and errors dealt with? (SWC-104)
- `X2` - If there is an error, could it cause a DoS. Like `balanceOf()` causing revert. (SWC-113)
- `X3` - What if it uses all gas?
- `X4` - Is an external contract call needed?
- `X5` - Is a lock used? If so are the external calls protected?

## Static Calls

- `S1` - Is it actually marked as view in the interface?
- `S2` - If there is an error, could it cause a DoS? Like `balanceOf()` causing revert. (SWC-113)
- `S3` - What if it uses all gas?
- `S4` - Is an external contract call needed?

## Events

- `E1` - Should any arguments be indexed?
- `E2` - Is the creator of the relevant action included and indexed?
- `E3` - Do not index strings or bytes.

## Contract

- `T1` - Are events emitted for every storage mutating function?
- `T2` - Check for correct inheritance, keep it simple and linear. (SWC-125)
- `T3` - Use a `receive() external payable` function if the contract should accept transferred ETH.

## Project

- `P1` - Use the right license (you must use GPL if you depend on GPL code, etc)
- `P2` - Use SPDX headers.
- `P3` - Unit test everything.
- `P4` - Fuzz test as much as possible.
- `P5` - Use the SMTChecker to prove invariants.
- `P6` - Run and review all Slither findings.
- `P7` - Write down and test invariants about relationships between stored state.

## DeFi

- `D1` - Check your assumptions about what other contracts do and return.
- `D2` - Don't mix internal accounting with actual balances.
- `D3` - Be careful of relying on the raw token balance of a contract to determine earnings.
- `D4` - Watch out for ERC-777 tokens. Even a token you trust could preform reentrancy if it's an ERC-777.
- `D5` - Don't use spot price from an AMM as an oracle.
- `D6` - Use sanity checks to prevent oracle/price manipulation.
- `D7` - Do not trade on AMMs without receiving a price target off-chain or using an oracle.
