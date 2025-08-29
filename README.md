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

# AI Tools & Workflow in Development  

This project applied AI tools to assist in code analysis, refactoring, and pruning, ensuring a structured and safe process for maintaining the repository. The workflow emphasizes repeatability and clarity for both new and experienced developers.  

## AI Tools Used  
- **Grok (by xAI):** Primary LLM chosen for:  
  - Handling large contexts (e.g., ~9M+ character merges).  
  - Deep repository “prospecting” (dependency mapping, structure analysis from `CS710S-C-Sharp-ENTIRE-CONTEXT.txt` or `CS710S-C-Sharp-ENTIRE-CONTEXT.xml`).  
  - Generating safe prune steps with backups, global searches, and build verifications.  
  - Creating reusable templates (file trees with ❌ modify/delete, ✅ keep, and [NOTE] context).  
- **Alternatives:**  
  - **ChatGPT (OpenAI):** Broader IDE integration.  
  - **Claude (Anthropic):** Stronger for structured/markup parsing.  

## General Workflow  
1. **Preparation**  
   - Backup repo (`git branch prune-<feature>`).  
   - Load in Visual Studio (Windows/Mac).  

2. **AI-Assisted Prospecting**  
   - Upload `CS710S-C-Sharp-ENTIRE-CONTEXT.txt` or `CS710S-C-Sharp-ENTIRE-CONTEXT.xml` to your preferred LLM.
   - Query LLM with specific context (structure, preserved/pruned features, diffs).  

3. **Iterative Pruning**  
   - Target one feature at a time (e.g., “Register Tag”).  
   - Trace dependencies:  
     - Primary, Secondary, Tertiary, etc: ensure all nested references are cleanly removed:
        - Reference prompts from (`prompts.md`)
     - Remove usings, update refs, confirm builds remain clean.  

4. **Verification**  
   - Rebuild
   - Test preserved features, ensure application runs as expected.
   - Run on emulator/device, check logs.
   - Publish incremental commits ensuring backtracking is possible.  

6. **Documentation**  
   - Update README and related supporting files.  
   - Review and borrow prompts/templates from (`prompts.md`).  

---

## Notes on Repomix
The `CS710S-C-Sharp-ENTIRE-CONTEXT.*` files are comprehensive, packed representations of the entire codebase, generated by Repomix—a tool that consolidates source code, projects, and docs into a single, AI-consumable format (e.g., .txt or .xml). The `repomix.config.json` file configures Repomix to include/exclude files, define output structure, and ensure full content without truncation, tailoring the pack to our needs. This setup makes our codebase "AI Ready" by enabling deep analysis of BLE/RFID flows, dependency mapping, and optimization suggestions via AI tools like Grok, Claude, ChatGPT, etc, streamlining development and maintenance for the CS710S app.