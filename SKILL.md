---
name: recruiting-contact-investigator
description: Recruiting contact investigation for finding recruiter, talent acquisition, HR, or hiring-manager contacts from a vacancy, company, person name, HH/HeadHunter vacancy, Habr Career page, LinkedIn profile, Telegram lead, or public web traces. Use when the user asks to identify who to contact about a job, find a recruiter or hiring manager, enrich hiring contacts, or produce an outreach target list.
---

# Recruiting Contact Investigator

## Operating Rule

Run this as an investigation, not a search dump: follow leads, keep evidence, and rank contacts by likelihood that they can influence the hire.

Use only user-provided or publicly available traces. Do not invent contacts, bypass access controls, or treat a stale profile as current without evidence.

## Inputs

Extract what the user provided:

- company name
- vacancy title
- HH/HeadHunter vacancy ID or URL
- vacancy source URL
- region or language market
- known person names
- target specialty or department

If a required input is missing, infer from the source when possible. Ask only when neither the company nor the vacancy/source is known.

## Investigation Loop

1. Build search roles before searching.
   - If the input includes an HH/HeadHunter vacancy ID or URL, first run `firehh <vacancy-id>` to extract vacancy title, employer, region, skills, and description. Example: `firehh 133561763`.
   - Generate likely titles for this hire: recruiter, talent acquisition, HR partner, head of department, CPO, CTO, team lead, and role-specific leaders.
   - Completion: HH vacancy details are captured when applicable, and the role list includes recruiting roles plus at least one plausible hiring-manager path.

2. Search the company and vacancy across source branches.
   - Use the contact search playbook below.
   - Always run the Telegram branch for the vacancy, company, and every promising person unless Telegram is unavailable in the environment.
   - Run other branches that match available evidence: vacancy title + company, Habr Career, LinkedIn, Google X-Ray, person name + company, company site, public posts.
   - Completion: every available branch has either produced candidate contacts or has an evidence-backed reason to stop.

3. Convert traces into contact candidates.
   - Capture name, role, company fit, source URL or source description, contact route, evidence quality, and confidence.
   - Label evidence quality as `verified page`, `public indexed snippet`, `cross-source match`, or `unverified lead`.
   - Prefer direct Telegram handles or Telegram-channel contact routes over generic email and weaker social DMs. Treat plain email as weak unless the user asks for email.
   - Completion: each candidate has a contact route or is explicitly marked as unresolved.

4. Verify relevance.
   - Check that recruiters hire for the user's specialty when evidence allows.
   - Check that hiring managers work in the relevant department, product, or sector.
   - Prefer current employees over former employees unless current contacts fail.
   - Downgrade blocked, login-only, stale, or snippet-only evidence unless another public source confirms it.
   - Completion: ranked candidates have a reason tied to the role, department, or hiring function.

5. Escalate if direct contacts fail.
   - Try adjacent recruiters, hiring managers, HR leadership, department leadership, agency/contract recruiters, public employees, and social-publication trails.
   - Completion: the report names the fallback path used, not just that no contact was found.

## Contact Search Playbook

Use each branch as evidence, then return to ranking contacts.

### HH / HeadHunter Vacancy

If the user provides an HH vacancy ID or URL, run the `firehh` CLI before browsing:

```bash
firehh <vacancy-id>
```

For URLs like `https://hh.ru/vacancy/133561763`, extract `133561763` and run `firehh 133561763`.

Capture vacancy title, employer, region, skills, publication URL, and description clues about team, product, department, or recruiter. If `firehh` fails because HH auth/token is missing, note the failure in evidence checked and continue with public-web search from the HH URL, company, and vacancy title.

### Vacancy Title + Company

Search `[vacancy title] [company]`. Check several results besides HeadHunter. If the vacancy appears on Telegram, Habr Career, LinkedIn, or another job site, inspect it for named recruiters, company representatives, Telegram handles, social links, department/team names, and repost authors.

### Google X-Ray

Use boolean or X-Ray queries with source sites, job titles, and names:

```text
site:linkedin.com [company] recruiter
site:linkedin.com [company] "talent acquisition"
site:linkedin.com [company] [department lead title]
site:habr.career [company]
site:habr.career [name]
site:t.me [company] [vacancy title]
site:t.me [company] recruiter
site:t.me [person name]
```

When search terms are weak, first generate a query plan: likely hiring roles, exact LinkedIn/Google X-Ray/Telegram/HH queries, 5-10 company titles to search, message priority, and fallback paths.

### Habr Career Company Page

On a company page, check the company contact area for responsible people. If a recruiter or hiring contact is listed, open the profile, look for Telegram, then search Telegram by profile nickname or first name + last name. If no responsible contact is listed, inspect company employees for recruiting, HR, leadership, and relevant department roles; keep only profiles whose specialty or department matches the vacancy.

### Person Name + Company

Search `[first name last name] [company]`. If LinkedIn appears, inspect contact/about text and use LinkedIn DM only when no stronger route exists. If Instagram appears, inspect links and bio, copy the nickname, and search it in Telegram. If a personal Telegram channel appears, inspect the description and posts for `ищу`, `нанимаю`, or `вакансия`. If no direct route appears, find another recruiter or hiring manager and continue.

### Evidence Quality

Classify each lead before ranking it:

- `verified page`: the opened public page directly states the role, company, or contact route.
- `cross-source match`: two or more independent public sources connect the same person, company, role, or handle.
- `public indexed snippet`: search result text suggests the connection, but the page itself is blocked, unavailable, or login-only.
- `unverified lead`: plausible but not confirmed enough to contact first.

Use snippet-only leads as backups unless no verified or cross-source contact exists.

### Telegram Search

Run this branch for the company, vacancy, and every promising person.

Search Telegram directly when available. Also use public web queries:

```text
site:t.me [company]
site:t.me [company] [vacancy title]
site:t.me [company] backend
site:t.me [company] golang
site:t.me [company] recruiter
site:t.me [person name]
site:t.me [nickname]
```

For company channels, inspect channel description, vacancy posts, bottom-of-post contacts, linked recruiters, repost authors, and linked personal channels. For people, search exact Russian and English names, transliterations, shortened forms, and nicknames from LinkedIn, Habr Career, GitHub, Instagram, email prefix, or profile URL. Confirm handles by cross-checking bio, avatar/name, company mention, vacancy posts, or linked source.

Rank Telegram routes highest when the public source explicitly invites contact for hiring or vacancies. Downgrade a Telegram handle if it is only name-matched and not tied to the company or role.

### LinkedIn Company Page

If responsible people are visible, inspect profiles for contacts, especially Telegram. If only email is visible, search the email nickname in Telegram, then first name + last name in Russian and English layouts. If no responsible people are visible, inspect current employees for recruiters, talent acquisition, HR, department leads, and likely hiring managers.

### Company Site And Public Channels

Inspect company site/team pages for HR director, CPO, CTO, department head, or team lead. Search Habr Career profiles, Telegram company channels, and publications mentioning teams, vacancies, and bottom-of-post contact details.

For Telegram company channels, check channel description contacts, vacancy posts, and posts naming team members or recruiters.

### Stop Conditions

Stop only when one is true:

- at least one strong direct contact and one fallback contact are ranked
- every relevant branch has been checked and the report explains why no contact route was found
- the missing input is blocking and cannot be inferred from public traces

## Output

Return a concise investigator report:

```text
Best contacts
1. Name — role — contact route — confidence — evidence quality — why this person

Evidence checked
- Source/branch — result

Next actions
- Message priority order
- Any missing data that would materially improve the search
```

Never output raw unranked links without explaining why each matters.
