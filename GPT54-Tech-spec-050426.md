Updated Technical Specification: AgenticDoc Pro Intelligence Platform
Version: 2.0
Deployment Target: Hugging Face Spaces
Primary Runtime: Streamlit
Configuration Assets: agents.yaml, skill.md
Supported AI Providers: Gemini API, OpenAI API, Anthropic API, Grok API

1. Executive Summary
AgenticDoc Pro is an upgraded document intelligence platform designed for users who need to extract, trim, summarize, restructure, annotate, and operationalize content from PDFs and pasted text through a polished, multilingual, AI-driven interface. This updated specification preserves all original capabilities and extends them into a more powerful Streamlit-based workspace optimized for Hugging Face Spaces deployment.

The updated platform emphasizes five principles:

Keep all original features intact
Add rich document handling for PDFs, trimming, and downloadable outputs
Introduce deep AI configurability across all LLM features
Provide a WOW-class visual and interactive user experience
Support modular agent execution through agents.yaml and skill.md
The application now supports:

PDF upload, page selection, trimming, and trimmed PDF download
Traditional Chinese as the default output language, with English as an option
Comprehensive AI-generated markdown summaries of trimmed documents in the 3000–4000 word range
Mandatory structured output including 5 tables and 20 contextual entities
Editable markdown/text results
Global model, prompt, and max-token control for all LLM features
User-provided API keys only when environment secrets are absent
Light/dark theme switching
20 painter-inspired UI styles with a random “Jackslot” selector
WOW interactive indicators, live execution logs, interactive dashboards, and 6 post-execution visual effects
AI Note Keeper for transforming pasted text into editable markdown
AI Magics suite with 6 WOW document actions
Skill execution and skill improvement workflows using skill.md
3 additional cross-app WOW AI features for prompt memory, comparison intelligence, and action extraction
This specification supersedes the earlier front-end-centric draft and standardizes the product on a Python/Streamlit architecture appropriate for Hugging Face Spaces while preserving the original intent of a highly polished, agentic intelligence platform.

2. Product Goals
2.1 Primary Goals
Transform static PDFs and pasted text into structured, editable, multilingual intelligence outputs
Give users full control over prompts, models, token limits, and output style
Make every LLM execution visible, trackable, and explainable through logs, indicators, and dashboards
Support reusable agent skills via skill.md
Enable non-technical users to operate advanced AI workflows from a clean UI
2.2 Non-Goals
This version does not aim to be a full document management system
This version does not require offline local model execution
This version does not attempt persistent multi-user collaboration in real time
3. Preserved and Expanded Scope
This update explicitly preserves all prior core capabilities:

AI summary generation
AI reorganization and other “magic” actions
live log
interactive dashboard
visualization effects
entity extraction
markdown-first editing
previous-context reuse
professional analytical UI
It additionally introduces or formalizes:

PDF upload and trim workflow
trimmed PDF download
language selection for outputs
global model and prompt settings across all LLM features
UI theming and artistic styles
API key entry fallback on the webpage
AI Note Keeper
skill-based execution and skill improvement
richer metrics, better execution visibility, and stronger session memory
4. High-Level Architecture
AgenticDoc Pro will use a layered Streamlit architecture suited to Hugging Face Spaces.

Layer	Primary Technology	Responsibility
Presentation Layer	Streamlit + custom CSS/components	UI, controls, editors, dashboards, theme/style rendering
Document Layer	PyMuPDF / pypdf	PDF upload, page preview, extraction, trimming, download generation
Intelligence Layer	Provider adapters for Gemini/OpenAI/Anthropic/Grok	Prompting, summarization, chat, note formatting, magic actions
Orchestration Layer	agents.yaml, session state, prompt manager	Feature routing, model defaults, fallback logic, execution policy
Skill Layer	skill.md, skill executor, skill improver	Apply skill instructions to content and evolve skill definitions
Observability Layer	session log buffer + metrics registry	Live log, indicators, execution history, dashboards, visual effects
4.1 Deployment Model
Runs on Hugging Face Spaces with Streamlit
Uses server-side environment variables/secrets when available
Falls back to session-scoped user API keys entered via masked UI fields
Stores transient state in st.session_state
Optionally supports export/import of workspace state as JSON or markdown bundles
5. User Experience and Interface Specification
5.1 Global Visual Design
The interface will be “WOW light, sleek, and intelligent,” balancing elegance with density. It must feel premium but remain practical for long-form AI work.

Mandatory global UI controls
Theme toggle: Light / Dark
Language toggle: Traditional Chinese (default) / English
Painter style selector: 20 styles
Jackslot selector: random style generator that cycles through painter themes and lands on one
Global model selector
Global prompt editor
Global max tokens field with default 12000
Execution visualization selector with 6 options
Live log panel toggle
Interactive dashboard toggle
20 painter-inspired styles
These are UI presentation styles, not content-writing styles. They alter palette, borders, cards, typography accents, and motion flavor while preserving readability:

