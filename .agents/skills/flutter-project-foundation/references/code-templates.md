# Code Templates Reference

Exact copy-paste implementations for every file the `flutter-project-foundation` skill generates.
Use these as the **source of truth** when the spec in `SKILL.md` is ambiguous.

---

## README.md Template

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

## lib/core/di/app_initializer.dart

> When Bloc/Cubit is selected, include `flutter_bloc` import and `Bloc.observer` line.
> When another state management is selected, omit both.

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

## lib/core/utils/app_logger.dart

> ALWAYS generated. Uses `logger` package.

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

> Add Crashlytics import and real `FirebaseCrashlytics.instance.recordError(...)` call inside `reportToFirebase` only if Firebase was selected in Step 1.

---

## lib/core/utils/app_bloc_observer.dart

> Generated ONLY when Bloc/Cubit is selected.

```dart
import 'dart:async';

import 'package:flutter_bloc/flutter_bloc.dart';
import 'app_logger.dart';

class AppBlocObserver extends BlocObserver {
  @override
  void onCreate(BlocBase<dynamic> bloc) {
    super.onCreate(bloc);
    AppLogger.info('[Bloc Created] ${bloc.runtimeType}');
  }

  @override
  void onEvent(Bloc<dynamic, dynamic> bloc, Object? event) {
    super.onEvent(bloc, event);
    final eventStr = event.toString();
    final truncatedEvent =
        eventStr.length > 200 ? '${eventStr.substring(0, 200)}...' : eventStr;
    AppLogger.info('[Event] ${bloc.runtimeType} -> $truncatedEvent');
  }

  @override
  void onChange(BlocBase<dynamic> bloc, Change<dynamic> change) {
    super.onChange(bloc, change);
    final stateStr = change.nextState.toString();
    final truncatedState =
        stateStr.length > 200 ? '${stateStr.substring(0, 200)}...' : stateStr;
    AppLogger.info('[State Change] ${bloc.runtimeType} -> $truncatedState');
  }

  @override
  void onError(BlocBase<dynamic> bloc, Object error, StackTrace stackTrace) {
    unawaited(AppLogger.reportToFirebase(
      '[BlocError] ${bloc.runtimeType}',
      error: error,
      stackTrace: stackTrace,
    ));
    super.onError(bloc, error, stackTrace);
  }

  @override
  void onClose(BlocBase<dynamic> bloc) {
    super.onClose(bloc);
    AppLogger.info('[Bloc Closed] ${bloc.runtimeType}');
  }
}
```

---

## lib/core/theme/app_sizes.dart

> ALWAYS generated. 4-point grid spacing scale.

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

---

## lib/core/common/gap.dart

> ALWAYS generated. Supports Gap.v() and Gap.h() independently.

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

## lib/core/extensions/keyboard_extensions.dart

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

---

## lib/core/extensions/navigation_extensions.dart

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

---

## lib/core/extensions/media_query_extensions.dart

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

---

## lib/core/extensions/context_extensions.dart (barrel)

```dart
export 'keyboard_extensions.dart';
export 'media_query_extensions.dart';
export 'navigation_extensions.dart';
```

---

## lib/core/theme/theme_cubit.dart

> Generated ONLY when Dual Theme is selected.

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

## main.dart Template

> Replace `{PascalCaseProjectName}` with the actual class name (e.g. `AiFoodDeliveryApp`).
> Adjust localization and theme blocks based on user choices — see SKILL.md § 5.3 rules.

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
      // darkTheme: AppTheme.darkTheme,   // Uncomment ONLY for Dual Theme
      // themeMode: ThemeMode.light,       // Uncomment ONLY for Dual Theme
      initialRoute: Routes.initial,
      onGenerateRoute: AppRouter.onGenerateRoute,
      localizationsDelegates: const [
        GlobalMaterialLocalizations.delegate,
        GlobalWidgetsLocalizations.delegate,
        GlobalCupertinoLocalizations.delegate,
      ],
      supportedLocales: const [
        Locale('ar'), // Adjust per user language choice
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

## analysis_options.yaml

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
