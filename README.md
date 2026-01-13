# Claude Code Review Orchestrator

## Objective
- Perform code review using a total of 3 agents.
  - 1 Master Agent
    - Acts as the orchestrator for the code review process.
    - Claude Code serves as the main agent; it accepts code review requests from the user and collaborates with two sub-agents to conduct the initial code review.
    - If discrepancies arise in the review results between sub-agents, it requests a secondary verification from the counterpart sub-agent.
      - Shared review opinions => Merge review results.
      - Conflicting review opinions => Request cross-verification.
      - One sub-agent flags an issue while the other misses it => Request cross-verification.
    - Based on the secondary verification results, it consolidates the reviews and may iterate up to a 3rd round.
    - Aggregates the final results and records the code review outcome as a document in the `docs/` folder.
  - 2 Code revie SubAgents
    - Claude Reviewer
      - Receives code review requests and performs the review.
    - Codex Reviewer
      - Receives code review requests and performs the review.

## Install CLI
### Install Claude CLI
See details: https://code.claude.com/docs/en/setup
```
% curl -fsSL https://claude.ai/install.sh | bash
```
Verify authentication
```
% claude
```

### Install Codex CLI
See https://developers.openai.com/codex/cli/
```
% npm i -g @openai/codex
```
Verify authentication
```
% codex
```

## Install Codex stdio MCP in Claude Code
```
% claude mcp add --transport stdio codex --scope user -- codex mcp-server

% claude mcp list
  ...
  codex: codex mcp-server - ✓ Connected

```

## Install commands and agents in Claude Code
```
% git clone https://github.com/kimjj-geek/cc-code-review-orchestration.git
% cp cc-code-review-orchestration/.claude/commands/cro.md ~/.claude/commands
% cp cc-code-review-orchestration/.claude/agents/claude-reviewer.md ~/.claude/agents
% cp cc-code-review-orchestration/.claude/agents/codex-reviewer.md ~/.claude/agents
```

## Perform code review with Claude Code
```
% claude
  ...
  > /cro do code review
  ...
```