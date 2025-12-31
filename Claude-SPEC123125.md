Technical Specification: FDA 510(k) Agentic AI Review System
Executive Summary
This document provides a comprehensive technical specification for an advanced multi-agent AI system designed to assist FDA reviewers in conducting 510(k) medical device submissions review. The system will be deployed as a Hugging Face Space application using Streamlit, leveraging multiple large language models (LLMs) through a sophisticated agent orchestration framework.
Version: 2.0
Target Platform: Hugging Face Spaces
Primary Framework: Streamlit 1.31+
Architecture Pattern: Multi-Agent System with Dynamic Orchestration
Supported Models: OpenAI GPT-4, Google Gemini, Anthropic Claude, xAI Grok

1. System Architecture Overview
1.1 High-Level Architecture
The system implements a sophisticated multi-agent architecture where specialized AI agents collaborate to perform comprehensive regulatory review tasks. The architecture follows a microservices-inspired pattern where each agent represents a focused capability that can be invoked independently or as part of an orchestrated workflow.
Copy┌─────────────────────────────────────────────────────────────┐
│                    Streamlit Web Interface                   │
│  (Multi-tab UI with WOW styling and internationalization)   │
└──────────────────────┬──────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────┐
│                 Agent Orchestration Layer                    │
│  • Agent Registry (agents.yaml)                             │
│  • Dynamic Agent Generator                                   │
│  • Execution Pipeline Manager                                │
│  • State Management & Session Persistence                    │
└──────────────────────┬──────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────┐
│                   LLM Router & Provider Layer                │
│  ┌──────────┬──────────┬──────────┬──────────┐             │
│  │ OpenAI   │ Gemini   │ Anthropic│  Grok    │             │
│  │ GPT-4    │ 2.5      │ Claude   │  xAI     │             │
│  └──────────┴──────────┴──────────┴──────────┘             │
└──────────────────────┬──────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────┐
│              Document Processing Pipeline                    │
│  • PDF Extraction (pypdf)                                   │
│  • DOCX Processing (python-docx)                            │
│  • Markdown Transformation                                   │
│  • Text Chunking & Preprocessing                            │
└─────────────────────────────────────────────────────────────┘
1.2 Core Components
1.2.1 Agent Registry (agents.yaml)
A declarative configuration file defining 31+ specialized review agents. Each agent specification includes:

Agent Metadata: Unique ID, name, version, category
Model Configuration: Default model, temperature, max_tokens
Prompt Engineering: System prompt and user prompt templates
Output Requirements: Structured output specifications (tables, sections, format)
Validation Rules: Quality checks and required elements

1.2.2 Streamlit Web Interface
A sophisticated multi-tab web application providing:

Dashboard: Analytics, usage metrics, token consumption tracking
Specialized Review Tabs: 510(k) intelligence, PDF processing, comparison, checklist generation
Orchestration Interface: Device-specific workflow planning and execution
Dynamic Agent Generator: AI-driven agent creation from FDA guidance documents
Global Settings: Theme customization, language selection (English/繁體中文), API key management

1.2.3 LLM Router
Intelligent routing layer supporting multiple LLM providers:

OpenAI: GPT-4o-mini, GPT-4.1-mini for general-purpose tasks
Google Gemini: 2.5-flash variants for fast processing and document transformation
Anthropic Claude: 3.5 Sonnet/Haiku for complex reasoning and report generation
xAI Grok: Specialized models for comparative analysis and reasoning-heavy tasks

1.2.4 Document Processing Pipeline
Robust document ingestion and transformation:

Multi-format Support: PDF, DOCX, TXT, Markdown
Page-range Extraction: Selective content extraction for large documents
Structure Preservation: Maintaining headings, tables, lists during conversion
Markdown Normalization: Consistent formatting for downstream processing


2. Agent Catalog Specification
2.1 Core Review Agents (Agents 1-7)
2.1.1 FDA Search Agent (fda_search_agent)
Purpose: Intelligence gathering and comprehensive device overview generation
Model: GPT-4o-mini
Max Tokens: 12,000
Key Capabilities:

Device information aggregation from public FDA databases
Generation of 3000-4000 word structured review memoranda
Creation of 5+ markdown tables covering device basics, indications, technical characteristics, performance testing, and risk management
Inference-based analysis when direct data unavailable (clearly marked)

Output Structure:
markdownCopy# Device Overview
## Basic Information [Table]
## Indications for Use [Table]
## Technological Characteristics [Table]
## Performance Testing Matrix [Table]
## Risk Management Summary [Table]
2.1.2 PDF to Markdown Agent (pdf_to_markdown_agent)
Purpose: Document structure extraction and transformation
Model: Gemini 2.5-flash
Max Tokens: 12,000
Key Capabilities:

Clean markdown conversion preserving document hierarchy
Table reconstruction from unstructured text
Section identification and heading normalization
Content prioritization (device description, indications, performance data, risk management)

2.1.3 Summary & Entities Agent (summary_entities_agent)
Purpose: Comprehensive summarization and key entity extraction
Model: GPT-4.1-mini
Max Tokens: 12,000
Key Capabilities:

3000-4000 word detailed technical summaries
Extraction of 20+ regulatory entities with context
Entity classification (indication, risk, test, mitigation, design feature)
Reviewer-focused annotations and considerations

Entity Table Format:
Entity TypeEntity NameContextReviewer CommentLocationIndication[extracted][context][comment]Section X
2.1.4 Diff Agent (diff_agent)
Purpose: Version comparison and change detection
Model: Grok-4-fast-reasoning
Max Tokens: 12,000
Key Capabilities:

Identification of 100+ substantive differences between document versions
Classification of changes (indications, technical specs, testing, risks, labeling)
Impact assessment on safety and effectiveness
Side-by-side comparison with regulatory significance analysis

2.1.5 Checklist Agent (checklist_agent)
Purpose: Review checklist generation from guidance documents
Model: Claude 3.5 Haiku
Max Tokens: 12,000
Key Capabilities:

Parsing of FDA guidance and internal SOPs
Structured checklist creation with 10+ review domains
Item-level traceability to source guidance
Yes/No/N/A response options with reviewer note fields

2.1.6 Report Agent (report_agent)
Purpose: Comprehensive review memorandum compilation
Model: Claude 3.5 Sonnet
Max Tokens: 12,000
Key Capabilities:

Integration of checklist results and review findings
Substantial equivalence analysis documentation
Benefit-risk assessment synthesis
Recommendation generation (clearance, deficiency, major concerns)

2.1.7 Note Keeper Agent (note_keeper_agent)
Purpose: Reviewer note organization and structuring
Model: Gemini 3-flash-preview
Max Tokens: 8,000
Key Capabilities:

Fragment-to-structured-document transformation
Topic identification and hierarchy creation
Action item and open question highlighting
No hallucination guarantee with uncertainty markers

