# Cherrytwist - Technical Design Introduction
This document provides an introduction to the technical design for Cherrytwist. It is still evolving and input is very welcome. It is assumed that the reader has already read the [Conceptual Design](./conceptual-design.md) document. 

The architecture is described along the following aspects:
*   Design Principles
*   Users: Accounts, Identities & Access Management
*   Data model
*   Logical Design
*   Configuration & Deployment

In some cases this document will also provide a rationale for decisions that have been made - and of course the initial reference implementation of Cherrytwist also requires choices to be made regarding actual languages used, hosting, user management stacks etc. 


# Design Principles

The following high level choices guide the technical design:

*   **Build for the future, but expose in familiar ways**
    *   E.g. web 3 under the hood, but exposed using web 2.3 for the user
    *   Leverage latest proven technical solutions (e.g. SSI, Smart Contracts), but shield the end user from the technical complexity until ready
    *   This implies creating the platform with digital identities (SSI) for all entities in the system with associated wallets etc, but that this is all kept internal to the platform in first versions.
*   **Maintain flexibility to evolve**
    *   In practice this means efforts to minimise / isolate deployment dependencies
    *   The initiative is still learning and evolving, so it is deemed prudent to avoid establishing long term dependencies that may not be the right choice.
*   **Regulated value (e.g. money) out of band i.e. via familiar channels / units**
    *   In case the platform should enable the transfer of regulated value then a whole range of obligations fall on any organisation deploying a Cherrytwist instance - e.g. KYC, AML, insurance etc.
        *   Worth noting that the Ecoverse Host can play a role in this scenario as facilitator of the regulated value exchange e.g. hold money in escrow for stakeholders + then pay parties based on signals from the platform that a Project has completed successfully
    *   Similarly, the Ecoverse Host for now is able to specify the currency to be used in the Ecoverse challenges (e.g. euro, dollar). 
*   **Formalise Trust**
    *   This implies making explicit how agreements are made between actors of the platform, and how the representation of those interactions can be trusted. 
    *   One example of this is the deployment of a smart contract platform within the context of and limited to the Ecoverse. 
        *   Of course any Smart Contract platform is dependent on the network of nodes & their global state for trust. In initial versions the Ecoverse Host would be responsible for running this platform as a standalone island - so not connected to global trust sources e.g. Ethereum main chain.
        *   This ties into earlier principles around building for the future but exposing in familiar paradigms, whilst retaining platform evolution flexibility both from a runtime dependency & upgradability point of view.
*   **Simple to operate**
    *   The platform should be easy to configure, deploy and run - also implying that the number of external dependencies needs to be very carefully managed. 

It is worth noting that some of these choices, especially regarding the Ecoverse Host role in mediating regulated value exchange & trust, are made in the context of getting a first version of Cherrytwist deployable. The expectation is fully that these aspects can be decentralised as the platform and wider context within which it is used matures - and indeed the architecture is set up to ensure that this is feasible in an incremental manner.


# Users: Accounts, Identities & Access Management

This section describes the user model that is used for all interactions with Cherrytwist.  


## User 

One of the key challenges faced by Cherrytwist is how to combine the different user / access management approaches implied from classical web deployments versus those from Self Sovereign Identity (SSI) approaches. 

To manage this duality, the core entity that is reflected in Cherrytwist is the **User**, which then combines both a web2 as well as a web3 ways of interacting. This is reflected in the following terminology:
*   **Account**: Initially, all interactions with the Cherrytwist platform require an account. This is the Web2 pattern familiar to most end users
*   **Identity**: All users of Cherrytwist also have a Self Sovereign Identity (SSI) created on their behalf. This identity can then further have artifacts such as wallets that the identity controls, and can interact with other web3 artifacts such as smart contracts. The identity is initially largely invisible to Users.

At a high level this then implies that ![drawing](./images/Web2plusWeb3.png)

This can also be seen as putting a web2 layer around a web3 core. Critically, all identities (SSI) created by the platform are managed by the platform on behalf of the user i.e. acting as a proxy for actions authorised by the user via their Account. The goal is to have these identities held outside of the platform - but that is in itself a journey with multiple steps on the way.

Worth noting that this hybrid approach also allows platform users to leverages some of the benefits of Self Sovereign Identity (SSI) such as pairwise unique identifiers etc for peer interactions, albeit with the Ecoverse as a trusted party until that trust role is decentralised. 


## User Group

Users can be groups into a sets using the concept of a UserGroup. A UserGroup can then have a focal point identified that would have a special role within that group of users.


## Web2 Access Management

The operation and management of the initial version of Cherrytwist is primarily via web2, for which the following applies:
*   **Account**: interactions with CT require an account. There are multiple types of accounts:
    *   User Account: natural person
    *   Service Account: for services / bots that interact with the system.
*   **Security Role**: is a set permission policies that determines what an Account can and cannot do in CT (i.e. authorisation). A role is intended to be assumable by any CT Account who needs it assigned. 
*   **Security Group**: is associated with a specific User Group, and can have one or more Security Roles assigned. Each user in the associated User Group automatically inherits these roles.


