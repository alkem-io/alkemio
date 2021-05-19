### Components, provisional API definition:

The CBAS component will allow the developer to easily create and consume Verifiable Credentials granting authority to perform specific actions on resources (guided by  grant and delegation rules described in a corresponding [Trust Framework document](./data_interface_specification_of_CBAS_component.md#TrustFrameworkDocument)). The CBAS component will allow developers to specify if the permissions they issue can be delegated further, as well as specify additional constraint objects which will be evaluated when the capability is verified.

Please note that while the functionality supported by CBAS is not intended to change, the described API is provisional, and might be modified as development furthers and based on new learnings (both from the attempted use cases, as well as from communication with partners). 

The following subsections describe the functions exposed by the CBAS module, detailing the expected inputs / outputs, and dependencies (in terms of other ESSIF Lab functional components, or other specifications)

## Granting Authorization

Given a resource trust framework document, CBAS offers an API to enable developers to create Verifiable Credentials delegating authority / permissions:


```javascript

  const cbas = new CBAS({
    trustFramework,
    createVC,
    verifyVC,
    createVP,
    verifyVP
  })

  const capability = await cbas.issueCapability({
      "type": "UBS-backup",
      "backupID": "aaabbbccc",
      "vaultID": "bbbcccfff",
      "locations": ["https://ubs.jolocom.io"]
    },
    {
      authorizations: ["read", "write"]
      // Constraints are currently not evaluated by the CBAS module.
      constraints: [{
        dailyUploadLimit: "1024MB",
      }],
    },
    'did:example:alice', 
  )
```

Inputs:
- [resourceDescription](./) - a JSON object describing the resource to which access is being delegated. The resource description is embedded in the issued credential.
- [delegationOptions](./) - a JSON object containing the following fields:
    - [authorizations](./) - a JSON array listing delegated authorizations. The listed permissions can be the same, or more restricted, than the ones held by the delegator. The permissions should either match privileges defined in the governance framework document, or be mentioned in `rules`.
    - [constraints](./) - a object encoding additional constraints to be included in the capability during delegation. These constraints must be defined in the corresponding governance framework document.
- subject - the DID of the intended subject / holder.

Output:
- A Signed Verifiable Credential delegating `did:example:alice` the authorization to "read", "write" the described resource.

*This credential can be presented when accessing the protected resource, as well as used as a base for further attenuated authorization delegations.*

*This method requires a `createVC` implementation to be configured on the `cbas` instance.*

## Evaluating capabilities

A CBAS instance configured with a governance framework document can be used to verify capabilities and return associated privileges as follows:

```typescript

await cbas.evaluateCapability(capability)

const expectedReturnValue = {
  resource: {
    "type": "UBS-backup",
    "backupID": "aaabbbccc",
    "vaultID": "bbbcccfff",
    "locations": ["https://ubs.jolocom.io"]
  },
  privileges: ['update-backup', 'delete-backup', 'read-backup'],
  issuer: 'did:example:ubsServer',
  subject: 'did:example:ubsUser',
  constraints: [{
    dailyUploadLimit: "1024MB",
  }]
}
```

Inputs:
- capability - a Verifiable Credential to be evaluated against the `rules` defined in the governance framework document.

In case the verification succeeded, a JSON object containing the following properties is returned:
  - resource - the resource description embedded in the credential
  - privileges - a JSON array of privileges for which at least one of the associated grant `rules` (defined in the governance framework document) passed validation
  - issuer and subject
  - constraints - a JSON array encoding additional constraints associated with the capability

If the evaluation result lists the required privilege, access to the `resource` described in the capability can be granted.
Evaluating constraints is currently delegated to the application developer.

*This method requires a `verifyVC` implementation to be configured on the `cbas` instance.*

## Creating an invocation

When a capability needs to be used in a request, a signed Verifiable Presentation of the corresponding Verifiable Credential must be created. This can be achieved as follows: 

```typescript

const invocation = await cbas.invokeCapability(capability)
```

Returns a signed Verifiable Presentation in JSON form. The invocation can be included with the request in various ways depending on the use case (e.g. via HTTP signatures, or via a DIDComm based protocol).

*This method requires a `createVP` and `createVC` implementation to be configured on the `cbas` instance.*

## Evaluating an invocation

A CBAS instance configured with a governance framework document can be used to verify capability invocations and return associated privileges. This function delegates to `evaluateCapability` after performing the required validation on the Verifiable Presentation.


```typescript

await cbas.evaluateInvocation(invocation)

const expectedReturnValue = {
  resource: {
    "type": "UBS-backup",
    "backupID": "aaabbbccc",
    "vaultID": "bbbcccfff",
    "locations": ["https://ubs.jolocom.io"]
  },
  privileges: ['update-backup', 'delete-backup', 'read-backup'],
  issuer: 'did:example:ubsServer',
  subject: 'did:example:ubsUser',
  constraints: [{
    dailyUploadLimit: "1024MB",
  }]
}
```

The return value is the same as when calling `cbas.evaluateCapability`.

*This method requires a `verifyVC` and `verifyVP` implementation to be configured on the `cbas` instance.*


## Delegating a capability

Given a capability, the CBAS module allows the developers to further delegate the associated authorizations to other agents. Additional constraints can be added during the delegation process.

```typescript
// Provisional API,
// CBAS is exposed through the SSI Protocols and Crypto layer

const delegatedCapability = CBAS.delegateCapability(rootCapability, {
authorizations: ["read", "write"]
constraints: [{
  "startTime": "2020-05-20T14:00Z",
  "endTime": "2020-06-20T14:00Z",
}],
}, 'did:example:bob')
```

The inputs are:
- parentCapability - a Verifiable Credential which describes a capability. The `parentCapability` must include the `authorization` to `delegate`.
- An object with delegation options, these include:
    - The delegated authorizations (must be the same, or restricted compared to the authorizations on the root capability)
    - Additional constraints which will be added to the existing constraints defined on the delegation chain. At each delegation step new constraints may be added, and can not be removed through delegation. When the Verifiable Credential is being verified (e.g. as part of a request to a protected resource), the constraints must be validated in the context of the request. Outside of a set of commonly useful predicates, constraints are expected to be application / use case specific, and can be described in the corresponding machine-readable Trust Framework document.

Output:
- A Signed Verifiable Credential delegating `did:example:bob` the authorization to "read" and "write" from / to the described resource location. The returned credential references the capability from which it was derived via the `delegationProofSection`, as described in the [data model] section.
