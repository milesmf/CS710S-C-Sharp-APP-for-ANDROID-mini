## Overview
This file contains prompt templates for use with large language models (LLMs) like Grok, ChatGPT, or Claude to analyze, prune, or optimize large codebases, such as this Xamarin/MvvmCross RFID reader app for Android. These prompts are designed to ensure *methodical* traversal ("prospecting"), dependency tracing, safety (error-free builds), and structured chat log outputs (file trees with emojis/notes). Copy/paste these into LLM chats, replacing placeholders (e.g., [FEATURE_NAME]) with specifics.

## Prompt Templates

### 1. Initial Big-Picture Pruning Plan
**Prompt**:

Given this large Xamarin/MvvmCross codebase (~9M+ chars from `CS710S-C-Sharp-ENTIRE-CONTEXT.xml` or `CS710S-C-Sharp-ENTIRE-CONTEXT.txt`), we need to prune deprecated features while keeping core ones (Inventory, Geiger, Configuration, Connect, Voltage Display). Pruned: [LIST_PRUNED_FEATURES].

Provide a big-picture plan: Analyze the entire repository structure, map preserved vs. pruned features based on UI diffs (e.g., old/new PageMainMenu.xaml), and trace dependencies recursively (primary: direct files; secondary: navigation/commands/bindings; tertiary: shared code/events/platform-specific; quaternary+: resources/tests/docs/edge cases like deep-linking).

Highlight risks: Intertwined events (e.g., global RFID OnAccessCompleted in BleMvxApplication.cs or CSLibrary), shared utilities (e.g., in Helpers/Settings.cs), platform renderers (e.g., in BLE.Client.Droid/Renderers/), resources (strings/styles/images), code comments, unit tests, analytics/logging, and config files.

Suggest order of removal (e.g., isolated features first). Ensure 100% build safety with incremental steps: Backups (git branches), global searches ("Find in Files" for feature names/strings), verifications (rebuilds, runtime tests on Android emulator/device, log checks for errors/warnings, preserved feature testing). Output: Overview with analysis/comprehension, then high-level per-feature steps with tools (e.g., VS "Find All References", git commits).

---

**Usage**: Initiates a pruning session by outlining the strategy for removing deprecated features. Use to understand the codebase and plan safe modifications, proactively addressing common risks and gaps from layered dependencies.

### 2. Feature-Specific Pruning Instructions
**Prompt**:

Prune [FEATURE_NAME] from the codebase. Use the `CS710S-C-Sharp-ENTIRE-CONTEXT.xml` or `CS710S-C-Sharp-ENTIRE-CONTEXT.txt` structure for a comprehensive file tree traversal.

Output a file tree with full paths:
- ❌ for changes (deletes/edits).
- ✅ for untouched.
- [NOTE] for details: What to cut (line-by-line/code blocks with examples), why (e.g., prevents compile/runtime errors from deleted refs, avoids global event leaks), verify (e.g., rebuild—no errors; run app—test preserved features like Inventory for regressions).

Cover all levels exhaustively: 
- Primary (direct files/folders, e.g., PagesViewModelsSet/[FEATURE_NAME]/).
- Secondary (navigation/commands/bindings, e.g., in ViewModelMainMenu.cs or BleMvxApplication.cs; XAML references across all pages).
- Tertiary (shared code/events/platform-specific, e.g., global event handlers in BleMvxApplication.cs, shared utilities in Helpers/, renderers in BLE.Client.Droid/).
- Quaternary+ (resources like strings.xml/styles/images; tests in BLE.Client.Tests/; docs/comments via global searches; edge cases like navigation stacks/deep-linking, analytics/logging, config files).

Use global searches ("Find in Files" case-insensitive for [FEATURE_NAME], commands, ViewModels) for strays. Proactively check common gaps: Stray XAML bindings, navigation history/backstack, global events (e.g., _reader.rfid.OnAccessCompleted with feature-specific logic), shared utils/constants, platform renderers/services, resources (strings/styles/images/assets), code comments/inline docs, unit tests/mocks, analytics (e.g., logging feature events), configs (e.g., feature toggles). Ensure no regressions in preserved features. End with summary, potential issues, and commit suggestion (e.g., "git commit -m 'Pruned [FEATURE_NAME]: Removed files/refs. Verified builds.'").

---

**Usage**: Core template for pruning a specific feature (e.g., "Read/Write"). Ensures granular, safe removal with full dependency cleanup, now with explicit proactive gap checks for exhaustive coverage.

### 3. Refining Pruning Output
**Prompt**:

Review my previous pruning instructions for [FEATURE_NAME]. Check depth exhaustively: Did we cover primary/secondary/tertiary/quaternary+? Identify additional gaps like: Stray XAML references across pages, navigation history/deep-linking/backstack logic (e.g., in BleMvxApplication.cs or App.xaml.cs), global event handlers (e.g., _reader.rfid.OnAccessCompleted with feature-specific conditions in BleMvxApplication.cs or helpers), shared utilities/constants (e.g., in Helpers/Settings.cs), platform-specific renderers/services (e.g., in BLE.Client.Droid/ or iOS/UWP equivalents), resources (strings.xml, styles in App.xaml, images/assets), code comments/inline docs (global search for feature names), unit tests/mocks (e.g., in BLE.Client.Tests/), analytics/logging (e.g., feature-specific events in helpers), configuration files (e.g., toggles in .json/.config).

Refine: Add missed steps (e.g., global searches for feature strings in comments/resources, platform .csproj edits, edge case tests like deep-linking). Output updated instructions in file tree format with ❌/✅ and [NOTE] prefixes (what/why/verify). Ensure 100% build safety with emphasis on post-prune testing (rebuilds, runtime on Android emulator/device, preserved feature verification, logs for warnings/errors).


**Usage**: Iterates on prior outputs to address gaps or refine instructions, ensuring exhaustive coverage with expanded common gap examples drawn from hands-on experience.

---

## Tips for Using These Prompts
- **Context Inclusion**: Always prepend full repo structure (e.g., `CS710S-C-Sharp-ENTIRE-CONTEXT.xml` or `CS710S-C-Sharp-ENTIRE-CONTEXT.txt`) and feature lists (preserved/pruned) for accuracy.
- **Iteration**: If output needs tweaks, follow up with specifics (e.g., "Add more on edge cases like analytics/logging").
- **LLM Choice**: Use Grok for reasoning/large contexts, Claude for structured Markdown, or ChatGPT for quick iterations.
- **Safety**: Backup before applying changes (e.g., `git branch prune-<feature>`); test preserved features post-prune.