Leonardo da Vinci
Claude Monet
Vincent van Gogh
Katsushika Hokusai
Gustav Klimt
Piet Mondrian
Pablo Picasso
Rembrandt
Vermeer
Salvador Dalí
Wassily Kandinsky
Paul Cézanne
J.M.W. Turner
René Magritte
Edward Hopper
Georgia O’Keeffe
Alphonse Mucha
Zhang Daqian
Hiroshige
Frida Kahlo
UI layout zones
Top bar: branding, theme, language, style, Jackslot
Left sidebar: upload, page trim controls, API/provider settings
Main workspace: document preview, summary output, editor tabs, note keeper, skill module
Right utility rail or bottom dock: live log, indicators, dashboard, visualizations
6. Core Functional Modules
6.1 PDF Intake and Trimming Module
Functional requirements
User uploads a PDF
System extracts metadata and page count
User previews page thumbnails or page numbers
User selects pages/ranges to keep
System creates a trimmed PDF
User downloads the trimmed PDF
The trimmed document becomes the basis for AI analysis unless the user chooses otherwise
Page range input
Support:

single pages: 3
ranges: 1-5
mixed ranges: 1-3,5,8-10
Validation rules
Reject invalid page numbers
Reject empty selections
Preserve original order
Display total pages selected and approximate extracted text size
Output options
Download trimmed PDF
Send trimmed text to summary pipeline
Use trimmed text in Note Keeper or Skill Executor
6.2 Summary Generation Module
This module is the platform’s flagship workflow.

Summary contract
For trimmed PDFs, the system must generate:

a comprehensive markdown summary
target length: 3000–4000 words
output language: Traditional Chinese by default, English optional
5 tables minimum
20 entities with context
editable output in markdown view and text view
20 comprehensive follow-up questions appended at the end
Required summary structure
Executive overview
Document purpose and scope
Core themes and findings
Section-by-section analysis
Risks, assumptions, or unresolved issues
5 structured tables
20 entities with context
Recommendations or action points
20 follow-up questions
Recommended tables
The system should aim to produce:

Key concepts table
Processes/workflows table
Stakeholders/entities table
Risks/issues table
Recommendations/action items table
User controls
prompt editor
model selector
max tokens selector, default 12000
language selector
“keep previous prompt” toggle
“use previous summary as context” toggle
regenerate button
stop generation button
Prompt persistence
If enabled, the previous summary prompt is retained and reused as the default basis for the next summary or magic action. This supports iterative refinement rather than isolated one-off generations.

6.3 AI Magics Suite
The application will include 6 WOW AI Magics for document transformation and enhancement. These are available for summaries, notes, and optionally skill outputs.

AI Magic 1: Reorganization
Rewrites content into a cleaner hierarchy such as:

executive summary
themes
workflows
risks
next actions
AI Magic 2: AI Keywords
User provides keywords and chooses colors. The system:

identifies keyword occurrences
highlights them in markdown-compatible form
optionally groups them by category
reports frequency and context
AI Magic 3: Entity Mapper
Builds or refines a 20-entity contextual table with:

entity name
type
role/context
importance
related terms
AI Magic 4: Table Architect
Converts narrative sections into structured markdown tables for better review and export.

AI Magic 5: Gap & Risk Detector
Finds:

ambiguity
missing definitions
contradictions
dependency risks
unanswered questions
AI Magic 6: Tone/Depth Rewriter
Lets users transform an output into:

executive style
technical deep-dive style
teaching style
decision memo style
Magic execution rules
Each magic inherits the global model/prompt/token defaults unless locally overridden
Each magic logs its own execution
Each magic can use previous output as context
Each magic updates dashboard metrics
6.4 AI Note Keeper Module
This module supports a second major workflow beyond PDFs.

Input
User pastes text content such as:

raw notes
meeting logs
copied documentation
web content
technical instructions
Transform flow
User pastes raw text
System converts it into markdown
User edits in either:
markdown view
plain text view
User applies AI actions
Note Keeper features
AI Formatting: improve headings, bullets, spacing, and structure
AI Keywords: user-defined keywords and color selections
AI Entities: generate 20 entities with context in markdown table format
AI Chat: chat against the note with custom prompt/model/max tokens
AI Summary: summarize note with custom prompt/model/max tokens
AI Magic actions: all 6 AI Magics available here as well
6.5 Skill Execution and Skill Evolution Module
This module operationalizes skill.md.

