# Architecture & Skeleton Specification

This reference guides the layout and code structure of a new project's foundation skeleton.

---

## 1. Directory Structure

Generate directories inside `lib/` strictly matching user selections in Step 1:

```
lib/
├── core/
│   ├── common/         # Dual-directional Gap widget
│   ├── di/             # GetIt locator, AppInitializer
│   ├── errors/         # Failure types, Result<S, F>
│   ├── extensions/     # Modular extensions (keyboard, navigation, media_query, barrel)
│   ├── network/        # (Conditional: Created ONLY if Dio, http, or REST client is selected)
│   ├── routing/        # AppRouter, Routes constants
│   ├── services/       # (Conditional: Created ONLY if SharedPreferences/SecureStorage/Hive is selected)
│   ├── theme/          # Color, spacing, typography tokens (ThemeCubit created ONLY if dual-theme selected)
│   └── utils/          # AppLogger (ALWAYS), AppBlocObserver (if Bloc/Cubit selected)
├── features/           # Empty feature directory
└── main.dart           # DI initialization & runApp ({PascalCaseProjectName}App with DevicePreview)
```

*Note: Do NOT generate `test/` folder or `AI_RULES.md` in project root.*

---

## 2. Core Foundation Implementations

### 2.1 Main Application Setup (`main.dart`)
Configures `MaterialApp` with:
- Dynamic root widget naming `{PascalCaseProjectName}App` derived from project name (never hardcode `MyApp`).
- `device_preview` enabled conditionally (`kIsWeb && kDebugMode`).
- Global keyboard unfocus wrapper (`builder`).
- Smart localization support matching user choice (Arabic/RTL, English, or Multi-Language via official Flutter localization delegates).
- Conditional theme integration (`theme: AppTheme.lightTheme`, omitting `darkTheme` if Light Theme Only was chosen).

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
      // Configured based on user language choice (ar, en, or multi-language delegates)
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

---

### 2.2 Modular Theme Architecture (`core/theme/`)

#### 1. Single Theme Architecture (Light Theme Only):
- `app_colors.dart`: Color palette tokens (`AppColors.primary`, `AppColors.surface`, `AppColors.grey`).
- `app_text_styles.dart`: Typography tokens (`AppTextStyles.font18Bold`, `AppTextStyles.font14Regular`).
- `app_theme.dart`: Composes `AppTheme.lightTheme` using `AppColors` and `AppTextStyles`.

#### 2. Dual Theme Architecture (Light + Dark Theme):
- `app_colors.dart`: Light and Dark color palettes (`AppColors.lightPrimary`, `AppColors.darkPrimary`).
- `app_text_styles.dart`: Adaptive typography tokens.
- `app_theme_extension.dart`: Custom `ThemeExtension<AppThemeExtension>` for dynamic color and asset resolution.
- `theme_cubit.dart`: `ThemeCubit` for dynamic theme toggling and mode persistence.
- `app_theme.dart`: Composes both `AppTheme.lightTheme` and `AppTheme.darkTheme`.

```dart
// core/theme/theme_cubit.dart (Generated when Dual Theme is selected)
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

### 2.6 Modular BuildContext Extensions (`core/extensions/`)

#### `keyboard_extensions.dart`
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

#### `navigation_extensions.dart`
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

#### `media_query_extensions.dart`
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

#### `context_extensions.dart` (Barrel Export)
```dart
export 'keyboard_extensions.dart';
export 'media_query_extensions.dart';
export 'navigation_extensions.dart';
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
