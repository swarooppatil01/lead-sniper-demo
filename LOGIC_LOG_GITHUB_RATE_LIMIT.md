# Logic Log — GitHub API Rate Limit Handling

The workflow uses an authenticated GitHub API connection where available and polls every 15 minutes rather than continuously polling. The stargazer request is limited to 100 results per request.

GitHub's REST API exposes `x-ratelimit-remaining` and `x-ratelimit-reset` response headers. If a rate-limit response includes `Retry-After`, the workflow should wait for that duration. If `x-ratelimit-remaining` is 0, it should wait until the UTC epoch time in `x-ratelimit-reset`. For secondary rate limits where neither value is usable, wait at least one minute and use capped exponential backoff with a maximum retry count.

Permanent authentication/permission failures should not be blindly retried.

The GitHub documentation recommends authenticated requests, efficient polling, conditional requests where practical, and avoiding excessive concurrent requests.
