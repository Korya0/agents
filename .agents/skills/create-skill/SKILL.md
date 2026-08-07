---
name: create-skill
description: "Use when asked to create, scaffold, or generate a new Gemini agent skill — builds the folder, SKILL.md, optional references/scripts, and updates skills-lock.json following the project's established skill format."
---
# Create Skill

Scaffold a complete, production-ready Gemini agent skill that follows the exact structure and conventions of the skills in this repository. The generated skill is immediately usable by the agent without manual adjustments.

## When to Use

Use this skill when:

* The user asks to create a new skill on any topic.
* The user wants to document a package, library, pattern, workflow, or best-practice as a reusable agent skill.
* The user says "make me a skill for X" or "add a skill about Y."
* Migrating external knowledge (docs, blog posts, internal guides) into the skill format.

---

## 1. Skill Anatomy

Every skill lives under `agent/skills/` and follows this structure:

```
agent/
└── skills/
    └── <skill-name>/              ← kebab-case, lowercase
        ├── SKILL.md               ← REQUIRED — the main skill file
        ├── references/            ← OPTIONAL — deep-dive reference docs
        │   ├── <topic-detail>.md
        │   └── <another-ref>.md
        └── scripts/               ← OPTIONAL — helper scripts/hooks
            └── <script-name>.sh
```

### When to create `references/`

Create a `references/` folder when:
- The skill topic has sub-areas that would make the main SKILL.md too long (>400 lines).
- There are detailed specs, checklists, or API references that the agent should load only when needed.
- The skill references external resources that benefit from a local summary.

Link references from the main SKILL.md like this:
```markdown
Read **[references/topic-detail.md](references/topic-detail.md)** for the full spec.
```

### When to create `scripts/`

Create a `scripts/` folder only when:
- The skill needs a `PreToolUse` or `PostToolUse` hook (e.g., token protection).
- There are automation scripts that the agent should run as part of the workflow.

---

## 2. SKILL.md Format

Every SKILL.md must follow this exact structure:

### 2.1 Frontmatter (REQUIRED)

The YAML frontmatter must contain two fields:

1. **`name`** — the skill's kebab-case identifier (must match the folder name). This is how users invoke the skill via slash commands.
2. **`description`** — a concise sentence describing **when** the skill should be triggered. This is how the agent decides whether to load the skill.

```yaml
---
name: <skill-name>
description: "Use when <trigger conditions — be specific and include key terms the user might say>."
---
```

**Good frontmatter** — name matches folder, description is specific and keyword-rich:
```yaml
# ✅ Good — name matches folder, specific triggers
---
name: bloc
description: "Use when creating a Cubit or Bloc, modeling state with sealed classes or status enums, wiring BlocBuilder/BlocListener/BlocProvider, writing bloc tests, or choosing between Cubit and Bloc."
---

# ✅ Good — clear scope boundaries with exclusions
---
name: accessibility
description: "Use when working on accessibility, a11y, WCAG, ARIA, screen readers, keyboard nav, focus order, contrast, alt text, captions, reduced motion, or target sizes; not language/culture/device (see inclusive-design)."
---
```

**Bad frontmatter:**
```yaml
# ❌ Bad — missing name field
---
description: "Use for state management."
---

# ❌ Bad — name doesn't match folder name
---
name: Bloc_State
description: "Use when building Flutter apps."
---
```

### 2.2 Title and Introduction

```markdown
# Skill Title

One to three sentences explaining what this skill covers and its purpose.
```

### 2.3 When to Use Section (REQUIRED)

```markdown
## When to Use

Use this skill when:

* Specific scenario 1.
* Specific scenario 2.
* Specific scenario 3.
```

Include 3–7 bullet points. Be concrete — each bullet should describe a task the user might request.

### 2.4 Numbered Content Sections

The core knowledge. Use numbered headings (`## 1.`, `## 2.`, etc.) with horizontal rules (`---`) between major sections. Content may include:

| Element | When to use |
|---|---|
| **Tables** | Comparisons, checklists, decision matrices |
| **Code blocks** | Patterns, examples, BAD/GOOD contrasts |
| **Nested lists** | Rules, steps, conditions |
| **Bold rules** | Critical constraints (e.g., "**Always do X.**") |

### 2.5 Code Examples

Always show **BAD** and **GOOD** patterns when illustrating rules:

````markdown
```dart
// BAD — explanation of why this is wrong
badCode();

// GOOD — explanation of why this is correct
goodCode();
```
````

### 2.6 Common Pitfalls (OPTIONAL)

A table summarizing frequent mistakes:

```markdown
## N. Common Pitfalls

| Pitfall | Fix |
|---|---|
| Doing X | Do Y instead |
| Forgetting Z | Always add Z because... |
```

### 2.7 References Section (OPTIONAL)

```markdown
## References

- [Reference Name](https://external-link.com)
- [references/local-ref.md](references/local-ref.md) — description
```

---

## 3. Step-by-Step Creation Workflow

When creating a new skill, follow these steps in order:

### Step 1 — Gather Information

Ask the user (or infer from context):

1. **Topic**: What is the skill about?
2. **Skill name**: Suggest a kebab-case name (e.g., `firebase-auth`, `bloc`, `code-review`).
3. **Trigger conditions**: When should this skill activate? (becomes the `description`).
4. **Scope**: What does the skill cover? What does it NOT cover?
5. **Skill type**: Which template fits best? (see Step 3).
6. **References needed?**: Is the topic complex enough to warrant a `references/` folder?

