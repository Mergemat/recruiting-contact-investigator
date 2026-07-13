# Recruiting Contact Investigator

Skill for investigating and verifying direct recruiter, HR, hiring-manager, team-lead, C-level, and internal-referral contacts from vacancy and company traces.

## Install

```bash
bunx skills add Mergemat/recruiting-contact-investigator
```

Or with npm-compatible docs:

```bash
npx skills add Mergemat/recruiting-contact-investigator
```

## Use

Ask your agent to use `recruiting-contact-investigator` with a vacancy URL, company name, person name, or hiring lead. It separates current-role evidence from contact-route ownership, satisfies the requested contact count when possible, and keeps inferred handles out of the verified result list.

## Files

- `skills/recruiting-contact-investigator/SKILL.md` - main skill instructions
- `skills/recruiting-contact-investigator/references/contact-search-playbook.md` - branch-by-branch contact search playbook
- `skills/recruiting-contact-investigator/agents/openai.yaml` - OpenAI/Codex display metadata
