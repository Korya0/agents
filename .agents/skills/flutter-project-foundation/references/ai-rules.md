# Project Coding Rules (AI_RULES.md Template)

These guidelines direct the AI Agent on coding practices, layers, and formatting rules.

---

## 1. Coding Rules

* **Formatting**: Keep code styled with `flutter format`. Match static analysis constraints perfectly.
* **Separation of Layers**: 
  - **Data Layer**: Repositories & DataSources. Handles network calls, local caching, parsing.
  - **Domain Layer**: Entities & Use Cases. Contains pure logic, completely independent of UI frameworks.
  - **Presentation Layer**: UI Widgets & Cubits/Blocs.
* **Immutable State**: State management models must be immutable. Use `freezed` or plain class states with `copyWith` and `Equatable`.
* **State Operations**: Views must only dispatch events or trigger functions on state controllers. They must never contain logic.
* **RTL & Localization**: Always use `Directional` variants (e.g. `EdgeInsetsDirectional`) and l10n strings.
* **No Raw Sizes**: Spacing, padding, colors, and margins must utilize design system tokens (`context.spacing`, `context.colors`).
