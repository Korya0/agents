---
name: flutter-project-foundation
description: "Use when asked to scaffold a brand-new Flutter project, clean template boilerplate, prompt or infer project details (repo name, package name, animated title), build Clean Architecture directories, configure DI/optional network/storage/Gap/Context extensions/Native Routing, or generate a minimal badge-driven README with mandatory HTML shields; not day-to-day feature work."
---
# Flutter Project Foundation

Scaffolds a brand-new Flutter project from scratch. Prompts for or infers project parameters, cleans boilerplate, builds the Clean Architecture skeleton, wires mandatory utilities (AppLogger, AppSizes, Gap, BuildContext extensions), applies conditional layers (network, storage, state management, theme), and generates a centered badge README.

> **Reference files** — Read these when you need exact implementations:
> - [references/code-templates.md](references/code-templates.md) — Complete dart/yaml source for every generated file.
> - [references/architecture-spec.md](references/architecture-spec.md) — Full folder layout and class structures.

## When to Use

Use this skill when:

* Starting a brand-new Flutter application from scratch.
* Bootstrapping skeleton directories and core foundation for a new project.
* Cleaning the default counter-app template code and removing the default `test/` folder.
* Setting up DI, Gap widget, BuildContext extensions, AppLogger, AppSizes, AppBlocObserver, Native Routing, Global Unfocus, DevicePreview, Localization, and Analysis Options in a new repository.
* Generating a minimal centered badge README with mandatory HTML shields and an auto-inferred animated typing header.
* Choosing and wiring state management (Cubit, Riverpod, Provider) and optional network/storage layers.

*NOT for day-to-day feature additions, monitoring setup, writing feature tests, or creating AI_RULES.md.*

---

## 1. Scope Boundary

| ✅ In Scope | ❌ Out of Scope |
|---|---|
| Auto-inferring/Prompting for project & GitHub details | Sample/dummy feature folders (`features/home`) |
| Empty Clean Architecture folder skeleton | Scaffolding `test/` directory or writing tests |
| DI initialization (`GetIt`) & `AppInitializer` | Creating `AI_RULES.md` in root directory |
| Mandatory `AppLogger` utility (always) | Adding `very_good_analysis` package |
| Mandatory `AppSizes` token system (always) | App icons & Splash screens |
| Conditional `AppBlocObserver` (Cubit/Bloc only) | CI/CD pipelines & monitoring |
| Custom `Gap` widget & `BuildContext` extensions | Building full UI feature screens |
| Native Flutter routing (`AppRouter` + extensions) | Complex business logic state management |
| Global Keyboard Unfocus in `MaterialApp.builder` | Git hooks & automated release pipelines |
| `DevicePreview` integration (`kIsWeb && kDebugMode`) | Injecting dummy Git author names in commits |
| Conditional network layer (only when user asks) | |
| Conditional storage layer (only when user asks) | |
| Conditional Theme architecture (single or dual) | |
| Mandatory HTML/Badge README with typing header | |

---

## 2. Step-by-Step Bootstrapping Workflow

### Step 1 — Infer First, Then Ask Once

**Infer silently (no questions):**
- Project/repo name → from `pubspec.yaml` `name:` field or folder name.
- GitHub username/org → from `git remote -v` or `git config user.name`.
- Root widget class name → PascalCase of project name + `App` suffix (e.g. `ai_food_delivery` → `AiFoodDeliveryApp`).
- Formatted README title → title-case + `+` separators (e.g. `AI+Food+Delivery`).

**Ask once in a single grouped message — these 6 decisions require explicit user input:**

1. **App Display Title** — Human-readable title for README (if not obvious from project name).
2. **Package Prefix / Bundle ID** — e.g. `com.company.appname`.
3. **Target Platforms** — Android, iOS, Web, Desktop (default: Android + iOS).
4. **Network & Backend** — Choose one: `Dio (REST)` / `http (REST)` / `Firebase` / `Supabase` / `None — skip`.
5. **Local Storage** — Choose one: `shared_preferences` / `flutter_secure_storage` / `Hive/Isar` / `Firestore offline` / `None — skip`.
6. **Language Strategy** — Choose one: `Arabic only (RTL)` / `English only` / `Multi-language`.
7. **Theme System** — Choose one: `Light theme only` / `Dual theme (Light + Dark)` + brand seed color.
8. **State Management** — Choose one: `Bloc/Cubit (recommended)` / `Riverpod` / `Provider`.

