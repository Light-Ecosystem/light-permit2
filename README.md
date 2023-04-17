# permit2

Permit2 introduces a low-overhead, next generation token approval/meta-tx system to make token approvals easier, more secure, and more consistent across applications.

## Features

- Signature Based Approvals: Any ERC20 token, even those that do not support [EIP-2612](https://eips.ethereum.org/EIPS/eip-2612), can now use permit style approvals. This allows applications to have a single transaction flow by sending a permit signature along with the transaction data when using `Permit2` integrated contracts.
- Batched Token Approvals: Set permissions on different tokens to different spenders with one signature.
- Signature Based Token Transfers: Owners can sign messages to transfer tokens directly to signed spenders, bypassing setting any allowance. This means that approvals aren't necessary for applications to receive tokens and that there will never be hanging approvals when using this method. The signature is valid only for the duration of the transaction in which it is spent.
- Batched Token Transfers: Transfer different tokens to different recipients with one signature.
- Safe Arbitrary Data Verification: Verify any extra data by passing through a witness hash and witness type. The type string must follow the [EIP-712](https://eips.ethereum.org/EIPS/eip-712) standard.
- Signature Verification for Contracts: All signature verification supports [EIP-1271](https://eips.ethereum.org/EIPS/eip-1271) so contracts can approve tokens and transfer tokens through signatures.
- Non-monotonic Replay Protection: Signature based transfers use unordered, non-monotonic nonces so that signed permits do not need to be transacted in any particular order.
- Expiring Approvals: Approvals can be time-bound, removing security concerns around hanging approvals on a wallet’s entire token balance. This also means that revoking approvals do not necessarily have to be a new transaction since an approval that expires will no longer be valid.
- Batch Revoke Allowances: Remove allowances on any number of tokens and spenders in one transaction.

## Architecture

Permit2 is the union of two contracts: `AllowanceTransfer` and `SignatureTransfer`.

The `SignatureTransfer` contract handles all signature-based transfers, meaning that an allowance on the token is bypassed and permissions to the spender only last for the duration of the transaction that the one-time signature is spent.

The `AllowanceTransfer` contract handles setting allowances on tokens, giving permissions to spenders on a specified amount for a specified duration of time. Any transfers that then happen through the `AllowanceTransfer` contract will only succeed if the proper permissions have been set.

## Integrating with Permit2

*Before integrating contracts can request users’ tokens through* *`Permit2`, users must approve the* *`Permit2`* *contract through the specific token contract. To see a detailed technical reference, visit the Uniswap* *[documentation site](https://docs.uniswap.org/contracts/permit2/overview).*

### Note on viaIR compilation

Permit2 uses viaIR compilation, so importing and deploying it in an integration for tests will require the integrating repository to also use viaIR compilation. This is often quite slow, so can be avoided using the precompiled `DeployPermit2` utility:

```Plaintext
import {DeployPermit2} from "permit2/test/utils/DeployPermit2.sol";

contract MyTest is DeployPermit2 {
    address permit2;

    function setUp() public {
        permit2 = deployPermit2();
    }
}
```

## Bug Bounty

### Setup

```TypeScript
git clone https://github.com/Light-Ecosystem/light-permit2.git
yarn
```

### Deploy

Run the command below to deploy hardhat node

```TypeScript
 yarn hardhat run ./scripts/deploy.ts
```

## Acknowledgments

Inspired by [merklejerk](https://github.com/merklejerk)'s [permit-everywhere](https://github.com/merklejerk/permit-everywhere) contracts which introduce permit based approvals for all tokens regardless of EIP2612 support.

# Audits and Security

Light DAO contracts have been audited by  SlowMist and Certic. These audit reports are made available on the [Audit](https://github.com/Light-Ecosystem/light-dao/tree/main/audit).

There is also an active [bug bounty](https://static.hope.money/bug-bounty.html) for issues which can lead to substantial loss of money, critical bugs such as a broken live-ness condition, or irreversible loss of funds.

## Community

If you have any questions about this project, or wish to engage with us:

- [Websites](https://hope.money/)
- [Medium](https://hope-ecosystem.medium.com/)
- [Twitter](https://twitter.com/hope_ecosystem)
- [Discord](https://discord.com/invite/hope-ecosystem)

# License

This project is licensed under the [[AGPL-3.0](https://github.com/Light-Ecosystem/light-dao/blob/dev/LICENSE)](LICENSE) license.