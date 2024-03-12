# Open Grant Proposal: `Keyless IPNS Service`

**Project Name:** `IPNS.dev: Keyless IPNS Service`

**Proposal Category:** `Applications`

**Individual or Entity Name:** `NameSys.eth`

**Proposer:** [`@namesys-eth`](https://github.com/orgs/namesys-eth)

**(Optional) Filecoin ecosystem affiliations:** `null`

**(Optional) Technical Sponsor:** `null`

**Do you agree to open source all work you do on behalf of this RFP under the MIT/Apache-2 dual-license?:** `YES`

# Project Summary
`IPNS.dev` is a no-nonsense, free, open-source, autonomous, trustless, keyless and secure IPNS pinning service. Users of IPNS have so far required to either share their private keys with scarce service providers such as [`dWebServices.xyz`](https://dWebServices.xyz) or publish privately to their IPFS nodes. `IPNS.dev` is a service which removes this severe security flaw and accessibility issue by employing a "keyless" interface to the IPFS network. Users of `IPNS.dev` do not need to share their IPNS keys with service providers, and can securely and 'keylessly' publish to IPFS network with anonymity. Users' IPNS keys are generated deterministically from their Ethereum wallet signatures **only during an update**, and the content is then pinned and published on `w3name` publishing infrastructure deployed on Cloudflare. To our knowledge, this is the first such service and public good in existence.

## Impact
By providing a trustless, keyless, and secure IPNS pinning service, `IPNS.dev` provides users with absolute control over their IPNS content while prioritising privacy and security. `IPNS.dev` could reshape how IPFS content is shared and accessed, making it more accessible to those who truly hold the 'not your keys, not your ~~coin~~ data' ethos dear. It also sets a new standard for IPFS services to incorporate IPNS in their hosting schemes, potentially expanding the reach of IPFS network. `IPNS.dev` has the potential to make secure data publishing more accessible and contribute to the decentralisation aspect of the IPFS network.

## Outcomes
`IPNS.dev` service is reliant on a well-defined standard for implementing a keyless and autonomous interface to IPNS. This standard forms the core of `IPNS.dev` and it can be implemented by any other service for cross-client compatibility. Our expectation is therefore two-pronged:

1. That users adopt `IPNS.dev` as their IPNS service provider, and

2. That other services begin to offer keyless IPNS pinning to their users following the standard set by `IPNS.dev`.

The standard for the keyless interface is described [in complete detail here](https://github.com/namesys-eth/ipns-eth-resources/blob/main/docs/README_DEV.md) and can be diagrammatically shown as follows:

![](https://raw.githubusercontent.com/namesys-eth/ipns-eth-resources/main/graphics/png/keygen_dev.png)

## Roadmap, Adoption and Growth Strategies
`IPNS.dev` is already a fully functioning infrastructure, and we are seeking funds for future integrations, maintenance and research & development, including upgrades to the current stack.

### Adoption & Growth
  - **Objective:** Collaborate with IPFS services such as `Pinata`, `Fleek`, `dWebServices` etc to facilitate `IPNS.dev` cross-client compatible integrations
  - **Tasks:** Engage with IPFS services to facilitate their adoption of `IPNS.dev` keyless infrastructure
  - **Deliverables:** Integrations by IPFS service providers with cross-client compatibility

## Total Budget Requested
| Milestone | Description            | Deliverables  | Completion Date     | Funding  |
| :-------: | :--------------------: | :-----------: | :-----------------: | :------: |
| Launch    | Launch of `IPNS.dev`   | `Completed`   | `26 January 2024`   | `$5,000` |
| Upgrades  | Upgrades to `IPNS.dev` | See `Roadmap` | `26 December 2024`  | `$5,000` |

## Maintenance and Upgrade Plans
### Faster IPNS
  - **Objective:** Reduce latency of updates by speeding up IPNS propagation
  - **Tasks:** Deploy access-controlled `w3name` API. Improve IPNS propagation speed by parallel publishing to public as well as private `w3name` APIs.
  - **Deliverables:** `w3name` Cloudflare deployment and IPNS multi-publishing

# Team
## Team Members
- [`sshmatrix`](https://sshmatrix.eth.limo)
- [`0xc0de4c0ffee`](https://freetib.eth.limo)

## Team Member ~~LinkedIn~~ GitHub Profiles
- [`sshmatrix`](https://github.com/sshmatrix)
- [`0xc0de4c0ffee`](https://github.com/0xc0de4c0ffee)

## Team Website
[`namesys-eth.github.io`](https://github.com/orgs/namesys-eth)

## Relevant Experience
The two team members are seasoned cryptographers and developers from India and Nepal, with a diverse technological background. Their homepages are [`@sshmatrix`](https://sshmatrix.eth.limo) and [`@0xc0de4c0ffee`](https://freetib.eth.limo)

## Team code repositories
[`github.com/namesys-eth`](https://github.com/orgs/namesys-eth)

# Additional Information
`null`
