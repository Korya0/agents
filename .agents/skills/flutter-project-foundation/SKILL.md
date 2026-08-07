---
name: flutter-project-foundation
description: "Use when asked to scaffold a brand-new Flutter project, clean template boilerplate, build Clean Architecture directories on disk, configure DI/Dio/local storage/custom Gap widget, set up native routing/localization, or commit/push initial main+develop branches; not day-to-day feature work."
---
# Flutter Project Foundation

Scaffolds a brand-new Flutter project from scratch and executes immediately without a plan-approval step (since no existing code is at stake). This builds the folder skeleton, configures Dependency Injection, Theme Cubits, Networking, Local Storage, custom Gap widget, Native Routing, and commits the initial repository structure.

## When to Use

Use this skill when:

* Starting a brand-new Flutter application.
* Bootstrapping the skeleton directories of a new project.
* Cleaning default counter-app template code.
* Setting up core foundation systems (DI, Dio client, Storage Service, custom Gap widget, Native Routing, l10n) in a new repository.

*NOT for day-to-day feature additions, monitoring setup, or test scaffolding.*

---

## 1. Scope Boundary

This is a bootstrapping skill. Understand what is within this bootstrap step and what must be handled separately:

| ✅ In Scope | ❌ Out of Scope |
|---|---|
| Creating project, package naming | App icons & Splash screen setups |
| Empty Clean Architecture folder skeleton | Unit/Widget/E2E test setup |
| DI configuration (`GetIt`) & initializers | Crashlytics & Analytics monitoring |
| Core networking (`Dio`) & errors package | CI/CD pipeline configuration |
| Custom `Gap` widget | Complex third-party route packages |
| Basic l10n configuration or strings constant | Building full UI pages |
| Local storage setup & Interface | Git hooks & commit conventions |

---

## 2. Step-by-Step Bootstrapping Workflow

### Step 1 — Conversational Setup Questions
Ask the user for:
1. **Project Name & Package Prefix** (e.g. `com.company`).
2. **Target Platforms** (Android, iOS, Web, Desktop).
3. **Repository Setup** (Remote url, branch names).
4. **Localization** (Single language Constants vs full ARB localization).
5. **Theme** (Light-only vs Light + Dark with ThemeCubit).
6. **Local Storage Choice** (SharedPreferences, Hive, SecureStorage, etc.).
7. **Ready-made shared UI** (Yes/No to drop existing widgets).

*Confirm back in a short summary before generating.*

### Step 2 — Specs Review
Read the following templates before creating files:
- **[references/architecture-spec.md](references/architecture-spec.md)**: folder details and core code patterns.
- **[references/ai-rules.md](references/ai-rules.md)**: standard guidelines (copied as `AI_RULES.md`).

### Step 3 — Project Creation & Cleanup
1. Run `flutter create` with `--platforms` flags.
2. Configure Bundle IDs / package names in platform directories.
3. Clean `main.dart` from comments/counters and trim `pubspec.yaml` boilerplate.

### Step 4 — Build Folder Skeleton
Create the folders on disk physically (using empty folders with `.gitkeep` for git tracking). See [references/architecture-spec.md](references/architecture-spec.md) for details.

### Step 5 — Code Generation
Generate foundation files (DI, Networking, Error types, Storage Service, Gap widget, Navigator helper, Theme Cubit if Dark mode requested).

### Step 6 — Repository & Push
Initialize git, commit all staged files, create branches (`main` and `develop`), add origin and push.

---

## 3. Common Pitfalls

| Pitfall | Fix |
|---|---|
| Blocking on plan approval | Start execution directly as no history or code is at stake |
| Adding third-party gap package | Build the Gap widget manually using `SizedBox` |
| Overwriting custom `AI_RULES.md` | Check for conflicts and ask before overwriting |
| Leaving platform directories uncleaned | Remove platform configurations not requested in Step 1 |

---

## References

- See [references/architecture-spec.md](references/architecture-spec.md) for exact directory names and class structures.
- See [references/ai-rules.md](references/ai-rules.md) for the rules copied into `AI_RULES.md` for AI project guidance.
