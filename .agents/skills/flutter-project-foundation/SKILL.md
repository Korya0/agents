---
name: flutter-project-foundation
description: "Use when asked to scaffold a brand-new Flutter project, clean template boilerplate, prompt or infer project details (inferring repo name, package name, and animated title from project/pubspec), build Clean Architecture directories on disk without tests, configure DI/Dio/Storage/Gap/Context extensions/Native Routing, or generate a minimal badge-driven README with mandatory HTML shields; not day-to-day feature work."
---
# Flutter Project Foundation

Scaffolds a brand-new Flutter project from scratch. It prompts for or infers project parameters (project name, github username, target platforms), cleans template boilerplate (removes counter app and test folder), builds the core Clean Architecture skeleton (`lib/core/...`), configures Dependency Injection with `WidgetsFlutterBinding.ensureInitialized()`, native routing extensions, standard BuildContext utilities, custom Gap widget, global keyboard unfocus in `MaterialApp`, conditional `DevicePreview` wrapper (`kIsWeb && kDebugMode`), Arabic/RTL localization setup, custom `analysis_options.yaml` (using `flutter_lints`), optional `ThemeCubit` for Light/Dark mode, and generates a minimal centered badge README with mandatory HTML shields and an auto-inferred animated typing header.

## When to Use

Use this skill when:

* Starting a brand-new Flutter application.
* Bootstrapping skeleton directories for a new project.
* Cleaning default counter-app template code and removing default `test/` folder.
* Setting up core foundation systems (DI, Dio client, Storage Service, Gap widget, Context Extensions, Native Routing, Global Unfocus, DevicePreview, Arabic Localizations, Analysis Options) in a new repository.

*NOT for day-to-day feature additions, monitoring setup, writing feature tests, or creating AI_RULES.md.*

---

## 1. Scope Boundary

This is a bootstrapping skill. Understand what is within this bootstrap step and what must be handled separately:

| ✅ In Scope | ❌ Out of Scope |
|---|---|
| Auto-inferring/Prompting user for project & GitHub details | Creating sample/dummy feature folders (e.g. `features/home`) |
| Empty Clean Architecture folder skeleton | Scaffolding `test/` directory or writing unit/widget tests |
| DI initialization (`GetIt`) & `WidgetsFlutterBinding.ensureInitialized()` | Creating `AI_RULES.md` in root directory |
| Core networking (`Dio`) & `Result<S, F>` pattern | Adding third-party routing or analysis packages (`very_good_analysis`) |
| Custom `Gap` widget & `BuildContext` extensions | Adding boilerplate comments in code |
| Native Flutter routing (`AppRouter` + extensions) | App icons & Splash screens |
| Global Keyboard Unfocus in `MaterialApp.builder` | CI/CD pipelines & monitoring |
| `DevicePreview` integration in debug (`kIsWeb && kDebugMode`) | Building full UI feature screens |
| Arabic/RTL Localization delegates (`flutter_localizations`) | Complex state management for business logic |
| Standard `analysis_options.yaml` (extending `flutter_lints`) | Git hooks & automated release pipelines |
| `ThemeCubit` for Light + Dark theme toggle (when requested) | Injecting dummy Git author names/emails in commits |
| Mandatory HTML/Badge README template with auto-inferred typing title | |

---

## 2. Step-by-Step Bootstrapping Workflow

### Step 1 — Comprehensive Interactive Prompting & Parameter Inference
When scaffolding a project foundation, ask or confirm with the user a set of targeted, diverse questions to fully customize the foundation setup according to their needs:

> [!IMPORTANT]
> **MANDATORY NETWORK & STORAGE PROMPTING RULE**: NEVER generate `lib/core/network/` or `lib/core/services/` automatically. You MUST ask the user first for their explicit preference for Network and Storage layers, and strictly respect their choice (including `None / Skip`).

1. **Project Identification & Repository Meta**:
   - **Project Name / Repo Name**: (snake_case, e.g. `ai_food_delivery` — auto-inferred from `pubspec.yaml` `name:` field or folder name if present).
   - **App Display Title**: (Title Case, e.g. `Ai Food Delivery` — used in README typing header).
   - **GitHub Username / Org**: (e.g., `Korya0` — auto-inferred from `git remote -v` or `git config user.name`, prompt if unavailable).
   - **Package Prefix / Bundle ID**: (e.g., `com.korya0.aifooddelivery`).
   - **Target Platforms**: (Android, iOS, Web, Desktop).

