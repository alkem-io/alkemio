---
name: Release
about: Track a release — scope, risks that need action, and the steps to production
title: 'Release: #N'
labels: 'release'
assignees: ''

---

<!--
Keep this story short: about one screen. List only what someone has to act on or decide.
Tick a box only when it is actually done. Rollout is not ordered: if services deploying
in any order causes a temporary failure, write it down as a risk, not as a sequence of steps.
-->

### **Release date**: <TBD> · Acceptance: <TBD> · Risk: **<Low / Medium / High>**

### Scope

| image | old | new | notes |
|---|---|---|---|
| `server` | `vX` | **`vY`** | `release/NN` |
| `client-web` | `vX` | **`vY`** | `release/NN` |

Unchanged: <other services and their versions>

- **<Feature>** — <repo#PR>, <repo#PR>
- **Fixes:** <one line each, with links>

**Not in this cut:** <work still in motion, with links>

**DB / API:** <migrations (call out irreversible ones) · GraphQL additive or breaking>

### Risk Profile
<!-- Only risks that need an action or a decision. Business sign-off is required when Risk is High. -->

| Risk | Likelihood | Impact | Remediation | Residual Risk |
|---|---|---|---|---|
| <risk> | Low/Med/High | Low/Med/High | <action> | Low/Med/High |

### Checklist
- [ ] Release branches cut, versions published, images on Docker Hub.
- [ ] Deployed to Acceptance.
- [ ] QA sign-off on Acceptance.
- [ ] Business sign-off (Delivery lead, Arch lead), only when Risk is High.
- [ ] Release update posted to the team in the [Release updates callout](https://alkem.io/building-alkemio-org/challenges/delivery/opportunities/rleases/collaboration/releaseupdates-1).
- [ ] Deployed to Production.
- [ ] <Release-specific operator action, e.g. authorization reset>
- [ ] Production smoke check.

### Stakeholders
- **Delivery Lead**: [@simonezaza](https://github.com/simonezaza)
- **Release Lead**: [@bobbykolev](https://github.com/bobbykolev)
- **Quality Lead**: [@Comoque1](https://github.com/comoque1)
