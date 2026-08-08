---
name: flutter-app-icons-splash
description: "Use when configuring, replacing, or generating app icons and native splash/launch screens using flutter_launcher_icons and flutter_native_splash packages in a Flutter project; not for project scaffolding (see flutter-project-foundation)."
---
# Flutter App Icons & Splash Screen

Add or update app icons, native splash screens, and optionally create a custom in-app splash feature for Flutter apps. Platforms are **auto-detected** from the project structure.

## When to Use

Use this skill when:

* The user wants to set up or change the app launcher icon.
* The user wants to build a native splash/launch screen.
* The user wants a custom in-app splash screen feature with animations and branding.
* The user says "set up the icons" or "add a splash screen" for an existing Flutter app.

*NOT for general project setup, UI creation, or asset configuration outside icons/splash screens.*

---

## 1. Scope Boundary

| ✅ In Scope | ❌ Out of Scope |
|---|---|
| Launcher icon generation (`flutter_launcher_icons`) | App Store listing assets preparation |
| Native splash screens (`flutter_native_splash`) | Custom Flutter-based onboarding screens |
| Custom in-app splash feature (optional) | Modifying core design tokens |
| Copying branding assets to `assets/` | CI/CD build scripts |
| Executing native generator scripts | |
| Auto-detecting target platforms from project | |

---

## 2. Auto Platform Detection

**NEVER guess, assume, or use default platforms.** You must dynamically detect which platforms are targeted by inspecting the project root folder. Target **only** the platforms that have their corresponding directory present:

| Directory | Target Platform |
|---|---|
| `android/` | Android |
| `ios/` | iOS |
| `web/` | Web |
| `linux/` | Linux |
| `macos/` | macOS |
| `windows/` | Windows |

Immediately print the detected platforms to the user:
> _"Detected target platforms from project directories: Android, iOS. Configurations will be generated strictly for these platforms."_

If no platform folders are found, stop execution immediately and report an error to the user asking them to run the tool from a valid Flutter project root.

---

## 3. Smart Bootstrapping Workflow

### Step 1 — Project Pre-Scan Analysis (MANDATORY First Step)

Before asking the user ANY questions, the agent must **silently inspect and analyze the project**. The agent must look for:
1. **Target Platforms**: Scan the project root for folders like `android/`, `ios/`, `web/`, etc. (No default/fallback assumptions, no asking).
2. **Routing & Context Extensions**: Look for navigation files (e.g. `lib/core/routing/`, `lib/core/extensions/context_extensions.dart`) to identify the exact navigation method the project uses (e.g. `context.pushReplacementNamed()`, `context.goNamed()`).
3. **Design System & Theme Tokens**: Scan `lib/core/theme/` or theme configuration files to discover background colors, seed colors, `AppColors` tokens, and `AppSizes` tokens.
4. **Branding Assets**: Look inside `assets/` or `assets/splash/` / `assets/icon/` to check if logos already exist, preventing asking for paths if they are standard.
5. **Existing Dependencies**: Inspect `pubspec.yaml` to check if `flutter_animate`, `flutter_launcher_icons`, or `flutter_native_splash` are already installed.

### Step 2 — Ask Intelligent & Context-Aware Questions

Once the analysis is complete, present the detected findings to the user and ask **only** what could not be inferred or needs choices. Group all questions into **a single message**:

---

**Findings Log (Print First):**
> 🔍 **Pre-Scan Findings:**
> - **Detected Platforms:** Android, iOS
> - **Detected Navigation Pattern:** Native navigation (`context.pushReplacementNamed()`)
> - **Detected Design Colors:** Background: `#FFFFFF` (from `AppColors.background`)
> - **Detected Assets:** Found logo at `assets/icon/icon.png`
> - **Dependencies:** `flutter_animate` already installed

---

**Questions Flow (Conditional on Scan Results):**

* **Task 1 — App Icon (Enabled by Default, user can choose to skip):**
  1. **Source icon asset** — (Ask only if no clear candidate found in `assets/icon/`) Do you want to use the existing `assets/icon/icon.png` or provide a new path?
  2. **Adaptive icon** (Android only) — Does the icon need an adaptive background? If yes, provide color/image.
  3. **Icon rounding** (iOS) — Use default iOS rounding or provide custom mask?

* **Task 2 — Native Splash Screen (Enabled by Default, user can choose to skip):**
  1. **Splash logo image** — (Ask only if no candidate found in `assets/splash/`) Path to splash logo PNG.
  2. **Background color** — Use the detected background color (e.g., `#FFFFFF`) or specify a custom hex?
  3. **Dark mode support** — Yes / No. If Yes: ask for dark background color and dark logo.

* **Task 3 — Custom Splash Feature (Optional):**
  1. **Create a custom in-app splash screen?** — Yes / No.
     - If **Yes**:
       - Choose **duration**: `2 seconds (default)` / `3 seconds` / `Custom`.
       - Describe the style or special widgets wanted.

---