2. **Network Client & Backend Choice**:
   - **REST API (Dio)**: Configures `Dio`, base options, custom logging interceptors, and `ApiService` in `lib/core/network/`.
   - **REST API (http)**: Lightweight native HTTP client wrapper in `lib/core/network/`.
   - **Firebase**: Configures Firebase packages (`firebase_core`, `cloud_firestore`, `firebase_auth`, etc.).
   - **Supabase**: Configures `supabase_flutter` backend setup.
   - **None / Skip**: DO NOT create `lib/core/network/` folder until explicitly requested.

3. **Local Storage Solution**:
   - **shared_preferences**: Standard key-value persistence in `lib/core/services/storage_service.dart`.
   - **flutter_secure_storage**: Encrypted storage for auth tokens and sensitive credentials.
   - **hive / isar**: Fast NoSQL local database for offline caching.
   - **Firebase Firestore Persistence**: Configures Firestore offline caching capabilities.
   - **None / Skip**: DO NOT create local storage implementations or `lib/core/services/` until explicitly requested.

4. **Localization & Language Strategy**:
   - **Arabic / RTL Only**: (Default: `supportedLocales: [Locale('ar')]`).
   - **English Only**: (`supportedLocales: [Locale('en')]`).
   - **Dual / Multi-Language (Arabic + English)**: Configures multi-language delegates and locale switching.

5. **Theme System & Appearance**:
   - **Light Theme Only**: Custom `AppTheme.lightTheme` with curated palette.
   - **Dual Theme (Light + Dark)**: Adds `ThemeCubit` for dynamic mode toggling.
   - **Primary Seed Color**: Asks for preferred brand color (e.g., `#1E293B`, `#0F766E`, `#6366F1`).

6. **State Management System**:
   - **Flutter Bloc / Cubit**: (Default Clean Architecture pattern).
   - **Riverpod**: Configures `ProviderScope` in `main.dart`.
   - **Provider / ChangeNotifier**: Configures native provider tree wrapper.

---

### Step 2 — Project Creation & Mandatory Cleanup
1. Run `flutter create --org <package_prefix> --platforms <platforms> <project_name>` (if creating a new directory).

> [!IMPORTANT]
> **MANDATORY TEST & BOILERPLATE CLEANUP RULE**: You MUST completely remove the `test/` folder and the default `test/widget_test.dart` file immediately after `flutter create` (using `Remove-Item -Recurse -Force test` or `rm -rf test`). DO NOT leave any `test/` directory or dummy test files.

2. Delete `test/` folder completely (`Remove-Item -Recurse -Force test` / `rm -rf test`).
3. Clean `main.dart` from boilerplate comments, counter app, and default stateful widget.
4. Add required dependencies to `pubspec.yaml` based on the user's answers in Step 1 (`flutter_localizations`, chosen network/storage packages, and `device_preview` in `dev_dependencies`).
5. Write custom `analysis_options.yaml` (extending `package:flutter_lints/flutter.yaml`). Do NOT install `very_good_analysis`.
6. Do NOT create `AI_RULES.md` in the project root.

---

### Step 3 — Minimal README Generation & Mandatory Badges
Infer `{repo_name}` from `pubspec.yaml` (e.g., `ai_food_delivery`), `{username}` from Git config/remote (e.g., `Korya0`), and `{Formatted+App+Name}` by title-casing the project name (e.g., `ai_food_delivery` -> `Ai+Food+Delivery` or `AI+Food+Delivery`).

> [!IMPORTANT]
> **MANDATORY BADGES RULE**: DO NOT omit or delete the HTML badge block (`<p>...</p>`) under any circumstances. You MUST generate the exact badge section below in every `README.md`.

Generate `README.md` using EXACTLY this centered, minimal template:

```html
<div align="center">

<!-- Badges at Top (MUST NOT BE DELETED OR OMITTED) -->
<p>
  <a href="https://github.com/{username}/{repo_name}/graphs/contributors">
    <img src="https://img.shields.io/github/contributors/{username}/{repo_name}" alt="contributors" />
  </a>
  <a href="https://github.com/{username}/{repo_name}/commits/main">
    <img src="https://img.shields.io/github/last-commit/{username}/{repo_name}" alt="last update" />
  </a>
  <a href="https://github.com/{username}/{repo_name}/stargazers">
    <img src="https://img.shields.io/github/stars/{username}/{repo_name}" alt="stars" />
  </a>
  <a href="https://github.com/{username}/{repo_name}/blob/main/LICENSE">
    <img src="https://img.shields.io/github/license/{username}/{repo_name}" alt="license" />
  </a>
</p>

<!-- Typing Logo -->
<img src="https://readme-typing-svg.herokuapp.com/?font=Inter&weight=800&size=50&center=true&vCenter=true&width=600&height=100&duration=4000&lines={Formatted+App+Name}+App"/>

</div>
```

---

### Step 4 — Build Conditional Folder Skeleton
Create the folder hierarchy inside `lib/` strictly matching user selections in Step 1:
```
lib/
├── core/
│   ├── common/         # Dual-directional Gap widget
│   ├── di/             # GetIt locator, AppInitializer
│   ├── errors/         # Result<S, F>, Failure types
│   ├── extensions/     # Modular extensions (keyboard, navigation, media_query, barrel)
│   ├── network/        # (Conditional: Created ONLY if Dio, http, or REST client is selected)
│   ├── routing/        # AppRouter, Routes constants
│   ├── services/       # (Conditional: Created ONLY if SharedPreferences/SecureStorage/Hive is selected)
│   ├── theme/          # Color, spacing, typography tokens (ThemeCubit created ONLY if dual-theme selected)
│   └── utils/          # AppLogger (ALWAYS), AppBlocObserver (if Bloc/Cubit selected)
├── features/           # Empty feature directory
└── main.dart           # AppInitializer.init() & runApp ({PascalCaseProjectName}App with DevicePreview)
```

---

### Step 5 — Code Generation Rules

#### 5.1 Strict No Comments Policy
Do NOT write comments in generated code (no `// TODO:`, `// Initialize...`, or explanatory comments). Code must be clean, elegant, and self-documenting.

#### 5.2 Dependency Injection & AppInitializer
Inside `lib/core/di/app_initializer.dart`:

> [!IMPORTANT]
> **MANDATORY**: Always wire `Bloc.observer = AppBlocObserver()` inside `AppInitializer.init()` when Bloc/Cubit is the chosen state management system.

```dart
import 'package:flutter/material.dart';
import 'package:flutter_bloc/flutter_bloc.dart';
import 'package:get_it/get_it.dart';
import '../utils/app_bloc_observer.dart';

final locator = GetIt.instance;

class AppInitializer {
  static Future<void> init() async {
    WidgetsFlutterBinding.ensureInitialized();
    Bloc.observer = AppBlocObserver();
  }
}
```

---

#### 5.3 AppLogger — Always Mandatory Utility (`lib/core/utils/app_logger.dart`)

> [!IMPORTANT]
> **ALWAYS GENERATED** regardless of network/storage/state choices. This is a core foundation utility. Generate WITHOUT any doc comments or explanatory comments.

```dart
import 'package:flutter/foundation.dart';
import 'package:logger/logger.dart';

class AppLogger {
  static final Logger _logger = Logger(
    printer: PrettyPrinter(
      dateTimeFormat: DateTimeFormat.onlyTimeAndSinceStart,
    ),
  );

  static void info(String message) {
    if (kDebugMode) _logger.i(message);
  }

  static Future<void> warn(
    String message, {
    Object? error,
    StackTrace? stackTrace,
  }) async {
    if (kDebugMode) {
      _logger.w(message, error: error, stackTrace: stackTrace);
    }
  }

  static void debug(String message) {
    if (kDebugMode) _logger.d(message);
  }

  static void success(String message) {
    if (kDebugMode) _logger.i('✅ SUCCESS: $message');
  }

  static Future<void> localError(
    String message, {
    Object? error,
    StackTrace? stackTrace,
  }) async {
    if (kDebugMode) {
      _logger.e(message, error: error, stackTrace: stackTrace);
    }
  }

  static Future<void> error(
    String message, {
    Object? error,
    StackTrace? stackTrace,
  }) async {
    await localError(message, error: error, stackTrace: stackTrace);
  }

  static Future<void> reportToFirebase(
    String message, {
    Object? error,
    StackTrace? stackTrace,
  }) async {
    if (kDebugMode) {
      _logger.e('🔥 TO FIREBASE: $message', error: error, stackTrace: stackTrace);
    }
  }
}
```

