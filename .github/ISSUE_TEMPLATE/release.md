---
name: Release
about: Template for tracking a new release, including risk profile and mitigations
title: 'Release: #N'
labels: 'release'
assignees: ''

---

### **Release date**: <date when the deployment is done>

### Scope <High level overview and a link to Zenhub release report> [Zenhub release report](<url>)
- <Epic1> <Feature1>

### Risk Profile

#### Identified Risks

E.g.

| Risk                            | Likelihood | Impact        | Remediation                                    | Residual Risk               |
|---------------------------------|------------|---------------|------------------------------------------------|------------------------------|
| Privilege escalation     | Low       | High          | Fix [the bug raised about self-approval of applications](https://app.zenhub.com/workspaces/alkemio-development-5ecb98b262ebd9f4aec4194c/issues/gh/alkem-io/server/4651)    | Low |
| Data loss due to failing backups | Low    | High          | Fix [ephemeral storage issues](https://app.zenhub.com/workspaces/alkemio-development-5ecb98b262ebd9f4aec4194c/issues/gh/alkem-io/infrastructure-operations/1615) | Medium |


<details>
  <summary>Perceived Residual Risk</summary>

    - **Risk 1 Residual**: [Description of the remaining risk after mitigations]
    - Example: If feature toggle fails, user experience may be disrupted for a limited number of users.
    - **Residual Likelihood**: High/Medium/Low
    - **Residual Impact**: High/Medium/Low
</details>

### Released images:
|image| old |new|
|----|-----|----|
server| x   |y
client| x   |y


### Release notes
- <Highlight that happened during the release if any>


### Release Checklist
- [ ] Scope is defined.
- [ ] Obtain approval from stakeholders.
---
- [ ] All linked issues and pull requests are merged.
- [ ] The pre-deployment checklist is done.
- [ ] Release is deployed to Acceptance.
- [ ] The post-deployment checklist is done.
- [ ] Release Email about the scope is sent to the team.
- [ ] Acceptance RC signed off. (QA)
- [ ] All risks are added to the data table, including new bugs.
- [ ] Get approval from business (Delivery lead, Arch lead) for high risk profile RCs.
---
- [ ] Ensure release notes are written and accurate.
- [ ] Check the product calendar for appropriate release time.
- [ ] Communicate with the team about the release schedule.
- [ ] The pre-deployment checklist is done.
- [ ] Perform the release deployment to Production.
- [ ] The post-deployment checklist is done.
- [ ] Monitor post-release for any issues.


### Pre-Deployment Tasks
- [ ] <?>

### Post-Deployment Tasks
- [ ] <Authorization Reset>


### Stakeholders
- **Delivery Lead**: [@simonezaza](https://github.com/simonezaza)
- **Release Lead**: [@bobbykolev](https://github.com/bobbykolev)
- **Quality Lead**: [@Comoque1](https://github.com/comoque1)
