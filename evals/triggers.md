# Trigger evals — daquele-jeito

Regression baseline for the activation rules in `SKILL.md`'s frontmatter `description` (the single source of truth for activation). **Never load this file into a live session's context.** It exists to test edits to the `description`: after any change, have a fresh model instance — blind to the labels below — classify each prompt using only the new `description`, then compare against the expected labels. Any flip is a regression signal until proven otherwise.

Convention: **TRIGGER** = the skill should activate; **NO-TRIGGER** = it should not. Cases marked *(planted)* are deliberate near-misses that naive trigger rules get wrong.

## Slash commands

| # | Prompt | Expected | Why |
|---|--------|----------|-----|
| 1 | `/daquele-jeito migrate the auth module to OAuth` | TRIGGER | slash, unambiguous |
| 2 | `/that-way rename the config module across the repo` | TRIGGER | slash, unambiguous |

## Portuguese phrases

| # | Prompt | Expected | Why |
|---|--------|----------|-----|
| 3 | "Quero criar um SaaS de gestão financeira. Faz daquele jeito." | TRIGGER | bare imperative, final |
| 4 | "Faça daquele jeito o setup do monorepo" | TRIGGER | phrase-initial imperative *(planted: "final-only" rules miss it)* |
| 5 | "Refatora o parser pra suportar CSV. faz daquele jeito" | TRIGGER | bare imperative, final, lowercase (case-insensitive) |
| 6 | "não faz daquele jeito" | NO-TRIGGER | negated *(planted)* |
| 7 | "faz daquele jeito alternativo a migração" | NO-TRIGGER | qualified — different method *(planted)* |
| 8 | "se fizer daquele jeito, o deploy demora mais" | NO-TRIGGER | conditional |
| 9 | "você fez daquele jeito ontem no outro repo" | NO-TRIGGER | past tense |
| 10 | "como funciona daquele jeito?" | NO-TRIGGER | meta-question |

## English phrases

| # | Prompt | Expected | Why |
|---|--------|----------|-----|
| 11 | "Build the CSV importer for the billing app... do it right." | TRIGGER | bare imperative, final |
| 12 | "Refactor the billing module the right way." | TRIGGER | bare imperative, final |
| 13 | "Set up CI for this repo. Just do it that way." | TRIGGER | "just do it that way" variant, final |
| 14 | "Do it right: migrate the database to Postgres." | TRIGGER | phrase-initial |
| 15 | "do it right after lunch" | NO-TRIGGER | temporal adverbial, not a method imperative *(planted)* |
| 16 | "make sure you do it right" | NO-TRIGGER | phrase embedded in a longer clause, not bare *(planted)* |
| 17 | "you did it right yesterday" | NO-TRIGGER | past tense *(planted)* |
| 18 | "I want to do it right" | NO-TRIGGER | modal / intent |
| 19 | "this is the right way to handle errors in Go" | NO-TRIGGER | descriptive |
| 20 | "do it the right way we discussed" | NO-TRIGGER | reference to a prior method |
| 21 | "what does 'the right way' mean here?" | NO-TRIGGER | meta-question |

## Boundary note

Case 16 is the sharpest near-miss: "make sure you do it right" contains a trigger phrase and is imperative in spirit, but the phrase is not bare — it's embedded in "make sure you...". If a future edit to the `description` flips this case to TRIGGER, treat it as a regression, not progress.
