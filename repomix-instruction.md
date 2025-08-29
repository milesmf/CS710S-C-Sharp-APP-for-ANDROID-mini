# Repomix Instructions — CS710S Android (C# / Xamarin / MvvmCross)

**Goal**  
Produce a single packed context for AI agents to analyze BLE and RFID flows in the CS710S handheld reader app on Android (Xamarin.Forms + MvvmCross), excluding build artifacts, binaries, and noise. Ensure the full output is generated without truncation.

---

## What to Include (Priority Order)
1) Solution & Projects  
- `CS710S-C-Sharp-APP-for-ANDROID.sln`  
- `Library/CSLibrary/*.csproj` (NETStandard, NET8-MAUI, .NET4 variants)  
- `MobileMvxApp/BLE.Client.csproj`  
- `MobileMvxApp/BLE.Client.Droid.csproj`

2) Core Library (RFID + HAL + Tools)  
- `Library/CSLibrary/Source/**.cs`  
  - RFID Unified API: `CSLUnifiedAPI/**` (Public/Private/Basic_*; CS710S, CS108 focus)  
  - Comm Protocol: `Ex10Commands/**`, `RX000Commands/**`  
  - HAL BLE: `HAL/**` (MvvmCross/Plugin.BLE integrations)  
  - Utilities: `Tools/**`, `SystemInformation/**`, `BarcodeReader/**`

3) Android App (UI + ViewModels)  
- `MobileMvxApp/BLE.Client/**.cs` (Shared logic, ViewModels, Pages)  
- `MobileMvxApp/BLE.Client.Droid/**.cs` (Platform-specific, e.g., MainActivity.cs, Setup.cs)  
- `MobileMvxApp/BLE.Client.Droid/Properties/AndroidManifest.xml`  
- `MobileMvxApp/BLE.Client.Droid/Resources/values/*.xml`  
- `MobileMvxApp/BLE.Client.Droid/Resources/layout/*.axml`

4) Docs / Metadata  
- `README.md`  
- `repomix-instruction.md`  
- `repomix.config.json` (For Repomix self-reference)

---

## Exclude (Noise & Generated)
- Build & IDE: `**/bin/**`, `**/obj/**`, `.vs/**`, `.idea/**`, `.vscode/**`  
- Packages & NuGet: `packages/**`, `**/Packages/**`  
- Designer/Generated: `**/Resource.designer.cs`, `**/*.g.cs`, `**/*.Designer.cs`  
- Binaries/Archives: `**/*.dll`, `**/*.so`, `**/*.a`, `**/*.jar`, `**/*.aar`, `**/*.nupkg`, `**/*.keystore`, `**/*.apk`, `**/*.aab`, `**/*.zip`  
- Images/Misc: `**/*.png`, `**/*.jpg`, `**/*.gif`, `**/*.pdf`  
- Local Config: `**/*.user`, `**/*.userprefs`, `**/*.csproj.user`

---

## Logical Read Order (for AI Prospecting)
Guide AI agents in "prospecting" interconnected paths:  
1) `CS710S-C-Sharp-APP-for-ANDROID.sln` → Project dependencies.  
2) App Entry: `BLE.Client.Droid/MainApplication.cs`, `MainActivity.cs`, `Setup.cs` (MvvmCross initialization).  
3) ViewModels: `BLE.Client/ViewModels/**` (e.g., `DeviceListViewModel.cs`, `ViewModelMainMenu.cs` for BLE scanning).  
4) BLE HAL: `Library/CSLibrary/Source/HAL/**` (Device discovery, GATT connections via Plugin.BLE).  
5) RFID Unified API (CS710S Focus): `CSLUnifiedAPI/Basic_API/CS710S/**` (Public/Private; Inventory/Read/Write/Antenna/Power—trace cross-links from public to private layers).  
6) Comm Protocol: `Comm_Protocol/**` (Ex10/RX000 commands).  
7) Utilities: `Tools/**`, `SystemInformation/**`.  

This order reflects app flow: Android bootstrap → MvvmCross setup → BLE connect/scan → RFID operations. Emphasize tracing dependencies (e.g., how `ClassRFID.Public.*` invokes private/driver layers).

---

## Summarization Hints (Chunking for AI)
- Group by Feature: **BLE Connect** (HAL + ViewModels), **Inventory** (CSLUnifiedAPI/Private.Inventory.cs), **Read/Write** (Private.Read/Write.cs), **Antenna/Power** (Antenna/**, Public.Power.cs), **Tag Filters/QT** (QTCommandParms.cs), **Errors/Status** (CS710SErrorCode.cs).  
- Limit chunks to ≤2k tokens; prefix each with file paths.  
- Cross-link public APIs to private implementations (e.g., `ClassRFID.Public.Operation.cs` → `ClassRFID.Private.*`).  
- Note response modes (compact/normal/extended) in tag operations.  
- Flag Token-Heavy Files (>1k tokens): `ClassRFID.cs`, `ClassRFID.Private.cs`, `DeviceListViewModel.cs`—suggest splitting in AI analysis.

---

## Important Callouts
- Android: `MainActivity.cs`, `AndroidManifest.xml`, `Resources/values/*.xml`.  
- ViewModels: `DeviceListViewModel.cs`, `ViewModelViewPage.cs`.  
- CS710S API: Public (`ClassRFID.Public.*.cs`, `ClassRFID.UnifiedAPI.cs`), Operations (`ClassRFID.Private.*.cs` for Read/Write/Inventory/Select), Antenna/Power (`Antenna/**`, `ClassRFID.Public.Power.cs`), Protocol (`Ex10Commands/**`, `RX000Commands/**`).  
- Utilities: `Tools/HexEncoding.cs`, `Tools/ClassCRC16.cs`.

---

## Output Requirements
- Single packed file: **Summary → Repo Info → Structure → Files (path + contents)**.  
- Preserve code blocks verbatim; include file headers.  
- Mark **excluded** areas clearly.  
- Note token-heavy files with byte/token counts.  
- Use relative paths from repo root.  
- Style: XML, with file summaries and directory structure.  
- Ensure full output without truncation (support up to 100MB).

---

## Build Context
- Targets: Android (armeabi-v7a, arm64-v8a, x86_64 for emulators).  
- Key Packages: MvvmCross.Plugin.BLE (2.2.0-pre5), Plugin.BLE (3.0.0).  
- Build: Open solution in VS 2022 → Select `BLE.Client.Droid` → Debug/Release → Deploy to device/emulator.

---

## Quality Checks (for AI Post-Packing)
- Flag BLE timing issues, UI thread marshaling, and disconnect edges (e.g., in `DeviceListViewModel.cs`).  
- Confirm frequency/power settings are configurable (e.g., `ClassRFID.Public.Country.cs`).  
- Verify no UI-blocking I/O; check cancellation tokens in scans/reads (e.g., `ClassRFID.Private.Inventory.cs`).