> **Note**: Add Crashlytics/Firebase integration to `reportToFirebase` only if Firebase was selected in Step 1.

---

#### 5.4 AppBlocObserver — Mandatory When Bloc/Cubit is Selected (`lib/core/utils/app_bloc_observer.dart`)

> [!IMPORTANT]
> **Generated ONLY when Bloc/Cubit state management is selected**. Wired in `AppInitializer` via `Bloc.observer = AppBlocObserver()`. Generate WITHOUT any doc comments or explanatory comments.

```dart
import 'dart:async';

import 'package:flutter_bloc/flutter_bloc.dart';
import 'app_logger.dart';

class AppBlocObserver extends BlocObserver {
  @override
  void onCreate(BlocBase<dynamic> bloc) {
    super.onCreate(bloc);
    AppLogger.info('[Bloc Created] \${bloc.runtimeType}');
  }

  @override
  void onEvent(Bloc<dynamic, dynamic> bloc, Object? event) {
    super.onEvent(bloc, event);
    final eventStr = event.toString();
    final truncatedEvent =
        eventStr.length > 200 ? '\${eventStr.substring(0, 200)}...' : eventStr;
    AppLogger.info('[Event] \${bloc.runtimeType} -> \$truncatedEvent');
  }

  @override
  void onChange(BlocBase<dynamic> bloc, Change<dynamic> change) {
    super.onChange(bloc, change);
    final stateStr = change.nextState.toString();
    final truncatedState =
        stateStr.length > 200 ? '\${stateStr.substring(0, 200)}...' : stateStr;
    AppLogger.info('[State Change] \${bloc.runtimeType} -> \$truncatedState');
  }

  @override
  void onError(BlocBase<dynamic> bloc, Object error, StackTrace stackTrace) {
    unawaited(AppLogger.reportToFirebase(
      '[BlocError] \${bloc.runtimeType}',
      error: error,
      stackTrace: stackTrace,
    ));
    super.onError(bloc, error, stackTrace);
  }

  @override
  void onClose(BlocBase<dynamic> bloc) {
    super.onClose(bloc);
    AppLogger.info('[Bloc Closed] \${bloc.runtimeType}');
  }
}
```

#### 5.3 Main Application Setup with DevicePreview & Global Unfocus (`main.dart`)

**Naming Rule**: DO NOT use generic `MyApp`. Infer the root widget class name in PascalCase from the project name plus `App` suffix (e.g., `ai_food_delivery` -> `AiFoodDeliveryApp`).

**Theme Rule**:
- **Light Theme Only**: DO NOT generate `darkTheme:` or `themeMode:`. Generate ONLY `theme: AppTheme.lightTheme`.
- **Dual Theme (Light + Dark)**: Include `darkTheme: AppTheme.darkTheme` and `themeMode: ThemeMode.light` (or state-driven via `ThemeCubit`).

**Localization Rule**:
- **Arabic Only**: Use `localizationsDelegates`, `supportedLocales: const [Locale('ar')]`, and default `locale: const Locale('ar')` for RTL layout support.
- **English Only**: Use `supportedLocales: const [Locale('en')]` and `locale: const Locale('en')`.
- **Multi-Language (Arabic + English)**: Use official Flutter localization delegates (e.g., `AppLocalizations.delegate` / `GlobalMaterialLocalizations`) and `supportedLocales: const [Locale('ar'), Locale('en')]` to allow dynamic locale switching.

