# C# CS710S MvvmCross App for Android

CS710S Xamarin C# Bluetooth Demo App and SDK 

- .NET Standard 2.0 and MvvmCross 7
- Support implementations on Android platform

This application provides demonstrations to the programming interface made available on the CS710S handheld reader for configuring, controlling, and accessing the RFID reader.  The development was based on the unified CS710S C# Callback-based API that made available on different CSL readers.  This API is applicable to Android environment.  

[Product Information](https://www.convergence.com.hk/CS710S/)
<br>

## Pre-requisite

The build environment consists of tools and the corresponding configurations of the Visual Studio 2022(Windows).  It is expected that the system integrator or the software system programming house will be developing the applications on Visual Studio 2022(Windows).  With this tool, typically he has to write programs on the PC.  The following are needed to set up the build environment.

## Basic configuration on PC

Operating System requirement:
-	Microsoft Windows 10 (English)

Software package required:
-	Microsoft Visual Studio 2022

To build demo application successfully, you need to install Microsoft Visual Studio 2022 or above. For more detailed information, please go to [Microsoft webpage](https://docs.microsoft.com/en-us/visualstudio/welcome-to-visual-studio).
<br><br>
[Visual Studio 2019](https://www.visualstudio.com/zh-hant/vs/whatsnew/)

In Visual Studio, you need to use the Visual Studio Installer to add the Xamarin package:<br>

<img src="app_img/CS108-DotnetMobileDev.png" width="800"/> <br><br>

To run the application on a desktop device, Visual Studio 2022 provides emulator capabilities for Android, supporting the x86_64 architecture. Ensure the project configuration in `BLE.Client.Droid.csproj` includes the following in the `<PropertyGroup>` for emulator compatibility:

```xml
<AndroidSupportedAbis>armeabi-v7a;arm64-v8a;x86_64</AndroidSupportedAbis>
```

## Project Files
1. CS710S-C-Sharp-APP-for-ANDROID.sln for Android platform (BLE.Clinet Target framework set to .net Standard 2.1)
<br><br>

## Callback-based API Library

The CS710S Callback-based API Library consists of the following files. 

|File   | Location of source code |Remarks  |
|-------|-------------------------|---------|
|CSLibrary.dll|Inside folder “/CSLibrary/bin/Debug” of the CS710S Demo Code|CSL C# Callback-based API Class Library. Different library files.|
<br>

## CS710S C# API: Theory of Operation

The CS710S C# Application Programming Interface (API) provides a programming interface for controlling CS710S integrated reader. The interface is loaded by a mobile application; the application in turn explicitly initializes the interface. The interface supports enumeration of attached RFID radio modules, returning unique identification information for each currently-attached RFID radio modules. An mobile application uses the CS710S C# API to establish a connection and grant the application exclusive control of the corresponding RFID radio module. After an application is granted exclusive control of an RFID radio module, the application can configure the RFID radio module for operation and tag protocol operations can be issued. The CS710S C# API allows an application control of low level functions of the Firmware, including but not limited to: 

- regulatory configuration of frequencies 
- antenna output power 
- air protocol parameters, such as Q value 

Some of these configuration parameters are abstracted by the CS710S and CS108 C# API. The application initiates transactions with ISO 18000-6C tags or tag populations by executing ISO 18000-6C tag-protocol operations. The interface exposes direct access to the following ISO 18000-6C tag-protocol operations: 

- Inventory 
- Read 
- Write 
- Kill 
- Erase 
- Lock 

When executing tag-protocol operations, the interface provides the application with the ability to configure tag-selection (i.e., ISO 18000-6C Select) criteria and query (i.e. ISO 18000-6C Query) parameters. The interface extends the ISO 18000-6C tag- protocol operations by additionally providing the application the ability to specify a post-singulation Electronic Product Code (EPC) match mask as well as the number of tags to which the operation is to be applied. Additionally, the interface supports configuration of dense-reader mode during ISO 18000-6C tag-protocol operations. The interface supports the configuration and control of the antenna. The application is given fine-grained control to configure: 

- A time limit for performing tag-protocol operations (dwell time) 
- The number of times a tag-protocol operation is executed (number of inventory rounds) 
- RF characteristics (for example, RF power). 

The interface supports a callback model for presenting tag-protocol operation response data to the application. When an application issues a tag-protocol request (i.e. inventory, read, etc.), it also provides a pointer to a callback function API invokes. To help simplify the packet-processing code the API provides complete tag-protocol operation response packets. Tag-protocol operation results include EPC values returned by the Inventory operation, read data returned by the Read operation, and operation status returned by the Write, Kill, Erase, and Lock operations. The application can request the returned data be presented in one of three formats: compact, normal, or extended. Compact mode contains the minimum amount of data necessary to return the results of tag-protocol operations to the application. Normal mode augments compact mode by interleaving additional status/contextual information in the operation results, so that the application can detect, for example, the start of inventory rounds, when a new antenna is being used, etc. Extended mode augments normal mode by interleaving additional diagnostic and statistical data with the operation results. The interface supports diagnostic and statistical reporting for radio, inventory, singulation, tag access, and tag performance as well as status packets to alert the application to unexpected errors during tag- protocol operations. 

The interface provides the application with access to (i.e., read and write) the configuration data area on the RFID radio module. The application can use the configuration data area to store and retrieve the specific hardware configuration and capabilities of the radio. The application can read a RFID radio module’s configuration data area immediately after gaining exclusive control of the RFID radio module and use that data to configure and control low-level radio parameters. The interface also supports low-level control of the RFID Firmware. 

## Callback-based API Classes, Methods and Events

Please refer to the documentation under the folder Library/CSLibrary/Readme.txt and CSLibrary/docs

---

## AI Tools and Workflow in Development

This project leveraged AI tools to assist in codebase analysis, refactoring, and pruning, ensuring a methodical and safe approach to maintaining the repository. Below is an overview of the tools used and the general workflow, aimed at helping new developers replicate or extend similar processes.

### AI Tools Used
- **Grok (by xAI)**: The primary LLM for this development cycle, chosen for its strong reasoning capabilities, handling of large contexts (e.g., ~9M+ character codebase merges), and tools integration (e.g., code execution, web search). Grok excelled in "prospecting" (deep traversal and dependency mapping) as per the `repomix-instruction.md`, providing iterative refinements, file tree visualizations, and granular pruning instructions. It was used for:
  - Analyzing repository structure from `repomix-output.txt`.
  - Generating safe, verifiable prune steps (e.g., feature-by-feature removal with backups, global searches, and build verifications).
  - Creating reusable templates (e.g., file trees with ❌/✅ emojis and [NOTE] prefixes).
  - Alternatives: For similar tasks, ChatGPT (via OpenAI) or Claude (via Anthropic) could substitute, as they support large contexts and structured outputs. Claude is particularly strong for code parsing due to its Markdown/XML handling, while ChatGPT offers broader integration with IDE plugins.

### General Workflow
1. **Preparation**: Backup the repo (e.g., `git branch prune-<feature>`) and load in Visual Studio (2022 for Windows or VS for Mac). Use `repomix-output.txt` for a merged codebase view.
2. **AI-Assisted Prospecting**: Query the LLM with the full context (e.g., repository structure, preserved/pruned features from PageMainMenu.xaml diffs). Request a file tree output for traversal, marking changes with ❌ (modify/delete) or ✅ (untouched), and [NOTE] for details (what/why/verify).
3. **Iterative Pruning**: Target one feature at a time (e.g., "Register Tag"). Use LLM to:
   - Trace dependencies (primary: files; secondary: nav/commands; tertiary: shared/events; quaternary+: resources/tests/docs).
   - Suggest line-by-line cuts, .csproj edits, and global searches (e.g., "Find in Files" for feature names).
   - Ensure safety: Remove usings, event refs, and test for build errors.
4. **Verification**: Rebuild (Ctrl+Shift+B), run on Android emulator/device, test preserved features (e.g., Inventory, Geiger), check logs, and commit incrementally.
5. **Refinements**: Feed LLM feedback (e.g., "Add more depth to edge cases") for iterations, optimizing outputs (e.g., visual file trees).
6. **Documentation**: Update README.md and create supporting files (e.g., PROMPTS.md) via LLM for reusability.

This workflow reduced bloat while keeping the codebase 100% compilable and functional. For new developers: Start with small prunes, use VS tools for searches, and leverage LLMs for dependency mapping to avoid regressions.