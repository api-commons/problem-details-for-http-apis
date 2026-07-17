# Problem Details for HTTP APIs

A reusable base for applying **Problem Details for HTTP APIs** to your own API. Problem Details is a standardized way to carry machine-readable error information in an HTTP response — a `type`, `title`, `detail`, `instance`, and `status` — so producers and consumers can share one predictable error format instead of every API inventing its own.

This base was originally developed by [Bump.sh](https://bump.sh/) as part of their Train Travel API template (also published in the commons as [train-travel](https://github.com/api-commons/train-travel)). Here it is reduced to just the Problem Details components on their own, so you can drop the pattern into any API and reuse it across your surface.

## What's in this repo

- [openapi.yml](openapi.yml) — An OpenAPI 3.1.0 base that defines a `Problem` schema plus ready-to-reuse `4xx`/`5xx` responses (`BadRequest`, `Unauthorized`, `Forbidden`, `NotFound`, `Conflict`, `TooManyRequests`, `InternalServerError`), each returning `application/problem+json` and `application/problem+xml`, along with `RateLimit` and `Retry-After` headers. Fork it and reference these components from your own OpenAPI.

## Links

- [RFC 7807 — Problem Details for HTTP APIs](https://datatracker.ietf.org/doc/html/rfc7807) — the specification that defines the "problem detail" object (updated and obsoleted by [RFC 9457](https://datatracker.ietf.org/doc/html/rfc9457)).
- [Train Travel API: A Modern OpenAPI PetStore Replacement](https://bump.sh/blog/modern-openapi-petstore-replacement) — background on the Bump.sh template this base was drawn from.

## How it fits API Commons

Problem Details is an [API Commons](https://apicommons.org) building block — a small, machine-readable pattern you can reuse across the APIs you produce so error handling stays consistent and interoperable across documentation, mocking, and testing tools.

## Support
If you have any questions feel free to [submit an issue on this repository](https://github.com/api-commons/problem-details-for-http-apis/issues/new), engage [directly with the IETF RFC](https://datatracker.ietf.org/doc/html/rfc9457), or contact [Bump.sh](https://bump.sh/) about their Train Travel API and services.

## Part of API Commons

A machine-readable building block from **[API Commons](https://apicommons.org)** — open specifications and schemas for the APIs you produce and consume. See all building blocks and tools at **[apicommons.org](https://apicommons.org)** and the tools at **[apicommons.org/tools](https://apicommons.org/tools/)**.

**Related building blocks**
- [train-travel](https://github.com/api-commons/train-travel) — the Bump.sh Train Travel API template this base was drawn from
- [json-api](https://github.com/api-commons/json-api) — JSON:API schemas and governance for standardizing API responses
- [examples](https://github.com/api-commons/examples) — shared request/response examples for API operations
- [rate-limits](https://github.com/api-commons/rate-limits) — a schema for the rate limits behind the `RateLimit`/`Retry-After` responses here
