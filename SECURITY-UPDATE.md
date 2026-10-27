# Dependency security update — 2026-10-27

Next.js 13 is replaced with 15.5.28. Legacy Contentlayer is replaced with contentlayer2 0.5.8 to support this Next.js version; fonts use next/font, route params are awaited, and the MDX renderer has an explicit client boundary. remark-gfm is updated for the MDX parser. The existing repository omitted both imported CSS files; minimal Tailwind and code-highlight styles are added to restore builds.

The optional Upstash view counter returns zero when credentials are absent; its write endpoint returns 503. With credentials configured, existing counter behavior is retained, and duplicate IP requests now return early as intended. x-forwarded-for is trusted only when a deployment proxy overwrites it.

Validation: full Next.js production build, 20 generated content documents, TypeScript check. No existing automated test suite. CI repeats frozen-lockfile installation, build and typecheck on Node 22. pnpm is pinned to 10.14.0.

## Residual upstream findings

The updated pnpm audit reports no Critical findings and three findings in the article-generation dependency tree:

- @opentelemetry/propagator-jaeger 1.30.1 (High): fixed in 2.9.0; the generation tool uses the OpenTelemetry 1.x SDK. A standalone major override risks SDK incompatibility.
- @opentelemetry/core 1.30.1 (Moderate): fixed in 2.8.0; same SDK constraint.
- zod 3.25.76 (High): fixed in 4.6.6; Contentlayer uses the v3 API for local content/config validation.

UUID is overridden to the patched 11.x line; article generation is verified against its v4 API.

These paths belong to trusted local article generation, not application handlers accepting public request data. Do not build untrusted MDX/configuration. Removing these findings requires an upstream generator release or a separately tested generator replacement; they are not dismissed or represented as fixed. No secrets or production data were used in local validation.

The last recorded production deployment before this change was failed (2024-11-16). A successful merge alone does not prove the public site has deployed successfully.