**Confirm before executing:**
Show a one-line summary — e.g. _"Creating: Dio + SharedPrefs + Arabic RTL + Light Theme + Cubit — proceed?"_ — and wait for approval.

> [!IMPORTANT]
> **NEVER auto-generate `lib/core/network/` or `lib/core/services/`** without explicit user selection. `None — skip` means those folders do NOT exist at all.

---

### Step 2 — Project Creation & Mandatory Cleanup

1. Run `flutter create --org <package_prefix> --platforms <platforms> <project_name>`.

> [!IMPORTANT]
> **MANDATORY**: Immediately after `flutter create`, delete the `test/` folder entirely (`Remove-Item -Recurse -Force test` / `rm -rf test`). No test files or test directory should remain.

2. Remove boilerplate: comments, counter app, default `MyHomePage`, default `StatefulWidget` from `main.dart`.
3. Add all required dependencies to `pubspec.yaml` (based on Step 1 answers): `flutter_localizations`, `get_it`, `logger`, `device_preview` (dev), chosen network/storage/state packages.
4. Write `analysis_options.yaml` — see [references/code-templates.md → analysis_options.yaml](references/code-templates.md). Do NOT install `very_good_analysis`.
5. Do NOT create `AI_RULES.md`.

---

### Step 3 — Minimal README Generation

Infer `{repo_name}`, `{username}`, `{Formatted+App+Name}` from git/pubspec.

> [!IMPORTANT]
> **MANDATORY BADGES RULE**: ALWAYS generate the HTML badge block (`<p>...</p>`). NEVER omit or delete it. See [references/code-templates.md → README.md Template](references/code-templates.md).

---

### Step 4 — Build Conditional Folder Skeleton

```
lib/
├── core/
│   ├── common/         # Gap widget (ALWAYS)
│   ├── di/             # GetIt locator, AppInitializer (ALWAYS)
│   ├── errors/         # Result<S, F>, Failure types (ALWAYS)
│   ├── extensions/     # keyboard, navigation, media_query, barrel (ALWAYS)
│   ├── network/        # ⚠️ CONDITIONAL — only if user selected Dio/http/REST
│   ├── routing/        # AppRouter, Routes constants (ALWAYS)
│   ├── services/       # ⚠️ CONDITIONAL — only if user selected SharedPrefs/Hive/etc.
│   ├── theme/          # app_colors, app_text_styles, app_sizes, app_theme (ALWAYS)
│   └── utils/          # AppLogger (ALWAYS), AppBlocObserver (Cubit/Bloc only)
├── features/           # Empty — do NOT add sample features
└── main.dart
```

---

### Step 5 — Code Generation Rules

#### 5.1 No Comments Policy
NEVER write comments in generated code — no `// TODO:`, no `// Initialize…`, no section headers. Code must be self-documenting.

---

#### 5.2 AppInitializer (`lib/core/di/app_initializer.dart`)

- Top-level `final locator = GetIt.instance;`
- Single static `init()` async method.
- Always calls `WidgetsFlutterBinding.ensureInitialized()`.
- **If Bloc/Cubit selected**: also sets `Bloc.observer = AppBlocObserver()` and imports `flutter_bloc` + `app_bloc_observer.dart`.
- **If other state management**: omit the observer line entirely.

See [references/code-templates.md → app_initializer.dart](references/code-templates.md) for exact code.

---

#### 5.3 AppLogger (`lib/core/utils/app_logger.dart`) — ALWAYS

> [!IMPORTANT]
> **Always generated** regardless of any other choices.

