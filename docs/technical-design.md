# Alkemio - Technical Design Introduction
This document provides an introduction to the technical design for Alkemio. It is assumed that the reader has already read the [Conceptual Design](./conceptual-design.md) document. 

The architecture is described along the following aspects:
*   Design Principles
*   Users: Accounts, Identities & Access Management
*   Data model
*   Logical Design
*   Configuration & Deployment

In some cases this document will also provide a rationale for decisions that have been made - and of course the initial reference implementation of Alkemio also requires choices to be made regarding actual languages used, hosting, user management stacks etc. 


# Design Principles

The following high level choices guide the technical design:

*   **Build for a decentralized future, but expose in familiar ways**
    *   E.g. web 3 under the hood, but exposed using web 2.3 for the user
    *   Leverage latest proven technical solutions (e.g. SSI, Smart Contracts), but shield the end user from the technical complexity until ready
    *   This implies creating the platform with digital identities (SSI) for all entities in the system with associated wallets etc, but that this is all kept internal to the platform in first versions.
*   **Maintain flexibility to evolve**
    *   In practice this means efforts to minimise / isolate deployment dependencies
    *   The initiative is still learning and evolving, so it is deemed prudent to avoid establishing long term dependencies that may not be the right choice.
*   **Regulated value (e.g. money) out of band i.e. via familiar channels / units**
    *   In case the platform should enable the transfer of regulated value then a whole range of obligations fall on any organisation deploying a Alkemio instance - e.g. KYC, AML, insurance etc.
        *   Worth noting that the Ecoverse Host can play a role in this scenario as facilitator of the regulated value exchange e.g. hold money in escrow for stakeholders + then pay parties based on signals from the platform that a Project has completed successfully
    *   Similarly, the Ecoverse Host for now is able to specify the currency to be used in the Ecoverse challenges (e.g. euro, dollar). 
*   **Formalise Trust**
    *   This implies making explicit how agreements are made between actors of the platform, and how the representation of those interactions can be trusted. 
    *   One example of this is the deployment of a smart contract platform within the context of and limited to the Ecoverse. 
        *   Of course any Smart Contract platform is dependent on the network of nodes & their global state for trust. In initial versions the Ecoverse Host would be responsible for running this platform as a standalone island - so not connected to global trust sources e.g. Ethereum main chain.
        *   This ties into earlier principles around building for the future but exposing in familiar paradigms, whilst retaining platform evolution flexibility both from a runtime dependency & upgradability point of view.
*   **Simple to operate**
    *   The platform should be easy to configure, deploy and run - also implying that the number of external dependencies needs to be very carefully managed. 

It is worth noting that some of these choices, especially regarding the Ecoverse Host role in mediating regulated value exchange & trust, are made in the context of getting a first version of Alkemio deployable. The expectation is fully that these aspects can be decentralised as the platform and wider context within which it is used matures - and indeed the architecture is set up to ensure that this is feasible in an incremental manner.

# Logical Data Model

The goal of the Alkemio platform is to manage the shared representation of Challenges and to faciliate collaboration within the context of those Challenges. To support this, the platform has a *logical data model* for storing the shared representation and for facilitating the collaboration. 

The following diagram shows at a high level the key entities in use within Alkemio:

<p>
<img src="images/design-logical-data-model.png" alt="Logical Data Model" width="600" />
</p>

This logical data model attempts to keep to a minimum, at least initially, the set of entities that are represented in the platform, while still being able to reflect the types described in the conceptual design. The rationale for this is to keep the approach as general as possible, allowing further innovation how Challenges are managed. 

The key entities in the model are:
*   **Challenge:**
    * **Ecoverse**: the hosting environment for the Challenges, which has an associated hosting organisation. 
    * **Challenge**: the Challenge itself, including the shared understanding, community and tracked collaboratin. 
    * **Opportunity**: a potential significant step towards the desired outcomes of the Challenge. Likely that multiple Opportunities are identified in the context of the Challenge, each with their own lifecycle & that need to be ranked / prioritised. 
