## Architectural Concept

The **Atomic Skeletal Hash Architecture (ASHA)** is a sovereign, database-backed code ingestion and drift analysis engine designed to optimize how AI systems interact with source code. Rather than treating code as a fluid blob of text or a fragile sequence of absolute line numbers, ASHA converts source code into a stateful, content-addressed, multi-tiered cryptographic space.

By combining bare-metal syntax formatting, abstract syntax tree (AST) structural isolation, and transactional database persistence, ASHA establishes a deterministic blueprint of code topology. This allows an AI model or a developer workspace to surgically identify, modify, or insert code blocks with character-perfect precision based entirely on immutable cryptographic handles.

---

## Technical Pipeline & Mechanics

The architecture executes code mapping through a structured five-stage pipeline to achieve relational integrity and isolate coordinate drift.

```
+-------------------------------------------------------------+
|                     1. NORMALIZATION GATE                   |
|  - Windows CRLF -> Unix LF   - Per-line trailing trim       |
|  - Subprocess Deno native Rust formatting (deno fmt)        |
+-------------------------------------------------------------+
                              |
                              v
+-------------------------------------------------------------+
|                  2. RELATIONAL INGESTION                    |
|  - Generate core modules relational anchor map               |
|  - Commit baseline historical snapshot (3NF schema layout)  |
+-------------------------------------------------------------+
                              |
                              v
+-------------------------------------------------------------+
|                    3. VECTOR ATOMIZATION                    |
|  - Stream content arrays into public.asha_lines             |
|  - Calculate indentation depth metrics and text SHA-256     |
+-------------------------------------------------------------+
                              |
                              v
+-------------------------------------------------------------+
|                 4. STRUCTURAL BLOCK ASSEMBLER               |
|  - Invoke TS-Morph AST crawler to detect syntax boundaries  |
|  - Deconstruct lines into unique content-addressed blocks   |
+-------------------------------------------------------------+
                              |
                              v
+-------------------------------------------------------------+
|                   5. WORKSPACE DRIFT ENGINE                 |
|  - Match active structures with proposed AI inputs         |
|  - Output surgical change instructions (MATCH/MODIFY/INSERT)|
+-------------------------------------------------------------+

```

### 1. The Normalization Gate (`asha_preflight_normalizer.ts`, `asha_normalization_engine.ts`)

To eliminate arbitrary aesthetic differences (such as mixed spacing or varied carriage returns) that cause cryptographic hash mutations, files are forced through a strict preflight layer.

* **Sanitization:** Windows CRLF variants are stripped down to uniform Unix LF line breaks, and trailing whitespace is culled.
* **Deterministic Formatting:** The engine spawns a native Deno Rust subprocess (`deno fmt`) directly against target paths on disk, ensuring absolute syntax styling layout before any hashes are read.

### 2. Base Relational Anchor Ingestion (`asha_snapshot_commit.ts`, `asha_coordinator_batch.ts`)

Once normalized, the script opens a high-performance database transaction.

* **3NF Inventory Tracking:** It populates the core relational mapping tables (`public.modules` / `public.asha_modules`), creating or fetching a localized anchor ID based on path variables.
* **Immutable Version Ledger:** A complete historical content footprint is pushed down into `public.module_snapshots`. If the incoming content hash matches the last entry recorded for that anchor, the pipeline cleanly skips redundant storage routines.

### 3. Vector Atomization (`asha_coordinator_batch.ts`, `asha_ingest_trial.ts`)

The engine splits files into arrays of lines and indexes them individually.

* **Deduplicated Lines:** Every unique line of text is passed into `public.asha_lines`, keyed uniquely by its personal SHA-256 signature alongside an explicit tracking metric for indentation depth.
* **Junction Mapping:** An operational junction entity (`public.asha_module_line_junction`) matches these line hashes to absolute indices, carrying supplemental array IDs for multi-token syntax matrices and compiler boundary classifications.

### 4. Structural Block Assembly (`asha_block_builder.ts`, `asha_nomenclature_crawler.ts`)

The system passes code metadata to an internal AST crawler engine (leveraging `ts-morph` parsing functionality).

* **Block Deconstruction:** Instead of tracking code lines arbitrarily, the assembler maps blocks according to structural semantics (functions, classes, interfaces, control blocks).
* **Content Addressing:** The engine pushes these nodes into `public.asha_unique_blocks` using a composite array signature (`line_hashes`, `block_type`, and a cryptographic `content_hash`).
* **Sequence Alignment:** Coordinates are instantly bound within `public.asha_module_block_instances` via sequence ordering weights, mapping exactly how blocks reconstruct linearly.

### 5. Workspace Drift Evaluation (`asha_drift_analyzer.ts`)

When a change request or an AI-generated string template hits the environment, ASHA avoids text-comparison tools like `diff`.

* **Live Database Comparison:** It queries the baseline topological map of the disk schema and runs a concurrent, in-memory AST spider parsing execution over the suggested block string layout.
* **Surgical Change Instructions:** By reconciling structural code indicators and unique token signatures, it builds an explicit structural delta ledger. Each modified component receives clear instruction data mapping its coordinate transformation:

| Operational Action | Structural Condition Triggers | Calculated Output Payload Metrics |
| --- | --- | --- |
| **`MATCH`** | Token indicator and cryptographic content hash remain unchanged. | Line delta is zero; layout parameters are preserved. |
| **`MODIFY`** | Token indicator matches an existing block, but content hash differs. | Tracks the raw line delta count and records before/after hash vectors. |
| **`INSERT`** | Token indicator or signature is completely absent from database records. | Registers a positive line count delta and flags a new structural entry. |
| **`DELETE`** | Database indicator is missing from the incoming change vector. | Registers a negative line count delta based on deleted code blocks. |

---

## Core Innovations & Architectural Advancements

* **Immutable Content-Addressed Identity:** Code changes are mapped securely using deterministic content hashes. If a function is moved to a different location in a file or split across directories, the system tracks its identity seamlessly, avoiding the broken references typical of line-based diff models.
* **Decoupled Structural Management:** Decoupling blocks into unique structural definitions (`asha_unique_blocks`) separate from local arrangement instances (`asha_module_block_instances`) allows for advanced code reuse and historical tracking across multiple branches.
* **High-Precision Context Management for AI:** By indexing files into precise, content-hashed lines and semantic blocks, AI agents can query and modify individual functions without processing large, unrelated code context windows. This minimizes token consumption and removes the risk of AI-generated syntax issues during file-merge operations.