### Step 2 — Create the Folder

Create the skill folder at:
```
agent/skills/<skill-name>/
```

Naming rules:
- **Always kebab-case**: `my-skill-name` (not `mySkillName` or `my_skill_name`)
- **Lowercase only**: `firebase-auth` (not `Firebase-Auth`)
- **Descriptive**: The name should clearly indicate the topic
- **Prefix for related skills**: Use common prefixes for skill families (e.g., `firebase-*`, `flutter-*`)

### Step 3 — Choose a Template and Write SKILL.md

Select the template that best matches the skill type. See [references/skill-templates.md](references/skill-templates.md) for the full templates. The three main types:

| Type | Use for | Example skills |
|---|---|---|
| **Package/Library** | A pub.dev package or SDK | `bloc`, `riverpod`, `provider`, `mocktail` |
| **Best Practice** | Coding standards, workflows, patterns | `code-review`, `testing`, `effective-dart`, `flutter-best-practices` |
| **Service/Integration** | External services, APIs, platforms | `firebase-auth`, `firebase-storage`, `firebase-ai` |

### Step 4 — Create References (if needed)

For each reference file in `references/`:
- Use kebab-case filename: `topic-detail.md`
- Start with a `# Title` heading
- Link it from the main SKILL.md
- Keep each reference focused on one sub-topic

### Step 5 — Update skills-lock.json

Add an entry to `skills-lock.json` at the project root:

```json
{
  "<skill-name>": {
    "source": "local",
    "sourceType": "local",
    "skillPath": "skills/<skill-name>/SKILL.md",
    "computedHash": "<sha256-of-SKILL.md-content>"
  }
}
```

- Use `"source": "local"` and `"sourceType": "local"` for custom skills (not from GitHub).
- Compute `computedHash` as the SHA-256 hex digest of the SKILL.md file content.
- Insert the entry alphabetically among existing skills.

### Step 6 — Quality Check

Before proceeding to verification, check:

- [ ] Folder is kebab-case under `agent/skills/`
- [ ] SKILL.md has valid YAML frontmatter with `description`
- [ ] `description` is specific and keyword-rich
- [ ] "When to Use" section has 3–7 concrete bullet points
- [ ] Content sections are numbered and separated by `---`
- [ ] Code examples show BAD/GOOD patterns where applicable
- [ ] References (if any) are linked from SKILL.md
- [ ] `skills-lock.json` is updated with the new entry
- [ ] No placeholder text remains — all content is real and actionable

### Step 7 — Project Verification (MANDATORY)

After the skill is fully created, **always** run these checks to ensure nothing is broken:

#### 7.1 Static Analysis

Run `flutter analyze` in the project root and verify **zero** issues at all severity levels:

```bash
flutter analyze
```

**Expected output:**
```
Analyzing <project>...
No issues found!
```

**If issues are found:**
- **Errors**: Must be fixed before completing. These indicate broken code.
- **Warnings**: Must be fixed. These indicate potential bugs or bad practices.
- **Info**: Must be fixed. These indicate style violations or improvement opportunities.

> **Do NOT consider the skill complete until `flutter analyze` shows "No issues found!"**

#### 7.2 Run All Tests

If the skill contains or modifies any Dart code (code examples that are part of the project, not just documentation), run **all** test types:

```bash
# Unit & Widget tests
flutter test

# Integration tests (if they exist)
flutter test integration_test/
```

**Rules:**
- All existing tests must continue to pass — **zero regressions**.
- If a skill introduces new Dart code to the project, add corresponding tests.
- If any test fails, investigate and fix before completing.
- Check the terminal output carefully — ensure no test is skipped or errored.

#### 7.3 Verification Summary

After both checks pass, confirm in the output:

```
✅ flutter analyze: No issues found
✅ flutter test: All X tests passed
```

If tests are not applicable (skill is documentation-only with no project code changes):

```
✅ flutter analyze: No issues found
ℹ️ flutter test: Skipped (no Dart code changes in project)
```

---

## 5. Naming Conventions Summary

| Element | Convention | Example |
|---|---|---|
| Skill folder | kebab-case | `flutter-best-practices` |
| Main file | Always `SKILL.md` (uppercase) | `SKILL.md` |
| Reference files | kebab-case `.md` | `cognitive-accessibility.md` |
| Script files | kebab-case with extension | `protect-token.sh` |
| Skill name in lock file | matches folder name | `"flutter-best-practices"` |

---

## 6. Common Pitfalls

| Pitfall | Fix |
|---|---|
| Vague `description` frontmatter | Include specific trigger words and scenarios — this is how the agent finds the skill |
| SKILL.md too long (>500 lines) | Split deep-dive content into `references/` files |
| Missing "When to Use" section | Always include it — the agent relies on it to decide relevance |
| Forgetting to update `skills-lock.json` | Always add the entry after creating the skill |
| Using camelCase or underscores in folder name | Always use kebab-case lowercase |
| Placeholder or generic content | Every sentence must be actionable and specific to the topic |
| No code examples for technical skills | Always include dart code blocks with BAD/GOOD patterns |
| Inconsistent section numbering | Use `## 1.`, `## 2.`, etc. with `---` separators |

---

## References

- [references/skill-templates.md](references/skill-templates.md) — Full copy-paste templates for Package, Best Practice, and Service skill types.