2.2 Magic Utility Agents (Agents 8-12)
These lightweight agents provide quick-turn productivity enhancements:
2.2.1 Magic Formatting Agent
Purpose: Markdown cleanup and readability optimization
Model: GPT-4o-mini
Max Tokens: 4,000
2.2.2 Magic Keywords Agent
Purpose: Regulatory keyword extraction with color highlighting
Model: GPT-4o-mini
Max Tokens: 4,000
Special Feature: HTML span injection with customizable color (default: coral #FF7F50)
2.2.3 Magic Action Items Agent
Purpose: Task extraction and prioritization
Model: GPT-4.1-mini
Max Tokens: 4,000
2.2.4 Magic Concept Map Agent
Purpose: Hierarchical concept extraction and relationship mapping
Model: Gemini 2.5-flash-lite
Max Tokens: 4,000
2.2.5 Magic Glossary Agent
Purpose: Terminology extraction and definition alignment
Model: Claude 3.5 Haiku
Max Tokens: 4,000
2.3 Specialized Analysis Agents (Agents 13-21)
2.3.1 Risk Matrix Agent (risk_matrix_agent)
Purpose: ISO 14971-compliant risk management analysis
Model: GPT-4.1-mini
Max Tokens: 10,000
Output: Risk matrices with hazard, harm, initial risk, mitigation, residual risk
2.3.2 Predicate Comparison Agent
Purpose: Substantial equivalence analysis
Model: GPT-4o-mini
Max Tokens: 10,000
2.3.3 Labeling Analysis Agent
Purpose: IFU and labeling adequacy assessment
Model: Gemini 2.5-flash
Max Tokens: 8,000
2.3.4 Software Safety Agent
Purpose: IEC 62304 software lifecycle review
Model: Claude 3.5 Haiku
Max Tokens: 10,000
2.3.5 Cybersecurity Assessment Agent
Purpose: FDA cybersecurity guidance compliance
Model: Grok-4-fast-reasoning
Max Tokens: 10,000
2.3.6 Biocompatibility Review Agent
Purpose: ISO 10993 biological safety evaluation
Model: GPT-4o-mini
Max Tokens: 8,000
2.3.7 Sterilization & Shelf-Life Agent
Purpose: Sterilization validation and aging study assessment
Model: Gemini 2.5-flash-lite
Max Tokens: 8,000
2.3.8 Clinical Evidence Agent
Purpose: Clinical data and statistical review
Model: Claude 3.5 Sonnet
Max Tokens: 12,000
2.3.9 Statistical Review Agent
Purpose: Study design and statistical method validation
Model: GPT-4.1-mini
Max Tokens: 8,000
2.4 Advanced Workflow Agents (Agents 22-31)
Additional specialized agents for substantial equivalence conclusions, deficiency letter drafting, sponsor response analysis, standards conformity tracking, adverse event analysis, human factors engineering, manufacturing QMS overview, regulatory timeline tracking, terminology consistency checking, and executive briefing generation.

3. Deployment Architecture
3.1 Hugging Face Spaces Configuration
Space Type: Streamlit
Hardware: CPU Basic (upgradeable to GPU for future enhancements)
Python Version: 3.10+
Persistent Storage: Hugging Face Datasets for session backup (optional)
Repository Structure:
Copyhuggingface-space-repo/
├── app.py                    # Main Streamlit application
├── requirements.txt          # Python dependencies
├── packages.txt             # System-level dependencies (if needed)
├── agents.yaml              # Agent catalog (31+ agents)
├── SKILL.md                 # Skills documentation
├── README.md                # User guide and setup instructions
├── .streamlit/
│   └── config.toml          # Streamlit configuration
├── utils/
│   ├── __init__.py
│   ├── llm_router.py        # Multi-provider LLM routing
│   ├── document_processor.py # PDF/DOCX handling
│   ├── agent_executor.py    # Agent invocation engine
│   ├── state_manager.py     # Session state utilities
│   └── ui_components.py     # Reusable UI elements
└── assets/
    ├── styles.css           # Custom CSS for painter themes
    └── localization.yaml    # i18n strings (EN/繁體中文)
3.2 Environment Variables & Secrets
Hugging Face Spaces secrets management for API keys:
bashCopyOPENAI_API_KEY=sk-...
GEMINI_API_KEY=...
ANTHROPIC_API_KEY=sk-ant-...
GROK_API_KEY=xai-...
Security Features:

API keys stored in HF Spaces secrets (not in code)
Fallback to user-provided keys via sidebar input (password type)
No logging of sensitive credentials
Session-scoped key storage (not persisted)

3.3 Streamlit Configuration (.streamlit/config.toml)
tomlCopy[theme]
primaryColor = "#FF6B6B"
backgroundColor = "#0E1117"
secondaryBackgroundColor = "#1F2937"
textColor = "#FAFAFA"
font = "sans serif"

[server]
maxUploadSize = 200
enableCORS = false
enableXsrfProtection = true

[browser]
gatherUsageStats = false

4. User Interface Specification
4.1 Global UI Features
4.1.1 Sidebar Configuration Panel
Components:

Theme Selector: Light/Dark mode toggle
Language Selector: English / 繁體中文 (Traditional Chinese)
Painter Style Selector: 20 artist-inspired visual themes

Van Gogh, Monet, Picasso, Da Vinci, Rembrandt, etc.
"Jackpot!" button for random style selection


Default Model Selector: Dropdown of all 9+ supported models
Token Budget: Number input (1000-120000)
Temperature: Slider (0.0-1.0, default 0.2)
API Key Inputs: Masked text inputs for OpenAI, Gemini, Anthropic, Grok
Agents Catalog Upload: Custom agents.yaml file upload for session override

4.1.2 Dynamic CSS Theming
Painter-inspired gradient backgrounds with dark/light mode adaptations:
cssCopy/* Example: Van Gogh Theme */
body {
  background: radial-gradient(circle at top left, #243B55, #141E30);
  transition: background 0.3s ease;
}
4.2 Tab Structure (9 Primary Tabs)
Tab 1: Dashboard
Purpose: System analytics and usage monitoring
Widgets:

Metrics Row: Total runs, 510(k) sessions, token consumption
Charts:

Bar chart: Runs by tab
Bar chart: Runs by model
Line chart: Token usage over time (time series)


Activity Table: Recent 25 runs with timestamp, tab, agent, model, tokens

Data Source: st.session_state["history"] (list of run events)
Tab 2: 510(k) Intelligence
Purpose: Device information gathering and initial overview
Input Fields:

Device name (text input)
510(k) number (text input)
Sponsor/manufacturer (text input)
Product code (text input)
Additional context (text area)

Agent: fda_search_agent
Output: 3000-4000 word markdown review memo with 5+ tables
Tab 3: PDF → Markdown
Purpose: Document conversion and structure extraction
Input:

PDF file uploader
Page range selector (from/to)
Extract button

Agent: pdf_to_markdown_agent
Output: Structured markdown with preserved hierarchy
Tab 4: Summary & Entities
Purpose: Comprehensive summarization and entity extraction
Input:

Markdown text area (can pull from Tab 3 output)

Agent: summary_entities_agent
Output:

3000-4000 word summary
20+ entity table with reviewer comments

Tab 5: Comparator (Document Diff)
Purpose: Version comparison analysis
Input:

Old version PDF upload
New version PDF upload
Extract text button

Agent: diff_agent
Output: 100+ differences in tabular format with regulatory impact assessment
Advanced Feature: Multi-agent chain execution on diff results
Tab 6: Checklist & Report
Purpose: Two-stage review workflow
Stage 1: Checklist Generation

Input: Guidance document (PDF/MD/TXT upload or paste)
Agent: guidance_to_checklist_converter
Output: Structured review checklist

Stage 2: Review Report

Input: Checklist + review results (file upload or paste)
Agent: review_memo_builder
Output: Formal 510(k) review memorandum

Tab 7: Note Keeper & Magics
Purpose: Productivity utilities for reviewer notes
Primary Agent: note_keeper_agent (note structuring)
Magic Sub-tools:

AI Formatting (cleanup)
AI Keywords (with color picker for highlight color)
AI Action Items (task extraction)
AI Concept Map (hierarchy building)
AI Glossary (terminology extraction)

Tab 8: FDA Reviewer Orchestration
Purpose: Device-specific review planning and agent workflow generation
Step 1: Device Description Ingestion

Raw text input or file upload (PDF/DOCX/TXT)
Transform to structured markdown with coral-highlighted keywords
Editable output

Step 2: Agents Catalog Review

Display current agents.yaml in table format
Option to upload custom agents.yaml

Step 3: Orchestration Plan Generation

Input fields: submission type, regulatory pathway, predicates, clinical data status, special circumstances
Analysis depth selector: Quick/Standard/Comprehensive
Editable orchestration prompt
Model and token configuration
Run orchestrator button

Agent: Custom FDA orchestrator using FDA_ORCH_SYSTEM_PROMPT
Output: Comprehensive review plan including:

Device classification analysis
Phase-based agent recommendations (Phases 1-4)
Execution sequence and parallelization opportunities
Timeline estimates
Critical focus areas
Anticipated challenges
Ready-to-use agent execution commands

Step 4: Sequential Agent Execution

Multi-select agent picker
Chain execution (output of one agent → input to next)
Progressive refinement workflow

Tab 9: Dynamic Agents Generator
Purpose: AI-driven agent creation from FDA guidance
Step 1: Guidance Ingestion

File upload (PDF/MD/TXT) or paste

Step 2: Optional Checklist Generation

Uses guidance_to_checklist_converter

Step 3: Dynamic Agent Generation

Model selector
Token configuration
Target agent count slider (3-8)
Generate button

Agent: Custom generator using DYNAMIC_AGENT_SYSTEM_PROMPT
Output:

3-8 new agent definitions in YAML format
Editable YAML text area
Download button for agents.yaml snippet
Merge instructions

Key Innovation: System analyzes guidance, existing agents catalog, and optional checklist to generate non-duplicative, complementary specialized agents

5. Agent Execution Engine
5.1 Execution Flow
pythonCopydef agent_run_ui(agent_id, tab_key, default_prompt, default_input_text, 
                 allow_model_override=True, tab_label_for_history=None):
    """
    Reusable agent execution interface
    
    Parameters:
    - agent_id: Reference to agents.yaml entry
    - tab_key: Unique session state key prefix
    - default_prompt: Pre-filled user prompt
    - default_input_text: Pre-filled input document/context
    - allow_model_override: Enable model selection dropdown
    - tab_label_for_history: Label for dashboard analytics
    """
Execution Steps:

Load agent config from agents.yaml
Display status indicator (pending/running/done/error)
Render UI: prompt text area, model selector, token input, input text area
On "Run Agent" button click:

Set status to "running"
Construct full prompt (system + user + input)
Call LLM via call_llm() router
Store output in session state
Update status to "done" or "error"
Log event to history


Display editable output (markdown or plain text view)
Support output chaining to next agent

5.2 LLM Router Implementation
pythonCopydef call_llm(model: str, system_prompt: str, user_prompt: str,
             max_tokens: int = 12000, temperature: float = 0.2,
             api_keys: dict = None) -> str:
    """
    Unified LLM interface supporting OpenAI, Gemini, Anthropic, Grok
    
    Returns: Generated text (str)
    Raises: RuntimeError if API key missing or call fails
    """
Provider Routing Logic:

Determine provider from model name
Retrieve API key (session state or environment variable)
Format request according to provider API:

OpenAI: client.chat.completions.create()
Gemini: genai.GenerativeModel().generate_content()
Anthropic: client.messages.create()
Grok: HTTPX POST to https://api.x.ai/v1/chat/completions


Extract and return generated text
Handle rate limits and errors with descriptive messages

5.3 Document Processing Pipeline
pythonCopydef extract_pdf_pages_to_text(file, start_page: int, end_page: int) -> str:
    """Extract text from PDF using pypdf (1-based page indexing)"""

def extract_docx_to_text(file) -> str:
    """Extract text from DOCX using python-docx"""
Features:

Page-range extraction for large documents
Graceful error handling (missing pages → empty string)
UTF-8 encoding normalization
Preservation of paragraph structure


6. Data Models and State Management
6.1 Session State Schema
pythonCopyst.session_state = {
    # Global settings
    "settings": {
        "theme": "Light" | "Dark",
        "language": "English" | "繁體中文",
        "painter_style": str (20 options),
        "model": str (default model),
        "max_tokens": int (1000-120000),
        "temperature": float (0.0-1.0)
    },
    
    # API keys
    "api_keys": {
        "openai": str,
        "gemini": str,
        "anthropic": str,
        "grok": str
    },
    
    # Agent catalog (overrideable)
    "agents_cfg": dict,  # Parsed agents.yaml
    
    # Usage history
    "history": [
        {
            "tab": str,
            "agent": str,
            "model": str,
            "tokens_est": int,
            "ts": ISO-8601 timestamp
        }
    ],
    
    # Tab-specific state (per tab_key)
    f"{tab_key}_status": "pending" | "running" | "done" | "error",
    f"{tab_key}_prompt": str,
    f"{tab_key}_input": str,
    f"{tab_key}_output": str,
    f"{tab_key}_output_edited": str,
    
    # Document processing state
    "pdf_raw_text": str,
    "old_text": str,
    "new_text": str,
    
    # Orchestration state
    "orch_device_md": str,
    "orch_device_md_effective": str,
    "orch_plan": str,
    "orch_plan_effective": str,
    
    # Dynamic agent state
    "dyn_agent_yaml": str
}
6.2 agents.yaml Schema
yamlCopyagents:
  agent_id_1:
    name: "Human-readable agent name"
    category: "Core Review" | "Specialized Analysis" | "Workflow Utility" | "Device-Specific Expert"
    version: "1.0"
    description: "Brief description of agent purpose"
    model: "default-model-name"  # From supported models list
    temperature: 0.2  # Float 0.0-1.0
    max_tokens: 12000  # Integer
    system_prompt: |
      Multi-line system prompt defining agent role,
      tasks, output requirements, and constraints.
    user_prompt_template: |
      Optional template for user prompts with {placeholders}.
    output_requirements:
      min_tables: 5
      required_sections: ["Overview", "Analysis", "Recommendations"]
      format: "markdown"
    validation_rules:
      require_sections: true
      check_table_count: true
      min_word_count: 3000
6.3 Event Logging
pythonCopydef log_event(tab: str, agent: str, model: str, tokens_est: int):
    """Append event to history for dashboard analytics"""
    st.session_state["history"].append({
        "tab": tab,
        "agent": agent,
        "model": model,
        "tokens_est": tokens_est,
        "ts": datetime.utcnow().isoformat()
    })

7. Internationalization (i18n)
7.1 Supported Languages

English: Primary language for UI labels and technical terms
繁體中文 (Traditional Chinese): Full UI translation with English term preservation

7.2 Translation Dictionary
pythonCopyLABELS = {
    "Dashboard": {"English": "Dashboard", "繁體中文": "儀表板"},
    "510k_tab": {"English": "510(k) Intelligence", "繁體中文": "510(k) 智能分析"},
    "PDF → Markdown": {"English": "PDF → Markdown", "繁體中文": "PDF → Markdown"},
    # ... (all tabs and UI elements)
}

def t(key: str) -> str:
    """Translate label based on current language setting"""
    lang = st.session_state.settings.get("language", "English")
    return LABELS.get(key, {}).get(lang, key)
7.3 Agent Output Language
Agent system prompts include language directives:
Copy使用繁體中文撰寫，保留關鍵術語的英文對照。
(Use Traditional Chinese, preserving English equivalents for key terms.)

8. Security and Compliance
