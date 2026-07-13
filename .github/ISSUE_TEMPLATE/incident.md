---
name: Incident
about: Template for tracking and resolving production incidents
title: 'Incident: <brief description>'
labels: 'incident'
assignees: ''

---

### Incident Summary

| Field              | Value                          |
|--------------------|--------------------------------|
| **Severity**       | SEV1 / SEV2 / SEV3 / SEV4     |
| **Status**         | Investigating / Identified / Monitoring / Resolved |
| **Detected at**    | YYYY-MM-DD HH:MM UTC          |
| **Resolved at**    | YYYY-MM-DD HH:MM UTC          |
| **Duration**       |                                |
| **Affected services** |                             |
| **Users impacted** | All / Subset / Internal only   |

### Severity Definitions

<details>
  <summary>Expand severity guide</summary>

  - **SEV1** – Complete service outage or data loss affecting all users. Requires immediate response.
  - **SEV2** – Major functionality degraded. Large portion of users affected.
  - **SEV3** – Minor functionality degraded. Limited user impact.
  - **SEV4** – Cosmetic or low-impact issue. No immediate user harm.
</details>

### Description

<!-- What happened? What is the user-visible impact? -->

### Timeline

| Time (UTC) | Event |
|------------|-------|
|            | Issue first detected (alert / user report / monitoring) |
|            | Incident declared |
|            | Root cause identified |
|            | Fix deployed |
|            | Incident resolved |

### Root Cause

<!-- What was the underlying cause? (fill in after investigation) -->

### Resolution

<!-- What was done to resolve the incident? -->

### Impact

- **Users affected**: 
- **Data impact**: None / Partial / Full (describe)
- **Revenue/SLA impact**: 

### Action Items

| Action | Owner | Due Date | Status |
|--------|-------|----------|--------|
|        |       |          | TODO / IN PROGRESS / DONE |

### Detection

- [ ] Detected by automated monitoring/alerting
- [ ] Detected by user report
- [ ] Detected by team member

### Lessons Learned

<!-- What went well? What could be improved? -->

- **What went well**: 
- **What could be improved**: 
- **Where did we get lucky**: 

### Incident Checklist

- [ ] Incident severity assigned.
- [ ] Communication sent to affected stakeholders.
- [ ] Root cause identified.
- [ ] Fix implemented and deployed.
- [ ] Incident timeline documented.
- [ ] Post-mortem / retrospective scheduled.
- [ ] Action items created and assigned.
- [ ] Monitoring/alerting improvements identified.

### Stakeholders

- **Incident Commander**: 
- **Technical Lead**: 
- **Communications Lead**: 
