# CS710S Android Repository Instructions

## Project Overview
This repository contains the CS710S Xamarin C# Bluetooth Demo App and SDK for Android, built with .NET Standard 2.0 and MvvmCross 7. It provides a programming interface for configuring, controlling, and accessing the CS710S handheld RFID reader via Bluetooth, using the unified CS710S C# Callback-based API.

## Coding Guidelines
- Follow C# coding standards as per Microsoft guidelines.
- Use MvvmCross patterns for view models and navigation (e.g., `BaseViewModel.cs`, `DeviceListViewModel.cs`).
- Maintain clear separation of concerns between platform-specific code (e.g., `BLE.Client.Droid/`) and shared code (e.g., `BLE.Client/`).
- Include comments for complex logic, especially in RFID operations (e.g., `ClassRFID.cs`).
- Ensure all code is compatible with Android, targeting `armeabi-v7a`, `arm64-v8a`, and `x86_64` architectures for emulator support.

## AI Traversal and 'Prospecting'
When analyzing this repository, adopt a methodical and deep approach to traversing the files, treating the exploration as 'prospecting' for interconnected insights. Many files, particularly in the `Library/CSLibrary/Source/` directory, contain layered abstractions and dependencies that span across modules (e.g., RFID operations in `ClassRFID.Private.cs` linking to Bluetooth handling in `BLE.Client/`). Keep in mind that the code is highly interoperable—components like antenna configurations, inventory operations, and Bluetooth protocols are not isolated but form a connected ecosystem. Start with high-level entry points (e.g., `CS710S-C-Sharp-APP-for-ANDROID.sln`, `DeviceListViewModel.cs`) and systematically trace dependencies to uncover how shared logic in `BLE.Client/` integrates with platform-specific implementations in `BLE.Client.Droid/`. This depth ensures a comprehensive understanding of the application's logic, revealing how unified APIs enable RFID reader control across the Android environment.

## AI Analysis Instructions
- **Code Review**: Analyze code for adherence to C# best practices, focusing on readability, maintainability, and Bluetooth/RFID functionality. Suggest optimizations for Android performance.
- **Refactoring**: Propose splitting large classes (e.g., `ClassRFID.cs`) into smaller, focused units where appropriate.
- **Testing**: Suggest unit tests for key components (e.g., `DeviceListViewModel.cs`, `ClassRFID.Private.Inventory.cs`), covering edge cases like Bluetooth disconnections.
- **Documentation**: Enhance inline comments and update `Readme.txt` in `Library/CSLibrary/` if outdated.
- **Security**: Verify no sensitive data (e.g., API keys) is exposed, especially in configuration files like `AndroidManifest.xml`.

## Output Expectations
- Generate comprehensive output without abbreviation, preserving all code and comments.
- Include git logs (last 50 commits) to provide development context.
- Highlight token-heavy files (1000+ tokens) for optimization suggestions.
- Ensure XML output is well-formed for AI parsing (e.g., Claude compatibility).