# AI Log — Primary Prompts

1. Parallel/new-star tracking:
"Create an n8n workflow that polls a GitHub repository's stargazers, detects users that were not seen in previous polling runs, and processes only those new stars."

2. Profile enrichment:
"For every new GitHub star, call the GitHub /users/{username} REST endpoint and use the returned profile fields such as name, company, bio, followers, and public_repos."

3. High-value filter:
"Filter a GitHub user as a High-Value Lead when followers > 100 OR public_repos > 50. Stop processing users who do not satisfy either condition."

4. Sales pitch:
"Analyze the user's GitHub bio and company and generate exactly one concise sales-pitch sentence explaining why the team should reach out. Do not invent facts."

5. Discord output:
"Format a Discord message containing the user's name, bio, company, follower count, public repository count, and the AI-generated sales pitch."