## Web3 Identities

A subset of the entities within CT have DID’s associated with them. These are the long running, and potentially independent actors in the system i.e.
*   Ecoverse
*   Users
*   Organisations
*   Challenges
*   Opportunities
*   Projects

As aspects of SSI mature the intention is to allow more of the platform interaction and management to be carried out using SSI mechanisms e.g. claims, signing etc. 

# Logical Data Model

The following diagram shows at a high level the key entities in use within Cherrytwist:

![Logical Data Model](./images/LogicalDataModel.png)

This logical data model attempts to keep to a minimum, at least initially, the set of entities that are represented in the platform, while still being able to reflect the types described in the conceptual design. The rationale for this is to avoid bringing in implicit context from known deployments of Challenges. 

The key entities in the model are:
*   **Ecoverse**: The root entity, which has an associated hosting organisation.
*   Community Entities:
    *   **User**: The primary way of interacting with the platform    
    *   **UserGroup**: To allow the aggregation of users into groups, which may or may not have a focal point that is in charge of the group
    *   **Organisation**: To reflect legal entities that interact with the platform via one or more users. 
*   Challenge related Entities
    * **Challenge**: the shared goal / vision. Progress towards the goal will require multiple stakeholders collaborating on a non-trivial journey, building up understanding, community and resources as the journey progresses.
    * **Opportunity**: a potential significant step towards the shared goal. Likely that multiple Opportunities are identified in the context of the Challenge, each with their own lifecycle & that need to be ranked / prioritised. 
        * **ActorGroups**: The different groups of actors who are identified and/or engaged in realising this Opportunity
        * **Aspects**: The aspects to solutions for this Opportunity
    * **Project**: a defined outcome, formalised as an agreement between parties collaborating in the context an Opportunity. Potentially multiple projects needed to deliver an Opportunity.
*   **Context**: The shared understanding, at either Ecoverse or Challenge level. 
*   **Web 2 Account**: A web2 based account that allows a user to interact with the platform in a familiar manner. A user can login if there is a known user whose email matches the email for the Account from the Identity Provider.
*   **SSI**: Self Sovereign Identity (SSI) entities
    * **DID**: the persistent decentralised identifier as per W3C standard
    * **DDO**: The document describing the DID, with roles etc. 
* **Template**: a template is a set of entity definitions that are used by an Ecoverse. For example holding different types of users (i.e. what tagsets are expected on a user and what value selection is allowed). 

To facilitate flexible usages of this data model, most key entities have either a **Tagsets** or a **Profile** (containing multiple Tagsets) associated with them, allowing for easy filtering + connecting
*   Tags allow for a fairly unstructured entity relationship model to be used in a variety of ways.
*   There are likely to be a variety of tag sorts in use: user profiles, ecoverse tags (e.g. partner), host tags (crew) etc. 

There is also a *physical data model* that is how the logical data model is stored in the underlying datastore. However for using the system that should be hidden from normal usage.


# Logical Design

The logical layers to the Cherrytwist architecture:
*   **Interaction**: for the interfaces used by the users as they interact within the context of an Ecoverse / Challenge
*   **Server**: for managing all aspects of the Ecoverse & Challenge lifecycle
*   **Data Storage**: for storing the persistent data related to challenges
*   **Smart Contract Platform**: for executing of agreements

The layering is shown in the following diagram:

![Logical Layers](./images/DesignLayers.png "design layers")


### Interaction

The server maintains the long term representation of the Challenge, with which users interact in many different ways over the lifecycle of the Challenge. As such the primary goal of the Interaction Layer is to ensure that many different types of interactions are feasible, while of course also allowing easy adoption of the platform via one or more reference user interfaces.

Examples types of interactions:
*   Dedicated website
*   Mobile clients
*   Immersive game / VR experience
*   Extensions of UX / Web frameworks

For the initial version of Cherrytwist, the focus will be on extending an existing UX / Web framework to be able to expose and represent visually the information maintained with Cherrytwist. 

The frontend layer will be in charge of the following responsibilities:
*   Login/Sign up via SSO
*   Ecoverse navigation
*   Challenge navigation
*   Project initiating and launching
*   Connecting within the community
*   (later) Navigating / connecting between Ecoverses that have established trust relationships.

The frontend will be a stateless application and will communicate with the backend application via GraphQL API.

### User Interaction
As the Ecoverse Template (see later) will vary between Ecoverses, this implies that the user interface presented will also need to be tailored to the particular Ecoverse - so that it is aware of the user groups, challenge information etc.


## Server

The core of Cherrytwist, facilitating all other aspects of the platform. The core sub-components are shown in the following diagram. 

![Server Components](./images/DesignServerComponents.png "server components")

