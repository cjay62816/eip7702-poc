# 🔓 EIP-7702 Proxy Exploit PoC

This repo contains a working proof-of-concept (PoC) showing how an attacker can take control of an `EIP7702Proxy` smart account by bypassing validator logic and setting a malicious implementation. This was developed for the [Base EIP-7702 audit competition](https://github.com/base/eip-7702-proxy).

---

##  What’s This About?

EIP-7702 introduces a powerful concept: letting EOAs become smart contract wallets. But with that power comes risk. The `EIP7702Proxy` is designed to securely handle smart account upgrades — but under the hood, there's a way to hijack that process.

This PoC shows how a malicious actor can:

- Deploy a proxy with a fake validator
- Forge a signature (or use a real one with a spoofed validator)
- Manually set the implementation storage slot
- Hijack the proxy and run arbitrary logic (e.g., `evilFunction()`)

---

##  PoC Structure

This repo has two main files:

- `test/exploit/Exploit_SetImplementation.t.sol`: The test that executes the exploit end-to-end.
- `test/mocks/MockImplementation.sol`: A set of mock contracts used for testing, including a malicious one with an `evilFunction()` that prints `"PWNED"`.

---

##  How to Run It

Make sure you have [Foundry](https://book.getfoundry.sh/getting-started/installation) installed.

Then:

```bash
forge install
forge test --match-test test_exploitWithBadValidator_setsMaliciousImpl -vvvv
