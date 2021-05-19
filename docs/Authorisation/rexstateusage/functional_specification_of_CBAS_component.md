Functional specification of the CBAS component
===========================================

## Outline

The CBAS component will allow the developer to easily create and consume Verifiable Credentials granting authority to perform specific actions on resources (guided by  grant and delegation rules described in a corresponding [Trust Framework document](./data_interface_specification_of_CBAS_component.md#TrustFrameworkDocument)). The CBAS component will allow developers to specify if the permissions they issue can be delegated further, as well as specify additional constraint objects which will be evaluated when the capability is invoked.

The following sections will describe the core functions exposed by the CBAS module.

*Please note that the API is provisional*
 
## Trust Framework document(Resource description)

The CBAS module allows the developers to leverage their existing Verifiable Credential implementations (including existing credential exchange protocols for issuing and requesting capabilities).

In order to simplify issuance / verification of credentials encoding various access rights for developers, the definition of authorization policies relevant for the CBAS user, a configuration / definition document describing the offered resources / services, as well as the associated authorization policies is required.

For this purpose, we intend to use a ARIES Trusft Framework document (specifically the Machine Readable variant)[LINK1, LINK2].

The CBAS component will allow developers to make use of Trust Framework documents they define to easily grant (potentially delegatable) Verifiable Credentials encoding permissions to perform certain actions outlined in the Framework document. The CBAS component also offers developers a simple way to evaluate presented Verifiable Credentials in the context of a Trust Framework document and get a list of allowed actions on resources.

Developers should be free to define custom authorization policies as they see fit to satisfy diverse use cases of varying complexity.

## Granting Authorizations

The CBAS component can make use of an underlying Verifiable Credential implementation (i.e. module or service for capable of issuing and verifying VCs) and an associated Trust Framework document to aid / simplify the issuance of object capabilities encoded as Verifiable Credentials.

The CBAS component will allow the developer to easily create Verifiable Credentials granting authorizations to perform specific actions on resources (guided by the listed grants and delegation rules encoded in a Trust Framework document).

The CBAS component will also allow developers to specify if the permissions they issue can be delegated further (given this is allowed by the Trust Framework / original `authorizations` set), as well as specify additional constraint objects (attenuations) which will be evaluated when the capability is presented.

The developer will be able to use their preferred Verifiable Credential issuance protocol (e.g. [ARIES RFC 0036](https://github.com/hyperledger/aries-rfcs/tree/master/features/0036-issue-credential)) to issue the delegated capabilities to the delegatee.

The developer will be able to use existing / integrated Verifiable Credential revocation mechanisms to revoke delegated authorizations (as well as all Verifiable Credentials derived from it).

## Evaluating Authorizations

Besides issuing delegated authorizations, the developer will frequently generate requests for Verifiable Presentations proving the counterparty is authorized to perform certain actions on a resource. The CBAS module aims to simplify this process by allowing the developer to easily generate queries for Verifiable Credentials (as defined by the underlying credential exchange protocol) based on defined authorizations. The resulting requests for credentials can be communicated to the counterparty attempting to access a prottected resource.

The CBAS module will also expose developer friendly APIs to determine what actions are permitted / if a particular action is permitted given a Verifiable Credential (presented by the counterparty) and a Trust Framework document. 

The evaluation process consists of first ensuring all presented Verifiable Credentials have been correctly signed, and are not mallformed. This step can be delegated to an underlying Verifiable Credential module.

CBAS must ensure that delegation happened correctly (i.e. constraints were only added, all delegations were authorized, no authorizations were added, etc.). Lastly, the presented capability can be evaluated against the policies / rules embedded within / advertised via a machine readable Trust Framework document.

## Capability Delegation, Attenuation, Revocation

In order to allow for more flexible and complex use cases, the capabilities issued by the CBAS library can be further delegated, by the subject / holder, to other agents (if the appropriate authorizations are allowed in the corresponding Trust Framework document).

As part of delegating authorization, the delegator can choose to restrict / modify the delegatee's resulting authorizations, e.g. via restricting the proxied permissions.

For instance, a developer holding a capability allowing them to "read", "write", and "delete" a resource can derive and delegate capabilities with restricted access (e.g. only granting the permission to "read") the resource to other agents / internal system components without having to share the original, more permissive, capability. 

The developer should also be able to add additional constraints (outlined in the Trust Framework)[link] document to the Verifiable Credential during the delegation phase. These constraints can serve as additional restrictions which must be considered / verified when evaluating the Verifiable Credential

Lastly, traditional Verifiable Credential revocation approaches can be employed within this component as well, to hanlde revocation of capabilities.

## Configuration / dependencies

The developer will be able to configure CBAS to use existing Verifiable Credential implementations (i.e. libraries for generating and consuming W3C Verifiable Credentials). This would allow developers to easily integrated with existing systems (without having to adopt a new Verifiable Credential format / representation), as well as select implementations catering to specific goals (e.g. privacy preserving ZKP based impelemtations, using JSON representation for VCs for smaller payload sizes and simpler signature verification / generation algorithms).

The CBAS component will be designed so that integration with remote VC issuance / verification services is supported as well (e.g. for simpler integration with existing ESSIF Lab software / functional componenets). to integrate the issuer and verifier functional components defined within the ESSIF Lab Functioanl architecture document to 

Since capabilities generated by CBAS are Verifiable Credentials, the developers should be free to use their preferred credential exchange protocols and credential revocation mechanisms, which should offer the aforementioned advantages as well.