- Static class using `logger` package with `PrettyPrinter(dateTimeFormat: DateTimeFormat.onlyTimeAndSinceStart)`.
- Methods (all guarded by `if (kDebugMode)`):
  - `info(String)` → `_logger.i`
  - `debug(String)` → `_logger.d`
  - `warn(String, {error, stackTrace})` → `_logger.w` (async)
  - `success(String)` → `_logger.i('✅ SUCCESS: $message')`
  - `localError(String, {error, stackTrace})` → `_logger.e` (async)
  - `error(String, {error, stackTrace})` → delegates to `localError` (async)
  - `reportToFirebase(String, {error, stackTrace})` → `_logger.e('🔥 TO FIREBASE: $message')` (async) — add real Crashlytics call only if Firebase was selected.

See [references/code-templates.md → app_logger.dart](references/code-templates.md) for exact code.

---

#### 5.4 AppBlocObserver (`lib/core/utils/app_bloc_observer.dart`) — Bloc/Cubit only

> [!IMPORTANT]
> **Generated only when Bloc/Cubit is the chosen state management.**

- Extends `BlocObserver`.
- Imports `dart:async` and `app_logger.dart`.
- Overrides:
  - `onCreate` → `AppLogger.info('[Bloc Created] ${bloc.runtimeType}')`
  - `onEvent` → truncates event string to 200 chars, logs `[Event] Type -> truncated`
  - `onChange` → truncates next state to 200 chars, logs `[State Change] Type -> truncated`
  - `onError` → calls `unawaited(AppLogger.reportToFirebase('[BlocError] ${bloc.runtimeType}', ...))`
  - `onClose` → `AppLogger.info('[Bloc Closed] ${bloc.runtimeType}')`

See [references/code-templates.md → app_bloc_observer.dart](references/code-templates.md) for exact code.

---

#### 5.5 main.dart

**Naming Rule**: Root widget = PascalCase(project name) + `App`. Example: `ai_food_delivery` → `AiFoodDeliveryApp`. NEVER use `MyApp`.

**Theme conditional**:
- Light only → `theme: AppTheme.lightTheme` only. Do NOT generate `darkTheme:` or `themeMode:`.
- Dual theme → add `darkTheme: AppTheme.darkTheme` and `themeMode: ThemeMode.light`.

**Localization conditional**:
- Arabic only → `supportedLocales: [Locale('ar')]`, `locale: Locale('ar')`, include `GlobalMaterialLocalizations` + `GlobalWidgetsLocalizations` + `GlobalCupertinoLocalizations` delegates (needed for RTL directionality).
- English only → `supportedLocales: [Locale('en')]`, `locale: Locale('en')`. No localization delegates needed unless explicitly requested.
- Multi-language → both locales, use `AppLocalizations.delegate` for string switching.

**Always**:
- `DevicePreview` conditional: `kIsWeb && kDebugMode`.
- `MaterialApp.builder` wraps child in `GestureDetector(onTap: () => context.dismissKeyboard(), ...)`.
- `DevicePreview.appBuilder(context, child)` called inside builder when `kIsWeb && kDebugMode`.

See [references/code-templates.md → main.dart Template](references/code-templates.md) for exact code.

---

#### 5.6 Theme Architecture (`lib/core/theme/`)

**Always generated (both single and dual theme):**
- `app_colors.dart` — color palette constants (`AppColors.primary`, `AppColors.surface`, etc.).
- `app_text_styles.dart` — typography constants (`AppTextStyles.font18Bold`, etc.).
- `app_sizes.dart` — **mandatory** sizing tokens (see below).
- `app_theme.dart` — composes `AppTheme.lightTheme` using `AppColors`, `AppTextStyles`.

**Dual theme only (additional files):**
- `app_theme_extension.dart` — `ThemeExtension<AppThemeExtension>` for dynamic color/asset resolution.
- `theme_cubit.dart` — `ThemeCubit extends Cubit<ThemeMode>` with `toggleTheme()` and `setThemeMode(ThemeMode)`.

See [references/code-templates.md → theme_cubit.dart](references/code-templates.md) for exact code.

---

#### 5.7 AppSizes Token System (`lib/core/theme/app_sizes.dart`) — ALWAYS