*   **Community**:
    *   **User**: The primary way of interacting with the platform    
    *   **UserGroup**: To allow the aggregation of users into groups, which may or may not have a focal point that is in charge of the group
    *   **Organisation**: To reflect legal entities that interact with the platform via one or more users.   
    *   **Profile**: a shared entity across Users, UserGroups and Organisations to represent their Profile in a consistent way. It manages the avatar of the entity, tagsets giving meta-data about the entity (e.g. industry, skill sets, intersts) and references for links related to the entity (e.g. website of an organisation, linkedin profile for a user etc)
*   **Context**: 
    * **Context**: The shared understanding, at either Ecoverse or Challenge level. 
    * **Ecosystem Model**: A representation (model) of the different types of **ActorGroups + Actors**, plus later the value types each receives / brings into the Ecosystem. 
    * **Aspects**: The aspects to solutions being worked on in the context of the Challenge
*   **Collaboration**: 
    * **Project**: a defined outcome, formalised as an agreement between parties collaborating in the context an Opportunity. Potentially multiple projects needed to deliver an Opportunity.
    * **Relation**: an interaction to be tracked between two Users / Organisations / Groups related to a particular Opportunity
*   **Agents**: representing an entity in the platform in interactions with other entities. Entities with Agents include: Users, Organisations, Challenges, Ecoverses etc. 
    * **Credentials**: a list of credentials held by the Agent. Important to note is that there are two types of credentials that can be associated with an Agent (a) simple credentials, which are managed by the platform (b) verified credentials, which are familiar W3C Verified Credentials. 

To facilitate flexible usages of this data model, most key entities have **Tagsets** associated with them, allowing for easy filtering + connecting
*   Tags allow for a fairly unstructured entity relationship model to be used in a variety of ways.
*   Tags are held in Tagsets that are named collections of Tags. 

There is also a *physical data model* that is how the logical data model is stored in the underlying datastore. However for using the system that should be hidden from normal usage.

# Logical Design

The logical layers to the Alkemio architecture:
*   **Clients**: the actual devices being used to interact by the users 
*   **Interaction**: the user experiences being provided to Users as they interact with other Agents for other entities in the platform.
*   **Server**: for managing all entities hosted by the platform

The layering is shown in the following diagram:
<p>
<img src="images/design-interactions.png" alt="Design interaction layers" width="600" />
</p>

## Interaction (aka user interfaces)

The server maintains the long term representation of all Challenges hosted by the platform. Users and Organisations interact in many different ways over the lifecycle of the Challenge. As such the primary goal of the Interaction Layer is to ensure that many different types of interactions are feasible, while of course also allowing easy adoption of the platform via one or more reference user interfaces.

Examples types of interactions:
*   Dedicated website
*   Mobile clients
*   Immersive game / VR experience
*   Extensions of UX / Web frameworks

For the initial version of Alkemio, the focus will be on extending an existing UX / Web framework to be able to expose and represent visually the information maintained with Alkemio. 

The interaction layer will be in charge of the following responsibilities:
*   Login/Sign up via SSO
*   Ecoverse navigation
*   Challenge navigation
*   Project initiating and launching
*   Connecting within the community
*   (later) Navigating / connecting between Ecoverses that have established trust relationships.

The interaction layer will typically be a stateless application and will communicate with the backend application via GraphQL API.

## Server

The core of Alkemio, facilitating all other aspects of the platform. The core sub-components are shown in the following diagram. 

<p align="center">
<img src="images/design-server-components.png" alt="Server Components" width="600" />
</p>


All interactions with the Server are via a set of APIs / services exposed by the platform, and actions are authorised based on the account associated with the user.
* **Authentication & Authorization**: Supporting multiple Authentication Providers, and managing User authorization once they have a valid session. 
* **GraphQL API**: This is the interface to the platform for all data exchange. 
* **Storage Handler**: To manage the different types of storage to be used by the platform. Manage the different platform identities (users, ecoverse, challenges, projects, teams, etc)
* **Identity, Wallets & Smart Contracts Handler**: Typically DIDs are stored as part of a users or identity registry in a decentralized network as part of a Smart Contract state. To reduce the potential dependency with an external blockchain network it could be possible to store the DID and DDO in the general purpose database where the key is the DID and the value is the DDO content. Similarly, initially wallets associated with an identity would be stored in the database but could be later moved to another type of storage. 

