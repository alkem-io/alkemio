# Epic: Templates revamp

## Description
Templates are one of Alkemio's most powerful features, and one of the hardest to use. A facilitator who builds a good callout, whiteboard or subspace in Space A and wants to reuse it in Space B has to take a long detour today: save it, go to their org profile, open the template pack, copy the content over, then go to Space B and look it up again. There is no way to send someone a link to a template. A template from the platform library can't be applied directly where you are. Previews are incomplete, and some content is silently lost on the way (community guidelines, tags, visuals, contributions).

This epic replaces the piecemeal approach of #1175 and #1734 with **one revamp of the full template lifecycle**: **create → find → preview → use → maintain → share & curate**, for every template type (Space, Subspace, Callout, Whiteboard, Innovation Flow, Community Guidelines, and Office docs later on).

It is a **parent epic**. The work is split into the sub-epics below, and each one can ship on its own.

**Primary personas**
- **Facilitator / Space admin**: builds and reuses methods across their own Spaces and Subspaces.
- **Template pack owner (organization / network)**: curates a methodology and publishes it.
- **Space member**: wants to know which methods are available in their Space and use them.
- **Platform admin / Alkemio**: manages the default templates and safeguards quality in the store.

## Goal
A smooth experience with no dead ends for anyone working with templates. From any content you can save a template to anywhere you are allowed to, and you can apply any template you can see directly where you are, without leaving your context.

Supports OKR #1781: *Ensure users can complete critical workflows seamlessly, with minimal friction and AI support.*

## Hypothesis
The value of templates is held back by the UX around them. If templates become easy to save, find, preview and apply, power users will reuse their methods more, new Spaces will start from better structures, and users will get more value from the platform.

