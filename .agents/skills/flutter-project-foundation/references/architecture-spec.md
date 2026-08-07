# Architecture & Skeleton Specification

This reference guides the layout and code structure of a new project's skeleton.

---

## 1. Directory Structure

Generate these directories inside `lib/`:

```
lib/
├── core/
│   ├── common/         # Gap widget, global UI models
│   ├── di/             # GetIt locator, AppInitializer
│   ├── errors/         # Failure types, Exception mappings
│   ├── localization/   # Localized strings, ARB managers
│   ├── network/        # Dio config, interceptors, ApiService
│   ├── routing/        # AppRouter, route definitions
│   ├── services/       # StorageService interface + implementation
│   └── theme/          # Color, spacing, typography, ThemeCubit
├── features/           # Feature folders (to be populated later)
│   └── placeholder/
│       └── .gitkeep
└── main.dart           # DI initialization & runApp
```

---

## 2. Core Foundation Implementations

### 2.1 The Gap Widget (`core/common/gap.dart`)
Do NOT use third-party packages. Build it manually:
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

### 2.2 Error Result Pattern (`core/errors/result.dart`)
Implement a lightweight Result wrapper for safe layered exceptions:
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

### 2.3 Dependency Injection & AppInitializer
Register all storage, navigation, theme services, and network instances synchronously before runApp.
```dart
import 'package:get_it/get_it.dart';

final locator = GetIt.instance;

class AppInitializer {
  static Future<void> init() async {
    // 1. Init Local Storage
    // 2. Init Network Clients
    // 3. Init Theme and DI instances
  }
}
```
