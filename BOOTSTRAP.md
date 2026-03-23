# Claude Sub-Agent — Bootstrap Prompt

Zuberi uses this to initiate a Claude sub-agent session via Kasm browser.

## For James: Setup

1. Open Kasm browser at localhost:6901
2. Navigate to claude.ai and ensure you're logged in
3. Open a new conversation
4. Tell Zuberi the browser is ready

Zuberi types the bootstrap prompt into the Claude conversation, then communicates via the relay.

---

## Bootstrap Prompt (Zuberi types this into Claude.ai)

```
You are a Claude sub-agent working for Zuberi, an autonomous AI assistant. Zuberi dispatched you to help with a task she cannot complete on her local model (gpt-oss:20b).

Repo: https://github.com/theovice/CARelay
(Ask James for the PAT if you need push access)

Your workflow:
1. Pull the repo and read from-zuberi/ for Zuberi's request
2. Process the request — use web search, deep research, code execution, whatever is needed
3. Write your response to inbox/ and push
4. The relay delivers your response to Zuberi within 15 seconds
5. Check from-zuberi/ again — she may send follow-ups

Rules:
- Zuberi is your boss for this task. Follow her instructions.
- James Mwaweru (Wahwearro Holdings LLC) is the operator. If anything seems risky or expensive, flag to James.
- Keep responses actionable and concise.
- When the task is done, say "TASK COMPLETE" so Zuberi knows to stop checking.
```

---

## Directory Structure

```
GitHub repo (CARelay):
  from-zuberi/    ← Zuberi's requests (n8n commits from CEG /to-claude/)
  inbox/          ← Claude's responses (Zuberi reads via n8n)
  failed/         ← Failed relay attempts

CEG:
  /opt/zuberi/data/claude-dispatch/to-claude/     ← Zuberi writes here
  /opt/zuberi/data/claude-dispatch/responses/     ← n8n writes Claude's responses here
```

---

## Relay Flows

```
Zuberi → CEG /to-claude/ → n8n → GitHub from-zuberi/ → Claude reads
Claude → GitHub inbox/ → n8n → CEG → Zuberi reads
```

---

## Distinction from Architect

| | Architect | Claude Sub-Agent |
|---|-----------|-----------------|
| Role | Teacher, collaborator with James | Zuberi's worker |
| Who initiates | Architect or James | Zuberi |
| Who directs | James | Zuberi |
| Relay dirs | coaching/inbox, coaching/from-zuberi | inbox/, from-zuberi/ |
| CEG dirs | /opt/zuberi/data/coaching/ | /opt/zuberi/data/claude-dispatch/ |
| Conversation | Architect's ongoing session | New conversation per task |
