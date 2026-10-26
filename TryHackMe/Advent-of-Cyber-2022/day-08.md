# Advent of Cyber 2022 - Day 8: Smart Contracts

{% embed url="https://tryhackme.com/room/adventofcyber4" %}

## Background
Ethereum smart contract security, EVM transactions, Solidity code review, and exploiting the classic **Reentrancy Vulnerability** (the same flaw behind the DAO hack).

## Walkthrough
The target contract `EtherStore.sol` tracks balances and allows withdrawals:
```solidity
function withdraw() public {
    uint bal = balances[msg.sender];
    require(bal > 0);
    (bool sent, ) = msg.sender.call{value: bal}("");
    require(sent, "Failed to send Ether");
    balances[msg.sender] = 0; // State updated AFTER transfer!
}
```
Because the state (`balances[msg.sender] = 0`) is updated *after* the external call, an attacking contract's `fallback()` or `receive()` function can call `withdraw()` again before the initial execution completes, draining the contract funds.
Deploy the `Attack.sol` exploit contract via Remix IDE, execute `attack()`, and drain the vault to receive the flag.

## 🏁 Questions & Answers

| Question | Answer |
|---|---|
| What flag is found after attacking the provided EtherStore Contract? | `flag{411_ur_37h_15_m1n3}` |
