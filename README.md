# security-checklist

Opinionated **security** and **code quality** checklist for **Solidity smart contracts**. Based off the [BentoBox checklist](https://github.com/sushiswap/bentobox/blob/master/documentation/checks.txt).

### General Auditing Approach:
- Read the projects docs, specs, and whitepaper to understand what the smart contracts are meant to do.
- Construct a mental model of what you expect the contract to look like before looking at the code.
- Glance over the contracts to get a sense of the project's architecture. Tools like Surya can come in handy.
- Compare the architecture to your mental model. How significant are the differences? Look into surprising areas.
- Create a threat model and make a list of theoretical high level attack vectors.
- Look at areas that can do value exchange. Especially functions like transfer, transferFrom, send, call, delegatecall, and selfdestruct. Walk backward from them to ensure they are secured properly.
- Look at areas that interface with external contracts and ensure all assumptions about them are valid like share price only increases, etc.
- Do a generic line-by-line review of the contracts.
- Do another review from the perspective of every actor in the threat model.
- Glance over the project's tests + code coverage and look deeper at areas lacking coverage.
- Run tools like Slither and Solhint and review their output.

## Variables

- `V1` - Can it be private/internal?
- `V2` - Can it be constant?
- `V3` - Can it be immutable?
- `V4` - Is its visbility set? (SWC-108)
- `V5` - Is the purpose of the variable and other important information documented using natspec?
- `V6` - Can it be packed with an adjacent storage variable?
- `V7` - Can it be packed in a struct with more than 1 other variable?
- `V8` - Use full 256 bit types unless packing with other variables.
- `V9` - If it's a public array, is a seperate function provided to return the full array?

## Structs

- `S1` - Is a struct necessary? Can the variable be packed raw in storage?
- `S2` - Are its fields packed together (if possible)?
- `S3` - Is the purpose of the struct and all fields documented using natspec?

## Functions

- `F1` - Can it be external?
- `F2` - Should it be private? (SWC-100)
- `F3` - Should it be payable?
- `F4` - Can it be combined with another similar function?
- `F5` - Check behavior for all function arguments when wrong or extreme.
- `F6` - Is the checks before effects pattern followed? (SWC-107)
- `F7` - Check for front-running possibilities, such as the approve function. (SWC-114)
- `F8` - Is insufficient gas griefing possible? (SWC-126)
- `F9` - Are the correct modifiers applied, such as `onlyOwner`/`requiresAuth`?
- `F10` - Are return values are always assigned?
- `F11` - Write down and test invariants about state before a function can run correctly.
- `F12` - Write down and test invariants about the return or any changes to state after a function has run.
- `F13` - Take care when naming functions, because people will assume behavior based on the name.
- `F14` - If a function is intentionally unsafe (to save gas, etc), use an unwieldy name to draw attention to its risk.
- `F15` - Are all arguments, return values, side effects and other information documented using natspec?

## Modifiers

- `M1` - Are no storage/memory changes made (except in a reentrancy lock)?
- `M2` - Do not make external calls if possible.
- `M3` - Is the purpose of the modifier and other important information documented using natspec?

## Code

- `C1` - Using SafeMath or 0.8 checked math? (SWC-101)
- `C2` - Are any storage slots read multiple times?
- `C3` - Are any unbounded loops/arrays used that can cause DoS? (SWC-128)
- `C4` - Use `block.timestamp` only for long intervals. (SWC-116)
- `C5` - Don't use block.number for elapsed time. (SWC-116)
- `C7` - Avoid delegatecall wherever possible, especially to external (even if trusted) contracts. (SWC-112)
- `C8` - Don't use function types.
- `C9` - Don't use blockhash, etc for randomness. (SWC-120)
- `C10` - Protect signatures against replay, use a nonce and `block.chainid`. (SWC-121)
- `C11` - Ensure all signatures use EIP-712. (SWC-117 SWC-122)
- `C12` - Output of `abi.encodePacked()` shouldn't be hashed if using >2 dynamic types. (SWC-133)
- `C13` - Careful with assembly, don't use any arbitrary data. (SWC-127)
- `C14` - Don't assume a specific ETH balance. (SWC-132)
- `C15` - Avoid insufficient gas griefing. (SWC-126)
- `C16` - Private data isn't private. (SWC-136)
- `C17` - Updating a struct/array in memory won't modify it in storage.
- `C18` - Never shadow state variables. (SWC-119)
- `C19` - No unused variables. (SWC-131)
- `C20` - Is calculating a value on the fly cheaper than storing it?
- `C21` - Are all state variables read from the correct contract: master vs. clone?
- `C22` - Is all usage of `>` or `<` or `>=` or `<=` correct?
- `C23` - Are logical operators correct `==`, `!=`, `&&`, `||`, `!`
- `C24` - Always mul before div, unless mul could overflow.
- `C25` - Are magic numbers replaced by a constant with an intuitive name?
- `C26` - Prefer using WETH over ETH whenever possible.
- `C27` - Use SafeERC20 or check return values safely.
- `C28` - Don't use `msg.value` in a loop or where delegatecalls are possible (like if it inherits Multicall).
- `C29` - Don't assume `msg.sender` is always a relevant user.
- `C30` - Don't use `assert` unless for fuzzing or formal verification. (SWC-110)
- `C31` - Don't use `tx.origin`. (SWC-115)
- `C32` - Don't use `address.transfer()` or `address.send()`. (SWC-134)
- `C33` - When using low-level calls, ensure the contract exists before calling.
- `C34` - When calling a function with many parameters, use the named argument syntax.
- `C35` - Do not use assembly for create2. Prefer the modern salted contract creation syntax.
- `C36` - Do not use assembly to access chainId or contract code/size/hash. Prefer the modern Solidity syntax.
- `C37` - Comment the "why" as much as possible.
- `C38` - Comment the "what" if using obscure syntax or writing unconventional code.
- `C39` - Comment example inputs and outputs next to complex and/or fixed point math.

## External Calls

- `X1` - Is an external contract call actually needed?
- `X2` - If there is an error, could it cause a DoS? Like `balanceOf()` reverting. (SWC-113)
- `X3` - Would it be harmful if the call reentered into the current function?
- `X4` - Would it be harmful if the call reentered into the another function?
- `X5` - Is the result checked and errors dealt with? (SWC-104)
- `X6` - What if it reaches the gas limit?

## Static Calls

- `S1` - Is an external contract call actually needed?
- `S2` - Is it actually marked as view in the interface?
- `S3` - If there is an error, could it cause a DoS? Like `balanceOf()` reverting. (SWC-113)
- `S4` - What if it reaches the gas limit?

## Events

- `E1` - Should any fields be indexed?
- `E2` - Is the creator of the relevant action included as an indexed field?
- `E3` - Do not index dynamic types like strings or bytes.
- `E4` - Document when the event is emitted and all fields using natspec.

## Contract

- `T1` - Use an SPDX license identifier.
- `T2` - Are events emitted for every storage mutating function?
- `T3` - Check for correct inheritance, keep it simple and linear. (SWC-125)
- `T4` - Use a `receive() external payable` function if the contract should accept transferred ETH.
- `T5` - Write down and test invariants about relationships between stored state.
- `T6` - Is the purpose of the contract and how it interacts with others documented using natspec?

## Project

- `P1` - Use the right license (you must use GPL if you depend on GPL code, etc).
- `P2` - Unit test everything.
- `P3` - Fuzz test as much as possible.
- `P4` - Use the SMTChecker to prove invariants.
- `P5` - Run Slither and review all findings.

## DeFi

- `D1` - Check your assumptions about what other contracts do and return.
- `D2` - Don't mix internal accounting with actual balances.
- `D3` - Be careful of relying on the raw token balance of a contract to determine earnings.
- `D4` - Watch out for ERC-777 tokens. Even a token you trust could preform reentrancy if it's an ERC-777.
- `D5` - Don't use spot price from an AMM as an oracle.
- `D6` - Use sanity checks to prevent oracle/price manipulation.
- `D7` - Do not trade on AMMs without receiving a price target off-chain or via an oracle.
