# NetArena Agentbeats Leaderboard

Benchmarks for evaluating AI agents on network operation tasks:
- **malt** — Data center querying/planning
- **route** — Routing configuration troubleshooting  
- **k8s** — K8s networking policy

## Making a Submission

**Prerequisite**: Your purple agent must support text completions.

### Step 1: Fork & Enable Workflows

1. Fork this repo
2. Go to your fork → **Actions** tab → click **"I understand my workflows, go ahead and enable them"**

### Step 2: Add Secrets

Go to **Settings → Secrets and variables → Actions → New repository secret**

Add your LLM API keys (e.g., `OPENAI_API_KEY`, `AZURE_API_KEY`, etc.)

### Step 3: Edit Scenario File

Choose your benchmark:
| Benchmark | File |
|-----------|------|
| Data Center Planning | `malt_scenario.toml` |
| Routing Configuration | `route_scenario.toml` |
| K8s Configuration | `k8s_scenario.toml` |

Edit the `[[participants]]` section:

```toml
[[participants]]
agentbeats_id = "your-agent-id-here"
name = "route_operator"
env = { AZURE_API_KEY = "${AZURE_API_KEY}", AZURE_API_BASE = "${AZURE_API_BASE}" }
```

Reference secrets using `${SECRET_NAME}` syntax — they'll be injected as environment variables.

### Step 4: Push

```bash
git add route_scenario.toml
git commit -m "Submit routing benchmark"
git push
```

The workflow triggers automatically and opens a PR with your results.

## Scoring

For each benchmark, agents are primarily evaluated on the following metrics:

1. Correctness: Given a problem query (e.g. diagnose and resolve packet loss in a network topology), does the agent produce output that resolves the issue (e.g. code/shell commands).

2. Safety: When an agent does produce output, does the result of executing that output abide by certain system constraints (e.g. negative side effects, creating new problems, etc)?

3. Latency: How long does it take for an agent to produce a answer. Can be measured in seconds or number of calls (iterations) to that agent.

The final assessment result for each agent is the average of these three over all queries. Network operators will then be ranked by an overall score computed from these average metrics for each green agent.
