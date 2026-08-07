# Skill Templates

Ready-to-use templates for the three main skill types. Copy the appropriate template and fill in the placeholders (`<...>`).

---

## Template 1: Package / Library Skill

Use for pub.dev packages, SDKs, or libraries (e.g., `bloc`, `riverpod`, `provider`, `mocktail`).

````markdown
---
name: <skill-name>
description: "Use when <specific triggers involving the package — list key classes, methods, and concepts>."
---
# <Package Name> Skill

Design, implement, and test <what the package does> using the [<package>](https://pub.dev/packages/<package>) library.

## When to Use

Use this skill when:

* <Creating/configuring something with this package>.
* <Implementing a specific pattern or feature>.
* <Choosing between options within this package>.
* <Writing tests for code using this package>.
* <Refactoring existing code to follow package conventions>.

---

## 1. Core Concepts

| Concept | Description |
|---|---|
| `<Class1>` | <What it does> |
| `<Class2>` | <What it does> |
| `<Class3>` | <What it does> |

---

## 2. Setup / Installation

```yaml
dependencies:
  <package>: ^<version>
```

```dart
import 'package:<package>/<package>.dart';
```

---

## 3. Basic Usage

```dart
// Example of basic usage
<code example>
```

Rules:
- <Rule 1>.
- <Rule 2>.
- <Rule 3>.

---

## 4. Advanced Patterns

### <Pattern Name>

```dart
// BAD — <why this is wrong>
<bad code>

// GOOD — <why this is correct>
<good code>
```

---

## 5. Architecture

```
<Layer diagram or dependency flow>
```

Rules:
- <Architectural rule 1>.
- <Architectural rule 2>.

---

## 6. Testing

Use `<test_package>` package. Mock dependencies with `mocktail`.

```dart
import 'package:<test_package>/<test_package>.dart';
import 'package:mocktail/mocktail.dart';
import 'package:test/test.dart';

class Mock<Dependency> extends Mock implements <Dependency> {}

void main() {
  group('<ClassUnderTest>', () {
    late <Dependency> <dependency>;

    setUp(() {
      <dependency> = Mock<Dependency>();
    });

    test('should <expected behavior>', () {
      // Arrange
      when(() => <dependency>.<method>()).thenReturn(<value>);
      // Act
      final result = <classUnderTest>.<method>();
      // Assert
      expect(result, <expected>);
    });
  });
}
```

---

## 7. Common Pitfalls

| Pitfall | Fix |
|---|---|
| <Common mistake 1> | <How to fix it> |
| <Common mistake 2> | <How to fix it> |
| <Common mistake 3> | <How to fix it> |

```dart
// BAD — <description>
<bad code example>

// GOOD — <description>
<good code example>
```

---

## References

- [<Package> GitHub Repository](<github-url>)
- [<Package> pub.dev page](https://pub.dev/packages/<package>)
````

---

## Template 2: Best Practice / Workflow Skill

Use for coding standards, review processes, design patterns (e.g., `code-review`, `testing`, `effective-dart`, `flutter-best-practices`).

````markdown
---
name: <skill-name>
description: "Use when <specific triggers — what tasks, questions, or scenarios activate this skill>."
---
# <Practice Name> Skill

<One to three sentences: what this practice achieves and why it matters.>

## When to Use

Use this skill when:

* <Scenario where the user needs this guidance>.
* <Another common trigger>.
* <A question the user might ask>.
* <A task that requires following these rules>.

---

## 1. <Principle / Phase / Category>

<Explanation of the principle.>

### <Sub-principle>

```dart
// BAD — <why this is wrong>
<bad code>

// GOOD — <why this is correct>
<good code>
```

Rules:
- <Rule>.
- <Rule>.

---

## 2. <Next Principle / Phase>

| Criterion | What to check |
|---|---|
| <Area 1> | <What to verify> |
| <Area 2> | <What to verify> |
| <Area 3> | <What to verify> |

---

## 3. Workflow / Checklist

### Step 1 — <Action>

1. <Concrete instruction>.
2. <Concrete instruction>.
3. <Concrete instruction>.

**Checkpoint:** <What to verify before moving on.>

### Step 2 — <Action>

<Instructions...>

---

## 4. Common Pitfalls

| Pitfall | Fix |
|---|---|
| <Mistake> | <Correction> |
| <Mistake> | <Correction> |

---

## References

- [<Source Name>](<url>)
````

---

## Template 3: Service / Integration Skill

Use for external services, APIs, platforms (e.g., `firebase-auth`, `firebase-storage`, `firebase-ai`).

````markdown
---
name: <skill-name>
description: "Use when <specific triggers involving the service — list key operations, setup, and configuration tasks>."
---
# <Service Name> Skill

Integrate and configure <service name> in Flutter/Dart projects using the [<package>](https://pub.dev/packages/<package>) package.

## When to Use

Use this skill when:

* <Setting up / configuring the service>.
* <Performing a specific operation (CRUD, auth, upload, etc.)>.
* <Handling errors or edge cases specific to this service>.
* <Writing tests for code that uses this service>.
* <Migrating or upgrading the service integration>.

---

## 1. Setup & Configuration

### Prerequisites
- <Prerequisite 1 (e.g., Firebase project, API key)>.
- <Prerequisite 2>.

### Installation

```yaml
dependencies:
  <package>: ^<version>
```

### Platform-specific setup

**Android:**
```
<android-specific steps>
```

**iOS:**
```
<ios-specific steps>
```

---

## 2. Initialization

```dart
// Initialize in main.dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await <Service>.initialize();
  runApp(const MyApp());
}
```

---

## 3. Core Operations

### <Operation 1 — e.g., Create / Read / Write>

```dart
<code example>
```

### <Operation 2>

```dart
<code example>
```

---

## 4. Error Handling

```dart
try {
  await <service>.<operation>();
} on <ServiceException> catch (e) {
  switch (e.code) {
    case '<error-code-1>':
      // Handle specific error
      break;
    case '<error-code-2>':
      // Handle specific error
      break;
    default:
      // Handle unknown error
      break;
  }
}
```

---

## 5. Security Rules / Configuration

```
<security rules or configuration examples>
```

Rules:
- <Security rule 1>.
- <Security rule 2>.

---

## 6. Testing

```dart
// Mock the service
class Mock<Service> extends Mock implements <Service> {}

void main() {
  group('<Feature using service>', () {
    test('should <behavior> when <condition>', () {
      // ...
    });
  });
}
```

---

## 7. Common Pitfalls

| Pitfall | Fix |
|---|---|
| <Service-specific mistake> | <How to fix> |
| <Configuration error> | <Correct configuration> |
| <Security mistake> | <Secure alternative> |

---

## References

- [<Service> Documentation](<official-docs-url>)
- [<Package> pub.dev](https://pub.dev/packages/<package>)
````

---

## Frontmatter Description Examples

Good `description` values from existing skills, for reference:

| Skill | Description |
|---|---|
| `bloc` | "Use when creating a Cubit or Bloc, modeling state with sealed classes or status enums, wiring BlocBuilder/BlocListener/BlocProvider, writing bloc tests, or choosing between Cubit and Bloc." |
| `testing` | "Use when writing or reviewing Flutter/Dart tests (unit, widget, golden), fixing flaky tests, adding coverage, or choosing between unit and widget tests." |
| `code-review` | "Use when asked to review a PR, MR, branch, or diff, audit changed files, or check code quality." |
| `accessibility` | "Use when working on accessibility, a11y, WCAG, ARIA, screen readers, keyboard nav, focus order, contrast, alt text, captions, reduced motion, or target sizes; not language/culture/device (see inclusive-design)." |
| `store-listing-assets` | "Use when writing store copy to character limits (name, subtitle, descriptions, keywords) or preparing listing images (icon, feature graphic, screenshots, preview video)." |

### Pattern

```
"Use when <action verbs describing tasks> (<key terms, abbreviations, synonyms>), <more tasks>, or <final task>; not <exclusions> (see <related-skill>)."
```

Key principles:
- **Start with "Use when"** — always.
- **List specific actions** — not abstract concepts.
- **Include synonyms and abbreviations** — users might say "a11y" instead of "accessibility."
- **Add exclusions** — if there's a related skill that covers overlapping territory, say "not X (see other-skill)."
- **Keep it to one sentence** — but make it information-dense.
