# docs

## Table of Contents

- [About FLEXID](#about-flexid)
- [Overview of Components](#overview-of-components)
  * [Credentials](#credentials)
  * [Identity](#identity)
  * [Credential Issuing and State Management](#credential-issuing-and-state-management)
  * [Verifying](#verifying)
  * [Wallet and Storage](#wallet-and-storage)
- [Design, requirements, threat model](#design--requirements--threat-model)
- [Repositories](#repositories)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## About DCC

[FlexID](https://flexid.asia/)  a platform and wallet for the issuance, storage, and sharing of verified digital identity credentials (e.g., driver’s license, land certificate etc.). Its solution replaces paper-based physical credentials, which are expensive to issue, manage, and verify in Emerging Market countries where there is no trusted identity infrastructure. With easy access to verified credentials, FlexID aims to enable individuals to get broader and faster access to a wide range of financial, insurance, healthcare, and agriculture services.


## Overview of Components

For additional context, the [FlexID whitepaper](https://flexid.asia) outlines our approach and high-level architecture. 

Overview of the FlexID components:

### Credentials

FlexID uses the [Verifiable Credentials Data Model](https://w3c.github.io/vc-data-model/) with linked data. 
- [Authoring guide](authoring/README.md) - authoring DCC credentials
- [Online credential editing tool](https://flexid.github.io/playground/) - experimental
- [Modeling Educational Verifiable Credentials](https://w3c-ccg.github.io/vc-ed-models/) 
  - see similar standards incubation efforts at the [W3C VC-EDU task force](https://w3c-ccg.github.io/vc-ed/)

### Identity

- [Identity design decisions](identity/design_decisions_id.md)
- [Issuer registry MVP design](identity/issuer_registry.md)
- [Issuer Key and DID Document Generation](identity/issuer_key_generation.md)
- [Setting up an issuer issuing identity](identity/issuer_demo_id.md)

### Verified Issuance

Onboard the issuer into the [issuer registry](issuer_registry.md)


### Credential Issuing and State Management

Issuing credentials and maintaining state involves:

- [Credential request flow](request/credential_request.md)
  - [Issuing a credential to the learner](credential/issue_credential.md)
- [Revoke a credential](credential/revoke_credential.md)
  - [Revocation design discussions](credential/revoke_credential_design_discussions.md)

### Verifying

Verifying credentials involves:
- [Verification of the credential](verification/verify_credential.md)
- Identity proofing, variable (coming soon)

### Wallet and Storage
FlexID uses the [Verifiable Credentials Data Model](https://w3c.github.io/vc-data-model/) and can  

- [cred-wallet](https://github.com/ssidwallet/cred-wallet): mobile wallet that manages learner's credentials and identifiers

### Design, requirements, threat model

- [system requirements](system/requirements.md)
- [threat model](system/threat_model.md)

## Repositories

- [docs](https://github.com/flexid/docs): (this repo) contains project documentation, broader architectural docs, etc
- [sign-and-verify](https://github.com/flexid/sign-and-verify): library to sign/issue and verify DCC credentials. 
- [credgen](https://github.com/flexid/credgen): command line tool to generate credential templates
- [learner-credential-wallet](https://github.com/flexid/learner-credential-wallet): repository for DCC react native mobile app to manage and store credentials 
- [designer](https://github.com/flexid/designer): web ui to design credential templates -- intended for developers