**Confirm before executing:**
Show a summary of all collected answers and wait for explicit approval.

---

### Step 3 — Propose a Plan & Wait for Approval

Show the user the proposed generator configurations, package changes, files to be touched, and (if Task 3 is selected) the splash feature architecture. Do not execute anything until the user explicitly approves.

---

### Step 3 — Execute Task 1: App Icons Generation

1. Add `flutter_launcher_icons` to `dev_dependencies` in `pubspec.yaml`.
2. Drop the source icon in `assets/icon/icon.png`.
3. Configure `flutter_launcher_icons` section in `pubspec.yaml` targeting only the **detected** platforms.
4. Run:
   ```bash
   dart run flutter_launcher_icons
   ```
5. Verify generated platform asset structures.

For configuration blueprints, see **[references/config-templates.md → App Icons](references/config-templates.md)**.

---

### Step 4 — Execute Task 2: Native Splash Screen

1. Add `flutter_native_splash` to `dev_dependencies` in `pubspec.yaml`.
2. Drop the splash logo in `assets/splash/splash_logo.png`.
3. Configure `flutter_native_splash` section in `pubspec.yaml`:
   - Use the user-provided or theme-extracted background color.
   - If dark mode: add `color_dark`, `image_dark`, and `android_12` dark variants.
   - Target only the **detected** platforms.
4. Run:
   ```bash
   dart run flutter_native_splash:create
   ```

For configuration blueprints, see **[references/config-templates.md → Native Splash](references/config-templates.md)**.

---

### Step 5 — Execute Task 3: Custom Splash Feature (If Selected)

> [!IMPORTANT]
> Only execute this step if the user opted in during Step 1.

#### 5.1 — Analyze the Existing App

Before writing any code, **thoroughly** read and understand:

1. **Routing system** — Find the app's router (`AppRouter`, `GoRouter`, `auto_route`, or native `Navigator`). Understand how routes are registered and what the current initial route is.
2. **Navigation extensions** — Check `lib/core/extensions/` for navigation extensions. Identify exactly which method the app uses (e.g. `context.pushReplacementNamed()`, `context.goNamed()`, `context.go()`). **Use the same pattern** — never introduce a different navigation method.
3. **Color system** — Read `AppColors`, theme data, `ColorScheme`, or any color tokens the app uses.
4. **Typography** — Read `AppTextStyles` or the app's `TextTheme`.
5. **Sizing tokens** — Read `AppSizes` or equivalent spacing/sizing system.
6. **Assets** — Check what assets (logos, images) are already available in `assets/`.

#### 5.2 — Add `flutter_animate` Dependency

Add `flutter_animate` to `pubspec.yaml` under the appropriate dependency group:

```yaml
dependencies:
  # Animations
  flutter_animate: ^4.5.2
```

> [!IMPORTANT]
> Place it in its own `# Animations` group or under the existing UI-related group in `pubspec.yaml`. Follow the project's existing dependency grouping convention.

#### 5.3 — Create the Splash Feature

Create a splash feature under the app's feature directory structure:

```
lib/features/splash/
├── presentation/
│   ├── views/
│   │   └── splash_view.dart
│   └── widgets/
│       └── (any sub-widgets needed)
```

**Animation approach** — Use `flutter_animate` for all animations:

```dart
import 'package:flutter_animate/flutter_animate.dart';

// Example: fade-in animation on splash elements
Widget.animate().fadeIn(
  duration: 1500.ms,
  curve: Curves.easeInOut,
),
```

**Rules:**
- **No Comments Rule**: Do NOT write any comments in generated Dart files (no `// TODO:`, no explanation comments). Comments are ONLY allowed in `pubspec.yaml` to categorize dependencies.
- **Structured Asset Organization**: Do **NOT** place assets directly under a flat folder like `assets/splash/`. Instead, partition assets cleanly by format and purpose:
  - Rasters (PNG/JPG) go under `assets/images/`
  - Vectors (SVG) go under `assets/svgs/`
