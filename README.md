
# The Epic Failures of Hedera Hashgraph Network

## Failure 1: Every Single Hashgraph Repository is an Unmaintained Slop Job

We will take [`@hashgraph/hedera-wallet-connect`](https://github.com/hashgraph/hedera-wallet-connect) and [`@hiero-ledger/sdk`](https://github.com/hiero-ledger/hiero-sdk-js) as examples. 

The current version of `@hiero-ledger/sdk` is **2.85.0** at the time of writing. But the lazy, incompetent, good-for-nothing developers have put a hard limit of using **2.79.0** in their wallet connect repo. 

By forcing this outdated dependency, they are exposing the entire ecosystem to a cascade of severe issues:

* **Unpatched Security Vulnerabilities:** You are missing out on critical security patches introduced between versions 2.79.0 and 2.85.0. In a wallet connection context, running an outdated SDK means your transaction signing, serialization, and key handling mechanisms could be exposed to known exploits.
* **Transaction Failures & Network Incompatibility:** Hedera's network protocols, fee structures, and consensus node requirements evolve constantly. An SDK stuck in the past will fail to format transactions correctly, leading to rejected transactions, failed smart contract interactions, and completely broken dApp functionality.
* **WalletConnect Desync:** WalletConnect relies on perfect serialization between the dApp and the user's wallet. An outdated SDK creates a mismatch where the dApp formats requests in a legacy way that modern wallets struggle to parse, resulting in silent failures, hanging prompts, and frustrated users.
* **Dependency Hell for Developers:** If you try to "fix" this by installing the newer `2.85.0` SDK in your own project, you end up with two conflicting versions of the SDK in your `node_modules`. This causes massive bundle bloat, broken `instanceof` checks, and unpredictable runtime crashes.

### Evidence: Critical NPM Audit Findings

Running a simple `npm audit` on a fresh project with only `@hashgraph/hedera-wallet-connect` and `@hiero-ledger/sdk` installed immediately flags multiple **Critical** and **High** severity vulnerabilities. 

The maintainers have left developers in a catch-22: the only automated fix (`npm audit fix --force`) forces a breaking change to `@hashgraph/hedera-wallet-connect@2.0.4`, proving there is no safe, non-breaking upgrade path provided by the ecosystem.

**Key Vulnerabilities Exposed:**
* **`protobufjs` (CRITICAL):** Arbitrary code execution, code injection through bytes field defaults, prototype injection, and multiple Denial of Service (DoS) vectors via unbounded recursion.
* **`@grpc/grpc-js` (HIGH):** A malformed request or incoming malformed compressed message can cause a complete client or server crash.
* **`elliptic` & `@ethersproject` (MODERATE/HIGH):** Uses a cryptographic primitive with a risky implementation, directly threatening the security of transaction signing keys.
* **`bn.js` (MODERATE):** Affected by an infinite loop vulnerability, opening the door to targeted Denial of Service attacks.

<details>
<summary><strong>Click to view the raw NPM Audit output</strong></summary>

```text
@grpc/grpc-js  1.12.0 - 1.12.6
Severity: high
@grpc/grpc-js: A malformed request can cause a server crash - https://github.com/advisories/GHSA-5375-pq7m-f5r2
@grpc/grpc-js: An incoming malformed compressed message can cause a client or server crash - https://github.com/advisories/GHSA-99f4-grh7-6pcq
fix available via `npm audit fix --force`
Will install @hashgraph/hedera-wallet-connect@2.0.4, which is a breaking change

bn.js  5.0.0 - 5.2.2
Severity: moderate
bn.js affected by an infinite loop - https://github.com/advisories/GHSA-378v-28hj-76wf
fix available via `npm audit fix --force`
Will install @hashgraph/hedera-wallet-connect@2.0.4, which is a breaking change

elliptic  *
Elliptic Uses a Cryptographic Primitive with a Risky Implementation - https://github.com/advisories/GHSA-848j-6mx2-7j84
fix available via `npm audit fix --force`
Will install @hashgraph/hedera-wallet-connect@2.0.4, which is a breaking change

protobufjs  <=7.6.2 || 8.0.0 - 8.5.0
Severity: critical
Arbitrary code execution in protobufjs - https://github.com/advisories/GHSA-xq3m-2v4x-88gg
protobuf.js: Code injection through bytes field defaults in generated toObject code - https://github.com/advisories/GHSA-66ff-xgx4-vchm
protobuf.js: Denial of service from crafted field names in generated code - https://github.com/advisories/GHSA-2pr8-phx7-x9h3
protobuf.js: Prototype injection in generated message constructors - https://github.com/advisories/GHSA-fx83-v9x8-x52w
[... and multiple additional DoS and code generation gadget vulnerabilities]
fix available via `npm audit fix --force`
Will install @hashgraph/hedera-wallet-connect@2.0.4, which is a breaking change
```
</details>

> **Result:** All Hedera projects now have critical, verifiable vulnerabilities because Hedera Hashgraph doesn't even know the content of its own repositories. Using the older version in the backend subjects your servers and your users' assets to extreme, documented risks.

---

## Conclusion

I would ask all developers to **STOP** developing anything on Hedera Hashgraph and abandon the network for better ones like [Solana](https://solana.com/) or [EVM networks](https://ethereum.org/en/developers/docs/evm/). 

Hedera Hashgraph doesn't deserve a nanosecond of your valuable time.