> [!IMPORTANT]
> **Always generated.** NEVER write raw numeric literals in widget code. Always reference `AppSizes`.

- `abstract class AppSizes` with only `static const double` members, no methods.
- **Spacing (4-point grid)**: `s2=2, s4=4, s6=6, s8=8, s10=10, s12=12, s14=14, s16=16, s20=20, s24=24, s28=28, s32=32, s40=40, s48=48, s56=56, s64=64, s80=80`.
- **Border Radius**: `radiusXs=4, radiusSm=8, radiusMd=12, radiusLg=16, radiusXl=24, radiusXxl=32, radiusCircle=1000`.
- **Icon Sizes**: `iconXs=14, iconSm=18, iconMd=24, iconLg=32, iconXl=40`.
- **Accessibility**: `minTouchTarget=48`.

> **No off-scale values rule**: If a design uses a value not in this scale (e.g. 14px spacing), **stop and report** to the user — do NOT add a one-off token. It is almost always a design mistake.

See [references/code-templates.md → app_sizes.dart](references/code-templates.md) for exact code.

---

#### 5.8 Gap Widget (`lib/core/common/gap.dart`) — ALWAYS

- `Gap extends StatelessWidget` with two named constructors: `Gap.v(double size)` and `Gap.h(double size)`.
- Default constructor takes optional `vertical` and `horizontal` both defaulting to 0.
- `build` returns `SizedBox(height: vertical > 0 ? vertical : null, width: horizontal > 0 ? horizontal : null)`.
- NEVER use a single square `SizedBox` for both axes — use `Gap.v()` for vertical spacing in `Column`, `Gap.h()` for horizontal spacing in `Row`.

See [references/code-templates.md → gap.dart](references/code-templates.md) for exact code.

---

#### 5.9 BuildContext Extensions (`lib/core/extensions/`) — ALWAYS

Three separate files by single responsibility + one barrel export:

| File | Extension name | Methods |
|---|---|---|
| `keyboard_extensions.dart` | `KeyboardExtensions` | `dismissKeyboard()`, `bottomInsetPadding`, `modalBottomKeyboard` |
| `navigation_extensions.dart` | `NavigationExtensions` | `pushNamed<T>`, `pushReplacementNamed<T,TO>`, `pop<T>`, `pushNamedAndRemoveUntil<T>` |
| `media_query_extensions.dart` | `MediaQueryExtensions` | `topSafe()`, `bottomSafe()`, `screenHeight`, `screenWidth` |
| `context_extensions.dart` | (barrel) | `export` all three above |

- Always use `context.dismissKeyboard()` — NEVER call `FocusManager.instance.primaryFocus?.unfocus()` directly.
- Always use `MediaQuery.sizeOf(context)` — NEVER `MediaQuery.of(context).size` (avoids full rebuilds on keyboard open).

See [references/code-templates.md → extensions](references/code-templates.md) for exact code.

---

#### 5.10 Analysis Options (`analysis_options.yaml`)

- Extends `package:flutter_lints/flutter.yaml`.
- `formatter: trailing_commas: preserve`.
- Excludes `**/*.g.dart` and `**/*.freezed.dart`.
- Ignores `invalid_annotation_target` errors.
- Linter rules: `require_trailing_commas`, `prefer_single_quotes`, `prefer_const_declarations`, `prefer_const_constructors`, `prefer_const_literals_to_create_immutables`, `unnecessary_this`, `prefer_final_locals`, `omit_local_variable_types`.

See [references/code-templates.md → analysis_options.yaml](references/code-templates.md) for exact yaml.

---

### Step 6 — Repository Setup & Git Author Safeguard

1. Run `git init` in project root.
2. Verify `git config user.name` and `git config user.email` — both must be set.
3. NEVER pass `--author` flags or inject dummy emails in `git commit`. Use the user's authentic local/global git config only.

---

### Step 7 — Initial Commit

Commit with `git add .` then `git commit -m "chore: initialize project foundation"` using the user's authentic git identity.

---

## 3. BAD vs GOOD Code Patterns

