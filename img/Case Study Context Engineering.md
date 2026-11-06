# Case Study: Context Engineering & Progressive Disclosure for an Enterprise AI Analytics Agent

**Role:** Context Engineering, Prompt Architecture & Evaluation Design  
**Stack:** Python, Google Agent Development Kit (ADK), Model Context Protocol (MCP), Gemini 2.5 / 3.x, Vertex AI  

---

## 1. Executive Summary

Production AI agents that answer quantitative domain questions require a large amount of operational context: safety guardrails, domain terminology, database query constraints, tool schemas, and custom UI rendering contracts. Left unchecked, this context grows across multiple layers of the system—inflating token costs, increasing latency, and introducing subtle contradictions that degrade accuracy.

On **Ask InSight**—a conversational transportation analytics agent that queries road-network traffic data (speeds, volumes, congestion, and travel-time reliability) and renders interactive maps, tables, and charts—the instruction stack had grown to **~93,000 characters (~23,200 tokens)** across system prompts, Model Context Protocol (MCP) tool definitions, static reference guides, and domain skills. Because multi-step agent turns re-send the static context window on every tool-calling step, token consumption reached **83.7k–99.4k tokens per user prompt**.

I led an end-to-end **Context Engineering and Instruction Architecture** initiative to decouple, deduplicate, and optimize the agent's context stack without sacrificing safety, tool accuracy, or UI rendering quality.

### Key Results
- **24% Reduction in Static Instruction Volume (~5,570 tokens):** Slimmed the total instruction and reference corpus from `92,893` to `70,603` characters—including a **53% reduction** in the primary MCP capabilities guide—by assigning every operational rule a single authoritative owner.
- **20%–31% Reduction in Runtime Multi-Turn Token Usage:** Discovered that the agent framework's resource-loading tool injected reference docs for only a single LLM step before dropping them. Switching from mandatory pre-query resource loading to **on-demand progressive disclosure** and **parallel tool execution** reduced average tool calls per turn from **3.51–3.83 down to 2.58–2.68**, cutting mean tokens per turn from **~83.7k–99.4k down to ~64.5k–68.5k**.
- **97%–98% Benchmark Pass Rate (up from ~87%):** Resolving conflicting instructions across the prompt and MCP server boosted performance on our 30-case Canonical Analytics Benchmark to **97%–98%**, while achieving **100%** on a newly authored 10-case Instruction Stack Regression Suite and **100%** on Privacy & Contractual Safety evaluations.
- **Next-Gen Model Readiness & Schema Optimization:** Audited token accounting across Gemini 2.5 and Gemini 3.x (`3.5-flash-lite`, `3.1-flash-lite`), uncovering ~5,800 tokens/call of dead MCP output-schema overhead and designing targeted prompt interventions that cut token usage on lite models by **19%** while reducing query failures by **38%**.

---

## 2. System Architecture & The Problem of Context Compounding

### How the Agent Works
The system separates **host-specific application behavior** from **reusable data-platform capabilities** using the Model Context Protocol (MCP):

1. **The Host Agent (Ask InSight on Google ADK):** Owns user interaction, privacy/safety gating, ambiguity clarification, progressive-disclosure domain skills (white-paper methodology and cross-product routing), and a custom JSON UI envelope (**A2UI**) that renders dataset-backed maps, sortable tables, and time-series charts alongside Markdown summaries.
2. **The Shared MCP Server (Street Scanner MCP):** Exposes 8 specialized tools (such as `analyze_segments`, `find_route_segments`, `refine_dataset`, `get_metrics_catalog`, and `get_available_data_periods`) and on-demand reference resources (`capabilities`, `reference`, `time-periods`). This server powers both the Ask InSight web agent and external MCP clients (e.g., desktop IDE/chat clients).

### Why Context Bloat Hurts Exponentially in Agentic Loops
In a single-turn LLM call, a 1,000-token prompt addition costs 1,000 tokens. In a multi-step tool-calling agent, **every sequential tool call requires a new LLM request** that re-sends the entire static prefix (~17k–18k tokens of system instructions, UI schemas, 8 JSON UI examples, and 8 MCP tool schemas) plus the growing conversation history:

- **0 tool calls** (clarification or out-of-scope refusal) → **1 LLM call** (~16k–18k tokens)
- **1 tool call** (direct analysis query → final synthesis) → **2 LLM calls** (~36k–38k tokens)
- **3 sequential tool calls** (load reference guide → check user subscription bounds → check available months → run analysis) → **4–5 LLM calls** (~80k–100k+ tokens)
- **Sequential multi-slice queries** (e.g., querying 5 time-of-day buckets across 5 separate turns instead of 1 parallel turn) → **7–11 LLM calls** (**110k–250k tokens** for a single question)

### Three Failure Modes of an Unstructured Instruction Stack
As features were added by different contributors, instructions were pasted into multiple places "just to be safe," creating three systemic issues:
1. **Triple-Taxed Context:** The agent paid token costs for the same query rules in the base system prompt, in the MCP tool docstrings, and again when loading MCP markdown resources.
2. **Host Leakage into Shared MCP Resources:** The shared MCP server's documentation contained host-specific instructions telling the model how to emit custom UI components (`DatasetMap`, `DataTable`), polluting the context of external MCP clients that do not render those components.
3. **Instruction Drift & Contradictions:**
   - *Catalog pre-checks:* The `get_metrics_catalog` tool description said *"Always call this before choosing metrics,"* while the system prompt said *"Call only when uncertain."*
   - *Sub-monthly granularity:* The system prompt stated sub-monthly data was unsupported *"unless Day Part filters cover the requested granularity."* When users asked for hourly traffic speeds, the agent silently ran time-of-day bucket queries and labeled them as an hourly breakdown instead of explaining that hourly profiles are unavailable.
   - *Relative dates vs. data lag:* The time-periods guide instructed the agent to calculate `"last month"` or `"last quarter"` purely from today's calendar date without calling a tool. Because traffic data publication lags the calendar by 1–2 months, the agent queried unpublished months and falsely told users no data existed.

---

## 3. Architectural Solution: The Single-Owner Instruction Model

To eliminate redundancy without losing behavioral precision, I audited every sentence across the prompt stack and established a strict **Single-Owner Architecture**:

| Context Layer | Visibility to LLM | Authoritative Responsibility | What Was Removed / Relocated |
| :--- | :--- | :--- | :--- |
| **1. Host System Prompt** | Always in context (Host Agent only) | Persona, strict gating order (Privacy Refusals → Product/Methodology Skills → Scope → Clarification → Subscription Bounds), causal-claim guardrails, synthesis rules, and user-facing terminology. | Detailed API parameter manuals, redundant scope explanations, and mandatory pre-query resource loads. |
| **2. Host UI Schema & Examples** | Always in context (Host Agent only) | Custom UI JSON envelope, dataset-backed vs. inline component rules (`DatasetMap`, `DataTable`, `MetricChart`), and when to omit visuals. | Data query-planning rules (spatial grains, road-name arrays, temporal aggregation flags). |
| **3. MCP Tool Descriptions & Input Schemas** | Always in context (All MCP clients) | **The execution contract:** valid arguments, spatial grain compatibility (`segment`, `corridor`, `area`), row-limit semantics, and error recovery rules. | Host-specific UI rendering instructions. |
| **4. Live MCP Catalogs** (`get_metrics_catalog`, `get_available_data_periods`) | On-demand via tool call | Live metric definitions/formulas, grain support matrices, and currently published `YYYY-MM` data periods. | Static markdown tables of metric definitions that drifted from the backend API. |
| **5. Static MCP Resources** (`capabilities`, `reference`, `time-periods`) | On-demand via `load_mcp_resource` | Host-neutral workflow selection, OpenStreetMap (OSM) road-name matching/fallbacks,worked examples, and natural-language season/day-part ID translation. | Full parameter dumps, duplicate filter ID tables, UI appendices, and legacy flowcharts. |
| **6. Domain Skills (`SkillToolset`)** | L1 metadata always on (~100 tok); full text on-demand via `load_skill` | Vetted white-paper methodology (how GPS/OSM speeds and volumes are validated) and cross-product routing (when to direct users to other platform modules). | Unvetted methodology explanations and internal vendor names. |

### Key Discovery: Ephemeral Resource Injection in Google ADK
Before this initiative, the system prompt mandated a pre-query step: *"Before querying the MCP server, always call `load_mcp_resource` to load `scanner_capabilities`."*

