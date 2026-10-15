# Huntly knowledge library

The files in this directory are runtime inputs, not general project
documentation. Huntly selects a small set for each agent and includes the
selected text in its working context.

## Stable playbooks

- `account_navigation.md` — collect useful facts from a company site.
- `email_frameworks.md` — structure concise, permission-aware outreach.
- `lead_qualification.md` — distinguish fit, timing, and evidence.
- `linkedin_outreach.md` — keep social outreach human and bounded.
- `objection_handling.md` — classify concerns and choose a measured response.
- `offer_strategy.md` — connect a prospect problem to a concrete next step.
- `prospecting_tactics.md` — search and prioritize companies without fabricating
  facts.
- `sales_methodology.md` — sequence work and define healthy pipeline movement.
- `signal_playbook.md` — interpret confirmed buying signals.

## Generated context

Training can create:

- `product_knowledge.md`
- `competitive_intel.md`

These files describe a particular operator's offer and market. They are
intentionally ignored by source control and may be regenerated when the offer
changes.

## Editing rules

Write instructions as constraints and decisions, not as a transcript or a
collection of slogans. Keep claims grounded in facts available to the agent.
Do not add credentials, personal data, or instructions to evade provider,
privacy, or sending limits.

After changing a playbook, run the agent tests that exercise the affected
workflow. A playbook change can alter outbound copy even when Python code is
unchanged.
