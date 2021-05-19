Data interface specification of the CBAS component
===============================================

## Trust Framework Document
In order to define the authorization policies relevant for the CBAS component, a collection of authorization rules describing the various privileges (as well as rules associated with granting and delegating them) is required / defined. The document is expected to list a number of [SGL](https://github.com/evernym/sgl) statements.

The aforementioned rules can be embedded in issued capabilities directly, as well as advertised publicly. For this purpose, a machine-readable ARIES Trust Framework document ([Trust Framework RFC](https://github.com/hyperledger/aries-rfcs/blob/master/concepts/0103-indirect-identity-control/guardianship-sample/trust-framework.md), [machine-readable Governance Framework ](https://github.com/hyperledger/aries-rfcs/blob/master/concepts/0430-machine-readable-governance-frameworks/README.md#field-details)) can be employed (this is also mandated by the [Chained Credentials](https://github.com/hyperledger/aries-rfcs/tree/33dc2a8b3235a54ecb483f6020bb20b977918978/concepts/0104-chained-credentials#reference) proposal employed by CBAS. Although the standardization / definition efforts are in early stages, the current example structure can suit the needs of the CBAS component.

A provisional version of a machine-readable document listing the privileges, and the associated granting rules, can look as follows:

```javascript
{
  "@context": [
    "https://github.com/hyperledger/aries-rfcs/blob/master/concepts/0430-machine-readable-governance-frameworks/context.jsonld", 
    "https://ubs.org/access-trust-fw"
  ],
  "name": "Universal Backup Service Resource Control"
  "version": "1.0",
  "logo": "http://ubs.org/logo.png",
  "description": "Governs access control for a Backup Service",
  "docs_uri": "http://ubs.org/access-trust-fw/v1",
  "data_uri": "http://ubs.org/access-trust-fw/v1/tf.json",
  "topics": ["utility", "backup"],
  "roles": ["user"],
  "privileges": [
    {"name": "read", "uri": "http://ubs.org/tf/read"},
    {"name": "write", "uri": "http://ubs.org/tf/write"},
    {"name": "delete", "uri": "http://ubs.org/tf/delete"},
    {"name": "delegate", "uri": "http://ubs.org/tf/delegate"},
  ],
  "duties": [
    {"name": "do-not-share", "uri": "http://ubs.org/tf/do-not-share"}
  ],
  "define": [
    {"name": "UBS": "id": "did:example:abc123"},
    {"name": "UBS": "id": "did:anotherexample:def456"},
  ],
  "rules": [{
      "grant": ["READ"], 
      "when": {
        "contains": {"authorizations": "read"}
      },
      "duties": ["do-not-share"]
    }, {
      "grant": ["read", "write", "delete"], "when": {
      // Proof request (see RFC 0037) specifying that
      // the User has signed up successfully
      },
    },
    ],
  // Is there an authority to whom requests for redress can
  // be made, if one party feels like another violates
  // the governance framework? 
  "redress": {
      "uri": "http://ubs.org/redress"
  }
}
```

The CBAS component will most likely impose some restrictions on the supported structures of listed [SGL statements](https://github.com/evernym/sgl) in order to simplify the APIs and implementation efforts. 

The exact semantics for describing `privileges` and associated rules are not final. A more concrete / formally defined set of rules will be provided shortly, after further research / definition efforts.

## Verifiable Credential Data Model

We intend to use the [Verifiable Credentials](https://www.w3.org/TR/vc-data-model/) (VC) specification as a foundation for expressing object capabilities. By doing so, we hope to be able to reuse existing tooling (e.g. libraries for creating / consuming Verifiable Credentials) and infrastructure (e.g. the Universal Issuer / Verifier, Credential schema / metadata registries, revocation registries, backup services, etc.). We also hope to benefit from previous / ongoing / future interoperability efforts in the Verifiable Credential area.

For the initial implementation of the CBAS module, we intend to support the JSON-LD representation of Verifiable Credentials ([example](https://www.w3.org/TR/vc-data-model/#example-1-a-simple-example-of-a-verifiable-credential)), as well
as JSON-LD Verifiable Presentations ([example](https://www.w3.org/TR/vc-data-model/#example-2-a-simple-example-of-a-verifiable-presentation)) for expressing (potentially delegated) object capabilities. At later stages of development, alternative Verifiable Credential formats (e.g. [Zero Knowledge](https://www.w3.org/TR/vc-data-model/#zero-knowledge-proofs) based implementations) as well as alternative Verifiable Presentation variants can be integrated.

CBAS imposes additional requirements on the Verifiable Credential structure (mandated by specifications it builds upon) in order to model authorizations, constraints, and delegated capabilities, these are defined below:

### Describing permissions

The CBAS module relies on Verifiable Credentials to encode permissions and permissions / capabilities. Permissions are expressed in the `credentialSubject` field in the Credential, structured similar to the one described in the [Chained Credentials ARIES RFC](https://github.com/hyperledger/aries-rfcs/tree/33dc2a8b3235a54ecb483f6020bb20b977918978/concepts/0104-chained-credentials#reference).


```javascript
{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://www.w3.org/2018/credentials/examples/v1"
  ],
  "id": "claim:rootCapability",
  "type": ["VerifiableCredential", "Proxy.D/ExampleBackupTF"],
  "issuer": "did:example:backup_service_operator",
  "issuanceDate": "2020-01-01T19:73:24Z",
  "credentialSubject": {
    "holder": {
      "id": "did:example:backup_service_user",
      "authorizations": ["read", "write", "delete", "delegate"],
    },
    "proxied": {
      "permissions": {
        "grant": ["read"],
        "when": {"contains": {"authorizations": "read"}}
      },
      "resource": {
        "locations": ["http://backup-service.com/record/abc"],
        "datatypes": ['application/zip']
      }
    }
  },
  "proof": {
    "type": "RsaSignature2018",
    "created": "2017-06-18T21:19:10Z",
    "proofPurpose": "assertionMethod",
    "verificationMethod": "did:example:backup_service_operator#keys-1",
    "jws": "eyJhbGciOiJSUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..TCYt5X
      sITJX1CxPCT8yAV-TVkIEq_PbChOMqsLfRoPsnsgw5WEuts01mq-pQy7UJiN5mgRxD-WUc
      X16dUEMGlv50aqzpqh4Qktb3rk-BuQy72IFLOqV0G_zS245-kronKb78cPN25DGlcTwLtj
      PAYuNzVBAh4vGHSrQyHUdBBPM"
  }
}
```

The values are based on the structure defined in the [Delegation section](https://github.com/hyperledger/aries-rfcs/tree/33dc2a8b3235a54ecb483f6020bb20b977918978/concepts/0104-chained-credentials#delegating-permissions) of the aforementioned Chained Credentials RFC.

- `holder` - used to express information about the holder (e.g. a role, a list of authorized actions). These values can be used when evaluating the queries listed in the `permissions` section to determine which authorizations can be granted to the capability holder).
- `proxied.permission` - used to describe rules under which permissions can be granted to the holder (in the form of a [SGL rules](https://github.com/evernym/sgl)). The associated rules can also be advertised through in a published, machine-readable, Trust Framework document. 
- `proxied.resource` - field is used to associate information about the resource to which access is being granted. This information can aid the client in initiating interactions / presenting capabilities.

### Delegation

As outlined in the [Chained Credentials Aries RFC](https://github.com/hyperledger/aries-rfcs/tree/33dc2a8b3235a54ecb483f6020bb20b977918978/concepts/0104-chained-credentials#special-sauce), obeying a number of special conventions (e.g. including a "provenance proof") above the core requirements of an ordinary Verifiable Credential can be used to model delegation (potentially constrained) of authority.

Below is an example of a delegated capability derived from the Verifiable Credential created in a previous example:

```javascript
{{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://www.w3.org/2018/credentials/examples/v1"
  ],
  "id": "claim:delegatedCredential",
  "provenanceProofs": {
    [["authorization"], {
      "@context": [
        "https://www.w3.org/2018/credentials/v1",
        "https://www.w3.org/2018/credentials/examples/v1"
      ],
      "type": "VerifiablePresentation",
      "verifiableCredential": [{
        // Parent Verifiable Credential
      }],
    }]
  },
  "type": ["VerifiableCredential", "AlumniCredential"],
  "issuer": "https://example.edu/issuers/565049",
  "issuanceDate": "2010-01-01T19:73:24Z",
  "credentialSubject": {
  // Clipped for brevity
  },
  "proof": {
    "type": "RsaSignature2018",
    "created": "2017-06-18T21:19:10Z",
    "proofPurpose": "assertionMethod",
    "verificationMethod": "https://example.edu/issuers/keys/1",
    "jws": "eyJhbGciOiJSUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..TCYt5X
      sITJX1CxPCT8yAV-TVkIEq_PbChOMqsLfRoPsnsgw5WEuts01mq-pQy7UJiN5mgRxD-WUc
      X16dUEMGlv50aqzpqh4Qktb3rk-BuQy72IFLOqV0G_zS245-kronKb78cPN25DGlcTwLtj
      PAYuNzVBAh4vGHSrQyHUdBBPM"
  }
}
```

The `provenanceProofs` property of the Verifiable Credential includes a Verifiable Presentation of the Verifiable Credential created in a previous example. The presentation serves as a base / root for the derived capability.

Besides linking the derived credential to the root credential, a number of extra properties can be modified during delegation, specifically in the form of adding constraints:

A capability VC may include one or more constraint objects, which describe additional restrictions which must be considered when evaluating the Verifiable Credential.

```javascript
  "credentialSubject": {
    "holder": {
      "constraints": {
        "startTime": "2020-05-20T14:00Z",
        "endTime": "2020-06-20T14:00Z",
      }
    }
  }
```