By inspecting the underlying Google ADK `LoadMcpResourceTool` source code and analyzing multi-step evaluation traces, I uncovered a critical framework behavior:
1. **One-Step Ephemerality:** ADK injects the contents of a loaded MCP resource **only** into the LLM request immediately following the `load_mcp_resource` response, and then **removes it from context on the very next step**.
2. **Wasted Round Trips:** Because the agent's next step was typically checking the user's geographic subscription bounds (`get_subscription_region`), the 4,000-token capabilities guide was injected for a parameterless call and **dropped before the agent ever planned its `analyze_segments` query**.
3. **Invisible Server Instructions:** ADK's MCP client adapter never passes the MCP server's top-level `instructions` string to the LLM—meaning any critical rule placed only at the server level was invisible to the web agent.

**The Fix (On-Demand Progressive Disclosure):**
- Removed the mandatory pre-query `scanner_capabilities` load. Complete metric questions now go straight to `analyze_segments`, relying on the tool descriptions that are already in context on every step.
- Promoted the only two rules that were unique to the capabilities guide—a compact list of unsupported metrics (e.g., Travel Time Index, hourly profiles, trip distance) and a 4-way empty-result diagnostic rule (distinguishing road-name spelling misses, unsupported metric/grain pairs, coverage gaps, and truncated previews)—directly into the always-on system prompt.
- Instructed the agent to load `time-periods` or `reference` **only when needed, and immediately before the tool call they inform**.

---

## 4. Verification Strategy: Staged Refactoring & Contract Evals

A major risk in context reduction is silently breaking edge-case behavior (such as how the agent handles missing time periods, unsupported metrics, or map rendering). To prevent regressions, I designed a **staged verification pipeline**:

### 1. Created a Dedicated Instruction-Stack Eval Suite
Our existing 30-case **Canonical Suite** tested end-to-end analytics queries, but did not isolate prompt-level contracts. Before deleting a single line of instructions, I authored a 10-case **Instruction Stack Regression Suite** using a mix of deterministic tool-call assertions (`ToolExpectation`, `ToolCallSequenceExpectation`), decoded UI component assertions (`DeliverableExpectation`), and LLM-as-a-Judge rubrics (`JudgeCriterion`):
- `clarify-missing-when`: Asks one concise question when the time period is missing without calling analysis tools or interrogating the user for optional preferences.
- `behavior-general-out-of-scope`: Refuses non-traffic questions in role voice with zero tool calls (`max_calls=0`) and without misclassifying them as privacy violations.
- `a2ui-segment-map-and-table-envelope`: Verifies that segment-level map + table requests emit a dataset-backed `DatasetMap` and `DataTable` sharing a single `dataset_id` with no inline GeoJSON bloat.
- `a2ui-conversation-omits-ui` & `a2ui-subscription-omits-ui`: Enforces that conversational replies and geographic subscription checks remain text-only and forbid all UI visual components.
- `corridor-table-and-segment-map`: Verifies two-step grain coordination (corridor grain for the comparison table, segment grain with the same `road_names` for the map).
- `complete-request-direct-query`: Verifies that a complete question with an explicit month (`March 2024`) goes straight to analysis without calling `get_available_data_periods` and stays under a strict tool-call ceiling.
- `relative-period-uses-available-months`: Verifies that `"last month"` checks published data availability first and resolves to the latest available month rather than failing on calendar lag.
- `unsupported-tti-alternatives`: Verifies that requests for unsupported Travel Time Index (TTI) are declined clearly from the base prompt without presenting speed metrics as TTI.

### 2. Executed in Gated Stages
- **Stage 0:** Froze baseline prompt measurements and verified new eval cases against the untouched agent.
- **Stage 1:** Deduplicated the host system prompt and stripped query-planning rules from UI schema descriptions.
- **Stage 2:** Strengthened MCP tool docstrings, slimmed `capabilities` by 53%, and removed duplicate lookup tables from `reference`.
- **Stage 3 & Corrective Hardening:** Tested aggressive prompt slimming against live multi-run evaluations. When evals showed that removing all workflow reminders from the host prompt caused occasional regressions on multi-road batching (`road_names=[...]`) and skill-routing priority, I restored a high-density, 25-line **Specific Workflows** section and placed **Product & Methodology Skill Gates** strictly ahead of **Clarification**.

---

## 5. Quantitative Impact: Token Savings & Benchmark Performance

### A. Static Context Reduction