**How we measure it** _(to confirm with Analytics, see #2167)_
- % of new Spaces / Subspaces created from a template
- Number of templates applied per month, by type and by source (space library, pack, platform library)
- Number of "save as template" actions, and where the template was saved to
- Number of shared template links opened
- Fewer template-related bugs and support requests

## Design principles
1. **Never leave your context.** Save, import and apply happen where the user already is.
2. **One template, one place to manage it.** No duplicates (for example, a whiteboard inside a post template shouldn't need to exist twice).
3. **What you preview is what you get.** Previews show all of the content, and applying a template reproduces it faithfully.
4. **The same component everywhere.** The same cards, dialogs and preview whether you're in a Space library, a pack or the platform library.
5. **A solid API first.** Every template operation is reliable through the API, so the UI, scripts, MCP and AI-assisted Space generation (#1641) all build on the same foundation.

## Sub-epics

### 1. Foundations: data integrity, API and performance
_Templates have to store and reproduce content faithfully before we build more UX on top of them._
- alkem-io/server#6581: creating a Space template directly via `createTemplate` fails or corrupts data in five ways (incomplete settings break the pack page; `innovationFlowData`, `subspaces` and post `nameID` handling; flow-state tagset not updated)
- alkem-io/client-web#8557: community guidelines are removed when a template is updated
- alkem-io/client-web#8278: preview visual missing after importing a whiteboard template into the space library
- alkem-io/client-web#9935: hidden phases still show up in templates
- alkem-io/client-web#8540: auth error on a private L1 created from a template
- Profile tags on `createTemplate` aren't stored; `sourceWhiteboardID` refuses platform library whiteboards (both noted in server#6581)
- Check images and authorization rights on all template content (from #1175)
- alkem-io/client-web#6914: speed up the Template Explorer
- alkem-io/client-web#4688, alkem-io/client-web#4040: restructure the client templates code base and `AdminTemplatesSection`

### 2. Create: save as template, from anywhere to anywhere
_Saving content as a template is one step, and the user chooses the destination._
- alkem-io/client-web#5067: save a callout as a template to any Space library or pack the user is allowed to write to
- alkem-io/client-web#9037: save a subspace as a template and choose where it goes (Space library or org pack)
- Save a whiteboard as a template (from #1734)
- alkem-io/server#5244: add contributions to a callout template in the library
- Save a (Sub)space straight into a template pack (to check whether this is already on the board, see the comment on #1175)

### 3. Find & preview
_Users understand what a template contains before they use it._
- alkem-io/client-web#8302: full Space template preview (including subspaces)
- alkem-io/client-web#8305: Space template banner
- alkem-io/client-web#8303: edit a Space template in place
- alkem-io/client-web#8972: visual distinction between Space and Subspace templates
- alkem-io/client-web#7491: show tags and references on subspace previews
- alkem-io/client-web#9003, alkem-io/client-web#7288: whiteboard template preview layout
- alkem-io/client-web#9790: share a template (copy link, share button)

### 4. Use: apply templates directly
_Use a template where you are, from any library you can see._
- Open or apply a template from the space or platform library directly into a chosen Space or Subspace (from #1734). For types lower in the hierarchy, offer a "copy, then paste at the destination" flow as a fallback.
- alkem-io/client-web#4924: add a template from the platform library to a space library directly
- alkem-io/client-web#8602: import a full template pack into a space library
- alkem-io/server#5542: edit or remove contributions before applying a template

### 5. Libraries & packs: the model
_Clear roles for the Space library, template packs and the platform library._
- Decide whether packs exist inside a Space library or are only copied into it (alkem-io/client-web#8602)
- Show whiteboards from post templates in the whiteboard template library, without duplicates (from #1175)
- alkem-io/client-web#4683: Space library visible to members; transparency for members about what's available (context from #1323)
- alkem-io/client-web#8682: global templates library: manage default templates and default callouts in the admin UI
- Fold in: #1079 (Innovation Library Management, InnovationPack extensions) and #1771 (UI/UX Bucket: Template library)

### 6. Governance & curation
_Quality and ownership of what gets published._
- alkem-io/client-web#9039: Alkemio approves store listings for template packs (and VCs)
- Suggest a template for publication at org level, with an approve/decline flow and notifications (context from #785)
- #1688: Roles on template packs (Host, Content Provider, Expert), linked as a sub-epic

### Related, out of scope for now
- #1843: OfficeDocs Phase 3 (templates for office docs). Should reuse the model from sub-epic 5.
- #1179: Flexible & templatable community roles
- #1570: Innovation Flow settings and default callout content
- #1641: AI-assisted Space generation (depends on sub-epic 1)

## Must-have scope (P1)
- [ ] Sub-epic 1 done: templates round-trip without data loss, and the Space template API is reliable
- [ ] Save as template from callout, whiteboard and (sub)space, with a choice of destination (space library or pack)
- [ ] Apply a Space / Subspace template from any library directly into the chosen location
- [ ] Import a full template pack into a space library
- [ ] Full preview of Space templates
- [ ] Shareable link to a template

## Open questions
- [ ] Do packs live inside the Space library, or are their templates copied in?
- [ ] Should the Space library be visible to all members, or only to admins?
- [ ] How far down the hierarchy does "apply directly" go (Space and Subspace only, or also callouts)?
- [ ] Whiteboards inside post templates: show them in the whiteboard library, or move brainstorm methods to post templates only?
- [ ] Is a template applied once (a copy), or does it keep a link to its source for later updates?

## Stakeholders
SiR Intergov, Opschalingstickets, DSIH, CE team, _..._

## Timeline
TBD. Proposed order: 1 → 2 + 3 in parallel → 4 → 5 → 6.

## Design
Figma: [Template Library](https://www.figma.com/design/y6fb75ft0qGyom31ghLXSb/Template-Library?node-id=64-128&t=17ht87jDPf08uymn-1)
Earlier spec: [client-web `001-template-pack-ux` spec](https://github.com/alkem-io/client-web/blob/001-template-pack-ux/specs/001-template-pack-ux/spec.md)
Brainstorm on user flows: [Improving the user experience: templates](https://alkem.io/building-alkemio-org/challenges/improvingtheusere-6400/collaboration/templates)
_Comments are highly appreciated. When commenting in Figma, please also leave a note in the comments box below this issue so we get notified._

## Supersedes
- #1175 Templates: Interaction Space library and platform library (2023)
- #1734 Templates: Interaction Space library and platform library 2026
- Earlier fold-ins: #1553, #785, #1323
