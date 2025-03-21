# EIP-7702 Proxy Vulnerability PoC

This repo showcases a proof-of-concept (PoC) exploit for a critical issue in the `EIP7702Proxy` contract found in the [`base/eip-7702-proxy` repository](https://github.com/base/eip-7702-proxy).

**Issue Summary:**  
A malicious actor can bypass the validator check and set a compromised implementation, thereby taking full control of wallets that rely on this proxy for security.

---

## ⚠️ Judges: Steps to Reproduce

Follow these instructions **within the official `base/eip-7702-proxy` codebase** to confirm the exploit:

### 1. Clone & Build the Original Audit Repo

```bash
git clone https://github.com/base/eip-7702-proxy.git
cd eip-7702-proxy
git checkout 371a141ed738a0328b356092a8df1bed0e1d9856
forge update && forge install
forge build
```

### 2. Add the PoC Tests

Copy over the exploit and mocks folders from this PoC repo into the test/ folder of the official code:

```bash
# assuming your PoC repo is next to the eip-7702-proxy folder
cp -r ../eip7702-poc/exploit test/
cp -r ../eip7702-poc/mocks test/
```

Your test/ directory in the original repo should look like:

```
test
├── exploit
│   └── Exploit_SetImplementation.t.sol
├── mocks
│   ├── MockImplementation.sol
│   └── MockMaliciousImplementation.sol
...
```

### 3. Run the Exploit Test

```bash
forge test --match-test test_exploitWithBadValidator_setsMaliciousImpl -vvvv
```

Expected Output:

```
Implementation set to: 0x...
Exploit successful! Proxy now delegates to malicious implementation.
PWNED
```

## ℹ️ Vulnerability Explanation

Because the validator is user-supplied, an attacker can simply return "success" for any implementation. This undermines the intended security of `setImplementation()`—the proxy effectively trusts whatever validator says is valid.

### Impact:
- **High** – Attackers can hijack the smart account's logic.

### Likelihood:
- **High** – Needs only a validator contract returning success plus a valid EOA signature.

*Use responsibly.*