```dart
// BAD — Generic MyApp name, raw runApp, no BlocObserver wiring
void main() async {
  runApp(const MyApp());
}

// GOOD — PascalCase from project name, AppInitializer wires BlocObserver, DevicePreview conditional
// Project: ai_food_delivery → AiFoodDeliveryApp
void main() async {
  await AppInitializer.init();
  runApp(
    kIsWeb && kDebugMode
        ? DevicePreview(builder: (context) => const AiFoodDeliveryApp())
        : const AiFoodDeliveryApp(),
  );
}
```

```dart
// BAD — Raw numeric padding and color, hardcoded sizing
Container(
  padding: const EdgeInsets.all(16),
  decoration: BoxDecoration(
    color: const Color(0xFF1E293B),
    borderRadius: BorderRadius.circular(12),
  ),
)

// GOOD — All values through AppSizes and AppColors tokens
Container(
  padding: const EdgeInsets.all(AppSizes.s16),
  decoration: BoxDecoration(
    color: AppColors.surface,
    borderRadius: BorderRadius.circular(AppSizes.radiusMd),
  ),
)
```

```dart
// BAD — Square SizedBox forced on both axes, breaks Column/Row constraints
SizedBox(height: 16, width: 16)

// GOOD — Directional Gap widgets
Gap.v(AppSizes.s16)  // vertical only — use in Column
Gap.h(AppSizes.s16)  // horizontal only — use in Row
```

```dart
// BAD — Global FocusManager call instead of context extension
GestureDetector(onTap: () => FocusManager.instance.primaryFocus?.unfocus())

// GOOD — Use context extension
GestureDetector(onTap: () => context.dismissKeyboard())
```

---

## 4. Common Pitfalls

| Pitfall | Fix |
|---|---|
| Assuming project details without checking | Infer from `pubspec.yaml` + `git config`, prompt only for unknowns |
| Auto-generating network/ or services/ | NEVER — always ask user first; `None` means folder does NOT exist |
| Using `MyApp` as root widget name | Always PascalCase from project name + `App` (e.g. `AiFoodDeliveryApp`) |
| Hardcoding raw numeric values in widget code | Use `AppSizes.s16`, `AppSizes.radiusMd` — never `16` or `BorderRadius.circular(12)` |
| Adding an off-scale value to AppSizes | Stop and report to user — almost always a design file mistake |
| Skipping AppLogger | Always generate `lib/core/utils/app_logger.dart` regardless of choices |
| Skipping AppBlocObserver when Cubit selected | Always generate and wire `Bloc.observer = AppBlocObserver()` in AppInitializer |
| Using square `Gap(size)` for both axes | Use `Gap.v()` in Column, `Gap.h()` in Row — never force both axes |
| Generating `darkTheme:` when Light Only chosen | Omit `darkTheme:` and `themeMode:` entirely — `theme:` only |
| Adding localization delegates without asking language | Ask language strategy before generating any localization code |
| Generating `test/` directory | Delete immediately after `flutter create` — no test files should remain |
| Overriding Git author with dummy email | Use authentic `git config user.name/email` — never `--author` flag |
| Installing `very_good_analysis` | Use standard `flutter_lints` + provided `analysis_options.yaml` |
| Forgetting Global Unfocus in MaterialApp | Always wrap child in `GestureDetector` calling `context.dismissKeyboard()` |
| Calling `FocusManager.instance.primaryFocus?.unfocus()` | Use `context.dismissKeyboard()` — the extension is always available |
| Calling `MediaQuery.of(context).size` | Use `MediaQuery.sizeOf(context)` to avoid unnecessary rebuilds |
| Omitting HTML badge shields from README | Always include the full `<p>...</p>` badge block — it MUST NOT be deleted |
| Creating dummy feature code in `features/` | Keep `features/` empty — foundation only |
| Adding comments to generated code | Strict no-comments policy — code must be self-documenting |
| Creating `AI_RULES.md` in root | Out of scope for this skill |

---

## References

- [references/code-templates.md](references/code-templates.md) — Exact dart/yaml source for every generated file.
- [references/architecture-spec.md](references/architecture-spec.md) — Full folder layout and class structures.
