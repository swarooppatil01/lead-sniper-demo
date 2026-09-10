# Lead Sniper — GitHub High-Value Star Tracker

This n8n workflow watches the stargazers of `tiangolo/fastapi`, enriches each new star with the GitHub user profile, filters for high-value leads, asks an LLM for a one-sentence sales pitch, and posts qualifying leads to Discord.

## Qualification rule
A GitHub user is a High-Value Lead when:
- followers > 100, OR
- public_repos > 50

## n8n flow
Schedule Trigger → Get Stargazers → Find New Stars → Fetch GitHub Profile → High-Value Lead? → AI Sales Pitch → Discord

## Credentials / secrets
Create GitHub and OpenAI credentials in n8n. Set the Discord webhook as the n8n environment variable `DISCORD_WEBHOOK_URL` (or replace the final HTTP Request URL with a secret stored in n8n credentials).

Never commit tokens, API keys, or webhook URLs.

## Important note about the stargazer API
The workflow uses the GitHub REST stargazers endpoint and remembers already-seen users with n8n workflow static data. The static data is intended for an active/triggered workflow. For a production integration, a GitHub webhook or a persistent datastore would be preferable to polling.

## Demo
For a quick demo, use a test repository you control and add a star from a second GitHub account, or temporarily test the enrichment/filter nodes with the supplied sample profile. Do not repeatedly star/unstar a popular public repository just for testing.

## Rate-limit strategy
Authenticated GitHub REST requests provide a much larger primary rate limit than unauthenticated requests. The workflow is intentionally polled every 15 minutes and requests only the first 100 stargazers. If GitHub returns 403/429 for rate limiting, honor `Retry-After` when present; if `x-ratelimit-remaining` is 0, wait until `x-ratelimit-reset`; otherwise use at least a one-minute delay and capped exponential backoff for repeated secondary-limit failures. Do not blindly retry authentication errors.
