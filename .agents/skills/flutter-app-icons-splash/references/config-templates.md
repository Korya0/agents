# Configurations Blueprint for Icons & Splash

 authoritative configuration shape for both packages inside `pubspec.yaml`.

---

## 1. App Icons Configuration

Add this block under the dev dependencies configuration:

```yaml
dev_dependencies:
  flutter_launcher_icons: ^0.13.1 # Under `# App Icons` comment

flutter_launcher_icons:
  android: "launcher_icon"
  ios: true
  image_path: "assets/icon/icon.png"
  web:
    generate: true
    image_path: "assets/icon/icon.png"
    background_color: "#ffffff"
    theme_color: "#ffffff"
```

---

## 2. Splash Screen Configuration

Add this block matching design theme color tokens:

```yaml
dev_dependencies:
  flutter_native_splash: ^2.4.0 # Under `# Splash Screen` comment

flutter_native_splash:
  color: "#ffffff" # Use project theme background token value
  image: assets/splash/splash_logo.png
  android_12:
    image: assets/splash/splash_logo.png
    color: "#ffffff"
  web:
    generate: true
    image: assets/splash/splash_logo.png
    color: "#ffffff"
```
