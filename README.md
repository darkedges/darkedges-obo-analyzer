# Token & Assertion Analyzer

Static token and assertion analyzer deployable to Cloudflare Pages.

## Features

- Manual analysis from the browser textarea
- Query-string ingest: `/?jwt=<token>`, `/?token=<token>`, `/?saml1.1=<assertion>`, or `/?saml2.0=<assertion>`
- Supports JWT payloads, raw JSON payloads, SAML 1.1 assertions, and SAML 2.0 assertions
- Preserves the existing OBO-focused JWT report for Entra on-behalf-of tokens

## Input Formats

- JWT: compact serialization such as `eyJ...`
- JSON: raw JSON payload pasted directly into the textarea
- SAML 1.1: raw XML assertion or a base64-encoded assertion payload
- SAML 2.0: raw XML assertion or a base64-encoded assertion payload

Example SAML query-string flows:

```text
/?saml1.1=<base64-encoded-saml11-assertion>
/?saml2.0=<base64-encoded-saml20-assertion>
```

## Local Development

```bash
pnpm install
pnpm dev
```

This starts Cloudflare Pages local dev using `public` as the site root. The `sync` script runs automatically to copy `jwt-analyzer.html` into `public/index.html` before the dev server starts.

## Editing the Analyzer

`jwt-analyzer.html` is the single source file. After editing it, run:

```bash
npm run sync
```

to copy it to `public/index.html`. This also runs automatically as part of `dev`, `deploy`, and `deploy:pages`.

## Deploy to Cloudflare Pages

1. Create a Cloudflare Pages project named `darkedges-obo-analyzer`.
2. Authenticate Wrangler:

```bash
npx wrangler login
```

3. Deploy:

```bash
pnpm deploy:pages
```

If you prefer npm:

```bash
npm run deploy
```

## Notes

- This tool inspects claims and assertions but does not validate JWT or XML signatures.
- Query-string mode can expose token or assertion content in browser history and logs.
