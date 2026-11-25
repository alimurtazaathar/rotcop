# 1. The cloud reduces media to text; the device understands text

Date: 2026-11-26

## Status

Accepted

## Context

Rotcop captures social-media links and turns them into categorised, actionable
items. It is built on Firebase AI Logic's hybrid inference SDK, which can run a
prompt on a local model (Gemini Nano, via Chrome's built-in AI) or in the cloud
(Gemini), chosen per call.

Two constraints shape how that choice can be made:

- The on-device model is **text-only**. It cannot accept video, audio, or PDF
  input, and Firebase caps on-device prompts at 9216 tokens.
- Only the cloud model can ingest a public YouTube URL directly and understand
  the video's audio and visuals.

The naive split — "small jobs local, big jobs cloud" — is arbitrary. It would
leave no principled answer to why any given item went one way or the other, and
the routing rule would drift as prompts changed.

## Decision

A single rule governs every call:

> **The cloud's only job is reducing media to text. The device's only job is
> turning text into meaning.**

Concretely, every Saved Item passes through a two-step pipeline:

1. **Reduce.** If the item is Deep tier (YouTube), the cloud model is given the
   public URL and returns a transcript-derived description. Text and Stub tier
   items skip this step entirely — their text already exists (post body, or the
   user's own note).
2. **Understand.** The on-device model takes that text and produces the
   Category, the Recap, and the Action Items.

Step 2 is identical for all three tiers. The on-device path is the default
(`PREFER_ON_DEVICE`), so it degrades to cloud automatically where no local model
exists.

Two explicit exceptions, both failing toward the cloud rather than toward an
error:

- Text estimated above ~7000 tokens is routed `ONLY_IN_CLOUD`, because
  exceeding the on-device cap throws rather than falling back.
- A failed cloud reduction retries once, then the item is demoted to Text tier
  and understood on-device from its title. The demotion is shown to the user.

## Consequences

**Good:**

- The routing rule is explicable in one sentence, and never needs re-litigating
  per feature.
- It yields a true, narrow privacy claim: the only thing sent to a cloud model
  is a URL that was already public. Notes, categories and recaps are processed
  locally. (This is a claim about *inference*, not storage — items still sync to
  the user's own Firestore.)
- Every item exercises the on-device path, so the hybrid behaviour is visible
  everywhere in the product rather than in one demo corner.
- Offline works for Text and Stub tiers with no special-casing: step 1 is a
  no-op and step 2 is local.

**Bad:**

- Deep-tier items need two round trips instead of one, and are slower.
- We discard some quality: the cloud model could produce a better Recap for a
  video in a single call than the local model can from a transcript. We accept
  a worse Recap to keep the rule intact.
- A device with no local model does both steps in the cloud, so the privacy
  claim holds for desktop Chrome users and not for mobile ones. The UI must say
  which is happening rather than imply the stronger claim everywhere.
