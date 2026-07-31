# Farthing docs

Public developer documentation for the Farthing agentic-checkout API, built with
[Mintlify](https://mintlify.com).

Everything here is written against the implementation in the private `checkoutvia` repo.
When a contract changes there, it changes here — the two are meant to be edited in the same
change, not reconciled later.

## Local preview

```bash
npx mintlify dev
```

Serves the site at `http://localhost:3000` with hot reload. No install step and no
`package.json` — the CLI reads `docs.json` and the `.mdx` files directly.

If a page renders blank or the nav is missing, the usual cause is a malformed `docs.json` or
a page listed in `navigation` that does not exist on disk:

```bash
npx mintlify broken-links   # dead internal links
```

Node 20+ is required. If `mintlify dev` misbehaves after an upgrade, clear its cache with
`rm -rf ~/.mintlify` and rerun.

## Deploys

Deploys are handled by Mintlify's GitHub app, not by CI in this repo.

1. Push to `main`.
2. The Mintlify app picks up the commit and rebuilds.
3. The site is live at the configured domain within a minute or two.

Pull requests get a preview deployment automatically. There is no build artefact to commit
and nothing to run locally before pushing beyond `mintlify dev`.

To connect a fresh clone: install the Mintlify GitHub app on the repo and point it at the
default branch. The dashboard is where the custom domain and analytics live.

## Layout

```
docs.json                   site config, theme, and the whole navigation tree
favicon.svg
introduction.mdx            what the product is
quickstart.mdx              end-to-end walkthrough, curl + TypeScript
authentication.mdx
checkout-lifecycle.mdx      status machine, failure reasons, timeline, receipt
user-actions.mdx            the human-in-the-loop contract
webhooks.mdx
live-view.mdx
buyer-profiles.mdx
buyer-sessions.mdx
sandbox.mdx
errors-and-limits.mdx
migrating-from-crossmint.mdx
api-reference/              one page per endpoint group
NOTES-FOR-REVIEW.md         internal — findings from writing these docs. NOT published.
```

`NOTES-FOR-REVIEW.md` is not in `docs.json`'s navigation, so Mintlify does not render it.
It is a working document for the team; delete it or move it out before it stops being
useful.

## House style

Worth preserving, because it is the reason these docs are usable:

- **Document what the code does, not what it should do.** If a behaviour is a wart, say so
  and tell the reader how to work around it. Nothing here should be a pleasant surprise or
  an unpleasant one.
- **Every JSON example must be a shape the API can actually produce.** Cross-check against
  `serializeCheckout` in `src/lib/checkouts.ts` before adding one.
- **Explain why**, especially for refusals and guardrails. "We never place an order without a
  card the user supplied" is a design decision with a reason; state the reason.
- **No marketing filler.** No "seamless", no "powerful", no "simply".
- **Never leak internals.** No account ids, ARNs, subnet or security-group ids, internal
  hostnames, deployment URLs, or key material. Use `https://api.farthing.ai`,
  `https://example-store.com`, and `your-sandbox-store.myshopify.com`.
