# Open Grant Proposal: `Keyless IPNS Service`

**Project Name:** `IPNS.dev: Keyless IPNS Service`

**Proposal Category:** `Applications`

**Individual or Entity Name:** `NAMESYS`

**Proposer:** [`orgs/namesys-eth`](https://github.com/orgs/namesys-eth)

**(Optional) Filecoin ecosystem affiliations:** `null`

**(Optional) Technical Sponsor:** `null`

**Do you agree to open source all work you do on behalf of this RFP under the MIT/Apache-2 dual-license?:** `YES`

# Project Summary
`IPNS.dev` is a no-nonsense, free, open-source, autonomous, trustless, keyless and secure IPNS pinning service. Users of IPNS have so far required to either share their private keys with service providers such as [`dWebServices.xyz`](https://dWebServices.xyz) or publish privately to their IPFS nodes. `IPNS.dev` is a service which removes this severe security flaw and accessibility issue by employing a "keyless" interface to the IPFS network. Users of `IPNS.dev` do not need to share their IPNS keys with service providers, and can securely and 'keylessly' publish to IPFS network with anonymity. Users' IPNS keys are generated deterministically from their Ethereum wallet signatures **only during an update**, and the content is then pinned and published on `w3name` publishing infrastructure deployed on Cloudflare. To our knowledge, this is the first such service and public good in existence.

## Impact
By providing a trustless, keyless, and secure IPNS pinning service, `IPNS.dev` provides users with absolute control over their IPNS content while prioritising privacy and security. `IPNS.dev` could reshape how IPFS content is shared and accessed, making it more accessible to those who truly hold the 'not your keys, not your ~~coin~~ data' ethos dear. It also sets a new standard for IPFS services to incorporate IPNS in their hosting schemes, potentially expanding the reach of IPFS network. `IPNS.dev` has the potential to make secure data publishing more accessible and contribute to the decentralisation aspect of the IPFS network.

## Outcomes


![](https://raw.githubusercontent.com/namesys-eth/ipns-eth-resources/main/graphics/png/keygen.png)

## Roadmap, Adoption and Growth Strategies

NameSys is already a fully functioning infrastructure, and we are seeking funds for future integrations, maintenance and research & development, including upgrades to the current stack.

### Adoption through Integrations
  - **Objective:** Collaborate with ENS registrars and marketplaces such as `GoDID`, `ENSVision`, `UnstoppableDomains` etc to facilitate NameSys integration
  - **Tasks:** Engage with registrars for integration, explore marketplace partnerships, and develop integration APIs
  - **Deliverables:** Integration with `GoDID`, `ENSVision` and similar registrars

### Team Growth and Development Acceleration
  - **Objective:** Strengthen the development team and accelerate project output
  - **Tasks:** Recruit one part-time developer, onboard and train the new team member, and allocate resources for faster development
  - **Deliverables:** New team member onboarded and put to dedicated work on integrations

### Anti-phishing dApp Store
  - **Objective:** Deploy dApp Store on IPFS for ENS users aimed at resolving phishing scams
  - **Tasks:** Code client-side dApp Store where users can access all major open-source dApps at `<dApp>.domain.eth`
  - **Deliverables:** Full dApp Store infrastructure deployment

## Total Budget Requested

| Milestone | Description         | Deliverables  | Completion Date     | Funding  |
| --------- | ------------------- | ------------- | ------------------- | -------- |
| Launch    | Launch of NameSys   | `Completed`   | `18 September 2023` | `$5,000` |
| Upgrades  | Upgrades to NameSys | See `Roadmap` | `18 March 2024`     | `$5,000` |

## Maintenance and Upgrade Plans

### Faster IPNS
  - **Objective:** Reduce latency of updates by speeding up IPNS propagation
  - **Tasks:** Deploy ENS-dedicated access-controlled `w3name` API. Improve IPNS propagation speed by parallel publishing to public as well as private `w3name` APIs.
  - **Deliverables:** `w3name` Cloudflare deployment and IPNS multi-publishing

# Team

## Team Members

- [`sshmatrix.eth`](https://sshmatrix.eth.limo)
- [`freetib.eth`](https://freetib.eth.limo)

## Team Member ~LinkedIn~ GitHub Profiles
- [`sshmatrix`](https://github.com/sshmatrix)
- [`0xc0de4c0ffee`](https://github.com/0xc0de4c0ffee)

## Team Website
[`orgs/namesys-eth`](https://github.com/orgs/namesys-eth)

## Relevant Experience
The two team members are seasoned cryptographers and developers from India and Nepal, with a diverse technological background.

## Team code repositories
[`orgs/namesys-eth`](https://github.com/orgs/namesys-eth)

# Additional Information
`null`