User flow
User pastes or uploads skill.md
User pastes the document or instructions to process
User optionally adds execution guidance
Agent applies the skill to the document
System produces:
processed output
execution notes
improved version of the skill
Skill improvement
The platform must include a Skill Creator workflow that upgrades the provided skill.md into a more advanced version.

3 WOW AI features for the skill module
Skill Deepening
Expands the skill with clearer goals, better constraints, examples, and stronger evaluation logic.

Skill Compression
Produces a shorter, production-ready version optimized for efficient execution.

Skill Benchmarking
Tests the skill conceptually against the current document and reports strengths, weaknesses, and likely failure points.

Skill outputs
applied result
improved skill.md
benchmark report
optional comparison between original and improved skill
6.6 Additional WOW AI Features Across the App
Beyond the 6 AI Magics and 3 skill-specific enhancements, the app adds 3 cross-platform WOW features:

1. Prompt Memory Vault
Stores prompts per feature during the session, with optional “reuse previous prompt” and “branch from prior prompt” behavior.

2. Insight Delta Compare
Compares two outputs, such as:

original summary vs regenerated summary
raw note vs formatted note
original skill vs improved skill
3. Action Extractor
Converts summaries or notes into:

task lists
decision points
follow-up checklist
suggested owners/placeholders
7. Model, Prompt, and Provider Management
7.1 Global LLM Controls
Every LLM-enabled feature must support:

editable prompt
editable max tokens
model selection
output language selection
provider-aware execution
previous-context reuse
Default settings
Default model: gemini-3-flash-preview
Default max tokens: 12000
Default output language: Traditional Chinese
Supported models
At minimum, the UI must support:

gpt-4o-mini
gpt-4.1-mini
gemini-2.5-flash
gemini-3-flash-preview
Anthropic models configured in agents.yaml
grok-4-fast-reasoning
grok-3-mini
Model governance
agents.yaml should define:

provider
model name
feature compatibility
token defaults
fallback routing
timeout thresholds
safety mode
language instructions
availability status in UI
7.2 API Key Behavior
Requirement
If provider keys are available from environment/secrets, the app must not display the key values
If a key is missing from the environment, the user may enter it in the webpage through a masked input
User-entered keys should be session-scoped and never echoed back
UI behavior
Show provider status as:
Available from environment
Missing, user input required
Session key loaded
Never reveal environment-derived secrets
Offer clear validation and error messages for malformed or rejected keys
8. WOW Observability Layer
8.1 Interactive Indicators
Indicators should provide immediate feedback on pipeline state:

uploaded
trimmed
extracted
ready
generating
validating
completed
warning
failed
Indicators may be shown as pills, progress steps, animated badges, or radial status components.

8.2 Live Log
A real-time log panel records each execution stage:

timestamp
feature name
document/page context
provider/model
prompt version
token target
status transitions
duration
success/failure reason
Two log modes should exist:

User mode: friendly explanations
Advanced mode: detailed operational trace
8.3 Interactive Dashboard
The dashboard should summarize:

page counts
selected page ranges
trimmed output size
extracted character count
estimated token count
summary word count
table count
entity count
keyword hits
model latency
error rate
magic usage counts
skill improvement delta metrics
9. Visualization Effects After Each LLM Execution
The user may select one of 6 WOW visualization effects. These are non-essential visual feedback elements tied to AI execution completion.

Aurora Stream – flowing gradient ribbons
Neural Constellation – nodes and links pulsing outward
Ink Bloom – painterly expansion effect matching style themes
Prism Wave – refracted geometric wave motion
Pulse Rings – radar-like circular emission
Matrix Drift – structured digital rain for technical aesthetics
These visualizations should:

be lightweight
not block the main app
respect light/dark themes
harmonize with painter style selections
optionally reflect success, warning, or error states
10. Data Flow and Session State
Core session objects
uploaded PDF binary
trimmed PDF binary
selected page ranges
extracted text
current summary
previous summary
current prompt
previous prompt
global model settings
per-feature overrides
note keeper content
skill content
improved skill output
live log buffer
dashboard metrics
visualization preference
theme/language/style state
Context chaining
Context chaining should be explicit and user-controlled. Users can choose whether a new operation uses:

no prior context
prior prompt only
prior output only
both prior prompt and output
This prevents uncontrolled context accumulation and helps reduce drift.

11. Security, Privacy, and Compliance Considerations
Do not display environment API keys
Session keys must be masked and ephemeral
Minimize retention of uploaded documents
Clearly indicate that text may be sent to third-party model APIs
Provide optional metadata sanitization before summary generation
Avoid logging raw secrets or full sensitive content in advanced logs
Allow users to clear session state and delete cached outputs
12. Performance and Reliability Requirements
Performance targets
PDF upload acknowledgment: near immediate
page count extraction: under a few seconds for normal PDFs
trim generation: fast enough for interactive use
summary generation: dependent on provider, with clear progress state
dashboard update: immediate after each operation
Reliability requirements
graceful failure for unsupported PDFs
provider-specific timeout handling
recoverable retries for transient API failures
explicit error reporting for safety filter blocks
stop/cancel control for long-running generations
13. Output Quality Controls
The system should validate generated outputs before presenting them as “complete,” especially for the PDF summary workflow.

