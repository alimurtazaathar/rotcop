# Glossary

Canonical vocabulary for Rotcop. No implementation details.

## Saved Item

A single piece of social-media content a user has captured, identified by its source URL. A Saved Item is *not* the media itself — Rotcop holds the URL, platform metadata, and text derived from it. The media stays where it was published.

## Tier

How much Rotcop can understand about a Saved Item, determined by what its source platform permits:

- **Deep** — full audio/visual understanding is available (YouTube, YouTube Shorts).
- **Text** — the post's full text is available, but no media understanding (X, Reddit).
- **Stub** — the platform exposes nothing usable; the user's own note is the only input (Instagram, TikTok).

Tier is a property of the *platform*, never a judgement about the item. It is always shown to the user rather than hidden.

## Backlog

The set of Saved Items that are neither Acted On, Kept, nor Archived. The Backlog is a debt, not a library: Rotcop's purpose is to shrink it.

## Pending

The state of a Saved Item that has been captured but not yet understood — no Category, no Recap, no Action Items. Capture is always instant; understanding may lag. A Pending item is **not** part of the Backlog and does not contribute to the Rot Score, because the user has not yet been given anything to act on. It joins the Backlog the moment it is understood.

## Revisit

A user returning to a Saved Item after capturing it. Revisiting resets an item's contribution to the Rot Score but does not remove the item from the Backlog — only a terminal state does.

## Rot Score

The size of the user's debt, in item-days:

> Sum, over every Saved Item in the Backlog, of the days since that item was **last revisited** (or since it was saved, if never revisited).

No weighting, no decay curve, no per-category tuning. An item leaves the sum only by reaching a terminal state.

## Terminal State

The three ways a Saved Item leaves the Backlog permanently:

- **Acted On** — its Action Item was completed.
- **Kept** — the user deliberately wants it in their permanent library. Kept items are not debt and are never shamed.
- **Archived** — the user killed it, or it aged out unwatched.

## Category

What kind of thing a Saved Item is, which determines the shape of its Action Item. Eight ship as defaults (Recipe, Workout, Place, Buy, Learn, Read Later, Idea, Entertainment); users may define their own.

## Action Item

The concrete obligation extracted from a Saved Item — the thing the user would actually *do*. An Action Item is typed by Category: a Recipe yields a shopping list, a Place yields somewhere to go. If Rotcop cannot produce a confident Action Item, it asks the user one clarifying question rather than inventing one.

## Recap

A short text distillation of a Saved Item, written when the item is saved and stored with it. The Recap exists so that Revisiting an item costs a few seconds of reading rather than re-opening the source. Rotcop's claim is that the Recap *replaces* the original: a user who reads it should not need to watch the video again.

## Triage

The daily flow in which Rotcop presents Backlog items and requires the user to move each to a Terminal State. Triage is the only mechanism that reduces the Rot Score at scale.

## Anti-slop

The stance that saving content without acting on it is the problem being solved. An anti-slop feature reduces consumption or forces an honest reckoning with the Backlog — it does not make hoarding more comfortable.
