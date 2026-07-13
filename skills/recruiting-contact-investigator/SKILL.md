---
name: recruiting-contact-investigator
description: Investigate recruiting contacts from a vacancy, company, or person. Use when the user wants a requested number of verified direct routes to recruiters, HR leaders, hiring managers, team leads, C-level leaders, or internal referrers, including Telegram-focused contact research and outreach-target enrichment.
---

# Recruiting Contact Investigator

## Contract

Run a contact investigation, not a search dump. Follow public leads, preserve evidence, and rank people by their ability to influence the hire.

Set `target_count` to the number of contacts requested by the user; default to `2`. A contact counts toward the target only when both are supported independently:

1. **Role evidence** — the person currently works for the company in a relevant hiring, leadership, team, or referral position.
2. **Route evidence** — the direct account or address belongs to that person.

Generic company inboxes, switchboards, and company social pages are fallback routes, not direct contacts. Use publicly available traces and the access methods permitted by the applicable project instructions.

## Inputs

Extract or infer:

- `target_count` and requested contact channel
- company and vacancy title
- vacancy ID or source URL
- region and language market
- target specialty, department, or product
- known people or team names

Ask only when neither the company nor a usable vacancy/source can be inferred.

## Investigation Loop

### 1. Frame the search

- For an HH/HeadHunter vacancy, run the HH branch in [contact-search-playbook.md](references/contact-search-playbook.md).
- Extract vacancy-specific role and technology keywords.
- Build an **organizational ladder**: recruiter or talent acquisition → HR manager or director → role-specific hiring manager or team lead → relevant C-level leader → current employee with credible referral access.

Completion: `target_count`, vacancy context, specialty keywords, and the organizational ladder are explicit.

### 2. Build the candidate pool

Read and run the applicable source branches in [contact-search-playbook.md](references/contact-search-playbook.md). Search the company, vacancy, and promising people; follow identity handles across sources.

For a requested Telegram route, run the Telegram branch under the tool and fallback policy in the applicable project instructions.

Completion: the pool contains at least `2 × target_count` plausible people from at least two independent source families, or every applicable branch has an evidence-backed exhaustion note.

### 3. Two-factor verification

Record role and route separately for every candidate.

Role status:

- `current verified` — a current first-party or opened professional page states the role.
- `current cross-source` — at least two independent recent sources agree.
- `stale or conflicted` — sources disagree or only old employment is visible.
- `unresolved` — company or role cannot be confirmed.

Route status:

- `verified direct` — an authorized lookup or opened public contact page connects the route to the person.
- `explicit direct` — the exact route is published by the person or company and identity matches across sources.
- `inferred handle` — the route is guessed from a name or reused username.
- `unavailable` — no direct route was found.

Only `verified direct` and `explicit direct` routes with `current verified` or `current cross-source` role evidence count toward `target_count`. Keep inferred handles in an unresolved section.

Completion: every counted contact passes both factors; every other candidate has an explicit unresolved reason.

### 4. Rank and escalate

- Rank by hiring influence, specialty relevance, currentness, route strength, and likelihood of a useful response.
- Continue up the organizational ladder until the target is met.
- If sources conflict, prefer the freshest direct evidence, report the conflict, and lower confidence.
- Keep generic hiring email, social DM, or company channel as a separate fallback.

Completion: either `target_count` qualifying direct contacts are ranked, or all applicable source branches and organizational-ladder rungs are exhausted and recorded. An inferred handle never satisfies the target.

## Output

```text
Best contacts
1. Name — current role — direct route
   Role evidence: status — source
   Route evidence: status — source
   Why this person: hiring influence and specialty relevance

Unresolved leads
- Name — inferred or unavailable route — missing verification

Evidence checked
- Source branch — result or exhaustion reason

Next actions
- Message priority order
- Generic fallback route, if useful
```

Return the strongest evidence near each claim. Explain any shortfall from `target_count` instead of filling the list with inferred contacts.
