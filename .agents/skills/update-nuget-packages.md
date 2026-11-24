---
name: update-nuget-packages
description: Audit all NuGet packages across a .NET solution and safely update them one by one to the latest versions compatible with each project's Target Framework Moniker (TFM), compiling after every individual update and automatically reverting any package that breaks the build.
---

# NuGet Solution Package Audit & Atomic Compatibility Update

## Purpose

Systematically inspect every project in a .NET solution (`.sln` or `.slnx`), identify outdated NuGet packages, resolve the highest version compatible with each project's target `.NET` version(s), and apply updates **one package at a time**—compiling the solution after each update and automatically reverting to the initial version if the build fails.

## Prerequisites & Guardrails

1. **Clean Working Tree:** Verify `git status` is clean before modifying files so individual package updates and rollbacks can be cleanly isolated. If uncommitted changes exist, warn the user and ask for confirmation before proceeding.
2. **Respect Target Frameworks:** Never upgrade a package to a major version that drops support for any `<TargetFramework>` or `<TargetFrameworks>` declared in the consuming project(s) (e.g., `netstandard2.0`, `net8.0`, `net9.0`, `net10.0`).
3. **Avoid Unintended Pre-Releases:** Do not upgrade stable packages to `-preview`, `-rc`, `-alpha`, or `-beta` versions unless the package is *already* referencing a pre-release version or the user explicitly requests pre-releases.
4. **Preserve Pinning & Comments:** If a `<PackageReference>` or `<PackageVersion>` has an inline XML comment explaining why it is pinned (e.g., `<!-- pinned due to breaking change -->`), skip it and report it in the summary.
5. **Strict One-by-One Isolation:** Never batch multiple package version edits together before compiling. Every candidate package must be updated, restored, and compiled independently before moving on to the next package.

---

## Step-by-Step Execution Workflow

### Step 1: Discover Solution Structure & Package Management Mode

1. Locate the `.sln` or `.slnx` file in the repository root (or target directory).
2. Check for **Central Package Management (CPM)** by looking for `Directory.Packages.props` in the root or parent directories and checking if `<ManagePackageVersionsCentrally>true</ManagePackageVersionsCentrally>` is enabled.
3. Check for shared MSBuild properties in `Directory.Build.props` (such as shared `<TargetFramework>`, `<TargetFrameworks>`, or common `<PackageReference>` items).
4. Inspect every `.csproj` in the solution to build a complete inventory of:
   - Project path
   - Target Framework(s) (`TargetFramework` / `TargetFrameworks`)
   - Direct NuGet dependencies and their **initial versions** (or `PackageVersion` entries if CPM is active)

### Step 2: Baseline Restore & Build

Run baseline checks before touching any versions so pre-existing failures are not mistaken for upgrade regressions:

```bash
dotnet restore
dotnet build --no-restore --configuration Release
