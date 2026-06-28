# Contact Search Playbook

Use this reference while executing the investigation loop. Treat each branch as a source of evidence, then return to ranking contacts.

## HH / HeadHunter Vacancy

If the user provides an HH vacancy ID or URL, run the `firehh` CLI before browsing:

```bash
firehh <vacancy-id>
```

Example:

```bash
firehh 133561763
```

For URLs like `https://hh.ru/vacancy/133561763`, extract `133561763` and run the same command.

Use the output to capture:

- vacancy title
- employer/company
- region
- skills and role keywords
- publication URL
- description clues about team, product, department, or recruiter

If the command fails because HH auth/token is missing, note the failure in evidence checked and continue with public-web search from the HH URL, company, and vacancy title.

## Vacancy Title + Company

Search:

```text
[vacancy title] [company]
```

Check several results besides HeadHunter. If a vacancy appears on Telegram, Habr Career, LinkedIn, or an alternative job site, inspect the publication for:

- named recruiters
- company representatives
- Telegram handles
- social links
- department or team names
- repost authors

## Google X-Ray

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

For a person example:

```text
site:linkedin.com катя смоленцева рекрутер
```

When search terms are weak, first generate a query plan:

```text
Мне нужно найти контакт рекрутера или потенциального нанимающего менеджера.

Исходные данные:
- должность, на которую нанимают: [вставить]
- регион: [город/страна]
- источник вакансии (если есть ссылка): [вставить]

Твоя задача:
1. Определи, какие роли в этой компании вероятнее всего отвечают за найм на эту позицию.
2. Сформулируй точные поисковые запросы для LinkedIn, Google X-Ray, Telegram и HH, если применимо.
3. Предложи 5-10 вариантов должностей, по которым стоит искать человека внутри компании.
4. Предложи приоритет: кому писать сначала, кому вторым, когда писать сразу нанимающему.
5. Если прямого рекрутера нет, предложи альтернативы: подрядчики, агентства, публичные сотрудники, email-структуры.

Ответ дай как алгоритм действий, без общих советов, с опорой на executive search.
```

## Habr Career Company Page

On a company page, check the lower-left/company contact area for responsible people.

If recruiter or hiring contact is listed:

1. Open the profile.
2. Look for Telegram in the profile description.
3. If Telegram is absent, copy the nickname from the profile URL/search string and search it in Telegram.
4. If that fails, search Telegram by first name + last name.
5. If no private contact appears, search for another recruiter or hiring manager.

If no responsible contact is listed:

1. Open the company employees list.
2. Filter or search for recruiting and HR roles.
3. For hiring-manager search, filter or inspect leadership roles and current-company status.
4. Open several profiles.
5. Keep only profiles whose specialty or department matches the vacancy.
6. Find 2-3 relevant profiles and investigate contact routes one by one.

## Person Name + Company

Search:

```text
[first name last name] [company]
```

If LinkedIn appears:

- inspect contact/about text
- if no contact exists, use LinkedIn DM as a possible route

If Instagram appears:

- inspect links and bio
- copy nickname and search it in Telegram
- inspect stories/posts for DM links or other social sources

If a personal Telegram channel appears:

- inspect channel description for contact details
- search posts for `ищу`, `нанимаю`, `вакансия`
- inspect those posts for DM links

If media, blog, or Telegram mentions appear:

- read the publication
- follow social links to the person
- then apply the relevant social branch above

If no direct route appears, find another recruiter or hiring manager and continue.

## Evidence Quality

Classify each lead before ranking it:

- `verified page`: the opened public page directly states the role, company, or contact route.
- `cross-source match`: two or more independent public sources connect the same person, company, role, or handle.
- `public indexed snippet`: search result text suggests the connection, but the page itself is blocked, unavailable, or login-only.
- `unverified lead`: plausible but not confirmed enough to contact first.

Use snippet-only leads as backup leads unless no verified or cross-source contact exists.

## Telegram Search

Run this branch for the company, vacancy, and every promising person.

Search Telegram directly when available. Also use public web queries against Telegram pages:

```text
site:t.me [company]
site:t.me [company] [vacancy title]
site:t.me [company] backend
site:t.me [company] golang
site:t.me [company] recruiter
site:t.me [person name]
site:t.me [nickname]
```

For company channels:

1. Inspect channel description for contacts.
2. Search posts for `вакансия`, `ищем`, `нанимаем`, `backend`, `golang`, `go`, `разработчик`.
3. Check bottom-of-post contact details and linked recruiters.
4. Check repost authors and linked personal channels.

For people:

1. Search exact name in Russian and English layouts.
2. Search transliterations and shortened forms.
3. Search nicknames from LinkedIn, Habr Career, GitHub, Instagram, email prefix, or profile URL.
4. Confirm the handle by cross-checking bio, avatar/name, company mention, vacancy posts, or linked source.

Rank Telegram routes highest when the public source explicitly invites contact for hiring or vacancies. Downgrade a Telegram handle if it is only name-matched and not tied to the company or role.

## LinkedIn Company Page

Company descriptions may name responsible people, but email alone is weak for this workflow.

If responsible people are visible:

1. Inspect each profile for contacts, especially Telegram.
2. If only email is visible, search the email nickname in Telegram.
3. Search first name + last name in Telegram in Russian and English layouts.
4. Try plausible transliterations, such as `alex`, `aleksei`, `aleksey`.

If no responsible people are visible:

1. Open employees.
2. Search or filter for recruiters, talent acquisition, HR, department leads, and likely hiring managers.
3. Continue only with current employees or clearly relevant former employees when current options fail.

## Company Site And Public Channels

Search the company name and inspect:

- company site/team pages for HR director, CPO, CTO, department head, or team lead
- Habr Career profile via `site:habr.career [company or name]`
- Telegram company channel
- publications mentioning team, vacancies, and bottom-of-post contact details

For Telegram company channels, check:

- channel description contacts
- posts about vacancies
- posts naming team members or recruiters

## Stop Conditions

Stop only when one is true:

- at least one strong direct contact and one fallback contact are ranked
- every relevant branch has been checked and the report explains why no contact route was found
- the missing input is blocking and cannot be inferred from public traces
