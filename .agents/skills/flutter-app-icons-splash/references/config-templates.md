# Configurations Blueprint for Icons & Splash

Authoritative configuration shapes for `flutter_launcher_icons`, `flutter_native_splash`, and custom splash feature setup inside `pubspec.yaml`.

> **Platform sections are conditional** — only include sections for platforms detected in the project (see SKILL.md § 2).

---

## 1. App Icons Configuration

Add `flutter_launcher_icons` under `dev_dependencies` and configure:

### Full Config (all platforms)

```yaml
dev_dependencies:
  flutter_launcher_icons: ^0.14.3 # Under `# App Icons` comment

flutter_launcher_icons:
  android: "launcher_icon"
  ios: true
  image_path: "assets/icon/icon.png"
  # Adaptive icon (Android only) — uncomment if needed:
  # adaptive_icon_background: "#ffffff"
  # adaptive_icon_foreground: "assets/icon/icon_foreground.png"
  web:
    generate: true
    image_path: "assets/icon/icon.png"
    background_color: "#ffffff"
    theme_color: "#ffffff"
  macos:
    generate: true
    image_path: "assets/icon/icon.png"
  windows:
    generate: true
    image_path: "assets/icon/icon.png"
    icon_size: 48
  linux:
    generate: true
    image_path: "assets/icon/icon.png"
```

### Minimal Config (Android + iOS only)

```yaml
flutter_launcher_icons:
  android: "launcher_icon"
  ios: true
  image_path: "assets/icon/icon.png"
```

---

## 2. Native Splash Screen Configuration

Add `flutter_native_splash` under `dev_dependencies` and configure:

### Light Mode Only

```yaml
dev_dependencies:
  flutter_native_splash: ^2.4.6 # Under `# Splash Screen` comment

flutter_native_splash:
  color: "#ffffff"              # Use project theme background token value
  image: assets/splash/splash_logo.png
  android_12:
    image: assets/splash/splash_logo.png
    color: "#ffffff"
  ios: true
  web:
    generate: true
    image: assets/splash/splash_logo.png
    color: "#ffffff"
```

### Light + Dark Mode

```yaml
flutter_native_splash:
  color: "#ffffff"
  color_dark: "#121212"
  image: assets/splash/splash_logo.png
  image_dark: assets/splash/splash_logo_dark.png  # Optional separate dark logo

  android_12:
    image: assets/splash/splash_logo.png
    color: "#ffffff"
    image_dark: assets/splash/splash_logo_dark.png
    color_dark: "#121212"

  ios: true
  web:
    generate: true
    image: assets/splash/splash_logo.png
    color: "#ffffff"
    image_dark: assets/splash/splash_logo_dark.png
    color_dark: "#121212"
```

### Platform Conditional Rules

| Detected Platform | Config Key | Notes |
|---|---|---|
| Android | `android_12:` block | Always include for Android 12+ branding |
| iOS | `ios: true` | No additional config needed |
| Web | `web: generate: true` | Include `image` and `color` under `web:` |
| macOS | Not natively supported | Skip — `flutter_native_splash` doesn't support macOS |
| Windows | Not natively supported | Skip |
| Linux | Not natively supported | Skip |

---

## 3. Custom Splash Feature Setup

### 3.1 — `flutter_animate` Dependency

Add to `pubspec.yaml` following the project's existing dependency grouping:

```yaml
dependencies:
  # Animations
  flutter_animate: ^4.5.2
```

> Place under an existing UI/Animations group if one exists, or create a `# Animations` group. Follow the project's convention.

---

### 3.2 — Feature Structure

```
lib/features/splash/
├── presentation/
│   ├── views/
│   │   └── splash_view.dart
│   └── widgets/
│       └── (sub-widgets as needed)
```

### 3.3 — Key Implementation Notes

- **Colors**: Import and use `AppColors` — never hardcode hex values.
- **Sizes**: Import and use `AppSizes` — never hardcode numeric values.
- **Typography**: Import and use `AppTextStyles` — never use raw `TextStyle`.
- **Animations**: Use `flutter_animate` — never use manual `AnimationController` for splash animations.
- **Navigation**: Read the app's context extensions and router — use the **exact same** `context.xxx()` method the app uses.
- **Duration**: Default to **2 seconds** if user doesn't specify.
- **Assets**: Reuse existing assets from `assets/` when possible.

### 3.4 — Example Splash View Skeleton

```dart
import 'package:flutter/material.dart';
import 'package:flutter_animate/flutter_animate.dart';
import 'package:app_name/core/theme/app_colors.dart';
import 'package:app_name/core/theme/app_sizes.dart';
import 'package:app_name/core/theme/app_text_styles.dart';
import 'package:app_name/core/extensions/context_extensions.dart';

class SplashView extends StatefulWidget {
  const SplashView({super.key});

  @override
  State<SplashView> createState() => _SplashViewState();
}

class _SplashViewState extends State<SplashView> {
  @override
  void initState() {
    super.initState();
    _navigateAfterDelay();
  }

  Future<void> _navigateAfterDelay() async {
    await Future.delayed(const Duration(seconds: 2));
    if (!mounted) return;
    context.pushReplacementNamed(Routes.home);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: AppColors.background,
      body: Center(
        child: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            Image.asset(
              'assets/icon/icon.png',
              width: AppSizes.s80,
              height: AppSizes.s80,
            ).animate().fadeIn(
              duration: 1500.ms,
              curve: Curves.easeInOut,
            ),
          ],
        ),
      ),
    );
  }
}
```

> [!IMPORTANT]
> This is a **skeleton only**. The actual implementation must:
> - Follow the user's description/specs
> - Use the app's real design tokens
> - Match the app's existing navigation pattern exactly
> - Pass `flutter analyze` with 0 errors, 0 warnings, 0 infos
> - Contain no comments inside the Dart files (only within pubspec.yaml)
