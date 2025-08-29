## Overview
This file contains prompt templates for use with large language models (LLMs) like Grok, ChatGPT, or Claude to analyze, prune, or optimize large codebases, such as this Xamarin/MvvmCross RFID reader app for Android. These prompts are designed to ensure methodical traversal ("prospecting"), dependency tracing, safety (error-free builds), and structured outputs (file trees with emojis/notes). Copy/paste these into LLM chats, replacing placeholders (e.g., [FEATURE_NAME]) with specifics.

## Prompt Templates

### 1. Initial Big-Picture Pruning Plan
**Prompt**:
```
Given this large Xamarin/MvvmCross codebase (~9M+ chars from repomix-output.txt), we need to prune deprecated features while keeping core ones (Inventory, Geiger, Configuration, Connect, Voltage Display). Pruned: [LIST_PRUNED_FEATURES].

Provide a big-picture plan: Analyze structure, dependencies, risks (e.g., shared events in CSLibrary), and order of removal. Ensure 100% build safety with incremental steps (backups, verifications). Output: Overview, then per-feature steps with tools (e.g., VS searches).
```

**Usage**: Initiates a pruning session by outlining the strategy for removing deprecated features. Use to understand the codebase and plan safe modifications.

### 2. Feature-Specific Pruning Instructions
**Prompt**:
```
Prune [FEATURE_NAME] from the codebase. Use the repomix-output.txt structure.

Output a file tree with full paths:
- ❌ for changes (deletes/edits).
- ✅ for untouched.
- [NOTE] for details: What to cut (line-by-line/code blocks), why (e.g., prevents errors from deleted refs), verify (e.g., rebuild—no errors).

Cover all levels: Primary (files), secondary (nav/commands), tertiary (shared/events), quaternary+ (resources/tests/docs/edge cases). Use global searches for strays. Ensure no regressions in preserved features. End with summary/commit suggestion.
```

**Usage**: Core template for pruning a specific feature (e.g., "Read/Write"). Ensures granular, safe removal with full dependency cleanup.

### 3. Refining Pruning Output
**Prompt**:
```
Review my previous pruning instructions for [FEATURE_NAME]. Check depth: Did we cover primary/secondary/tertiary/quaternary+? Identify gaps (e.g., resources, tests, comments).

Refine: Add missed steps (e.g., global XAML searches, platform renderers). Output updated instructions in file tree format with ❌/✅ and [NOTE] prefixes. Ensure 100% build safety.
```

**Usage**: Iterates on prior outputs to address gaps or refine instructions, ensuring exhaustive coverage.

### 4. Summarizing Process and Creating Templates
**Prompt**:
```
Summarize our pruning process: Exploration (traversal), pruning (steps), critiques (iterations). Then, detail desired output structure: File tree rules (emojis, [NOTE]), context (retention/removal/safety).

Output: Summary section, then "Desired Output and Intended Context" (general for any feature), then "Detailed Output Structure" with examples. Make it copy/pasteable for future chats.
```

**Usage**: Creates reusable templates for future pruning tasks, summarizing the process and output structure.

### 5. Optimizing Repository Instructions (.md)
**Prompt**:
```
Optimize repomix-instruction.md based on our pruning experience. Incorporate hands-on insights: Layered traversal, surgical pruning (feature-by-feature, dependency chains), verification (rebuilds/tests), visual aids (file trees with ❌/✅/[NOTE]).

Make it developer-centric: Actionable steps, Android/Xamarin focus, no prompt-like phrasing. Output full updated .md, copy/pasteable.
```

**Usage**: Updates repository documentation to reflect pruning best practices, tailored to the codebase.

## Tips for Using These Prompts
- **Context Inclusion**: Always prepend full repo structure (e.g., repomix-output.txt) and feature lists (preserved/pruned) for accuracy.
- **Iteration**: If output needs tweaks, follow up with specifics (e.g., "Add more on edge cases").
- **LLM Choice**: Use Grok for reasoning/large contexts, Claude for structured Markdown, or ChatGPT for quick iterations.
- **Safety**: Backup before applying changes (e.g., `git branch prune-<feature>`); test preserved features post-prune.