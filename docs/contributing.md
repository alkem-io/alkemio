# Contributing to Alkemio
Here you can find details of requirements for contributing to the project, an overview of key repositories as well as how coordination takes place.

## Licensing
Contributions to the project are made under the **[Developer Certificate of Origin 1.1 (DCO)](https://developercertificate.org/)** to ensure that the contents of the repository are covered from a legal perspective. Every commit must carry a `Signed-off-by` trailer (add it with `git commit -s`); this is enforced as a required check on all pull requests. Historical contributions made before the DCO cut-over remain covered by the retired [Contributor License Agreement](https://github.com/alkem-io/.github/blob/master/CLA.md).


## Repositories
The key repositories in use by the project are:
- **Coordination**: This is the main repository for orchestrating the project.
- [**Server**](https://github.com/alkem-io/Server): the primary back end server that manages interactions with the platform
- [**Client.Web**](https://github.com/alkem-io/client.web): browser based interface for interacting with the platfrom.
- [**Demo**](https://github.com/alkem-io/demo): for createing a demonstration instance of the platform, with data, locally.
- [**Client.Lib**](https://github.com/alkem-io/client.lib): wrapper around the server api for interacting programmatically with the platform.
- [**Populator**](https://github.com/alkem-io/populator): for populating a Subspace Space using data loaded from a spreadsheet.
- [**Notifications**](https://github.com/alkem-io/notifications): a separate service that provides notification services, using out of band channels such as email.
- [**Infrastructure**](https://github.com/alkem-io/infrastructure): additional information related to deploying the platform as a cluster

Each repository itself contains documentation that is specific for that component.

## Coordination
The coordination of this project is based primarily on GitHub issues, augmented by Zenhub for a more agile interface to the backlog.

Each repo has its own set of issues.

[This](https://github.com/alkem-io/alkemio) repo is used for issues / discussions that span the whole project. 

All Epics are in the Alkemio repo.

Our [Roadmap](https://github.com/orgs/alkem-io/projects/4/views/17) is public, so you can see what Epics are being worked on / planned. Note: timelines are an indication, if you see a particular Epic that is important for you please reach out to discuss its prioritization and expected delivery timeline. All Epics are in the Alkemio repo.

If you are contributing to the project the default path is to fork and submit a pull request.

There is a private [ZenHub board](https://app.zenhub.com/workspaces/Alkemio-5ecb98b262ebd9f4aec4194c) for active sprint planning (private due to Zenhub license model), if you want to get involved at that level then let's talk!

Finally and imporantly, there is a [Building Alkemio Space](https://alkem.io/building-alkemio) on the platform; this is a great place to go and connect to the community behind Alkemio. It is also the place for discussions about ideas, needing help, collaborations etc.

## Becoming a core contribrutor
If you are interested in becoming a core contributor then please reach out via <community@alkem.io>.

