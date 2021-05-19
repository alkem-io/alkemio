### Components, provisional API definition:

The CBAS component will allow the developer to easily create and consume Verifiable Credentials granting authority to perform specific actions on resources (guided by  grant and delegation rules described in a corresponding [Trust Framework document](./data_interface_specification_of_CBAS_component.md#TrustFrameworkDocument)). The CBAS component will allow developers to specify if the permissions they issue can be delegated further, as well as specify additional constraint objects which will be evaluated when the capability verified.

Please note that while the functionality supported by CBAS is not intended to change, the described API is provisional, and might be modified as development furthers and based on new learnings (both from the attempted use cases, as well as from communication with partners). 

The following subsections describe the functions exposed by the CBAS module, detailing the expected inputs / outputs, and dependencies (in terms of other ESSIF Lab functional components, or other specifications)

## Granting Authorization

Given a resource trust framework document, CBAS offers an API to enable developers to create Verifiable Credentials delegating authority / permissions.


```javascript
  // Provisional API,
  // CBAS is exposed through the SSI Protocols and Crypto layer
  const rootCapability = CBAS.newCapability(
    trustFramework,
    {
      "locations": ["http://backup-service.com/interact/"],
      "datatypes": ["application/zip"]
    },
    {
      authorizations: ["read", "write", "write", "delete"]
      constraints: [{
        boundary: "USA:TX",
      }],
    },
    'did:example:alice', 
  )
```

Inputs:
- [trustFramework](./) - a JSON object describing the services provided by a service. Can include rules and define privileges relevant for authorization purposes.
- [resourceDescription](./) - a JSON object describing the resource to which access is being delegated.
- subjectDID - the DID of the intended subject / holder (the `credentialSubject.holder.id` value in the VC). Only agents able to prove control over the subject DID will be able to present and further delegate the Verifiable Credential.
- [delegationOptions](./) - a JSON object containing the following fields:
    - [authorizations](./) - a JSON array listing delegated authorizations. The listed permissions can be the same, or more restricted, than the ones held by the delegator, and must map to corresponding `privileges` described in the `credential.credentialSubject.proxied` section of the capability, or the corresponding Trust Framework document.
    - [constraints](./) - a object encoding additional constraints to be added to the capability during delegation. These constraints must be defined in the corresponding Trust Framework document.

Output:
- A Signed Verifiable Credential delegating `did:example:alice` the authorization to "read", "write" and "delete" at the described resource location. The returned capability can be delegated further.

*This credential can be presented when accessing the protected resource, as well as used as a base for further attenuated authorization delegations.*

## Evaluating capabilities

Given a capability presented by another agent, and the corresponding Trust Framework document, the capability can be evaluated against the listed rules, and the resulting authorizations are returned.

```typescript
// Provisional API,
// CBAS is exposed through the SSI Protocols and Crypto layer

CBAS.evaluateCapability(rootCapability, trustFramework)
```

- resourceDescription - a JSON object describing the resource and the associated authorization rules. See Data Models - resource description.
- capability - a Verifiable Credential granting the holder the authority to perform actions on a resource. See Data Models - Verifiable Credentials, Schema for credentials.

Output:
- A JSON object containing the following properties:
    - authorizations - a JSON array listing the actions allowed after validating the presented Verifiable Credential against the rules listed in the `trustFramework` document
    - constraints - a JSON array encoding additional constraints associated with the capability

If the evaluation result lists the attempted action, access to the `resource` described in the capability can be granted accordingly.

## Delegating a capability

Given a capability, the CBAS module allows the developers to further delegate the associated authorizations to other agents. Additional constraints can be added during the delegation process.

```typescript
// Provisional API,
// CBAS is exposed through the SSI Protocols and Crypto layer

const delegatedCapability = CBAS.delegateCapability(rootCapability, {
authorizations: ["read", "write"]
constraints: [
  "startTime": "2020-05-20T14:00Z",
  "endTime": "2020-06-20T14:00Z",
],
}, 'did:example:bob')
```

The inputs are:
- parentCapability - a Verifiable Credential which describes a capability. The `parentCapability` must include the `authorization` to `delegate`.
- An object with delegation options, these include:
    - The delegated authorizations (must be the same, or restricted compared to the authorizations on the root capability)
    - Additional constraints which will be added to the existing constraints defined on the delegation chain. At each delegation step new constraints may be added, and can not be removed through delegation. When the Verifiable Credential is being verified (e.g. as part of a request to a protected resource), the constraints must be validated in the context of the request. Outside of a set of commonly useful predicates, constraints are expected to be application / use case specific, and can be described in the corresponding machine-readable Trust Framework document.

Output:
- A Signed Verifiable Credential delegating `did:example:bob` the authorization to "read" and "write" from / to the described resource location. The returned credential references the capability from which it was derived via the `delegationProofSection`, as described in the [data model] section.
