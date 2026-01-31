# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build Commands

```bash
# Restore dependencies (uses custom NuGet sources in nuget.config)
dotnet restore

# Build all projects (BepInEx 5 + IL2CPP)
dotnet build ConfigurationManager.sln

# Build specific versions
dotnet build ConfigurationManager/ConfigurationManager.csproj           # BepInEx 5 (net35)
dotnet build ConfigurationManager.IL2CPP/ConfigurationManager.IL2CPP.csproj  # BepInEx 6 (net6.0)

# Release build (creates zip files in bin/)
dotnet build ConfigurationManager.sln -c Release
```

Output directories:
- `bin/BepInEx5/` - BepInEx 5 build (Mono)
- `bin/IL2CPP/` - BepInEx 6 build (IL2CPP)

## Architecture

This is an in-game configuration UI plugin for BepInEx that provides two builds from shared code:

**Project Structure:**
- `ConfigurationManager.Shared/` - Core logic shared via MSBuild shared project (.shproj)
- `ConfigurationManager/` - BepInEx 5 build (net35, defines `Mono`)
- `ConfigurationManager.IL2CPP/` - BepInEx 6 build (net6.0, defines `IL2CPP`)
- `BepInEx.KeyboardShortcut/` - Keyboard input library for IL2CPP only

**Key Shared Files:**
- `ConfigurationManager.cs` - Main plugin class, IMGUI window rendering
- `SettingFieldDrawer.cs` - Renders individual settings (sliders, dropdowns, inputs)
- `SettingEntryBase.cs` / `ConfigSettingEntry.cs` - Setting wrapper classes
- `SettingSearcher.cs` - Discovers plugin configs (separate implementations per version)

**Conditional Compilation:**
Use `#if IL2CPP` / `#if Mono` for platform-specific code in shared files.

## Version Management

Master version is in `Directory.Build.props`. This file also:
- Generates `Constants.cs` with version/metadata at build time
- Creates release zip files on Release builds