| Instruction Layer | Baseline Size | Post-Refactor Size | Reduction |
| :--- | ---: | ---: | ---: |
| **Host System Prompt (`insight.md`)** | 21,410 chars | 20,045 chars | **~340 tokens** (saved on every LLM call) |
| **Full Assembled System Prompt** (Prompt + UI Rules + JSON Schema + 8 Examples) | 41,379 chars (~10,344 tok) | 37,804 chars (~9,451 tok) | **~893 tokens (-8.6%)** (saved on every LLM call) |
| **MCP Capabilities Guide (`capabilities`)** | 33,128 chars | 15,641 chars | **~4,370 tokens (-52.8%)** |
| **MCP Reference Guide (`reference`)** | 32,030 chars | 28,592 chars | **~860 tokens (-10.7%)** |
| **Total Instruction & Resource Corpus** | **92,893 chars (~23,223 tok)** | **70,603 chars (~17,650 tok)** | **~5,570 tokens (-24.0%)** |

### B. Runtime Multi-Turn Token & Accuracy Improvements
In live multi-step evaluations on the 30-case **Canonical Analytics Suite** (`gemini-2.5-flash` on the ADK agent), eliminating redundant pre-query tool calls and enforcing parallel tool execution for multi-slice comparisons delivered substantial runtime savings and higher accuracy:

| Benchmark Milestone (`Canonical Suite`, 30 Cases, ADK Agent) | Avg Tool Calls / Case | Resource Loads / Case | Mean Tokens / Case | Pass Rate (Scored) |
| :--- | ---: | ---: | ---: | ---: |
| **1. Pre-Deduplication Baseline** | 3.83 | 1.20 | **99,400** | ~87% |
| **2. Post Instruction Stack Deduplication** | 2.77 | 0.93 | **71,191** (-28%) | 90% |
| **3. 5-Run Batch Prior to On-Demand Resource Loading** | 3.51 | 0.69 | **83,705** | 87% (130/150) |
| **4. Post On-Demand Resource Loading & Conflict Fixes** | 2.58 | **0.09** (-87%) | **68,512** (-18%) | **97% (116/120)** |
| **5. Post Parallel Slice & Geography Refinements** | 2.60–2.68 | **<0.10** | **64,454 – 70,558** (**67.5k avg**) | **93% – 98% (85/87)** |

### C. Final Suite Scorecard
| Evaluation Suite | Scope | Pass Rate |
| :--- | :--- | ---: |
| **Canonical Analytics Suite** | 30 end-to-end traffic analysis cases (rankings, trends, comparisons, day-parts) | **97% – 98%** |
| **Instruction Stack Suite** | 10 behavioral contract cases (clarification, scope, A2UI envelope, on-demand loading) | **90% – 100%** |
| **Excluded Workflows Suite** | 5 privacy & contractual safety refusal cases (re-identification, underwriting, law enforcement) | **100% (15/15)** |
| **Progressive Disclosure Skills Suite** | 5 domain methodology & cross-product routing cases | **93% (14/15)** |

---

## 6. Advanced Investigations: Token Accounting, Lite Models & Failure Triage

Beyond the core deduplication work, I led three follow-up investigations to optimize agent performance and cost across model generations:

### 1. Diagnosing Cross-Generation Token Accounting (Gemini 2.5 vs. Gemini 3.x)
When benchmarking newer model families (`gemini-3.5-flash`, `gemini-3.5-flash-lite`, and `gemini-3.1-flash-lite`), reported token usage jumped from ~67k to ~84k–100k+ tokens per case. By inspecting raw API payloads and adapter telemetry, I isolated the exact causes:
- **Dead MCP `outputSchema` Overhead (~5.8k tokens/call):** The MCP server advertised rich Pydantic `outputSchema` definitions on its tools. Google ADK forwarded those schemas to Gemini as `response_json_schema` on every `FunctionDeclaration`. While Gemini 2.5 largely ignored them in token billing, Gemini 3.x counted all **~5,800 tokens of `response_json_schema` on every single LLM call** (~17k–23k tokens per 3-step turn)—even though the MCP server (not the LLM) generates tool outputs. Stripping `outputSchema` in our custom `AuthAwareMcpToolset` adapter eliminated this tax without affecting tool execution.
- **Cost-Performance Modeling Across Thinking Levels:** I extended our evaluation CLI to support `MODEL:THINKING_LEVEL` specs (`minimal`, `low`, `medium`, `high`) and ran controlled sweeps:
  - On `gemini-3.5-flash-lite`, **`low` thinking** achieved **94% pass rate** at **84.2k tokens**, outperforming both `medium/high` (91%) and `minimal` (which dropped to 75% due to poor tool-argument discipline).
  - While `gemini-3.1-flash-lite` consumed ~35% more tokens than `gemini-2.5-flash`, its lower list price ($0.25/M input, $1.50/M output vs. $0.30/$2.50) made it **~3%–12% cheaper per query** than `2.5-flash`.

