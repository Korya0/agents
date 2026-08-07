---
name: flutter-project-foundation
description: "Use when asked to scaffold a brand-new Flutter project, clean template boilerplate, prompt for project details, build Clean Architecture directories on disk without tests, configure DI/Dio/Storage/Gap/Context extensions/Native Routing, or generate a minimal badge-driven README; not day-to-day feature work."
---
# Flutter Project Foundation

Scaffolds a brand-new Flutter project from scratch. It prompts for project parameters (project name, github username, target platforms), cleans template boilerplate (removes counter app and test folder), builds the core Clean Architecture skeleton (`lib/core/...`), configures Dependency Injection with `WidgetsFlutterBinding.ensureInitialized()`, native routing extensions, standard BuildContext utilities, custom Gap widget, global keyboard unfocus in `MaterialApp`, conditional `DevicePreview` wrapper (`kIsWeb && kDebugMode`), Arabic/RTL localization setup, custom `analysis_options.yaml` (using `flutter_lints`), optional `ThemeCubit` for Light/Dark mode, and generates a minimal centered badge README.

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
| Prompting user for project & GitHub details | Creating sample/dummy feature folders (e.g. `features/home`) |
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
| Minimal HTML/Badge README template | |

---

## 2. Step-by-Step Bootstrapping Workflow

### Step 1 — Project Information & Parameters Prompt
ALWAYS ask or confirm with the user before creating any files:
1. **Project Name** (e.g., `car_register` — snake_case for Flutter).
2. **Display / Title Name** (e.g., `Car Register` — used in README typing header).
3. **GitHub Username / Org** (e.g., `Korya0`).
4. **Package Prefix / Bundle ID** (e.g., `com.company.carregister`).
5. **Target Platforms** (Android, iOS, Web, Desktop).
6. **Theme Mode** (Light-only vs Light + Dark with `ThemeCubit`).

---

### Step 2 — Project Creation & Cleanup
1. Run `flutter create --org <package_prefix> --platforms <platforms> <project_name>`.
2. Delete `test/` folder completely (`rm -rf test` / `Remove-Item -Recurse test`). Do NOT leave any test directory or dummy test files.
3. Clean `main.dart` from boilerplate comments and counter code.
4. Add `flutter_localizations` to `dependencies` and `device_preview` to `dev_dependencies` in `pubspec.yaml`.
5. Write custom `analysis_options.yaml` (extending `package:flutter_lints/flutter.yaml`). Do NOT install `very_good_analysis`.
6. Do NOT create `AI_RULES.md` in the project root.

---

### Step 3 — Minimal README Generation
Generate `README.md` using EXACTLY this centered, minimal template (substituting `{username}`, `{repo_name}`, and `{Typing+Title+App}`):

```html
<div align="center">

<!-- Badges at Top -->
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

### Step 4 — Build Folder Skeleton
Create the folder hierarchy inside `lib/`:
```
lib/
├── core/
│   ├── common/         # Gap widget
│   ├── di/             # GetIt locator, AppInitializer
│   ├── errors/         # Result<S, F>, Failure types
│   ├── extensions/     # context_extensions.dart (keyboard, safe area, routing)
│   ├── network/        # Dio config, ApiService
│   ├── routing/        # AppRouter, Routes constants
│   ├── services/       # StorageService interface + implementation
│   └── theme/          # Color, spacing, typography tokens, ThemeCubit (if requested)
├── features/           # Empty feature directory
└── main.dart           # AppInitializer.init() & runApp (MyApp with DevicePreview wrapper)
```

---

### Step 5 — Code Generation Rules

#### 5.1 Strict No Comments Policy
Do NOT write comments in generated code (no `// TODO:`, `// Initialize...`, or explanatory comments). Code must be clean, elegant, and self-documenting.

#### 5.2 Dependency Injection & AppInitializer
Inside `lib/core/di/app_initializer.dart`:
```dart
import 'package:flutter/material.dart';
import 'package:get_it/get_it.dart';

final locator = GetIt.instance;

class AppInitializer {
  static Future<void> init() async {
    WidgetsFlutterBinding.ensureInitialized();
  }
}
```

