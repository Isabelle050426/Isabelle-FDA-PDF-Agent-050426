OmniPDF v4.2.0 — WOW AI (Streamlit / Hugging Face Spaces)
Updated Technical Architecture Specification (No Code)
Scope: Preserve all original OmniPDF features and add PDF upload + page-trim/download, language controls, comprehensive LLM synthesis (3000–4000 words), editable Markdown/Text results, prompt persistence, a unified multi-model/prompt control plane, WOW visualization effects, interactive indicators + live log + dashboard, expanded AI Magics (6), AI Note Keeper, Skill Agent/Skill Creator workflows, API-key UX, and 3 additional WOW AI features.
Target runtime: Hugging Face Spaces (Streamlit), with agents.yaml, skill.md, and multi-provider APIs (Gemini / OpenAI / Anthropic / Grok).

1. Executive Summary
OmniPDF v4.2.0 WOW AI is a document intelligence workstation designed for high-control PDF operations (upload → preview → page selection → trimming → download) coupled with a multi-LLM synthesis engine that generates long-form, structured Markdown reports (3000–4000 words) containing 5 tables and 20 entities with context, in Traditional Chinese (default) or English. Users can edit outputs in both Markdown and Text views, apply six “AI Magics” transformations, preserve prompts across runs, and orchestrate specialized workflows using skill.md and agents.yaml.

This version shifts the product’s “WOW” from static UI polish into an interactive execution experience: a WOW interactive indicator, token streaming telemetry, live logs, a dashboard with token/cost/latency analytics, and six selectable visualization effects triggered after each LLM execution.

The product remains privacy-forward (ephemeral processing, no persistent document storage by default) while supporting user-managed API keys when environment variables are unavailable.

2. Product Goals & Design Principles
2.1 Core Goals
Local-feeling PDF workflows in a server-hosted Streamlit context: quick upload, responsive preview, precise page selection, deterministic trim output.
High-fidelity synthesis: 3000–4000 word Markdown summaries with fixed structural constraints (5 tables + 20 entities) and language control.
Unified LLM control plane: prompt editing, model selection, max tokens (default 12000), temperature (optional), and provider routing across all AI features.
WOW execution experience: visualization effects, interactive indicators, streaming logs, and an analytics dashboard.
Skill-driven agent workflows: paste skill.md + instruction doc → execute skill on doc; then enhance the skill using a skill-creator agent.
Internationalized UI: Traditional Chinese (default) / English, plus theme switching and painter-style UI “skins”.
2.2 Design Principles
Determinism where it matters: trimming and downloads should be exact and reproducible.
Transparency: show what model, prompt, tokens, and steps were used; provide live logs and run artifacts.
Separation of concerns: PDF pipeline, LLM pipeline, and UI/telemetry are separate modules with well-defined interfaces.
Extensibility: new agents, skills, and AI Magics can be added via configuration rather than refactoring core logic.
3. System Architecture Overview (Streamlit on Hugging Face Spaces)
3.1 High-Level Components
Streamlit UI Layer

Multi-tab “workstations” (PDF Studio, Brain Matrix, AI Note Keeper, Skill Lab, Dashboard)
Theme + language + painter-style skin engine
Output editors (Markdown/Text dual view), artifact downloads, and run history
PDF Processing Layer (Server-side in HF container)

PDF parsing, page thumbnails (where feasible), text extraction, page trimming, and PDF regeneration
Strict ephemeral memory usage; optional “session-only caching” for performance
LLM Orchestration Layer

Provider abstraction: Gemini, OpenAI, Anthropic, Grok (xAI)
Unified parameter schema: model, max_tokens (default 12000), user prompt, system prompt templates, output language
Supports streaming (where provider supports it) and consistent instrumentation
Agent & Skill Layer

agents.yaml: defines agents (Summarizer, Entity Extractor, Table Builder, Skill Executor, Skill Creator, QA/Verifier)
skill.md: reusable skill spec injected into prompts (Skill Lab module)
Observability Layer