```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';
import 'package:flutter_localizations/flutter_localizations.dart';
import 'package:device_preview/device_preview.dart';
import 'core/di/app_initializer.dart';
import 'core/extensions/context_extensions.dart';
import 'core/routing/app_router.dart';
import 'core/routing/routes.dart';
import 'core/theme/app_theme.dart';

void main() async {
  await AppInitializer.init();
  runApp(
    kIsWeb && kDebugMode
        ? DevicePreview(
            builder: (context) => const {PascalCaseProjectName}App(),
          )
        : const {PascalCaseProjectName}App(),
  );
}

class {PascalCaseProjectName}App extends StatelessWidget {
  const {PascalCaseProjectName}App({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: '{App Display Title}',
      theme: AppTheme.lightTheme,
      // Omit darkTheme & themeMode if user chose Light Theme Only
      initialRoute: Routes.initial,
      onGenerateRoute: AppRouter.onGenerateRoute,
      // Configured based on user language choice in Step 1
      localizationsDelegates: const [
        GlobalMaterialLocalizations.delegate,
        GlobalWidgetsLocalizations.delegate,
        GlobalCupertinoLocalizations.delegate,
      ],
      supportedLocales: const [
        Locale('ar'),
      ],
      locale: kIsWeb && kDebugMode
          ? DevicePreview.locale(context)
          : const Locale('ar'),
      builder: (context, child) {
        final previewChild = kIsWeb && kDebugMode
            ? DevicePreview.appBuilder(context, child)
            : child;
        return GestureDetector(
          onTap: () => context.dismissKeyboard(),
          child: previewChild ?? const SizedBox.shrink(),
        );
      },
    );
  }
}
```

#### 5.4 Modular Theme Architecture (`lib/core/theme/`)

##### 1. Single Theme Setup (Light Theme Only):
Separate design tokens into dedicated files according to responsibility:
- `app_colors.dart`: Color palette tokens (`AppColors.primary`, `AppColors.surface`, `AppColors.grey`, etc.).
- `app_text_styles.dart`: Typography tokens (`AppTextStyles.font18Bold`, `AppTextStyles.font14Regular`, etc.).
- `app_sizes.dart`: **Mandatory** spacing scale, border radius, icon sizes based on 4-point grid (see below).
- `app_theme.dart`: Composes `AppTheme.lightTheme` using `AppColors`, `AppTextStyles`, and `AppSizes`.

##### 2. Dual Theme Setup (Light + Dark Theme):
- `app_colors.dart`: Light and Dark color palettes (`AppColors.lightPrimary`, `AppColors.darkPrimary`, etc.).
- `app_text_styles.dart`: Adaptive typography tokens.
- `app_sizes.dart`: **Mandatory** — same 4-point spacing/radius system (see below). Shared across both themes.
- `app_theme_extension.dart`: Custom `ThemeExtension<AppThemeExtension>` for dynamic theme colors and asset resolution.
- `theme_cubit.dart`: `ThemeCubit` for dynamic theme toggling and mode persistence.
- `app_theme.dart`: Composes both `AppTheme.lightTheme` and `AppTheme.darkTheme`.

```dart
// lib/core/theme/theme_cubit.dart (Generated when Dual Theme is selected)
import 'package:flutter/material.dart';
import 'package:flutter_bloc/flutter_bloc.dart';

class ThemeCubit extends Cubit<ThemeMode> {
  ThemeCubit() : super(ThemeMode.light);

  void toggleTheme() {
    emit(state == ThemeMode.light ? ThemeMode.dark : ThemeMode.light);
  }

  void setThemeMode(ThemeMode mode) {
    emit(mode);
  }
}
```

##### 3. AppSizes — Mandatory Sizing Token System (`lib/core/theme/app_sizes.dart`)

> [!IMPORTANT]
> **ALWAYS GENERATED** regardless of theme choice (single or dual). This is the only source of truth for spacing, border radius, and icon sizes. NEVER use raw numeric literals (`16`, `8.0`, `BorderRadius.circular(12)`) anywhere in widget code — always reference `AppSizes`.

> **No off-scale values rule** (from design-tokens best practice): If a design specifies a value that doesn't exist in the scale (e.g. 14px spacing when the scale is 4/8/12/16), **DO NOT add a special token**. Stop and report it to the user — it is almost always a mistake in the design file. Silently encoding it makes the scale meaningless.

