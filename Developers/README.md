# Developer Documentation
This is the starting point for understanding the development setup used within CherryTwist as well as how to get started with contributing to the project.

The project is ramping up, so please be patient if you find gaps (and point them out to us!).

Please read the [contributing](https://github.com/cherrytwist/.github/blob/master/CONTRIBUTING.md) guide lines to understand how to participate in this project. The best place to get started now is looking at the issues - all work goes via the backlogs. 

## Repositories
The key repositories in use by the project are:
- **Coordination**: This is the main repository for orchestrating the project.
- [**Server**](https://github.com/cherrytwist/Server): the primary back end server that manages interactions with the platform
- [**Client.Web**](https://github.com/cherrytwist/client.web): browser based interface for interacting with the platfrom.
- **Server.Identity**: manages self sovereigh identity (SSI) profiles used within CherryTwist
- **Infrastructure**: for managing the environments, deployments & builds

In addition the platform uses an Identity Provider based on Azure Active Directory.

Each repository itself contains documentation that is specific for that component.

## Coordination
The coordination of this project is based primarily on GitHub issues, augmented by Zenhub for a more agile interface to the backlog. The development process in use is largely based on the [default process suggested by Zenhub](https://help.zenhub.com/support/solutions/articles/43000010341). 

Name | Use
---- | ---
GitHub Issues | Place to talk about issues, bugs, tasks, road map, etc. The primary backlog / source of information.
[ZenHub](https://app.zenhub.com/workspaces/cherrytwist-5ecb98b262ebd9f4aec4194c) | Alternative interface for GitHub Issues.

Each repo has its own set of issues.

The Coordination repo is used for issues that span the whole project. 

All Epics are in the Coordination repo.

The [ZenHub board](https://app.zenhub.com/workspaces/cherrytwist-5ecb98b262ebd9f4aec4194c) spans all active repositories and brings all open issues into a joined up backlog. 

### Additional Interaction Channels
In addition the for core contributors the following interaction channels are supported:
Name | Use
---- | ---
**Discord** | Place for quick conversations about CherryTwist, daily standups etc.
**Gdrive** | For temporary working material, especially working drafts that are evolving rapidly and where google docs are more effective. Once designs stabilise down they are converted into MD files within the repo.

If you are contributing to the project the default path is to fork and submit a pull request; if you are interested in becoming a core contributor then please reach out via <community@cherrytwist.org>.




## Development Setup
This section details the standards agreed for development within the project. 

### Versioning
The version numbers used within all repositories is based on [semantic versioning](https://semver.org/). 

Each repository evolves its version number independent of other repositories within the project. In particular the version number for the server is not explicitly aligned with the version number of the web client.

### Branches
The branching strategy used by the project is largely as [describined in this article](https://nvie.com/posts/a-successful-git-branching-model), with the caveat that _master_ is now called _main_ in light of recent updates to Github naming standards. 

As such the two primary branches in each repository are:
* **main**: for stable releases
* **develop**: for active development

In addition feature branches are used for issues and that are then merged into develop. 

The convention for naming of feature branches is that they should take the name of the issue being worked on in the branch e.g. "coordination#100". 

In general there should be one issue per feature branch.

### Shared Environments
In addition there are currently two staging environments that each have both the [client.web](https://github.com/cherrytwist/client.web) and server repos deployed:
- **[dev](https://dev.cherrytwist.org)**: for the continual deployment from the _develop_ branch 
- **[acc](https://acc.cherrytwist.org)**: for the continual deployment from the _main_ branch 
Both of these environments should be live at all times.

The _dev_ environment exposes the following end points:
* client: https://dev.cherrytwist.org
* server: https://dev.cherrytwist.org/graphql

Similar end points are exposes for the _acc_ environment.

### Development Environment 
Development takes place primarily on local machines.

The default developer IDE is VS Code, and repos may include configuration for that IDE that make sense to be shared (debug setups etc).

The following tooling is in use within the repos:
- ESLint: for checking code formatting. 

### Development Best Practices
It is recommended that environment variables are used for configuring environment specific settings, typically via a `.env` file. 

The environment variables should be then be picked up by docker images etc. 

### Builds
There are currently CI builds on the _develop_ branches of the following repositories:
- [**Server**](https://github.com/cherrytwist/server)
- [**Client.Web**](https://github.com/cherrytwist/client.web)

The set of builds will be expanded further near term. 

## Docker Images + Image Registries
Docker is used to containerize the results from each repository. 

DockerHub is used as the primary registry for images. 

Images are pushed to the registry manually by the core team. 

The currently supported repos on dockerhub are:
- cherrytwist/server
- cherrytwist/client-web 

The _Demo.Simple_ repo has a docker-composed application that takes the latest image from both of these repos and creates a simple running demonstrator. The version numbers used within a demo should be explicit. 

It is likely that there is later a separate repository location added for more development oriented images. 

### Tags 
All docker images should have a version number tag assigned. 

Note: there is work planned to automate the generation of docker images that are correctly tagged and pushed to dockerhub.

The `latest` tag is reserved for the latest image generated from the _main_ branches. 

### Kubernetes & Terraform
The deployment environment is based on Kubernetes, generated via Terraform.

## Need help

If at any point you would like more information, have an issue, want to be added to our chat platform, etc. reach out via via <community@cherrytwist.or> or directly to **GhostOnTheFiber** and they will point your in the right direction. It might take a couple of days, so please be understanding.

### Connectors within the project

Handel | Role
------ | ----
techsmyth | Coordinator
valentinyanakiev | Development
GhostOnTheFiber | Contributor and developer community contact point
ReneHonig | Partnerships, Strategy

