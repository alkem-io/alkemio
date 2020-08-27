# Developer Documentation
This is the starting point for understanding the development setup used within CherryTwist as well as how to get started with contributing to the project.

The project is ramping up, so please be patient if you find gaps (and point them out to us!).

Please read the [contributing](https://github.com/cherrytwist/.github/blob/master/CONTRIBUTING.md) guide lines to understand how to participate in this project. The best place to get started now is looking at the issues - all work goes via the backlogs. 

## Repositories
The key repositories in use by the project are:
- **Coordination**: This is the main repository for orchestrating the project.
- **Server**: the primary back end server that manages interactions with the platform
- **Server.Identity**: manages self sovereigh identity (SSI) profiles used within CherryTwist
- **Client.Web**: browser based interface for interacting with the platfrom.
- **Infrastructure**: for managing the environments, deployments & builds

In addition the platform uses an Identity Provider based on Azure Active Directory.

Each repository itself contains documentation that is specific for that component.

## Coordination
The coordination of this project is based primarily on GitHub issues, augmented by Zenhub for a more agile interface to the backlog.

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


### Development Process
The development process in use is largely based on the [default process suggested by Zenhub](https://help.zenhub.com/support/solutions/articles/43000010341). 

### Branches
The primary branch in each repository is the _master_ branch.

The adopted practice is that development branches are named after the issue that the branch is addressing e.g. "coordination#100". 


## DevOps 
Development takes place primarily on local environments.

### Builds
There are currently CI builds on the _master_ branches of the following repositories:
- Server
- Client.Web

The set of operational builds will be clearly expanded near term...

### Shared Environments
In addition there are currently two staging environments:
- **dev**: for the continual deployment of CI build rests
- **test**: for manually triggered updates 
Both of these environments should be live at all times.

The CI builds trigger updates to _dev_.

### Images + Image Registries
Docker is used to containerize the results from each repository. 

DockerHub is used as the primary registry for images. 

Images are pushed to the registry manually by the core team. 

The currently supported repos on dockerhub are:
- cherrytwist/server
- cherrytwist/client-web 
The _Demo.Simple_ repo has a docker-composed application that takes the latest image from both of these repos and creates a simple running demonstrator.

It is expected that there is later a separate repository location added for more development oriented images. 

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

