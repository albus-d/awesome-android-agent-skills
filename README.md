# Awesome Android Agent Skills

A curated collection of [Agent Skills](https://agentskills.io) for Android development with [Claude Code](https://claude.ai/code).

These skills give Claude deep knowledge of modern Android patterns — Jetpack Compose, Kotlin Coroutines, Gradle, architecture, testing, and more — so it produces idiomatic, production-quality code out of the box.

## Available Skills

| Skill | Description |
|-------|-------------|
| `android-architecture` | Clean Architecture, modularization, and Hilt DI setup |
| `android-coroutines` | Structured concurrency, lifecycle-safe collection, Flow patterns |
| `android-data-layer` | Repository pattern, Room + Retrofit, offline-first sync |
| `android-gradle-logic` | Convention Plugins and Version Catalogs (NiA-style) |
| `android-retrofit` | Type-safe HTTP networking with OkHttp and Hilt |
| `android-testing` | Unit, integration, Hilt, and screenshot testing (Roborazzi) |
| `android-viewmodel` | StateFlow/SharedFlow state management in ViewModels |
| `android-accessibility` | Accessibility audit checklist for Compose UI |
| `coil-compose` | Image loading with Coil in Jetpack Compose |
| `compose-navigation` | Type-safe navigation, deep links, nested graphs |
| `compose-performance-audit` | Diagnose and fix recomposition storms, jank, instability |
| `compose-ui` | State hoisting, modifiers, theming, and previews |
| `gradle-build-performance` | Build scan analysis, caching, CI/CD optimization |
| `kotlin-concurrency-expert` | Review and fix coroutine bugs, race conditions, leaks |
| `xml-to-compose-migration` | Convert XML layouts to idiomatic Jetpack Compose |

## Installation

### Option 1: Install All Skills (Recommended)

Copy the entire skills collection into your Android project:

```bash
# From your Android project root
mkdir -p .claude/skills
cp -r path/to/awesome-android-agent-skills/.github/skills/* .claude/skills/
```

Then add the skills directory to `.gitignore` so each developer can choose which skills to use:

```bash
echo ".claude/skills/" >> .gitignore
```

### Option 2: Install Specific Skills

Pick only the skills you need:

```bash
# From your Android project root
mkdir -p .claude/skills
cp -r path/to/awesome-android-agent-skills/.github/skills/compose-ui .claude/skills/
cp -r path/to/awesome-android-agent-skills/.github/skills/android-coroutines .claude/skills/
```

### Option 3: Install as Personal Skills

Make skills available across all your projects (not shared with team):

```bash
cp -r path/to/awesome-android-agent-skills/.github/skills/* ~/.claude/skills/
```

## How Skills Work

Each skill is a directory containing a `SKILL.md` file with:

- **Frontmatter** — metadata (name, description) that tells Claude when the skill is relevant
- **Instructions** — patterns, rules, and code examples Claude follows when activated

```
.claude/skills/
├── android-coroutines/
│   └── SKILL.md
├── compose-ui/
│   └── SKILL.md
└── android-retrofit/
    └── SKILL.md
```

### Automatic Activation

Claude reads skill descriptions at the start of each session. When your request matches a skill's description, Claude automatically loads and applies it. For example:

- Ask *"set up networking with Retrofit"* → `android-retrofit` activates
- Ask *"fix this memory leak"* → `android-coroutines` or `kotlin-concurrency-expert` activates
- Ask *"migrate this XML layout to Compose"* → `xml-to-compose-migration` activates

### Manual Invocation

You can also invoke any skill directly with a slash command:

```
/compose-ui
/android-testing
/gradle-build-performance
```

Pass arguments for context:

```
/kotlin-concurrency-expert review the coroutine usage in UserRepository.kt
```

## Verifying Installation

After installing, start a Claude Code session in your project and run:

```
/android-coroutines
```

If the skill loads, you'll see Claude apply its structured concurrency rules. You can also ask Claude a question that matches a skill's domain and confirm it follows the skill's patterns automatically.

## Project Structure

```
awesome-android-agent-skills/
└── .github/
    └── skills/
        ├── android-accessibility/
        │   └── SKILL.md
        ├── android-architecture/
        │   └── SKILL.md
        ├── android-coroutines/
        │   └── SKILL.md
        ├── android-data-layer/
        │   └── SKILL.md
        ├── android-gradle-logic/
        │   └── SKILL.md
        ├── android-retrofit/
        │   └── SKILL.md
        ├── android-testing/
        │   └── SKILL.md
        ├── android-viewmodel/
        │   └── SKILL.md
        ├── coil-compose/
        │   └── SKILL.md
        ├── compose-navigation/
        │   └── SKILL.md
        ├── compose-performance-audit/
        │   └── SKILL.md
        ├── compose-ui/
        │   └── SKILL.md
        ├── gradle-build-performance/
        │   └── SKILL.md
        ├── kotlin-concurrency-expert/
        │   └── SKILL.md
        └── xml-to-compose-migration/
            └── SKILL.md
```

## Contributing

To add a new skill:

1. Create a directory under `.github/skills/` with a descriptive name
2. Add a `SKILL.md` file with YAML frontmatter:
   ```yaml
   ---
   name: my-skill-name
   description: When and why Claude should use this skill.
   ---
   ```
3. Write clear instructions, rules, and code examples below the frontmatter
4. Open a pull request

### Guidelines

- Keep `SKILL.md` focused and under 500 lines
- Write the `description` so Claude knows **when** to activate the skill
- Include both correct and incorrect code examples where applicable
- Reference official Android/Google documentation
- Ensure code examples follow security best practices

## License

This project is open source. See [LICENSE](LICENSE) for details.
