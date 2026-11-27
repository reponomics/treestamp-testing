# GitHub compatibility validation policy

This repository is a publicly visible, ephemeral workspace under the control of the [treestamp](https://github.com/reponomics/treestamp) maintainer. Write access is currently restricted to the maintainer. Its tentative purpose is live testing of GitHub rendering behavior.

The repository is isolated from treestamp's supply chain. Apart from the maintainer's identity, credentials are not shared, and it has no workflow access to the main repository. Its inputs and outputs have no direct integration with treestamp's development processes, and no project workflow depends on its existence.

It produces no assets intended for public consumption. Its contents are available for inspection, but are disposable and do not constitute authoritative statements about public treestamp behavior.

## Ownership and access

The maintainer controls the repository's fixtures, settings, visibility, access decisions, and publication. Public readers may inspect, clone, and propose changes through issues or pull requests. Read access does not grant write authority.

Only the maintainer may push or merge. Agents may prepare patches and conduct read-only validation; publication through the maintainer's account requires explicit maintainer authorization for that change. Test runs and scheduled jobs have no independent authority to publish fixtures, change visibility, or grant access. Routine treestamp CI receives no write credential for this repository.

The maintainer reviews access when changing membership or automation. Any delegation of write authority must be recorded here before use. Repository permissions enforce access; prose or CODEOWNERS alone does not restrict pushes.

## Choosing a method

| Method | Evidence supplied | Not established |
| --- | --- | --- |
| Local deterministic tests and fake GitHub | Repeatable document, curation, and application behavior | Actual GitHub transport or rendering |
| GitHub `POST /markdown` | HTML returned for the submitted text, mode, and context | Image loading, appearance, original file-byte decoding, or Git filename support |
| Actual GitHub README and file views | Rendering in the observed browser, page, and appearance | Semantic round-trip fidelity or untested browsers |
| Real Git trees and blobs API | Acquisition behavior and identities actually exposed by GitHub | Fidelity merely assumed by a fake |
| Local/hosted comparison | Agreement for the recorded source and operations | Universal parity, future availability, or persisted-session recovery |

Use the smallest set that answers the question. Markdown embedding or escaping changes call for an API probe; SVG appearance and README image changes need actual page inspection; acquisition or filename-encoding changes need real API checks; shared document changes need local/hosted comparison when they could affect parity. Ordinary regression suites remain offline and deterministic. Live checks are explicitly invoked supplementary validation, not an implicit dependency on mutable `main`.

## Fixture lifecycle

1. Prepare synthetic, non-sensitive data. Add the question, expected behavior, required observations, and input paths to [FIXTURES.md](FIXTURES.md). Distinguish supported cases, known limitations, and experiments; a current failure is not automatically the desired contract.
2. Include source inputs, regeneration commands, and the producer's version or commit for generated fixtures. Identify hand-adjusted experiments. Parity fixtures need source trees that actually match the entries described in their documents.
3. Review the complete change, including embedded SVG metadata and attachments, before publication. Never publish credentials, private repository material, user drafts, or captured user sessions. Boundary fixtures use inert synthetic payloads without third-party network destinations.
4. The maintainer publishes the change and records its commit. Preserve visual controls and commits cited by evidence. Add follow-up commits rather than force-pushing published history. Mark retired scenarios; security-sensitive removal is a separate maintainer decision.
5. Record observations using the evidence fields below. Keep essential inputs and concise results. Retain screenshots or raw responses only when needed to substantiate findings; remove unneeded local captures when the check ends.

A fixture can remain reproducible without its old commit remaining loadable in the hosted editor. Pin source and evidence to commits; do not use a cached editing-session URL as an archive.

## Markdown API protocol

Use [`POST /markdown`](https://docs.github.com/en/rest/markdown/markdown#render-a-markdown-document) with explicit `mode: "gfm"` and `context: "reponomics/treestamp-testing"`. Context resolves repository references; it does not select a commit or prove an image exists. Public-resource requests can be unauthenticated. If authentication is needed, use approved credentials with the least required read access; never copy tokens into fixtures or reports.

Prepare a UTF-8 JSON request from a committed synthetic sample, preserving its exact Markdown text. For example:

```json
{
  "text": "![Repository tree](assets/treestamp-adaptive.svg)\n",
  "mode": "gfm",
  "context": "reponomics/treestamp-testing"
}
```

An explicit GitHub CLI probe is:

```sh
gh api --method POST markdown \
  -H 'Accept: text/html' \
  -H 'X-GitHub-Api-Version: 2026-03-10' \
  --input request.json --include > response.http
```

Inspect status, rate-limit headers, and HTML. Assert intended structure and literal content rather than incidental HTML formatting. For example, inspect whether an image reference becomes an `img` and whether inline SVG survives sanitization; neither establishes image loading or appearance. Do not execute untrusted returned HTML outside an isolated test context.

The JSON endpoint receives decoded text, so it cannot certify GitHub's decoding of an original Latin-1 or UTF-16 repository file. Encoding questions need committed byte fixtures and actual repository inspection. `/markdown/raw` is a separate mode without GFM support; do not label its results as GFM validation.

## Actual GitHub visual checks

Inspect the README on `github.com`, including its embedded image, and distinguish that view from the SVG's own file preview. For adaptive SVG work, inspect explicit light and dark GitHub appearance and system-following appearance. Include a case where OS appearance differs from GitHub's selected appearance. Record observations rather than assuming which setting drives the media query.

Record browser/version and OS, image loading, foreground/background legibility, transparency, clipping, branch continuity, annotation placement, and the result of changing appearance. Compare fixed light/dark controls when available. Mark untested combinations as not reviewed. Successful screenshot capture or an API response is not visual inspection.

## Real acquisition and editor parity

For tree/path questions, obtain expected identities from Git objects using `git ls-tree -rz --full-tree COMMIT` and raw blobs. Do not infer original filename bytes from an OS checkout or an already-decoded JSON string. Compare real API path representations, object IDs, modes/kinds, and relevant blob bytes. Record refusals, truncation, and lossy representations explicitly. See the [Git trees API](https://docs.github.com/en/rest/git/trees).

For local/hosted comparison, record the current default-branch SHA, use a fresh clean checkout at that SHA, and select the same root, document, settings, and edit sequence. Record checkout attributes or filters that change bytes. Compare meaning, exact entry identities, kinds, annotations, folds, hidden paths, and applicable managed-region formatting. Check local surrounding-byte preservation and publication conflicts separately from hosted export.

Check the hosted response's source commit and recheck default-branch HEAD after the journey. If HEAD moved, mark the comparison stale and establish a new matched baseline. The current hosted acquisition policy does not promise to reopen an uncached historical commit. Cache expiry or service unavailability is an operational result, not evidence of format incompatibility. Editing-session URLs are not durable fixture identities or a supported sharing workflow.

## Execution and evidence

Use bounded requests and time, serial GitHub API access, and rate-limit response headers. Respect `Retry-After` and reset times; stop on quota or service problems instead of retrying indefinitely. Separate product failures from upstream/environment failures and checks not performed. Any future live CI job must be explicit, read-only, and independently reported; it must not expose privileged credentials to untrusted PR code or publish fixtures as a side effect.

Record meaningful runs in a maintainer-reviewed report or the related treestamp change, including:

- the question, fixture identifier, exact repository commit and paths, and treestamp commit/version or deployed build
- method, reproduction steps, timestamp, expected versus observed result
- API endpoint/version/mode/context and relevant status/rate-limit information, without credentials
- browser/OS/appearance and page URL for visual checks; source commit and checkout conditions for parity
- necessary evidence links, limitations, and outcome: pass, product failure, environment/upstream failure, stale, or not reviewed

Reports describe observations. Product contracts belong in treestamp's durable design and testing documentation, not in decision-history records or unexplained fixture expectations.
