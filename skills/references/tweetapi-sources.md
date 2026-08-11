# Current public sources

Read the narrowest source that answers the task. Recheck these sources when exact endpoints, parameters, response fields, or installation steps matter.

## Canonical TweetAPI sources

- Documentation index: <https://tweetapi.com/llms.txt>
- Consolidated API documentation: <https://tweetapi.com/ai-docs-v2.txt>
- Product overview: <https://tweetapi.com/docs/getting-started/overview>
- Agent integration guide: <https://tweetapi.com/docs/getting-started/agents>
- Hosted MCP page: <https://tweetapi.com/mcp>
- Hosted MCP endpoint: <https://mcp.tweetapi.com/mcp>
- Node SDK: <https://github.com/tweetapi/node>
- Python SDK: <https://github.com/tweetapi/python>
- Privacy policy: <https://tweetapi.com/privacy>
- Terms of service: <https://tweetapi.com/terms>

Treat `llms.txt` as the discovery index and `ai-docs-v2.txt` as the current machine-readable documentation bundle. Prefer the endpoint's public documentation page when it contains more specific constraints.

## Source rules

- Do not copy the complete endpoint catalog into this skill. It will drift.
- Do not assume a documented operation is inside this skill's read-only scope.
- Do not infer parameter names from an SDK method or vice versa.
- Do not invent missing response fields, cursor behavior, or limits.
- If public sources conflict, name the conflict and ask the user before making a live request.
- If a source cannot be reached, say what could not be verified and avoid presenting remembered details as current.

## Artifacts that are not currently canonical

The current public index does not advertise a public OpenAPI specification or public Postman collection. Do not invent URLs for either artifact. Recheck the public index before making this claim in future work.

The public API documentation also describes write, account, and private-data capabilities. Those remain outside this skill even when their documentation is public.
