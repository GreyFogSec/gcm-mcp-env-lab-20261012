# GCM project-MCP environment lab

This repository is a controlled reproduction of a GitHub Actions trust-boundary
failure: a privileged `issue_comment` workflow checks out an external fork pull
request, automatically starts a project-defined MCP process, and exposes the
action process environment to that process.

The lab uses:

- the exact pinned action commit used by `facebookresearch/gcm`;
- a deliberately invalid, controlled `ANTHROPIC_API_KEY` canary;
- the job-scoped `GITHUB_TOKEN` rather than a production GitHub App token;
- hash-only evidence uploaded as a workflow artifact.

No real API key is required. The external fork PR adds `.mcp.json` and a benign
MCP startup probe. An organization member comments `@claude reproduce` on the
PR. The Claude API request is expected to fail, while the later verification
step proves that project MCP startup already occurred with the credential-
bearing environment.

## Expected result

The `verify controlled hash-only proof` step succeeds and the
`gcm-mcp-controlled-proof-<run-id>` artifact contains:

- `gcm-mcp-proof.json`, holding SHA-256 digests only;
- `verification.json`, holding pass/fail assertions only.

The workflow is intentionally interaction-gated and exists only for controlled
security testing.