### Data Storage

The artifacts managed by the server need to be safely and securely managed by the server. The following are key artifacts to be stored by the platform:
*   **Entities from the data model** i.e. Ecoverse, Challenges & Project state
*   **Digital Identities**, so both the DID & the DDO associated with a DID
*   **Smart Contracts**, on an executable platform
*   **Digital Wallets**, for the management of digital assets such as recognition tokens. Note that no value that falls under regulation would be support initially.

The following locations are identified for the storage of data associated with Alkemio:
*   **Database**: for the data and the relationships between the entities, metadata etc. This could be relational or NoSQL based.
*   **Content Addressable Storage (CAS)**: for any images or documents that should be stored in a content addressable (CAS) way, whereby the data is also potentially distributed (resilience etc). Primarily used for now as a Content Distribution Network (CDN).
*   **Ledger**: for smart contracts, and transfer of value. The smart contracts can cover a variety of usages on the platform e.g. execution of projects, digital identities & the definition / control of any tokens created by the Ecoverse.
*   **Vault**: For management of keys in a readily accessible but secure way. 
    *   Note: it may be that later users of the platform would want to store value that would warrant usage of cold storage, but the expectation is that this would be done leveraging a third party service. 

The platform currently has both Database and CAS available - additional storage mechanisms will be added as needed later. 


# Security, Decentralization & Identity

This section describes how users interact with the platform in a secure manner. 

The key design goals driving the setup below are:
* **Security**: security is critical for a platform that is facilitating the collabration between multiple stakeholders: the trust is needed for interactions.
* **Decentralization ready**: the interaction pattern has to be able to support operating in a decentralized setup. By decentralized in this case we focus on having entities such as Challenges, Opportunities, Users, Organisations being able to interaction *without* being on the same server. The initial implementation does have all entities on the same server, but the interaction pattern has to support being decentralized. 
* **Extendable**: there are many ways that entities (e.g. users) can authenticate themselves, so the design needs to be flexible in terms of how entities are authenticated.  

