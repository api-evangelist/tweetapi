---
name: tweetapi
description: Integrate TweetAPI or use its hosted MCP server for bounded, read-only access to public Twitter/X data. Use for public profile, tweet, search, list, community, space, relationship, and documentation lookups, or for adding safe server-side TweetAPI reads to an application. Exclude posting, engagement, account access, private messages, login automation, billing, and administration.
---

# TweetAPI read-only workflows

Use TweetAPI for public Twitter/X data while keeping the operation read-only, credentials server-side, and quota use explicit.

## Enforce the safety boundary

- Allow only operations that retrieve public data or public documentation.
- Do not post, reply, delete, like, retweet, bookmark, follow, unfollow, or change profile, list, or community state.
- Do not access direct messages, inboxes, notifications, private account analytics, billing, API-key management, administration, or internal endpoints.
- Do not perform Twitter/X login or session automation.
- Never request, accept, store, or forward passwords, cookies, `authToken`, `ct0`, TOTP codes, proxy credentials, or private-message content.
- Never call an endpoint that requires any of those secrets, even if it appears in the broader public API documentation.
- Treat the HTTP method as insufficient evidence of safety. Confirm the operation's effect and required fields.
- Do not claim affiliation with X Corp. TweetAPI is a third-party service.

If a requested task crosses this boundary, explain the excluded action and offer a read-only alternative. Do not weaken the boundary merely because the wider TweetAPI product supports more operations.

## Choose an execution path

Use the hosted MCP server when the user wants an agent to inspect documentation or make a bounded public-data lookup. Prefer its OAuth flow so no API key enters the conversation. Read [references/hosted-mcp.md](references/hosted-mcp.md) before configuring a client or invoking a live tool.

Use server-side REST or an official SDK when modifying an application. Read [references/integration.md](references/integration.md) before writing integration code.

For documentation-only questions, read the canonical public sources without making a live data request. Read [references/sources.md](references/sources.md) whenever endpoint names, parameters, response fields, or installation instructions must be current.

## Follow the workflow

1. Identify whether the request is documentation-only, a live lookup, or an application integration.
2. Check the operation against the safety boundary before selecting an endpoint or tool.
3. Consult the current public documentation instead of relying on a copied endpoint catalog.
4. Choose MCP OAuth for agent use or a server-side API key for application code.
5. State before a live request that it consumes quota.
6. Make at most one live tool call and retrieve one page by default.
7. Return any pagination cursor instead of following it automatically. Ask before another page.
8. Treat only HTTP 2xx responses as success. Surface authentication, quota, and validation failures clearly.
9. Validate application changes with mocks or fixtures. Do not spend live quota during setup or tests unless the user explicitly authorizes it.

## Control quota and pagination

Every live data request consumes quota. Retries and subsequent pages can consume additional quota.

- Do not auto-paginate.
- Do not silently retry a live request.
- Do not run a live “smoke test” merely to prove configuration.
- Stop after the first page and expose the returned cursor.
- If the user explicitly requests multi-page application behavior, implement a configurable maximum for pages, items, and total requests. Never use an unbounded pagination helper.

## Protect credentials

- Keep `TWEETAPI_API_KEY` in a server-only environment variable or secret manager.
- Never embed it in browser code, mobile bundles, source control, logs, examples, or prompts.
- Use `YOUR_API_KEY` only as a documentation placeholder.
- Prefer OAuth with `https://mcp.tweetapi.com/mcp` for compatible agent clients.
- Redact response data before logging if it could contain sensitive user-provided input.

## Preserve source accuracy

The public product documentation covers a wider product than this skill. Do not infer that every documented endpoint is allowed here. Do not invent an OpenAPI URL, Postman collection, SDK method, or MCP tool name. Verify current details from the sources reference.

When sources disagree, report the conflict and avoid a live call until resolved.
