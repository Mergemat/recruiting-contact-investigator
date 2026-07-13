# Contact Search Playbook

Use the branches that match the available evidence. Return every lead to the main investigation loop for two-factor verification.

## HH / HeadHunter

For an HH vacancy ID or URL, run:

```bash
firehh <vacancy-id>
```

Capture the employer, title, region, skills, team clues, and named contacts. If authentication fails, record the exact failure and continue from the public vacancy URL, employer, and extracted title.

## Official Vacancy And Company Sources

Inspect the vacancy, careers page, team page, company blog, and official public channels for:

- named recruiters or interviewers
- team, product, and department names
- leadership biographies
- vacancy authors and repost authors
- direct contact routes

Treat a generic hiring inbox as a fallback. Use names and team clues to continue the investigation.

## Professional Networks

Search company and employee pages on LinkedIn, Habr Career, Setka, and relevant regional networks.

For each promising person, capture:

- exact name and current title
- employment dates or current-company signal
- profile slug and listed social routes
- specialty, department, and hiring activity

Search the organizational ladder rather than only recruiter titles. Prefer opened profiles; treat search snippets as discovery leads until corroborated.

## Employee Directories And Org Charts

Use public employee directories and org charts to discover names that company pages omit. Search every relevant organizational-ladder rung in both the local language and English.

```text
[company] employees recruiter
[company] "HR Manager"
[company] "HR director"
[company] "[specialty] Team Lead"
site:theorg.com/org [company] HR
site:signalhire.com/companies [company] HR
```

Treat directory titles as discovery evidence. Confirm current employment through a fresher first-party or professional source before counting the person.

## Identity Pivots

Use public GitHub or GitLab profiles, Product Hunt, Stack Overflow, conference speaker pages, podcasts, personal sites, and authored articles to connect a person to stable usernames.

Useful pivots include:

- exact profile slugs
- email prefixes
- consistent avatars or biographies
- repositories or talks tied to the company and specialty
- links from one owned profile to another

A reused username is a candidate route until a contact source confirms ownership.

## Web X-Ray

Derive queries from the vacancy rather than fixed technologies:

```text
site:linkedin.com/in [company] [organizational-ladder role]
site:career.habr.com [company] [specialty]
site:setka.ru/users [company] [organizational-ladder role]
"[person name]" "[company]"
"[company]" "[organizational-ladder role]"
"[company]" "[vacancy title]" [contact keyword]
"[person name]" [known username]
```

Open the strongest results and follow their owned-profile links. A blocked or login-only result remains snippet evidence until another source confirms it.

## Telegram

First read the applicable project instructions for the required Telegram tool and permitted public fallback. Record authentication or availability failures exactly, then use only the allowed fallback.

Search the company, vacancy, and each promising person using:

- exact names in native and English layouts
- transliterations and shortened forms
- profile slugs from professional networks
- usernames from developer and speaker profiles
- public hiring posts and company-channel contact lines

When public `t.me` lookup is permitted, useful fallback queries include:

```text
site:t.me "[person name]"
site:t.me "[person name]" "[company]"
site:t.me "[person name]" HR
site:t.me [known username]
site:t.me [company] [vacancy-derived keyword]
site:t.me [company] recruiter
```

For a person, connect the account through its name, bio, company mention, owned links, or explicit hiring post. For a company channel, inspect the description, vacancy posts, post signatures, linked recruiters, and repost authors.

An exact account that merely exists is not ownership evidence. Return name matches and cross-platform username guesses as `inferred handle` until identity is confirmed.

## Currentness Conflicts

Compare publication or crawl dates and employment dates. Prefer a recent first-party or directly opened professional profile over an older directory. Preserve the conflicting source in the report and lower confidence until a second current source agrees.

Former employees are fallback referrers only when current contacts fail and their connection remains useful.

## Failure Recovery

- Tool unavailable or unauthorized: record the failure and continue through the locally permitted public branch.
- Profile blocked: retain the snippet as a discovery lead and corroborate elsewhere.
- Direct route absent: derive identity pivots, then move to the next organizational-ladder rung.
- Only a generic inbox found: report it separately and keep searching for people.
- Requested contact count unmet: exhaust the remaining source families and ladder rungs before reporting the shortfall.