```dart
abstract class AppSizes {
  // ── Spacing Scale (4-point grid) ─────────────────────────
  static const double s2 = 2;
  static const double s4 = 4;
  static const double s6 = 6;
  static const double s8 = 8;
  static const double s10 = 10;
  static const double s12 = 12;
  static const double s14 = 14;
  static const double s16 = 16;
  static const double s20 = 20;
  static const double s24 = 24;
  static const double s28 = 28;
  static const double s32 = 32;
  static const double s40 = 40;
  static const double s48 = 48;
  static const double s56 = 56;
  static const double s64 = 64;
  static const double s80 = 80;

  // ── Border Radius ─────────────────────────────────────────
  static const double radiusXs = 4;
  static const double radiusSm = 8;
  static const double radiusMd = 12;
  static const double radiusLg = 16;
  static const double radiusXl = 24;
  static const double radiusXxl = 32;
  static const double radiusCircle = 1000;

  // ── Icon Sizes ────────────────────────────────────────────
  static const double iconXs = 14;
  static const double iconSm = 18;
  static const double iconMd = 24;
  static const double iconLg = 32;
  static const double iconXl = 40;

  // ── Minimum Touch Target (accessibility) ─────────────────
  static const double minTouchTarget = 48;
}
```

#### 5.5 Dual-Directional Gap Widget (`lib/core/common/gap.dart`)
Generate a Gap widget that supports vertical (`Gap.v`) and horizontal (`Gap.h`) spacing independently so it never forces unwanted square constraints in flex layouts:

```dart
import 'package:flutter/material.dart';

class Gap extends StatelessWidget {
  const Gap({
    super.key,
    this.vertical = 0,
    this.horizontal = 0,
  });

  const Gap.v(double size, {super.key})
      : vertical = size,
        horizontal = 0;

  const Gap.h(double size, {super.key})
      : vertical = 0,
        horizontal = size;

  final double vertical;
  final double horizontal;

  @override
  Widget build(BuildContext context) {
    return SizedBox(
      height: vertical > 0 ? vertical : null,
      width: horizontal > 0 ? horizontal : null,
    );
  }
}
```

#### 5.6 Modular BuildContext Extensions (`lib/core/extensions/`)
Separate context extensions into dedicated files by single responsibility:

##### 1. Keyboard Extensions (`lib/core/extensions/keyboard_extensions.dart`)
```dart
import 'package:flutter/material.dart';

extension KeyboardExtensions on BuildContext {
  void dismissKeyboard() {
    if (FocusScope.of(this).hasFocus) {
      FocusScope.of(this).unfocus();
    }
  }

  EdgeInsets get bottomInsetPadding {
    return EdgeInsets.only(bottom: MediaQuery.of(this).viewInsets.bottom);
  }

  EdgeInsets get modalBottomKeyboard {
    return EdgeInsets.only(
      bottom: MediaQueryData.fromView(View.of(this)).viewInsets.bottom,
    );
  }
}
```

##### 2. Navigation Extensions (`lib/core/extensions/navigation_extensions.dart`)
```dart
import 'package:flutter/material.dart';

extension NavigationExtensions on BuildContext {
  Future<T?> pushNamed<T>(String routeName, {Object? arguments}) {
    return Navigator.of(this).pushNamed<T>(routeName, arguments: arguments);
  }

  Future<T?> pushReplacementNamed<T, TO>(
    String routeName, {
    Object? arguments,
    TO? result,
  }) {
    return Navigator.of(this).pushReplacementNamed<T, TO>(
      routeName,
      result: result,
      arguments: arguments,
    );
  }

  void pop<T>([T? result]) {
    Navigator.of(this).pop<T>(result);
  }

  Future<T?> pushNamedAndRemoveUntil<T>(
    String newRouteName,
    bool Function(Route<dynamic>) predicate, {
    Object? arguments,
  }) {
    return Navigator.of(this).pushNamedAndRemoveUntil<T>(
      newRouteName,
      predicate,
      arguments: arguments,
    );
  }
}
```

##### 3. Media Query / Safe Area Extensions (`lib/core/extensions/media_query_extensions.dart`)
```dart
import 'package:flutter/material.dart';

extension MediaQueryExtensions on BuildContext {
  double topSafe() {
    final view = MediaQueryData.fromView(View.of(this));
    return view.padding.top;
  }

  double bottomSafe() {
    final view = MediaQueryData.fromView(View.of(this));
    return view.padding.bottom;
  }

  double get screenHeight => MediaQuery.sizeOf(this).height;
  double get screenWidth => MediaQuery.sizeOf(this).width;
}
```

