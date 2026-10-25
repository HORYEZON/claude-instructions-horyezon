# Swagger update rule

Whenever creating or modifying an API route, also update `lib/openapi.ts` to keep the OpenAPI 3.0 spec in sync.

The Swagger UI lives at `/admin/api-docs` and is served by `app/api/openapi.json/route.ts`. The spec is defined as a single TypeScript object (`openApiSpec`) in `lib/openapi.ts` — not auto-generated from JSDoc.

**Why:** these projects have 100+ endpoints. The spec was written manually and is the only API reference. A new endpoint that isn't in the spec won't appear in the Swagger UI, making it undiscoverable for automation/testing.

**How to apply:**

- After adding a new route file, add the corresponding path(s) to `openApiSpec.paths` in `lib/openapi.ts`.
- Admin-only endpoints must include `security: adminSecurity` (the `adminSecurity` const is defined at the top of the file).
- Public endpoints omit `security`.
- Run the pre-push checks (`tsc --noEmit` + `yarn next lint` + `yarn build`) before pushing.