Validation checks
language compliance
markdown structure presence
approximate word count in 3000–4000 range
5 tables present
20 entities present
follow-up questions appended
no empty major sections
If validation fails, the system should:

warn the user
allow partial output review
offer one-click repair/regeneration
14. Export and Editing Requirements
Users must be able to:

edit outputs in markdown view
edit outputs in text view
copy results
download markdown
download trimmed PDF
preserve edited output in current session
rerun AI actions on edited content
This ensures the app is not just a generator but a working intelligence workspace.

15. Acceptance Criteria Summary
The updated app is considered complete when:

All original capabilities remain available
PDF upload, page trim, and download work reliably
Summaries can be generated in Traditional Chinese or English
Summary output follows the 3000–4000 word, 5-table, 20-entity contract
Results are editable in markdown/text view
All LLM features support prompt/model/max-token control
Default model is gemini-3-flash-preview
Default max tokens is 12000
API keys can be entered only when missing from environment
Environment keys are never displayed
Light/dark themes and 20 painter styles work
Jackslot style randomizer works
6 visualization effects work after LLM execution
live log, interactive indicators, and dashboard are functional
AI Note Keeper is fully integrated
Skill execution and skill improvement work with skill.md
3 additional WOW AI features are available across the app
The app runs within Hugging Face Spaces using Streamlit
16. 20 Comprehensive Follow-Up Questions
For the PDF workflow, should summaries be generated strictly from the trimmed document only, or should users be allowed to compare the trimmed subset against the full PDF in a side-by-side analysis mode?
For page selection, do you want a simple range-input experience only, or also a thumbnail-based visual page picker with click-to-select and click-to-exclude behavior?
Should the trimmed PDF preserve original metadata, bookmarks, internal links, and annotations, or should the system optionally sanitize these for privacy before download and AI processing?
For the 3000–4000 word summary requirement, should the platform enforce a hard validation gate before marking a job complete, or should it allow shorter outputs when source content is too limited and label them as “source-constrained”?
For the 5 required tables, do you want fixed table templates across all summaries for consistency, or should the system dynamically choose table types based on document content?
For the 20 entities with context, should the entities prioritize named entities only, or should they also include conceptual entities such as policies, frameworks, processes, risks, and dependencies?
In Traditional Chinese mode, should the application target Taiwan-style terminology by default, and do you want a future option for Hong Kong terminology as a separate localization profile?
For prompt persistence, should “keep previous prompt” apply globally to all features, or should each feature maintain an independent prompt memory so that summary, chat, notes, and skills evolve separately?
For the global model selector, should users be able to set one universal model for the entire app, or should the UI also support a feature-by-feature override matrix in advanced mode?
Which Anthropic models should be exposed by default in agents.yaml, and do you want those options displayed as friendly names only or as exact model IDs for expert users?
For webpage API key entry, should user-supplied keys exist only for the current browser session, or do you want an optional encrypted local persistence mode to reduce repeated entry during future visits?
For the 20 painter-inspired styles, should these affect only the shell UI, or do you also want them to influence chart accents, visualization motion, markdown card styling, and exported markdown theme tokens?
For the “Jackslot” randomizer, should it simply choose a random painter style, or should it act like a theatrical animated slot machine with soundless visual motion and a celebratory lock-in state?
For the 6 visualization effects, should the selected effect appear only after LLM completion, or should there also be a lightweight in-progress visualization state that transitions into a completion animation?
How detailed should the live log be for advanced users: should it include prompt hashes, estimated token usage, provider response time, validation failures, and fallback routing decisions from agents.yaml?
In the AI Note Keeper, should markdown transformation preserve the user’s original paragraph order exactly, or should the formatter be allowed to aggressively reorganize content into headings, bullets, tables, and callouts?
For Skill Execution, should the improved skill.md be generated as a full replacement, or should the app present a structured diff showing what was added, removed, tightened, or clarified?
For the 3 additional WOW AI features, do you want them available everywhere in the app by default, or should some of them be context-sensitive and appear only when the system detects enough structured content?
Should the dashboard support historical session analytics, such as per-model latency trends and magic-usage frequency across multiple runs, or should it remain limited to the current session for simplicity and privacy?
For final exports, do you want the app to support only trimmed PDF and markdown downloads in this phase, or should the next version include richer export packages such as HTML reports, DOCX, JSON session bundles, and comparison reports?