- **Theme-Aware Asset Resolution**: When loading the branding image (e.g. app logo) for light and dark modes:
  1. Inspect the theme system for a custom `ThemeExtension` containing light/dark asset paths (e.g., `'assets/svgs/app_icon.svg'`). If found, resolve the asset path dynamically using `Theme.of(context).extension<AppThemeExtension>()` (or the app's specific extension access pattern).
  2. If no theme extension exists, register the asset paths inside `lib/core/constants/app_assets.dart` and use them.
- **Design & Background System Integration**: The in-app splash view **MUST NOT** define its own arbitrary background color or decoration. It must inherit or follow the app's existing theme system/tokens (e.g. `AppColors.background` or `Theme.of(context).scaffoldBackgroundColor`) to respect the app's overall dark/light system behavior.
- Use **ONLY** the app's existing design system (colors from `AppColors`, sizes from `AppSizes`, typography from `AppTextStyles`). Never hardcode raw values.
- Build the UI according to the user's description/specs.
- Use `flutter_animate` for all animation effects (fadeIn, slideIn, scale, etc.).
- Use the splash duration selected by the user (default: **2 seconds**).

#### 5.4 — Wire into Router

- Register the splash screen as the **initial route** in the app's routing system.
- After the splash duration/animation completes, navigate to the app's previous initial route (typically home/onboarding).
- **MUST follow the app's existing navigation pattern**: read the context extensions and router setup, then use the exact same method the app already uses (e.g. `context.pushReplacementNamed()`, `context.goNamed()`, `context.go()`).
- Never introduce a new navigation method that differs from the app's convention.

#### 5.5 — Preserve Existing Behavior

- Do NOT modify existing routes or screens.
- Do NOT change the app's theme or color definitions.
- Only ADD the splash feature and update the initial route.

---

### Step 7 — Quality Gate & Verification

> [!IMPORTANT]
> This step is **mandatory**. Never skip it. The skill must deliver code with **zero issues**.

1. Run `flutter analyze` and ensure the output shows **0 errors, 0 warnings, and 0 infos**. If any issues are found, fix them before delivering.
2. Visually describe changes or show file outputs to verify:
   - Icon dimensions are correct for each detected platform.
   - Splash colors match the theme.
   - (If Task 3) Splash feature is properly wired in the router.
   - (If Task 3) `flutter_animate` import is correct and used properly.
3. Do NOT deliver until `flutter analyze` passes with a completely clean output.
4. **Performance & Run Recommendation**: Once the quality gate passes successfully, explicitly recommend that the user executes the following command in their terminal to test the new visual assets, transition effects, and splash animations in performance-optimized Profile mode:
   ```bash
   flutter run --profile
   ```

---

## 4. BAD vs GOOD Patterns

### Custom Splash Implementation

```dart
// BAD — Manual AnimationController boilerplate, hardcoded color, raw navigation call bypassing extensions
class SplashView extends StatefulWidget {
  const SplashView({super.key});
  @override
  State<SplashView> createState() => _SplashViewState();
}
class _SplashViewState extends State<SplashView> with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  @override
  void initState() {
    super.initState();
    _controller = AnimationController(vsync: this, duration: const Duration(seconds: 2))..forward();
    Future.delayed(const Duration(seconds: 2), () {
      Navigator.pushReplacement(context, MaterialPageRoute(builder: (_) => const HomeView()));
    });
  }
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: const Color(0xFFFFFFFF), // Hardcoded color!
      body: Center(child: FadeTransition(opacity: _controller, child: Image.asset('assets/logo.png'))),
    );
  }
}

// GOOD — Using flutter_animate, AppColors & AppSizes design tokens, and context navigation extensions
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
    context.pushReplacementNamed(Routes.home); // Reuses the project's custom navigation extension!
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: AppColors.background, // Reuses design tokens!
      body: Center(
        child: Image.asset(
          'assets/icon/icon.png',
          width: AppSizes.s80,
        ).animate().fadeIn( // Reuses flutter_animate!
          duration: 1500.ms,
          curve: Curves.easeInOut,
        ),
      ),
    );
  }
}
```

---

## 5. Common Pitfalls

| Pitfall | Fix |
|---|---|
| Asking the user for target platforms | Auto-detect from project directories — never ask |
| Generating placeholders silently | Always request real branding assets from the user first |
| Guessed theme colors | Look up the background color token in `core/theme/` or the app's `ColorScheme` |
| Overwriting web manifest | Keep manifest backups or use merging when configuring web icons |
| Hardcoding colors in custom splash | Use `AppColors` / theme tokens — never raw hex in widget code |
| Hardcoding sizes in custom splash | Use `AppSizes` tokens — never raw numeric literals |
| Breaking the router's initial route | Save the old initial route and redirect to it after splash |
| Configuring platforms that don't exist | Only configure platforms that were detected in the project |
| Ignoring dark mode for native splash | Always ask about dark mode support in Task 2 questions |
| Using a different navigation method than the app | Read context extensions and router — use the exact same `context.xxx()` pattern |
| Skipping `flutter analyze` | Always run and fix until 0 errors, 0 warnings, 0 infos |
| Adding `flutter_animate` in wrong pubspec section | Follow the project's dependency grouping convention |
| Hardcoding animation without `flutter_animate` | Always use `flutter_animate` for Task 3 animations |
| Writing comments in Dart code | Keep Dart files free of comments; only add comments in `pubspec.yaml` |
| Defining custom background color on SplashView | Follow the app's existing theme background tokens (e.g. `AppColors.background`) |
| Flat or messy asset folders like `assets/splash/` | Split assets cleanly into `assets/images/` and `assets/svgs/` |
| Hardcoding light/dark asset paths in views | Resolve them dynamically via `ThemeExtension` or `AppAssets` constants |

---

## 6. References

- See `flutter-project-foundation` for setting up the initial application directory skeleton.
- See [references/config-templates.md](references/config-templates.md) for full configuration blueprints for all platforms and dark mode variants.
