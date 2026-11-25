# Open questions

Decisions deliberately deferred. Each one names what has to be verified, what it
would change, and when it has to be answered by. Resolve one by deleting it here
and recording the outcome where it belongs (an ADR if it is hard to reverse, the
ticket otherwise).

---

## OQ-1 — Can queued items be processed in the background, with no tab open?

**Status:** unresolved. Research started and deliberately stopped; pick it up at
implementation time, not before.

**Why it matters.** Rotcop runs entirely client-side (no Cloud Functions by
design — see ADR 0001). So inference runs in the page, and when the last tab
closes, nothing runs. The agreed baseline is deferred processing: capture is
instant and writes a `Pending` item; understanding happens whenever the app is
next open. `Pending` items are outside the Backlog and do not count toward the
Rot Score, so the number stays honest either way.

**The upside if it works.** A service worker could finish queued items after the
tab closes. That adds a second dimension to the routing rule in ADR 0001 —
inference location would depend on **presence**, not only on hardware:

> You're here → it thinks on your device. You're away → it finishes in the cloud.

The service worker has no local model, so it would run `ONLY_IN_CLOUD` and the
item's badge would read `IN_CLOUD`. That is honest, and it is a genuinely
non-obvious use of the hybrid SDK.

**What has to be verified before committing to it:**

1. Does Background Sync (`registration.sync.register()`) fire in desktop Chrome
   after every tab for the origin is closed, with Chrome still running? What are
   the real constraints — time limits, retry caps, prior-engagement requirements,
   installed-PWA-only?
2. Can the Firebase JS SDK v13+ be used inside a `ServiceWorkerGlobalScope` —
   specifically `firebase/app`, `firebase/firestore`, `firebase/ai` and
   `firebase/app-check`?
3. **Most likely blocker:** can App Check mint a token in a service worker, or
   does the reCAPTCHA Enterprise provider require a `document`/`window`? Firebase
   AI Logic requires App Check. If the token cannot be minted there, this whole
   idea dies and the baseline stands.
4. Does Firestore write cleanly from a service worker, given the main-thread
   instance may hold the IndexedDB persistence lock?

**Note:** the planned Chrome extension may make this moot. An extension service
worker can already do background work with no tab open, so capture-and-process
from the extension covers most of the same ground.

**Decide by:** day 5, and only if the extension has already landed. If it is not
resolved, ship the deferred-processing baseline — which is a complete, honest
product on its own.
