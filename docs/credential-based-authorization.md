# Credential Based Authorization
This document provides a detailed overview of how Credentials are used for Authorization within the Alkemio platform. 

## Background
The prototype version of Alkemio platform used centralized groups for managing roles, which then determined what access and actions could be carried out. All role assignment was then done centrally, which is actually the typical model that is used in the vast majority of systems currently deployed. However this does not match with being able to operate the platform in a decentralized manner, so the choice was made to use Credentials for all authorizations on the platform. 

A simple analogy to explain the difference. A person wants to get access to a conference:
* **Access Control Lists**: the person goes to the conference entrance, and identifies themselves. Their identity is checked against a list at the door by the representative. If the identity is on the list the user is granted access.
* **Credentials**: the person goes to the conference entrance, and presents a credential tied to their identity. If that credential is valid, and the conference rules state that a person holding a valid instance of that credential can access the conference, then the person is granted access.

Note that the latter is inherently more scalable and decentralized as there is not a centralized list that is used at the entrance to the conference. 

The medium term goal is to move to using full SSI with Verified Credentials. However as an intermediate step the platform is moving to using simple Credentials , so not cryptographically secure. This works as the platform is still centralized but it is important to switch already to Credential based authorization now so that the path to using decentralized authorisation is clear moving forward. 

