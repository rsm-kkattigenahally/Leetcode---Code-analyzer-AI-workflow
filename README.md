# Leetcode---Code-analyzer-AI-workflow

# LeetCode Solution Analyzer – AI Workflow (n8n)

An automated n8n workflow that monitors your LeetCode submissions, analyzes accepted solutions using an LLM, and emails you a detailed code review — hands-free.

## What It Does

Every time you solve a LeetCode problem, this workflow:
1. Fetches your most recent submission via the LeetCode GraphQL API
2. Checks if the submission status is **Accepted**
3. Retrieves the actual submitted code along with runtime and memory stats
4. Sends the code to **GPT** for a senior engineer-style review
5. Emails the analysis report directly to you

## Workflow Architecture

```
Schedule Trigger
     ↓
Fetch Recent Submissions (LeetCode GraphQL API)
     ↓
Check if Status = "Accepted"
     ↓
Fetch Submission Code (LeetCode GraphQL API)
     ↓
LLM Analysis (OpenAI GPT via n8n LangChain node)
     ↓
Send Email Report (SMTP)
```

## What the AI Analysis Covers

For each accepted solution, the LLM returns:
- **Time Complexity** – with explanation
- **Space Complexity**
- **Possible Optimizations**
- **Cleaner / more idiomatic solution**
- **Edge Cases** to consider
- **Better algorithm** (if applicable)

## Tech Stack

- **n8n** – workflow automation
- **LeetCode GraphQL API** – submission data
- **OpenAI GPT** – code analysis (via n8n's LangChain integration)
- **SMTP** – email delivery

## Setup

1. Import `leetcode_analyzer.json` into your n8n instance
2. Configure credentials:
   - **LeetCode session cookie** – grab `LEETCODE_SESSION` and `x-csrftoken` from your browser's DevTools after logging in
   - **OpenAI API key** – add via n8n's OpenAI credential node
   - **SMTP credentials** – configure your email provider
3. Update the LeetCode `username` variable in the first HTTP Request node
4. Activate the workflow — it runs on a schedule automatically

## Notes

- The workflow only triggers analysis for **Accepted** submissions; failed attempts are ignored
- Session cookies expire periodically and will need to be refreshed
- You can adjust the schedule trigger to run as frequently or infrequently as you like
