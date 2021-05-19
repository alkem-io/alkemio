In order to show how the CBAS module can be used in the context of an example use case, a set of tests were written and included in the repository. These tests showcase the currently supported API, and serve as a good example of how a developer would use the CBAS library to implement authorization for their use case. *We are also currently integrating the CBAS module with the UBS server / client components. We hope that the resulting integration can serve as a more complete usage example.**

The following supporting data structures are defined to enable the test use case:
- A governance framework document describing a generic backup service. This document includes definitions for the `privileges` relevant to the use case and associated grant rules. These rules are used in the tests to evaluate capabilities.
- A set of objects describing resources relevant to the test use case. These represent access to API endpoints (in the case of "backup-management-api" and "vault-management-api") and specific resource (in case of "UBS-backup", "UBS-vault"). Access to these resources is granted during the tests.

We first show how an instance of the CBAS library can be used to issue capabilities authorizing the holder to perform some actions on certain resources.
The tests for `getGrantsForCapability` and `canGrantPrivilege` further show how the issued capability can be evaluated against a governance framework document (specifically the [SGL statements](https://github.com/evernym/simple-grant-lang) in the `rules` section) to grant certain defined privileges. These useful helpers can aid the client in finding the appropriate capabilities for a request, and the server in evaluating / verifying received capabilities.

The rest of the tests focus on the `evaluateCapability` and `evaluateInvocation` functions, which can be used to verify the signature and expiry date on a capability / invocation, and evaluate it against the `rules` defined in the governance framework.

*The tested API is further described in the "[Interface Specification document]()"*

We are currently focusing on the following areas of research / development for the next iteration of the CBAS module:
- Adding support for defining and processing constraints ([as shown here](https://github.com/hyperledger/aries-rfcs/blob/33dc2a8b3235a54ecb483f6020bb20b977918978/concepts/0103-indirect-identity-control/guardianship-sample/trust-framework.md#constraints)) (which can be included in a credential during the issuance process or delegation) using a governance framework document.
- Further research on how delegation rules can be associated with privileges via a governance framework document. Combined with the ability to add constraints to credentials, this would allow for use cases where the client wants to share access to a resources stored on a backup server with other SSI agents (perhaps with additional restrictions / constraints).
- Align the VC / VP issuance interface required by the CBAS module with the [Universal Wallet specification document](https://w3c-ccg.github.io/universal-wallet-interop-spec/).

We intend to explore some of these topics in more detail as part of upcoming interoperability sessions.
