**401 vs 403 — the classic pair, expect this one.**

- **401 Unauthorized**  - request is made by an unauthorized user. 
- **403 Forbidden** — request is made from a authorized user but they dont have permission to view the content.

**200 vs 201 on a POST.**

- 201 - returns the content that was called with POST e.g. a new user or list item.
- **200 OK** - returns a generic success call, but no data is returned.

**409 Conflict.** - We tried adding a user with a duplicate email already used for example.
- Ties straight to your FitLog `users_email_key` unique constraint — a duplicate email insert is exactly a **409** at the API layer. _Use that example if it comes up_ — it connects your DB work to your API design.
429 - Too many requests.


**500 vs 503.**

- **500 Internal Server Error** — something broke on our side, unexpectedly. Unhandled exception, a bug. Generic server failure.
- **503 Service Unavailable** — we're up but can't serve right now: overloaded, or down for maintenance.

**200 vs 204 on a PUT/DELETE.**

- **200 OK** — worked, and here's an update resource in json.
- **204 No Content** — worked, and there's deliberately nothing to send back. Common on DELETE, or a PUT where the client doesn't need the echo.
