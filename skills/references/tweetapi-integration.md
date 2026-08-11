# Application integration

Use this guide when adding TweetAPI reads to an application. Verify the exact endpoint and parameters from the current public sources before coding.

## Keep the request server-side

Read the API key from `TWEETAPI_API_KEY` on the server. Fail closed when it is missing, and never return the key to a browser or mobile client.

```ts
const apiKey = process.env.TWEETAPI_API_KEY;

if (!apiKey) {
  throw new Error("TWEETAPI_API_KEY is not configured");
}

const url = new URL("https://api.tweetapi.com/tw-v2/user/by-username");
url.searchParams.set("username", username);

const response = await fetch(url, {
  headers: { "X-API-Key": apiKey },
});

if (!response.ok) {
  throw new Error(`TweetAPI request failed with status ${response.status}`);
}

const data = await response.json();
```

This example makes one request. Validate `username`, set an application-level timeout, and avoid logging headers or complete upstream payloads.

## Select REST or an SDK

- Prefer direct REST when the application needs a small number of stable reads.
- Prefer an official SDK when its current public API matches the application's runtime and the selected method is read-only.
- Verify the SDK method at <https://github.com/tweetapi/node> or <https://github.com/tweetapi/python> before using it.
- Disable automatic retries and automatic pagination when the client supports them.
- Do not use SDK namespaces for posting, engagement, account sessions, direct messages, or other excluded operations.

## Bound pagination

Return the first page and its cursor by default. For an explicitly requested multi-page feature, require all of the following:

- a small configurable maximum page count;
- a maximum item count;
- a maximum total request count;
- a timeout or cancellation signal;
- visible handling for quota exhaustion and partial results.

Never loop until the cursor is empty without a separate hard limit.

## Test without live quota

- Mock the upstream HTTP boundary.
- Test successful 2xx responses, 401/403 authorization failures, 429 quota responses, validation errors, timeouts, and malformed payloads.
- Assert that the API key never appears in returned errors or logs.
- Do not call the live API from unit tests, continuous integration, previews, or build steps.

## Avoid undocumented contracts

No public OpenAPI specification is listed in the current documentation index. Do not generate a client against a guessed schema. Use the public endpoint documentation and keep response parsing defensive until a canonical specification is published.
