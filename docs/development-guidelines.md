
# Development Guidelines
Here you can find the standards agreed for development within the project. 

## Versioning
The version numbers used within all repositories is based on [semantic versioning](https://semver.org/). 

Each repository evolves its version number independent of other repositories within the project. In particular the version number for the server is not explicitly aligned with the version number of the web client.

## Process
The development process in use is largely based on the [default process suggested by Zenhub](https://help.zenhub.com/support/solutions/articles/43000010341). 

## Branches
The branching strategy used by the project is largely as [describined in this article](https://nvie.com/posts/a-successful-git-branching-model), with the caveat that _master_ is now called _main_ in light of recent updates to Github naming standards. 

As such the two primary branches in each repository are:
* **main**: for stable releases
* **develop**: for active development

In addition feature branches are used for issues and that are then merged into develop. 

The convention for naming of feature branches is that they should take the name of the issue being worked on in the branch e.g. "coordination#100". 

In general there should be one issue per feature branch.

## Shared Environments
In addition there are currently two staging environments that each have both the [client.web](https://github.com/alkem-io/client.web) and server repos deployed:
- **[dev](https://dev.alkem.io)**: for the continual deployment from the _develop_ branch 
- **[test](https://test.alkem.io)**: for validating production deployments from the _main_ branch 
Both of these environments should be live at all times.

The _dev_ environment exposes the following end points:
* client: https://dev.alkem.io
* server: https://dev.alkem.io/graphql

Similar end points are exposes for the _test_ environment.

## Development Environment 
Development takes place primarily on local machines.

The default developer IDE is VS Code, and repos may include configuration for that IDE that make sense to be shared (debug setups etc).

The following tooling is in use within the repos:
- ESLint: for checking code formatting. 

## Development Best Practices
It is recommended that environment variables are used for configuring environment specific settings, typically via a `.env` file. 

The environment variables should be then be picked up by docker images etc. 

## Builds
There are currently CI builds on the _develop_ branches of the following repositories:
- [**Server**](https://github.com/alkem-io/server)
- [**Client.Web**](https://github.com/alkem-io/client.web)
- [**Demo Authentication Provider**](https://github.com/alkem-io/AuthenticationProvider)


## Docker Images + Image Registries
Docker is used to containerize the results from each repository. 

DockerHub is used as the primary registry for public images. New Images are pushing automatically to DockerHub upon tagged releases in the repos. 

The currently supported repos on dockerhub are:
- alkemio/server
- alkemio/client-web 

The _Demo_ repo has a docker-composed application that takes the latest image from both of these repos and creates a simple running demonstrator. 

It is likely that there is later a separate repository location added for more development oriented images. 

### Tags 
All docker images should have a version number tag assigned. 

The `latest` tag is reserved for the latest image generated from the _main_ branches. 

### Kubernetes & Terraform
The deployment environment is based on Kubernetes, generated via Terraform. 


