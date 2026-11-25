# Rotcop

Save a social-media link; Rotcop understands it once, reduces it to a Recap and
an Action Item, and makes you decide: Acted On, Kept, or Archived.

Built on Firebase AI Logic hybrid inference. One rule governs every model call:
**the cloud reduces media to text, the device turns text into meaning.**

## Agent skills

### Issue tracker

Issues live in GitHub Issues on `alimurtazaathar/rotcop`, via the `gh` CLI.
See `docs/agents/issue-tracker.md`.

### Triage labels

The five canonical triage roles, each label named after its role.
See `docs/agents/triage-labels.md`.

### Open questions

Decisions deliberately deferred, each with what to verify and a decide-by date.
Read `docs/open-questions.md` before building anything it names.

### Debugging

Rotcop runs in Chrome. Firebase AI Logic failures — App Check attestation,
on-device `QuotaExceededError`, model-download errors — surface only in the
browser console. Read it with the `claude-in-chrome` tools rather than guessing
from the UI.

### Domain docs

Single-context: `GLOSSARY.md` and `docs/adr/` at the repo root.
See `docs/agents/domain.md`.
