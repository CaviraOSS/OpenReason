# OpenReason

Adaptive AI reasoning with constitutional constraints, using LangChain for AI API

[![npm](https://img.shields.io/npm/v/openreason)](https://www.npmjs.com/package/openreason) [![license](https://img.shields.io/badge/license-Apache-blue.svg)](LICENSE)

---

# Benchmark

| Model                               | Reasoning Accuracy | Latency (thinking) | Cost Efficiency | My Verdict (2026-tier)                           |
| ----------------------------------- | ------------------ | ------------------ | --------------- | ------------------------------------------------ |
| OpenReason (Using gemini-flash-2.5) | 75–82%             | 4–12 sec           | ★★★★★ (Best)    | Fastest + cheapest; needs deeper chain logic     |
| ChatGPT-5.1-Thinking                | 88–92%             | 6–15 sec           | ★★☆☆☆           | Best all-around cognition, not the fastest       |
| DeepSeek-R1-V3                      | 91–95%             | 8–18 sec           | ★★★★☆           | Best raw math/logic; weaknesses in nuance        |
| Kimi-K2-V3                          | 83–86%             | 3–10 sec           | ★★★☆☆           | Strong Chinese-first cognition; mid global logic |

## installation

```bash
npm install openreason
```

manual installation:

```bash
git clone https://github.com/nullure/Reasoner.git
cd Reasoner && npm install && npm run build
tsx src/index.ts "your query"
```

---

## usage

```typescript
import openreason from "openreason";

openreason.init({
  provider: "openai", // required: openai | anthropic | google | xai
  apiKey: "sk-...", // required
  model: "gpt-4o", // required

  simpleModel: "gpt-4o-mini", // optional: reflex mode
  complexModel: "gpt-4o", // optional: reflective mode

  weights: { logical: 0.4, rule: 0.4, empathy: 0.2 },
  memory: { enabled: true, path: "./data/memory.db" },
  performance: { maxRetries: 3, timeout: 30000 },
});

const result = await openreason.reason("your query");
console.log(result.verdict); // answer
console.log(result.confidence); // 0-1
console.log(result.mode); // reflex | analytic | reflective
```

---

## reasoning modes

**reflex** (complexity 0-0.25) - instant deterministic compute, 0ms, no llm  
**analytic** (0.25-0.65) - structured reasoning with constraints, 1-3s  
**reflective** (0.65+) - deep multi-perspective analysis, 3-10s

---

## features

**adaptive depth control** - auto-routes queries based on complexity  
**constitutional constraints** - enforces reasoning rules (no contradictions, evidence required)  
**deterministic compute** - instant 0ms math/fact responses  
**hierarchical memory** - episodic/semantic/procedural memory layers  
**self-improving** - learns thresholds, evolves prompts  
**multi-provider** - openai, anthropic, google, xai

---

## configuration

```typescript
openreason.init({
  provider: "anthropic",
  apiKey: process.env.ANTHROPIC_API_KEY,
  model: "claude-3-5-sonnet-20241022",
  simpleModel: "claude-3-5-haiku-20241022",
  weights: { logical: 0.5, rule: 0.3, empathy: 0.2 },
});
```

environment variables:

```bash
PROVIDER=openai
OPENAI_API_KEY=sk-...
OPENREASON_MODERATE_MODEL=gpt-4o
OPENREASON_SIMPLE_MODEL=gpt-4o-mini
```

see [examples/configuration.md](examples/configuration.md) for full options

---

## providers

| provider  | models                  | speed  | cost   |
| --------- | ----------------------- | ------ | ------ |
| openai    | gpt-4o, gpt-4o-mini     | fast   | medium |
| anthropic | claude-3-5-sonnet/haiku | medium | high   |
| google    | gemini-1.5-pro/flash    | fast   | low    |
| xai       | grok-beta               | medium | medium |

---

## advanced usage

```typescript
import { hierarchical_memory } from "openreason";

const memory = new hierarchical_memory();
await memory.store_episodic({ query, verdict, confidence });
const episodes = await memory.load_episodic({ limit: 10 });
```

```typescript
import { adaptive_learner } from "openreason";

const learner = new adaptive_learner();
learner.log_trace({ query, complexity, mode_used, pass, efficiency });
const stats = learner.update_learning();
```

```typescript
import { adaptive_reasoning_throttle, load_cfg } from "openreason";

const cfg = load_cfg();
cfg.models.complex.provider = "anthropic";
cfg.memory.enabled = false;

const art = new adaptive_reasoning_throttle();
const result = await art.reason("query");
```

see [examples/advanced-usage.md](examples/advanced-usage.md) for details

---

## architecture

```
query  safety filter  context analysis  complexity scoring

routing (reflex/analytic/reflective)

deterministic compute OR prompt building  llm invocation

response validation  constraint checking  confidence calc

memory storage  result
```

core modules:

- `core/kernel.ts` - orchestrator
- `core/router.ts` - complexity scoring
- `core/art.ts` - reasoning throttle
- `constraint_core/` - rule engine
- `memory_unit/` - memory system
- `api/llmdriver.ts` - llm client

---

## examples

```typescript
// math (deterministic, 0ms)
await openreason.reason("144 * 12");
//  { verdict: "1728", confidence: 1.0, mode: "reflex" }

// logic (analytic, 1-3s)
await openreason.reason("if AB and BC, does AC?");
//  { verdict: "yes", confidence: 0.98, mode: "analytic" }

// ethics (reflective, 3-10s)
await openreason.reason("should ai have rights?");
//  { verdict: "...", confidence: 0.75, mode: "reflective" }
```

---

## testing

```bash
npm test
npx tsx tests/test_core.ts       # routing + deterministic + learning
npx tsx tests/test_learning.ts   # adaptive learning
npx tsx tests/test_memory.ts     # memory system
```

results: routing 80%, deterministic 100%, learning working, memory functional

---

## performance

| operation     | latency | notes      |
| ------------- | ------- | ---------- |
| deterministic | 0ms     | math/facts |
| reflex        | 10-50ms | simple     |
| analytic      | 1-3s    | structured |
| reflective    | 3-10s   | deep       |
| memory        | <10ms   | sqlite     |

---

## formulas

**confidence**: `c = (0.4logical) + (0.4rule_compliance) + (0.2empathy)`  
**compliance**: `k = 1 - (violations / total_rules)`  
**complexity**: `(keywords0.4) + (structure0.3) + (domain0.3)`

---

## contributing

1. fork repo
2. create feature branch
3. follow lowercase style
4. add tests
5. submit pr

see [CONTRIBUTING.md](CONTRIBUTING.md)

---

## license

MIT - see [LICENSE](LICENSE)

---

## links

[github](https://github.com/nullure/Reasoner) [issues](https://github.com/nullure/Reasoner/issues) [examples](examples/) [troubleshooting](examples/troubleshooting.md) [architecture](architecture.md) [security](SECURITY.md)
