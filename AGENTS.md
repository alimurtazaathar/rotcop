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

### Firebase AI Logic

The `firebase-ai-logic-basics` skill covers setup, App Check and multimodal, but
its hybrid-inference section is five lines that link out. For the on-device and
cloud-fallback wiring this project is built on, read
https://firebase.google.com/docs/ai-logic/hybrid/web/get-started and
https://firebase.google.com/docs/ai-logic/hybrid/web/configuration-options
directly. Requires Firebase Web SDK v12.5.0+; on-device inference is desktop
Chrome only, capped at 9216 tokens, and text-only.

### Debugging

Rotcop runs in Chrome. Firebase AI Logic failures — App Check attestation,
on-device `QuotaExceededError`, model-download errors — surface only in the
browser console. Read it with the `claude-in-chrome` tools rather than guessing
from the UI.

### Domain docs

Single-context: `GLOSSARY.md` and `docs/adr/` at the repo root.
See `docs/agents/domain.md`.