## Terminology
The terminology used within Alkemio for Credential Based Authorization has borrowed heavily from initiatives such as [Cability Based Authorization System](https://essif-lab.eu/capability-based-authorization-system-by-jolocom/). This is to both learn from the experience within the wider SSI field, as well as to make the subsequent move to using Verified Credentials easier. 

Terms:
* **Credential**: a combination of a **Type** and **Resource** that can be assigned to an Agent.  
* **Resource**: the Entity __to__ which the credential is bound e.g. Challenge
* **Privilege**: A particular type of access that can be granted (e.g. create, update, read, delete, grant etc)
* **Authorization Policy**: specifies which Credentials grant what type of access (i.e. what privileges) for a particular entity. 

Initially the 
* **Trust Framework**: a definition of privileges and how access is delegated in the system


Note: For now the Subject is implicit in Cherrytwist by virtue of the Credential being held by a User. For Verified Credentials this needs to be part of the cyrptographically secured credential. 

## Phasing
Now:
* Switch to Credentials for Authorization: Remove usage of Groups for global authorization
* Extend to Context Aware Authorization: Add support for context aware authorization, and expand to usage in Lifecycle triggered actions

Note by context aware authorization is meant that the authorization can check the actual instance of a particular entity e.g. challenge-7 vs Challenge, neil-smyth vs User.

Later:
* Trust Framework: declaratively specify the trust framework, with defined privileges, roles etc similar to what is done in the Simple Grant Language. The Trust Framework would then be used at runtime to evaluate authorization. 
* Customizable Trust Frameworks: Allow different contexts to have the different Trust frameworks. 

# Implementation

## Phase 1 - Global Credentials; Initial context guards

### Server
Add support for Credentials being held by Users:
* Add a Credential entity to the domain model
* Add an Agent entity to the domain model that holds + manages Credentials
* Extend the User entity to have a single Agent

Note: the implementation of Credentials is generic, and not tied to Authorization i.e. other Credential types could easily also be stored in the same mechanism. 

Expand Authorization to use AuthorizationCredentials
* New AuthorizationService that takes care of 
    * Assigning and removing AuthorizationCredentials from Users
    * Retrieving all Users that hold a particular AuthorizationCredential
* New AuthorizationRules guard that can work with Credentials as for granting / refusing access
    * This replaces the usage of UserGroups
    * Essentially this is an array of checks that is executed to see if the User is authorized
* New Decorator for specifying what Global roles are allowed
* New AuthorizationRule implementations for
    * Checking if a user holds a Credential for a global role
    * Checking if the access is allowed in the context of self-management

Authorization definitions and assignment:
* Global Roles:
    * `global-admin`: able to assign `global-community-admin` && `global-challenge-admin` credentials; able to do everything.
    * `global-registered`: for if we allow other types of access later
    * `global-community-admin`: albl to create users, groups; able to assign users to groups, update organisations
    * `global-challenge-admin`: able to add create Ecoverse's / challenges / opportunities

Functionality updates:
* Bootstrap to assign Credentials to newly created users
    * Replacing usage of UserGroups for ecoverse-admin, global-admin etc. 
* Creating a new user autotmatically assigns the new user the credential `global-registered`
* Authorization api
    * Add new query to get all users that match a credential search input
    * Expand AuthorizationService to support querying all users with a given Authorizationredential
* Credentials instead of Restricted UserGroup names
    * Replace usage of members UserGroup by checking for users with a credential for a given community
    * Remove usage of restricted groups on Community, Organisation. 
    * Also remove supporting functions that were on UserGroup service.
* Context dependendent authorization
    * New authorization rule to check for community membership
* Dropped focal point from UserGroup? Never been used
* Remove memberOf as can now use credentials instead of group memberships
* Dropped UpdateReference / UpdateTagset mutations as they should only be called in the context of a higher level update.


## Phase 2 - Expand Context Dependent Authorization
Extend functionality to support the issuing and usage of context credentials i.e. a credential that is tied to a particular resource(s).


## Givens
Distinguish between the _granting_ of a Credential, and _validating_

### Granting:
Specifies the resource to which access is being delegated. 
* Assigns a credential to an Agent to represent the access being granted
    
### Validating
Validating is the action checking whether a particular action is authorized. It has the following parts:
* **Agent**: The Agent carrying out the action has credentials that determine if it has sufficient privileges to be able to carry out that action
    * The Agent is available via the User that is authenticated i.e. it comes from the request / event that the server is responding to
    * Note: the credentials are on the Agent so that other entities such as Challenges / Organisations can also carry out actions. 
* **Target**: The context is the particular entity upon which a command (action) is being carried out.
    * In particular the fact that the actual entity being acted on is *not* guaranteed to available to guards on the GraphQL implies that doing authorization via guards on the Graphql api is not sufficient. 
    * For global roles this was not an issue hence graphql guards were sufficent. 
    * However for context aware authorization (e.g. an agent can update a particular ecoverse) then using graphql guards is not sufficient.
        * Note: context aware authorization also works well with the actor model in general, which is expected to be expanded on near term. 
* **Rules**: The authorization rules that are determine which privileges are granted in a particular context.
    * Importantly the set of Rules is fairly fixed, at least for now. 
        * Later there may need to be some flexibility / make the rules data dependent.  
    * The set of rules are picked up from the Target that is being acted upn. 


### Server
The key design elements are:
* All entities to have an authorizationRules field, that stores a JSON string description of the authorization rules for that entity
* Authorization service extended to have the following function:
    * **grantAccessOrFail(agent: IAgent, authorizationRules: string, privilege: Privilege, msg: string): Promise<Boolean>**
        * agent: the holder of credentials that is carrying out the action
        * authorizationRules: the JSON representation of what actions are allowed on the entity
        * privilege: the type of action being carried out: create, read, update, delete
        * msg: a helper string that will be returned as part of the ForbiddenException if access is not granted. 
        
Key is that the rules are fairly static, and they are assigned to an entity at the time that it is created. A worked example:
* A Community entity is created as part of creating a Challenge
* The Community entity receives a rule that states that an agent with a credential matching (ecoverse-admin, ecoverse-uuid) can have the privileges "create, read, update, delete". 
* When a UserGroup is created on the Community that entity receives a copy of the rules from the Community
Thus ensuring that an Ecoverse-admin for that Ecoverse is able to update all aspects of the Community and its containing entities.   

A sample authorization rules:
cerdentialRules: [
    {
        type: ecoverse-admin,
        resourceID: '1234',
        authorizations: ['read', 'update'],
    },
    {
        type: global-admin,
        resourceID: '',
        authorizations: ['create', 'read', 'update', 'delete'],
    }
]

### Contextual AuthorizationCredentials
The following context credentials are initially supported:
* ecoverse-admin
* ecoverse-member
* challenge-admin
* challenge-member
* user-update: grants a user the privilege to be able to update their own profile

### Authorization Logic update
The key change is that all guard based authorization logic will be replaced by calls within the Services. 



## Phase 3: Leverage a claims framework
It is likely that there will need to be additional flexibility later regarding the authorization rules that are supported by the platform. The rules introduced in phase 2 are fairly rigid and custom - so later would like to move to leverage a claims authorisation package.

One such candidate is [CASL](https://casl.js.org/), which is also supported by NestJS.
 






## Reference

### NestJS / Passport
* UseGuards decorator, from NestJS framework, injects a guard around a class or method. 
    * Pass in the guard implementation to use
    * A new instance is created for each time it occurs on the execution path
* Our implementation, GqlGuard, is derived from the PassportGuard
    * Actually it does not have any real GraphQL knowledge in there
    * Usable for simple method authorization?
* If put mutliple guards then they are an 'And', but inside a Guard the logic is flexible


### Material
* [Machine Readable Governance Frameworks](https://github.com/hyperledger/aries-rfcs/blob/master/concepts/0430-machine-readable-governance-frameworks/README.md)
* [Simple Grant Language (SGL)](https://evernym.github.io/sgl/)
* Capability Based Access System (CBAS)
* [Trust Framework RFC](https://github.com/hyperledger/aries-rfcs/blob/master/concepts/0103-indirect-identity-control/guardianship-sample/trust-framework.md)







===========================
## Examples
### Amazon
![](https://i.imgur.com/zRznfHe.png)

### CBAS
![](https://i.imgur.com/MDmqsIK.png)


