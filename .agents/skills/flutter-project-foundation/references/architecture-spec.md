# Architecture & Skeleton Specification

This reference guides the layout and code structure of a new project's foundation skeleton.

---

## 1. Directory Structure

Generate these directories inside `lib/`:

```
lib/
├── core/
│   ├── common/         # Gap widget
│   ├── di/             # GetIt locator, AppInitializer
│   ├── errors/         # Failure types, Result<S, F>
│   ├── extensions/     # context_extensions.dart (keyboard, safe area, routing)
│   ├── network/        # Dio config, ApiService
│   ├── routing/        # AppRouter, Routes constants
│   ├── services/       # StorageService interface + implementation
│   └── theme/          # Color, spacing, typography tokens, ThemeCubit (if Light+Dark requested)
├── features/           # Empty feature directory
└── main.dart           # DI initialization & runApp (MyApp with DevicePreview wrapper)
```

*Note: Do NOT generate `test/` folder or `AI_RULES.md` in project root.*

---

## 2. Core Foundation Implementations

### 2.1 Main Application Setup (`main.dart`)
Configures `MaterialApp` with:
- `device_preview` enabled conditionally (`kIsWeb && kDebugMode`).
- Global keyboard unfocus wrapper (`builder`).
- Arabic / RTL localization support (`flutter_localizations`).
- `ThemeCubit` integration for Light/Dark theme switching if selected.

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

---

### 2.2 Theme Management & ThemeCubit (`core/theme/theme_cubit.dart`)
Generated when user requests Light + Dark theme toggle support:

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

---

### 2.3 The Gap Widget (`core/common/gap.dart`)
```dart
import 'package:flutter/material.dart';

class Gap extends StatelessWidget {
  const Gap({
    super.key,
    this.vertical = 0,
    this.horizontal = 0,
  });

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

---

### 2.4 Error Result Pattern (`core/errors/result.dart`)
```dart
sealed class Result<S, F extends Failure> {
  const Result();
}

class Success<S, F extends Failure> extends Result<S, F> {
  const Success(this.value);
  final S value;
}

class FailureResult<S, F extends Failure> extends Result<S, F> {
  const FailureResult(this.failure);
  final F failure;
}

abstract class Failure {
  const Failure(this.message);
  final String message;
}
```

---

### 2.5 Dependency Injection & AppInitializer (`core/di/app_initializer.dart`)
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

---

### 2.6 BuildContext Extensions (`core/extensions/context_extensions.dart`)
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

---

### 2.7 Native Routing (`core/routing/`)

#### `routes.dart`
```dart
abstract class Routes {
  static const String initial = '/';
}
```

#### `app_router.dart`
```dart
import 'package:flutter/material.dart';
import 'routes.dart';

class AppRouter {
  static Route<dynamic>? onGenerateRoute(RouteSettings settings) {
    switch (settings.name) {
      case Routes.initial:
        return MaterialPageRoute(
          builder: (_) => const Scaffold(
            body: Center(
              child: Text('App Initialized'),
            ),
          ),
        );
      default:
        return MaterialPageRoute(
          builder: (_) => Scaffold(
            body: Center(
              child: Text('No route defined for ${settings.name}'),
            ),
          ),
        );
    }
  }
}
```

---

### 2.8 Analysis Options Specification (`analysis_options.yaml`)
Do NOT install `very_good_analysis`. Generate `analysis_options.yaml` with:

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