#### 5.3 Main Application Setup with DevicePreview & Global Unfocus (`main.dart`)
```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';
import 'package:flutter_localizations/flutter_localizations.dart';
import 'package:device_preview/device_preview.dart';
import 'core/di/app_initializer.dart';
import 'core/routing/app_router.dart';
import 'core/routing/routes.dart';
import 'core/theme/app_theme.dart';

void main() async {
  await AppInitializer.init();
  runApp(
    kIsWeb && kDebugMode
        ? DevicePreview(
            builder: (context) => const MyApp(),
          )
        : const MyApp(),
  );
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'App Title',
      theme: AppTheme.lightTheme,
      darkTheme: AppTheme.darkTheme,
      themeMode: ThemeMode.light,
      initialRoute: Routes.initial,
      onGenerateRoute: AppRouter.onGenerateRoute,
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
          onTap: () => FocusManager.instance.primaryFocus?.unfocus(),
          child: previewChild ?? const SizedBox.shrink(),
        );
      },
    );
  }
}
```

#### 5.4 ThemeCubit for Light + Dark Mode (`lib/core/theme/theme_cubit.dart`)
Generated when user requests dual-theme support:
```dart
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

#### 5.5 BuildContext Extensions (`lib/core/extensions/context_extensions.dart`)
Generate context utilities natively without third-party dependencies:
```dart
import 'package:flutter/material.dart';

extension ContextExtensions on BuildContext {
  void dismissKeyboard() {
    if (FocusScope.of(this).hasFocus) {
      FocusScope.of(this).unfocus();
    }
  }

  double topSafe() {
    final view = MediaQueryData.fromView(View.of(this));
    return view.padding.top;
  }

  double bottomSafe() {
    final view = MediaQueryData.fromView(View.of(this));
    return view.padding.bottom;
  }

  EdgeInsets get bottomInsetPadding {
    return EdgeInsets.only(bottom: MediaQuery.of(this).viewInsets.bottom);
  }

  EdgeInsets get modalBottomKeyboard => EdgeInsets.only(
        bottom: MediaQueryData.fromView(View.of(this)).viewInsets.bottom,
      );

  Future<T?> pushNamed<T>(String routeName, {Object? arguments}) {
    return Navigator.of(this).pushNamed<T>(routeName, arguments: arguments);
  }

  Future<T?> pushReplacementNamed<T, TO>(String routeName,
      {Object? arguments, TO? result}) {
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
// BAD — Direct runApp without DevicePreview conditional check or global unfocus wrapper
void main() async {
  runApp(const MaterialApp(home: HomeScreen()));
}

// GOOD — Enclosed with AppInitializer, DevicePreview for kIsWeb && kDebugMode, and global unfocus gesture detector
void main() async {
  await AppInitializer.init();
  runApp(
    kIsWeb && kDebugMode
        ? DevicePreview(
            builder: (context) => const MyApp(),
          )
        : const MyApp(),
  );
}
```

---

## 4. Common Pitfalls

| Pitfall | Fix |
|---|---|
| Assuming project name without asking user | ALWAYS prompt user for Project Name, GitHub username, and target parameters |
| Generating `test/` directory | Delete `test/` folder immediately after running `flutter create` |
| Overriding Git author with dummy name/email | Ensure git commits use authentic `git config user.name` and `git config user.email` without injecting `--author` flags |
| Installing `very_good_analysis` package | Use standard `flutter_lints` and write the custom `analysis_options.yaml` provided |
| Forgetting `DevicePreview` conditional wrap | Use `kIsWeb && kDebugMode` to wrap `DevicePreview` in `main.dart` and `MaterialApp` |
| Forgetting Global Unfocus in `MaterialApp` | Always wrap `child` inside `MaterialApp.builder` with a `GestureDetector` that unfocuses `primaryFocus` |
| Omitting `flutter_localizations` for Arabic | Include `GlobalMaterialLocalizations` and `supportedLocales: [Locale('ar')]` |
| Creating dummy feature code (`features/home`) | Keep `features/` directory clean; only build foundation |
| Adding boilerplate code comments | Enforce strict no-comments policy |
| Creating `AI_RULES.md` in root | Skip creating `AI_RULES.md` in the project root |
| Complex README files | Use only the simple centered badge & typing SVG README template |

---

## References

- See [references/architecture-spec.md](references/architecture-spec.md) for full folder layout and core class structures.