## Actor Model & Agents
The target interaction pattern is the Actor Model, whereby entities are represented as independent actors that interact via the exchange of messages. This is a [well known approach](https://arxiv.org/vc/arxiv/papers/1008/1008.1459v8.pdf) that matches well to entities interacting in a decentralized manner. 

Core autonomous entities in the platform have an **Agent** that represents that entity in platform interactions. 

Entities with Agents currently are: User, Organisation, Challenge, Ecoverse & Opportunity. 

Each Agent has _Credentials_ which can interact on behalf of the user / entity with other elements of the platform. 

The usage of Agents is currently largely invisible to Users - but it is critical to setup the platform from the start with Agent based interaction patterns to enable moving more decentralized later.

Note: the text below primarily talks about Users for Authentication and Authorization, but the approach is also applicable for other entities with Agents e.g. Challenges, Organisations etc. 

## Authentication Providers
The Authentication of users is handled by Authentication Providers, a pluggable mechanism whereby multiple types of Authentication can be supported by the platform. After succesful Authentication, the platform retrieves the Agent for the User and that Agent is then used to carry out actions on behalf of the User. 

The platform currently supports [Ory Kratos and Ory Oathkeeper](https://ory.sh) for Authentication. The Ory platform is an open source, enterprise class Identity Provider. Note that the platform supports OIDC so that it is straightforward to integrate external authentications, e.g. Github, Google etc, into the platform. 

<p align="center">
<img src="images/security-authentication-ory.png" alt="Security: Ory Authentication" width="600" />
</p>

This design is chosen as it essentially isolates the Authentication process from the rest of the platform, so that it can be later extended / adjusted if needed with additional Authentication Providers. In particular one clear goal is to allow users to their own SSI, a topic of [very active development](https://identity.foundation/did-siop/) in the SSI community. 

In essence the Server is protected by Oathkeeper, which then relies on Kratos for the management of the identities (including trusted external authentications) that are allowed to authenticate. This is represented by the Authentication Zone in the diagram - currently only the Alkemio Server works with Oathkeeper / Kratos but as the platform gets more decentralized other services will likely need to also leverage Oathkeeper / Kratos for authenticating incoming requests. 

## Authorization: Credentials
All Authorization within the Alkemio platform is based on Credential Based Authorization. This section will provide a high level overview of this approach. For additional details please [read this document](./credential-based-authorization.md).

<p align="center">
<img src="images/security-authorization-framework.png" alt="Credential Based Authorization" width="600" />
</p>

Essentially upon a User authenticating, the following happens:
* The platform retrieves the Agent that represents that User
* The Credentials held by that Agent are used to decide if the requested action from the User (e.g. Read a Challenge details, update their profile, update a Challenge definition etc) is Authorized. 

Worth noting that this approach also allows for Credentials to be later held outside of the platform e.g. the user holds their own wallet and manages their own credentials. The ideal scenario for Users is that ultimately they can have their own SSI that can then control the Agents managed by the platform - or replace the platform Agents. 

# Configuration & Deployment

This section details out how an Ecoverse instance is deployed, and then how entities such as Challenges hosted in the Ecoverse are created.

A core driver is to ensure that the platform is configurable, and in a replicable way - both to enable reliably development / deployment but also to ensure that over time a community pool of best practice templates emerges that can be leveraged for new innovations in the field of Alkemio.

For this inspiration is taken from other process template environments such as Azure DevOps, GitHub Actions etc. 


## Base Install

The Alkemio platform is initially deployed in an “empty” state, without an Ecoverse. It does however already include the following roles:
*   Global admin: able to deploy templates ane manage assignments to key platform roles
*   Ecoverse admin: able to carry out all sub roles, create new user groups etc
*   Community admin: able to manage users, existing user groups, membership of security groups, etc
*   Members: able access to the full query api. Able to see details of the challenges that the user is a member of. Able to edit their own user profile.

The platform is currently limited to have a single Ecoverse deployed onto it. 

## Populating an Ecoverse
This section describes the steps and supporting entities for working with an Ecoverse. 

### Structure
The structural setup of the Ecoverse is held in a “Ecoverse Setup file, external to Alkemio that contains:

*   The Identity for the Ecoverse e.g. Odyssey, YES!Delft, OdysseyTest etc
*   The description of the Ecoverse, including all related information such as the Ecoverse Host etc
*   The UserGroups to be used at the Ecoverse level
    *   E.g. Jedis, ChallengeLeads, Crew, …

### Templates
A key design goal for Alkemio is the sharing of best practices, so the platform needs to be customizable. This is achieved with Templates.

Templates support is high up the backlog for the platform, as it is important that key entities (e.g. Ecoverse, Challenge, Opportunity, Project etc) can be instantiated based on a particular template. 

Key areas where we expect that templates support will be useful include:
* Lifecycles: what is the lifecycle (also sometimes referred to as process) that your entities should follow? What do each of the states mean? What actions should be triggered e.g. if a Challenge is approved?
* Ecosystem Models: what types of actors / values are being exchanged within the ecosystem?
* Aspects: what are the key aspects for a Challenge / Opportunity that need to be captured within a given Challenge / Opportuntiy?
* Community: what type of community do you intend to create? What User groups make sense?
* Whiteboards: what collaboration canvases should be available and what is the goal of each canvas?
* (later) what are custom credentials / actions that can be carried out within a particular context?

The management of Templates needs to be both at the global level (global catalog), as well as locally controllable i.e. that an Ecoverse can provide a defined set of options (e.g. what Lifecycles can be used).

Currently there is only limited global templates support; if you have suggestions or wish to work with us on this aspect please engage!

#### Challenge, Project & User Group Templates

Further, the following entities will have enhanced templating support:
*   Challenge Template
*   Opportunity Template
*   Project Template
*   Community Template

In particular the Project Template is likely to have multiple variations possible to reflect the multiple ways a project may want to be executed. An Opportunity Lead would then select a template to use when launching the challenge.

These additional templates may be part of the Ecoverse Template or uploaded later, and they initially all require the “global admin” role.



