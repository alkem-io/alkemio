# Developer Documentation
This is the starting point for understanding the development setup used within CherryTwist as well as how to get started with contributing to the project.

Currently this documentation might be a bit sparse but hopefully in time it can help you get up to speed quickly.
Please read the [contributing](https://github.com/cherrytwist/.github/blob/master/CONTRIBUTING.md) guide lines to understand how to participate in this project.

## Getting Started
Currently we are in the process of creating a road map, landing the technical design, and getting the first code down. Therefore the best place to get started now is looking at the issues and helping us bootstrap.

Once we have a working base system, we will update this documentation to help you get started.

## Repositories

This section describes the different repositories in use and their dependencies:
- **Coordination**: This is the main repository for orchestrating the project.
- **Server**: the primary back end server that manages interactions with the platform
- **Server.Identity**: manages self sovereigh identity (SSI) profiles used within CherryTwist
- **Client.Web**: browser based interface for interacting with the platfrom.

In addition the platform uses an Identity Service based on Azure Active Directory.

Each repository itself contains documentation that is specific for that component.

## Backlog 
The coordination of this project is based primarily on GitHub issues, augmented by Zenhub for a more agile interface to the backlog.

Name | Use
---- | ---
GitHub Issues | Place to talk about issues, bugs, tasks, road map, etc. The primary backlog / source of information.
[ZenHub](https://app.zenhub.com/workspaces/cherrytwist-5ecb98b262ebd9f4aec4194c) | Alternative interface for GitHub Issues.

Each repo has its own set of issues, and the Coordination repo is used for issues that span the whole project. The Zenhub board spans all active repositories and brings all open issues into a joined up backlog. 

## Interactions
The primary means of interaction is via GitHub issues. In addition the for core contributors the following interaction locations are supported:
Name | Use
---- | ---
Discord | Place for quick conversations about CherryTwist.
Gdrive | For temporary working material, especially working drafts that are evolving rapidly and where google docs are more effective. Once designs stabilise down they are converted into MD files within the repo.

If you are contributing to the project the default path is to fork and submit a pull request; if you are interested in becoming a core contributor then please reach out via <community@cherrytwist.org>.

## Development Process
The development process in use is largely based on the [default process suggested by Zenhub](https://help.zenhub.com/support/solutions/articles/43000010341). 

## Need help

If at any point you would like more information, have an issue, want to be added to our chat platform, etc. reach out via via <community@cherrytwist.or> or directly to **GhostOnTheFiber** and they will point your in the right direction. It might take a couple of days, so please be understanding.

## Connectors within the project

Handel | Role
------ | ----
techsmyth | Coordinator
valentinyanakiev | Development
GhostOnTheFiber | Contributor and developer community contact point
ReneHonig | Partnerships, Strategy