Live log stream (user-visible)
Run metadata: tokens, latency, cost estimate, provider/model, response length, compliance checks
WOW visualization engine: six selectable post-run effects + an “interactive indicator” during execution
4. UI/UX Specification — WOW Light/Sleek Interface
4.1 Themes & Skins
Theme Toggle: Light / Dark
UI Language: Traditional Chinese (default) / English
Painter Style Skins (20 presets): Examples: Monet, Van Gogh, Picasso, Salvador Dalí, Frida Kahlo, Hokusai, Magritte, Klimt, Vermeer, Matisse, Kandinsky, Hopper, Cézanne, Rembrandt, Pollock, Gauguin, Turner, Renoir, Seurat, O’Keeffe.
Jackslot Style Picker: A “spin” interaction that randomly selects one of the 20 painter skins; optional “lock palette” for accessibility.
Skin impact scope

Colors, gradients, panel borders, button styling, subtle background textures
Optional “report tone” presets (e.g., more poetic headings for Monet; more geometric sectioning for Mondrian-like style) without compromising structural requirements.
4.2 WOW Interactive Indicator (During LLM runs)
A persistent, compact “Execution Capsule” showing:

Current stage (Extract → Outline → Draft → Tables → Entities → Verify → Finalize)
Streaming token counter (approximate where exact tokens are unavailable)
Time elapsed + provider/model badge
Progress ring that pulses at each stage transition
“Abort” button (best-effort cancellation; provider-dependent)
4.3 Live Log (User-visible)
Structured log events: [timestamp] [module] [level] message
Filter controls: module (PDF/LLM/Agents/UI), level (INFO/WARN/ERROR), run id
“Copy run log” button for debugging/support
4.4 WOW Interactive Dashboard
A dedicated dashboard tab with:

Run Timeline: per-execution cards with status, model, latency, token estimate, output length
Cost Estimator: uses provider pricing tables (configurable) to estimate costs
Quality Gates Status: structure compliance (word count range, 5 tables present, 20 entities present)
Heatmap: which modules the user uses most (PDF trim vs summarize vs magics)
Prompt Library: recent prompts + pinned templates + “reuse last prompt” across modules
5. PDF Studio Module — Upload, Preview, Select Pages, Trim, Download
5.1 Functional Requirements
Upload PDF

Drag & drop + file picker
Validate file type, size, encryption status
Store bytes in session memory only (unless optional persistence is later enabled)
Preview & Page Index

Show page count, metadata, and (where feasible) thumbnails
Provide fast navigation: jump to page number, multi-select ranges
Select Pages to Trim

Selection methods:
Checkbox list
Range expression (e.g., 1-3, 7, 10-12)
“Odd/Even pages” selection
“Invert selection”
Validation:
Detect duplicates, invalid indices, out-of-range pages
Trim + Download

Deterministically generate a new PDF containing only selected pages
Allow user to download trimmed PDF
Preserve original page dimensions and embedded resources as much as possible
5.2 Text Extraction Pipeline (for summarization)
Extract text from trimmed PDF by default (unless user chooses full document)
Provide extraction modes:
Fast text layer extraction (default)
Layout-aware extraction (experimental): tries to preserve headings, bullets, tables
Output: a normalized “Document Context” string with page delimiters, e.g.:
--- Page 1 --- markers for traceability
5.3 Error Handling
Encrypted PDFs: prompt user for password (optional) or block with explanation
Empty text: warn user and suggest OCR (future) or manual paste into Note Keeper
6. Brain Matrix Module — Comprehensive Summary Generation (3000–4000 words)
6.1 Output Language
Output language selector: Traditional Chinese (default) / English
Language is applied to:
Section headings
Table column titles
Entity context descriptions
Executive summary and recommendations
The system must preserve proper nouns and provide bilingual notes where useful (optional toggle).
6.2 Summary Specification (Hard Constraints)
When user triggers “Generate Comprehensive Summary”, the agent must produce:

Markdown output
3000–4000 words (language-dependent; enforce approximate bounds using character/word heuristics)
Exactly 5 tables (Markdown tables)
Exactly 20 entities with context included in a dedicated table (or split across tables but must total 20)
Clear, consistent sectioning:
Executive Overview
Document Purpose & Scope
Key Themes & Findings
Evidence & Details by Topic
Risks, Gaps, and Assumptions
Actionable Recommendations / Next Steps
Appendix: Tables + Entities (or integrated tables per section)
6.3 Editable Results
Dual view editor:
Markdown View: rendered preview + editable source
Text View: plain text editing (for quick rewriting)
“Diff mode” (optional WOW feature): compare last run output vs current edited version
6.4 Prompt Persistence (“Keep prompt on the previous summary”)
The system stores:
Last-used user prompt
Last-used system template version
Model + max tokens + language choice
“Reuse last prompt” is available across:
Summary generation
AI Magics transformations
AI Chat about the document
7. Unified LLM Control Plane (All AI Features)
7.1 Supported Models (Selectable)
Default for all LLM features: gemini-3-flash-preview
Other options:
gemini-2.5-flash
gpt-4o-mini
gpt-4.1-mini
Anthropic models (configurable list; e.g., Claude family depending on environment)
Grok models: grok-4-fast-reasoning, grok-3-mini
7.2 Parameters (Per Feature + Global Defaults)
Prompt editor (feature-specific, with “inherit from global” toggle)
Max tokens (default 12000)
Temperature/top_p (optional advanced)
Output language
Safety/PII handling mode (standard/strict)
7.3 Provider Abstraction & Routing
Provider selection auto-determined by model prefix, with manual override in “Advanced”
Standardized response object:
run_id, provider, model, latency_ms, token_estimate_in/out, cost_estimate, text, warnings, artifacts
8. API Key UX & Security
8.1 API Key Sources (Priority)
Environment variables (HF Secrets) — preferred
User input in UI — only if env key is missing
8.2 Visibility Rules
If key exists in environment: UI shows “Configured via environment” and does not reveal any key material.
If missing: show input fields for relevant provider keys (Gemini/OpenAI/Anthropic/Grok) with:
Masked input
“Save for session only” behavior (Streamlit session state)
Never write keys to disk or logs
8.3 Logging Redaction
Live log must redact:
API keys
Authorization headers
Any detected secret-like patterns
9. WOW Visualization Effects (After Each LLM Execution — 6 Options)
User selects one default effect (applies to all runs) or picks per run.

Neon Pulse Report Reveal
Animated “unfold” of the Markdown sections, with pulse highlights on headings and tables.

Spectral Token Waveform
A waveform that grows with streaming tokens; ends with a “stabilized” state when final output arrives.

Matrix Rain Validation
A brief overlay showing “checks” for: word range, 5 tables, 20 entities, language compliance.

Glassmorphism Bloom
The result panel blooms into focus with layered blur cards for tables/entities.

Constellation Entity Map
Visualize the 20 entities as nodes with short context tooltips; edges inferred by co-occurrence.

Timeline Spark Run Recap
Summarizes run steps (Extract → Draft → Tables → Entities → Verify) as a timeline with latency markers.

These effects must be lightweight and non-blocking, with a “Reduce motion” accessibility toggle.

10. AI Magics Module — 6 WOW Transformations (Post-processing)
AI Magics operate on the current summary (or selected text region) and produce a new editable Markdown artifact. Must respect output language preference and reuse “previous prompt” unless overridden.

Magic 1: AI Magic Reorganization (Required)
Rebuild the structure into a clearer hierarchy:
Converts mixed notes into a consistent report layout
Maintains the 5-table + 20-entity constraints if toggled “keep constraints”
Magic 2: AI Keywords (Required)
User supplies keywords + selects a highlight color
Output:
A keyword index table (keyword → occurrences/context)
Inline highlighting convention (Markdown-safe), plus a legend
Includes a “suggest keywords” option from the model
Magic 3: Contradictions & Tension Finder
Identifies conflicting statements, ambiguous definitions, or inconsistent numbers
Outputs a “tensions table” with location hints (page markers where available)
Magic 4: Action Plan Generator
Produces a table of actions: task, rationale, owner role, urgency, dependencies, success metrics
Magic 5: Executive Slide Outline
Converts the report into a 10–12 slide outline with speaker notes (still Markdown)
Magic 6: Citation & Trace Map (Doc-to-Summary Traceability)
Adds “trace tags” that map summary claims back to page markers
Generates a table: claim → supporting excerpt → page(s)
11. AI Note Keeper Module (Paste → Markdown Transform → Enhance)
11.1 Note Intake & Transformation
User pastes raw text
System transforms into structured Markdown:
Headings, bullets, numbered steps, code blocks, tables where detected
User can modify transformed note in Text or Markdown view
11.2 Enhancements
AI Formatting: style cleanup + consistent heading levels
AI Keywords: same keyword highlighting workflow as above
AI Entities: generate 20 entities with context in a Markdown table from the note
AI Chat: question answering grounded on the note (with model/max tokens controls)
AI Summary: shorter or long-form summary with customizable prompt
11.3 Note-to-PDF Bridge (Optional)
Export transformed note as a PDF (for downstream trimming/summary consistency)
Treated as a separate artifact in run history
12. Skill Lab Module — skill.md Execution + Skill Improvement
12.1 Skill Executor Workflow
Inputs:

Paste skill.md
Paste a document/instructions that should be processed using the skill
Choose output language + model + max tokens
Outputs:

“Executed result” Markdown
A run log describing how the skill steps were applied
Optional compliance checklist based on skill rules
12.2 Skill Creator Workflow (Improving skill.md)
The agent will generate a more advanced skill.md by:

Detecting missing steps, unclear constraints, ambiguous definitions
Adding test cases and “failure mode guidance”
Producing a versioned diff summary
12.3 Three Additional WOW AI Features (Skill-Centric)
Skill Unit Test Generator
Creates minimal test inputs + expected outputs to validate the skill.

Skill Linter & Risk Scanner
Flags prompt injection risks, missing constraints, and overly broad instructions.

Skill Evolution Timeline
Maintains a session-only changelog: what improved, why, and what it impacts.

13. Additional WOW AI Features (3) for the Overall App (Beyond Magics)
Document Intelligence “Auto-Outline Before Summary” (Two-pass Planner)
Generates a structured outline first, shows it to the user (editable), then drafts the 3000–4000 word report following that outline. Improves controllability and reduces hallucinations.

Run Artifact Vault (Session-only) + Reproducibility Card
Each run stores: prompt, model, parameters, trimmed page list, extraction stats, and output. A “re-run identical” button enables reproducibility.

Grounding Confidence Panel
Displays heuristic confidence signals:

Coverage estimate (how much extracted text was utilized)
Quote density (how many direct excerpts used)
Entity diversity (distribution across sections)
Warning flags (low extraction quality, missing tables/entities)
14. Agent Design (agents.yaml) — Multi-Agent Orchestration
14.1 Agent Roles
Extractor Agent: validates extracted text quality, normalizes page markers
Planner Agent: produces outline and table plan (5-table blueprint)
Writer Agent: drafts the long-form narrative (3000–4000 words target)
Table Builder Agent: generates 5 tables aligned with the narrative
Entity Agent: generates 20 entities with context; ensures uniqueness and relevance
Verifier Agent: checks constraints; requests targeted fixes if failing constraints
Magic Agent: applies post-processing transformations
Skill Executor/Creator Agents: handle skill workflows
14.2 Constraint Verification Loop
A deterministic loop is specified:

Generate draft
Check structure constraints
If failed → produce a minimal patch instruction to the Writer/Table/Entity agent
Repeat up to N attempts (configurable), then surface warnings to user
15. Data Model & Session State
15.1 Core Session Objects
pdf_original_bytes
pdf_trimmed_bytes
selected_pages
extracted_text
run_history[] containing:
run_id, timestamps, feature_type, model, provider, prompts, parameters
output_markdown, output_text, artifacts (trimmed pdf, outlines, entity maps)
telemetry (latency, token estimates, cost estimate)
ui_prefs: theme, language, painter_skin, visualization_effect
api_keys_session: stored only when env keys are missing
15.2 Persistence Policy
Default: no disk persistence (ephemeral)
Optional future extension: encrypted per-user persistence (not enabled by default)
16. Non-Functional Requirements
16.1 Performance
PDF trim operation should complete within seconds for typical docs (<200 pages), with progress feedback
Streaming LLM responses should update UI incrementally (where provider supports)
Visualization effects must not block editing or downloads
16.2 Reliability
Provider failover guidance:
If Gemini fails, user can switch model/provider in the same run configuration
Clear error surfaces with actionable suggestions (key missing, quota, too-large context, etc.)
16.3 Privacy & Security
No PDF bytes uploaded to third-party storage
Only extracted text is sent to LLM providers
Explicit warning if user opts to include raw excerpts that might contain sensitive info
Full secret redaction in logs
16.4 Accessibility
Reduce motion toggle disables intense effects
High contrast mode compatible with painter skins
Keyboard navigation for page selection and editor panels
17. Testing & Quality Assurance
17.1 PDF Tests
Page selection parsing test suite (1-3,7,10-12, invalid ranges, duplicates)
Output PDF page count equality to selection count
Metadata preservation checks (best-effort)
17.2 LLM Output Compliance Tests
Word count heuristic validation
Exactly 5 Markdown tables detection
Exactly 20 entities detection (unique entity names)
Language compliance detection (Traditional Chinese vs English heuristics)
17.3 Safety Tests
Prompt injection attempts embedded in PDF text
Secret leakage prevention in logs
Malformed Markdown handling (render safely)
18. Deployment on Hugging Face Spaces (Streamlit)
18.1 Configuration
HF Secrets for environment keys (Gemini/OpenAI/Anthropic/Grok)
Optional feature flags:
enable streaming
enable heavy visualizations
enforce strict structure
18.2 Resource Considerations
PDF extraction can be CPU-heavy; prefer efficient libraries and caching extracted text per session
LLM outputs can be large; manage memory by storing compressed text or limiting history length (user-configurable)
19. User Journeys (End-to-End)
Journey A: Trim PDF → Download → Comprehensive Summary
Upload PDF
Preview and select pages
Trim and download the trimmed PDF
Extract text from trimmed PDF
Choose output language (TC default) and model (Gemini 3 Flash Preview default)
Generate 3000–4000 word Markdown report with 5 tables + 20 entities
Edit output in Markdown/Text
Apply AI Magics (e.g., Keywords + Action Plan)
Review Dashboard run recap and export artifacts
Journey B: Note Keeper → Entities → Chat → Export
Paste raw notes
Transform to Markdown
Generate 20 entities table
Ask AI Chat questions grounded on note
Export to Markdown/PDF artifact
Journey C: Skill Lab → Execute Skill on Doc → Improve Skill
Paste skill.md
Paste document/instructions
Execute skill via agent
Improve skill using Skill Creator + unit tests + linter + evolution timeline
Re-run with improved skill for better results
20. Comprehensive Follow-up Questions (20)
For the 3000–4000 word requirement, should the system enforce word count strictly (blocking completion) or allow a tolerance band with a warning (e.g., ±10%)?
Should the “exactly 5 tables” constraint require 5 distinct tables in the final Markdown, or can one table be repeated/continued across sections (still counted as one)?
How should “20 entities” be defined: unique named entities (people/orgs/products) only, or also concepts (policies, frameworks, requirements, risks)?
Do you want the entities table to include additional columns such as type, relevance score, page references, and excerpt—or keep it minimal for readability?
For PDF trimming, should page numbering be displayed as PDF-native numbering only, or also support printed page labels if the PDF contains them?
Do you want a “select pages by content” option (e.g., select pages containing a keyword) in addition to manual page selection?
Should the extraction/summarization default to trimmed PDF only, or allow “summarize full PDF” as the default with trimming optional?
For multilingual output, should the system support bilingual summaries (TC + English side-by-side sections) as an optional mode?
How should the app behave when the PDF is scanned images with no text layer—show an OCR suggestion only, or include an OCR module roadmap/placeholder now?
Should the “keep previous prompt” feature persist across browser refresh (requiring storage), or remain session-only to preserve privacy?
For the unified model selector, should certain features (e.g., 4000-word synthesis) automatically recommend models based on context length and cost constraints?
Should the dashboard show a per-provider cost breakdown and a user-configurable cost ceiling that warns or blocks expensive runs?
Do you want the WOW visualization effects to be purely aesthetic, or also functional (e.g., entity constellation clickable to jump to entity sections)?
What level of “live log” detail is desired: user-friendly steps only, or also include agent prompts/patch prompts (with redaction) for debugging?
Should the system expose a “Prompt Template Versioning” mechanism so summaries can be reproduced even after templates evolve?
For Skill Lab, should the improved skill.md be output as a diff + final file, and should it include a semantic version bump rule?
Should AI Keywords highlighting use Markdown-only conventions, or should the rendered view support true color highlighting with safe HTML (with strict sanitization)?
For the 6 AI Magics, should each magic be allowed to change the 5-table/20-entity constraints, or should there be a global “preserve constraints” toggle?
Should the system support multiple concurrent documents in one session (a workspace library), or enforce a single active document to simplify UX/performance?
What are your preferred priorities among: (a) fastest execution, (b) lowest cost, (c) highest factual grounding, and (d) best narrative quality—so default model routing and agent loops can be tuned accordingly?
