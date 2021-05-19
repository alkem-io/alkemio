Architecture review
===================

The CBAS component will be made available to developers via the ["SSI Protocols and Crypto layer"](https://essif-lab.pages.grnet.gr/framework/docs/functional-architecture#23--ssi-protocols-and-crypto-layer), as a library / module which can be used to issue and verifiy [Verifiable Credential](./) based capabilities.

The CBAS component can be used in combination with other modules exposed on the "SSI Protocols and Crypto layer", namely:

- Verifiable Credential modules - The CBAS component will delegate credential generation and verification operations to existing implementations / modules which can be integrated. 
- Revocation modules - Capabilities issued by the CBAS module can follow conventional / integrated revocation flows applicable to Verifiable Credentials.
- Distributed storage / blockchains - Configuration files describing policies relevant to the CBAS component can potentially be persisted using these technologies.

As mentioned in the [functional description document](./), the CBAS component is intended to be used with existing VC issuance and verification tooling (e.g. libraries, remote services). To this extent, the CBAS component can also leverage the functionality exposed by a number of defined functional components to leverage existing abstractions / adapters as part of generating and verifying Verifiable Credentials / capabilities.

- [Verifier functional component](https://essif-lab.pages.grnet.gr/framework/docs/functional-architecture#32---verifier-component-and-its-policypreferences) - CBAS can leverage the Verifier component to create "Presentation Requests" which can be issued to counterparties when authorization is required as part of a request on a protected resource. This would reduce the integration overhead for developers and allow for existing abstractions to be reused.
- [Issuer functional component](https://essif-lab.pages.grnet.gr/framework/docs/functional-architecture#35--issuer-component-and-its-policypreferences) - CBAS can make issuing capabilities easier for developers by dynamically registering credential definitions required to express the defined capabilities with the Issuing component. This would reduce the integration / configuration overhead for developers, and allow them to start issuing credentials (delegating permissions / authorizations) relevant to their use case to other agents.
- [Holder functional component](https://essif-lab.pages.grnet.gr/framework/docs/functional-architecture#33---holder-component-and-its-policypreferences) - Given that CBAS builds on top of verifiable credentials, the Holder component will be able to store and present the delegated capabilities without additional integration overhead.

Deeper integration with the Verifier and Issuer components can potentially be achieved through the alignment of the [Validation Policy](https://essif-lab.pages.grnet.gr/framework/docs/terms/validation-policy) and [Issuing Policy](https://essif-lab.pages.grnet.gr/framework/docs/terms/data-collector-policy) configuration objects with authorization related use cases is possible, but might be out of scope.
