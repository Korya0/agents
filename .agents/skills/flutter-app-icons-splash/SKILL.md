---
name: flutter-app-icons-splash
description: "Use when configuring, replacing, or generating app icons and native splash/launch screens using flutter_launcher_icons and flutter_native_splash packages in a Flutter project; not for project scaffolding (see flutter-project-foundation)."
---
# Flutter App Icons & Splash Screen

Add or update app icons and native splash screens for Flutter apps across targeted platforms (Android, iOS, Web) using standard generators.

## When to Use

Use this skill when:

* The user wants to set up or change the app launcher icon.
* The user wants to build a native splash/launch screen.
* The user says "set up the icons" or "add a splash screen" for an existing Flutter app.

*NOT for general project setup, UI creation, or asset configuration outside icons/splash screens.*

---

## 1. Scope Boundary

| ✅ In Scope | ❌ Out of Scope |
|---|---|
| Launcher icon generation (`flutter_launcher_icons`) | App Store listing assets preparation |
| Native splash screens (`flutter_native_splash`) | Custom Flutter-based onboarding screens |
| Copying branding assets to `assets/` | Modifying core design tokens |
| Executing native generator scripts | CI/CD build scripts |

---

## 2. Bootstrapping Workflow

### Step 1 — Gather Platforms and Assets
Confirm targeted platforms (Android, iOS, Web). Ask for the source asset (a single 1024×1024+ PNG file, or pre-rendered assets from Icon Kitchen). If missing, request the branding files before proceeding.

### Step 2 — Propose a Plan & Wait for Approval
Show the user the proposed generator configurations, package changes, and files to be touched. Do not execute any generator until the user explicitly approves.

### Step 3 — App Icons Generation
Configure `flutter_launcher_icons` in `pubspec.yaml`. Drop the source icon in `assets/icon/icon.png` and run:
```bash
dart run flutter_launcher_icons
```
Verify generated platform asset structures.

### Step 4 — Splash Screen Setup
Configure `flutter_native_splash` in `pubspec.yaml` matching theme token colors. Generate files using:
```bash
dart run flutter_native_splash:create
```

For configuration blueprints of both generators, see **[references/config-templates.md](references/config-templates.md)**.

### Step 5 — Quality Gate & Verification
Run `flutter analyze` to ensure zero static errors. Visually describe changes or show file outputs to allow verification of icon dimensions.

---

## 3. Common Pitfalls

| Pitfall | Fix |
|---|---|
| Generating placeholders silently | Always request real branding assets from the user first |
| Guessed theme colors | Look up the background color token in `core/theme/` |
| Overwriting web manifest | Keep manifest backups or use merging when configuring web icons |

---

## References

- See `flutter-project-foundation` for setting up the initial application directory skeleton.
- See [references/config-templates.md](references/config-templates.md) for full configuration blueprints for Android, iOS, and Web platforms.