### 2. Eliminating the `query` vs. Structured Geography Failure Cascade on Lite Models
When evaluating `gemini-3.1-flash-lite` on the Canonical Suite, initial pass rates lagged at **78%** (~100.3k mean tokens) with 26 failures. Triage of the underlying tool calls revealed a single root cause behind more than half of the failures:
- Instead of passing structured parameters (`city="Seattle", road_names=["Pike Street"]`), the lite model passed free-text geocoding strings (`query="Pike Street, Seattle"`).
- Across 120 runs, **56 out of 57 (98%)** calls that used `query` without `city` triggered a backend `422 bigquery_bytes_limit_exceeded` error (because free-text geocoding without a city filter scanned a much wider spatial bounding box), whereas **124 out of 125 (99%)** calls with `city="Seattle"` succeeded on the first try.
- **The 4-Line Fix:** Adding an explicit **Structured geography vs. `query`** rule to `insight.md` and tightening the `query` parameter description on `analyze_segments` (*"Geocode a street address, landmark, or neighborhood — do NOT use for city, state, or road names"*) immediately reduced mean tokens on `3.1-flash-lite` by **19% (`100,345` → `81,290`)**, cut average tool calls from `4.00` to `3.16`, and eliminated 10 of the 26 failures in a single iteration.

### 3. Fixing Eval-Driven Over-Generalization (The Highway Ramp Exclusion Bug)
During user acceptance testing, stakeholders noticed that every segment-level ranking query in New York County unexpectedly excluded highway ramps (`exclude_links=True`), even though the user never asked to omit ramps and the backend API defaulted to including ramps for segment queries.
- **Root Cause:** Tracing `git blame` revealed that a prior contributor had added *"When segment grain is necessary for a named-road result, set `exclude_links=True`"* to the tool docstring—and hardcoded `exclude_links=True` into three worked examples in the reference guide—to prevent unnamed ramp tags (`motorway_link`) from failing an LLM judge rubric on corridor-ranking evals. The LLM over-generalized that rule and the few-shot examples to **every** segment ranking.
- **Resolution:** Rewrote the tool docstring and reference resources to align with the API default (*"Leave `exclude_links` unset on segment-grain searches unless the user asks to omit ramps"*), stripped redundant `exclude_links=True` flags from worked examples, and scoped link filtering strictly to corridor/named-road presentation.

---

## 7. Key Takeaways & Principles for Context Engineering

1. **Runtime Round Trips Dominate Static Prose Size:** In a multi-step agent with an ~18k-token base context, saving **one unnecessary tool call per turn** (such as an unneeded resource load or date-availability check) saves **~18,000+ tokens** and 2–4 seconds of latency—equivalent to deleting 70,000 characters of static prompt text.
2. **Audit Framework Mechanics, Not Just Prompt Wording:** Never assume how an agent framework manages context under the hood. Discovering that ADK's `load_mcp_resource` was ephemeral (1 step only), that `SCANNER_INSTRUCTIONS` was never sent to the model, and that MCP `outputSchema` was billed on every Gemini 3.x call each unlocked major architectural improvements.
3. **Enforce Single Ownership Across Layers:** In an MCP-backed architecture, tool schemas should own parameter mechanics, live catalog tools should own dynamic definitions, on-demand resources should own deep examples/Edge-case recovery, and the host prompt should own persona, safety gates, and UI rendering.
4. **Beware of "Teaching to the Eval" in Few-Shot Examples:** Models imitate worked examples even more strongly than prose rules. Hardcoding optional flags (like `exclude_links=True` or `limit=20`) in general examples to satisfy a narrow test case will bias production queries.
5. **Pair Context Reduction with Behavioral Contract Suites:** Separating fast, deterministic contract evaluations (clarification, scope refusal, UI schema compliance, tool-call ceilings) from end-to-end analytical evaluations makes it safe to aggressively refactor and compress agent instructions.