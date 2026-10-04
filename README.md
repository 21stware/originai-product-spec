# originai-product-spec (Claude Code plugin)

Write product specs as RPML files in your repo and review them as a live canvas in a Claude Artifact.
Comment on the canvas, press **Send to Claude**, and Claude edits the spec. No account, no token.

## Install

```text
/plugin marketplace add 21stware/originai-product-spec
/plugin install originai-product-spec@originai-product-spec
```

(Also listed in the `21stware/originai-context` marketplace as `originai-product-spec@origin-claude-marketplace`.)

## Use

```text
/originai-product-spec:init      # create specs/ and publish the canvas
/originai-product-spec:preview   # rebuild from specs/ and republish
```

Needs a Claude Code host that has the `Artifact` tool and Node >= 18.
Specs live in `specs/`; the canvas link is kept in `specs/.artifact.json`.

## What it runs, writes and sends

- **Hook**: after Claude writes or edits a `.rpml` file under `specs/`, `scripts/validate.mjs` checks it locally and reports problems back to Claude. No network.
- **Build**: `scripts/build-artifact.mjs` (Node, no dependencies) reads `specs/*.rpml` and writes one self-contained HTML page to `.origin/artifact.html`. No network.
- **Publish**: Claude publishes that page through Claude's own `Artifact` tool, to your Claude account. The plugin itself makes no network requests, has no telemetry, and needs no account or token.
- **Files written**: `specs/*.rpml`, `specs/.artifact.json` (the canvas URL) and `.origin/artifact.html` (build output).
- **Bundled runtime**: `assets/gallery.js` is the minified RPML canvas runtime, built from the open-source `packages/rpui` of the Origin monorepo (https://github.com/21stware/origin) and inlined into the page so it works inside the Artifact sandbox. `scripts/validate.mjs` is a bundle of the RPML validator.

## License

MIT, see `LICENSE`.