All interactions with the Server are via a set of APIs / services exposed by the platform, and actions are authorised based on the account associated with the user.
* **Accounts & Access Handler**: This is likely in the first instance to be based on a hosted service such as Azure Active Directory.
* **GraphQL API**: This is the interface to the platform for all data exchange. 
* **Storage Handler**: To manage the different types of storage to be used by the platform. Manage the different platform identities (users, ecoverse, challenges, projects, teams, etc)
* **Identity, Wallets & Smart Contracts Handler**: Typically DIDs are stored as part of a users or identity registry in a decentralized network as part of a Smart Contract state. To reduce the potential dependency with an external blockchain network it could be possible to store the DID and DDO in the general purpose database where the key is the DID and the value is the DDO content. Similarly, initially wallets associated with an identity would be stored in the database but could be later moved to another type of storage. 


## Data Storage

The artifacts managed by the server need to be safely and securely managed by the server. The following are key artifacts to be stored by the platform:
*   **Entities from the data model** i.e. Ecoverse, Challenges & Project state
*   **Digital Identities**, so both the DID & the DDO associated with a DID
*   **Smart Contracts**, on an executable platform
*   **Digital Wallets**, for the management of digital assets such as recognition tokens. Note that no value that falls under regulation would be support initially.

The following locations are identified for the storage of data associated with Cherrytwist:
*   **Database**: for the data and the relationships between the entities, metadata etc. This could be relational or NoSQL based.
*   **Ledger**: for smart contracts, and transfer of value. The smart contracts can cover a variety of usages on the platform e.g. execution of projects, digital identities & the definition / control of any tokens created by the Ecoverse.
*   **IPFS**: for any documents that should be stored in a content addressable way (CAS), whereby the data is also potentially distributed (resilience etc). 
*   **Vault**: For management of keys in a readily accessible but secure way. 
    *   Note: it may be that later users of the platform would want to store value that would warrant usage of cold storage, but the expectation is that this would be done leveraging a third party service. 

The first versions of the platform will likely leverage the Database primarily, with other options added in due course. 


## Smart Contract Platform

In order to manage agreements via smart contracts, a smart contract platform will be deployed as part of the architecture. 

Initially this smart contract platform will be private, so relying on the Ecoverse host for the state of the platform. Solutions such as Quorum or other private Ethereum based chains are likely in order to be able to leverage the wide set of capabilities and infrastructure currently available. 

Later it should be possible to integrate either with public chains based on Ethereum or alternative smart contract platforms as makes sense. 


# Configuration & Deployment

This section details out how an Ecoverse instance is deployed, and then how entities such as Challenges hosted in the Ecoverse are created.

A core driver is to ensure that the platform is configurable, and in a replicable way - both to enable reliably development / deployment but also to ensure that over time a community pool of best practice templates emerges that can be leveraged for new innovations in the field of Cherrytwist.

For this inspiration is taken from other process template environments such as Azure DevOps, GitHub Actions etc. 


## Base Install

The Cherrytwist platform is initially deployed in an “empty” state, without an Ecoverse. It does however already include the following roles:
*   Global admin: able to deploy templates ane manage assignments to key platform roles
*   Ecoverse admin: able to carry out all sub roles, create new user groups etc
*   Community admin: able to manage users, existing user groups, membership of security groups, etc
*   Members: able access to the full query api. Able to see details of the challenges that the user is a member of. Able to edit their own user profile.

The platform can have a single Ecoverse deployed onto it. 


## Populating an Ecoverse
This section describes the steps and supporting entities for working with an Ecoverse. 

### Structure
The structural setup of the Ecoverse is held in a “Ecoverse Setup file, external to Cherrytwist that contains:

*   The Identity for the Ecoverse e.g. Odyssey, YES!Delft, OdysseyTest etc
*   The description of the Ecoverse, including all related information such as the Ecoverse Host etc
*   The UserGroups to be used at the Ecoverse level
    *   E.g. Jedis, ChallengeLeads, Crew, …

### Templates
A Template contains Entities that are used by the Ecoverse to create Entities with flexible usage patterns. Examples of the kind of information that could be stored within a Template include:
* Definitions of types of users, such as what tagsets are expected on a particular user type (e.g. skills, interests, ...)
* Challenge states, user groups, ...
* Opportunity aspects, actor groups, ...

The template setup is currently only used for holding User profile setup data, but the approach is designed to be extended and support e.g. having multiple Challenge / Opportunity / Project templates available for selection by Ecoverse users.

The evolution of the Ecoverse instance from deploying a template and then instantiating a challenge is shown below:

![Template deployment](./images/DesignTemplates.png "Template deployment")

#### Challenge, Project & User Group Templates

Further, the following entities will have enhanced templating support:
*   Challenge Template
*   Opportunity Template
*   Project Template
*   User Group Template

In particular the Project Template is likely to have multiple variations possible to reflect the multiple ways a project may want to be executed. An Opportunity Lead would then select a template to use when launching the challenge.

These additional templates may be part of the Ecoverse Template or uploaded later, and they initially all require the “global admin” role.


### Exportable & Uploadable Representation

In addition to supporting the usage of Templates, the platform also needs to be able to export and import fully defined Challenges. These should reference a Challenge Template that they are based on and then provide the actual data to be populated into that Challenge instance. 

Typically this would be used during the formation of an Ecoverse when Challenge data is being migrated from another environment, or between ecoverses. 