##### 4. Extensions Barrel File (`lib/core/extensions/context_extensions.dart`)
```dart
export 'keyboard_extensions.dart';
export 'media_query_extensions.dart';
export 'navigation_extensions.dart';
```

#### 5.6 Standard Analysis Options (`analysis_options.yaml`)
Do NOT add `very_good_analysis`. Generate `analysis_options.yaml` with:
```yaml
include: package:flutter_lints/flutter.yaml

formatter:
  trailing_commas: preserve

analyzer:
  exclude:
    - "**/*.g.dart"
    - "**/*.freezed.dart"

  errors:
    invalid_annotation_target: ignore

linter:
  rules:
    - require_trailing_commas
    - prefer_single_quotes
    - prefer_const_declarations
    - prefer_const_constructors
    - prefer_const_literals_to_create_immutables
    - unnecessary_this
    - prefer_final_locals
    - omit_local_variable_types
```

---

### Step 6 — Repository Setup & Git Author Safeguard
1. Run `git init` in project root.
2. Verify local git user configuration (`git config user.name` and `git config user.email`).
3. NEVER pass custom `--author` flags or override author credentials with fake/dummy emails during `git commit`. Use the user's authentic local/global git config only.

---

## 3. BAD vs GOOD Code Patterns

### Initializing App with DevicePreview & Global Unfocus Wrapper
```dart
// BAD — Direct runApp, hardcoded MyApp name, no DevicePreview, no BlocObserver
void main() async {
  runApp(const MaterialApp(home: HomeScreen()));
}

// GOOD — AppInitializer wires BlocObserver, DevicePreview for kIsWeb && kDebugMode
// Root widget named from project: ai_food_delivery -> AiFoodDeliveryApp
void main() async {
  await AppInitializer.init();
  runApp(
    kIsWeb && kDebugMode
        ? DevicePreview(
            builder: (context) => const AiFoodDeliveryApp(),
          )
        : const AiFoodDeliveryApp(),
  );
}
```

---

## 4. Common Pitfalls

| Pitfall | Fix |
|---|---|
| Assuming project details without checking | Infer parameters from `pubspec.yaml` and `git config` when available, or prompt user |
| Generating network/ or services/ without asking | NEVER auto-generate — ALWAYS ask user for Network and Storage preference first |
| Omitting or deleting HTML badge shields (`<p>...</p>`) in README | ALWAYS include the full HTML shields badge block in `README.md` |
| Hardcoding typing SVG header in README | Auto-infer and title-case `{Formatted+App+Name}` from project name |
| Generating `test/` directory | Delete `test/` and `widget_test.dart` immediately after `flutter create` |
| Using `MyApp` as root widget name | ALWAYS PascalCase from project name + `App` suffix (e.g. `AiFoodDeliveryApp`) |
| Hardcoding raw numeric values in widget code | Use `AppSizes.s16`, `AppSizes.radiusMd` — NEVER raw `16`, `EdgeInsets.all(16)`, `BorderRadius.circular(12)` |
| Adding off-scale spacing value to `AppSizes` | STOP and report to user — almost always a design file mistake |
| Skipping `AppLogger` | ALWAYS generate `lib/core/utils/app_logger.dart` regardless of choices |
| Skipping `AppBlocObserver` when using Cubit | ALWAYS generate and wire via `Bloc.observer = AppBlocObserver()` in `AppInitializer` |
| Generating `darkTheme` when user chose Light Only | Omit `darkTheme:` and `themeMode:` entirely when Light Theme Only is selected |
| Generating Localization delegates without asking | ALWAYS ask about language strategy — Arabic/English/Multi BEFORE generating |
| Overriding Git author with dummy name/email | Use authentic `git config user.name` and `git config user.email` without `--author` flags |
| Installing `very_good_analysis` package | Use standard `flutter_lints` and the provided custom `analysis_options.yaml` |
| Forgetting Global Unfocus in `MaterialApp` | Wrap `child` in `GestureDetector` calling `context.dismissKeyboard()` in `MaterialApp.builder` |
| Creating dummy feature code (`features/home`) | Keep `features/` empty — foundation only |
| Adding boilerplate code comments | Enforce strict no-comments policy |
| Creating `AI_RULES.md` in root | Skip — not part of this skill's scope |

---

## References

- See [references/architecture-spec.md](references/architecture-spec.md) for full folder layout and core class structures.
