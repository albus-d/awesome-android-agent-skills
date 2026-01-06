# Awesome Android Agent Skills

[![Agent Skills](https://img.shields.io/badge/Agent-Skills-blue?style=flat&logo=github)](https://agentskills.io)
[![Android](https://img.shields.io/badge/Android-Modern-green?style=flat&logo=android)](https://developer.android.com)

Welcome to **Awesome Android Agent Skills**, a repository of specialized "brains" for your AI coding assistants. This project provides a suite of **Agent Skills** designed to supercharge GitHub Copilot, Claude, and other agentic AI tools with expert knowledge of modern Android development.

## 🧠 What are Agent Skills?

**Agent Skills** are a standardized way to package capabilities, instructions, and best practices for AI agents. Instead of pasting the same prompt repeatedly ("How do I implement MVVM?", "Check this for accessibility"), you install these skills into your agent's environment.

When an agent detects you are working on a relevant task (e.g., "Create a verified repository"), it automatically loads the expert instructions from the corresponding `SKILL.md` file. This ensures:
*   **Consistency**: The agent always follows your defined architecture.
*   **Accuracy**: It uses the latest 2025 best practices (Compose, Hilt, Room).
*   **Efficiency**: No need for long context-stuffing prompts.

> Learn more at [agentskills.io](https://agentskills.io).

## 🚀 Available Skills

These skills are located in `.github/skills/` and are ready for use by compatible agents (like GitHub Copilot Workspace or custom Claude environments).

*   **[🏗️ Android Architecture](.github/skills/android-architecture/SKILL.md)** (`android-architecture`)
    *   Expert guidance on **Clean Architecture**, **Modularization**, and **Dependency Injection** with **Hilt**.
    *   Ensures strict separation of UI, Domain, and Data layers.

*   **[🎨 Jetpack Compose UI](.github/skills/compose-ui/SKILL.md)** (`compose-ui`)
    *   Best practices for building stateless, performant Composables.
    *   Focuses on **State Hoisting**, **Modifiers order**, and **Theming**.

*   **[🔄 ViewModel & State](.github/skills/android-viewmodel/SKILL.md)** (`android-viewmodel`)
    *   Proper implementation of **ViewModel** using `StateFlow` for UI state and `SharedFlow` for one-off events.
    *   Avoids common pitfalls with channel usage and lifecycle collection.

*   **[💾 Data Layer & Offline-First](.github/skills/android-data-layer/SKILL.md)** (`android-data-layer`)
    *   Implements the **Repository Pattern** with **Room** (local) and **Retrofit** (remote).
    *   Guides the agent to build robust **Offline-First** synchronization logic.

*   **[♿ Accessibility](.github/skills/android-accessibility/SKILL.md)** (`android-accessibility`)
    *   A rigorous checklist for auditing **Content Descriptions**, **Touch Targets**, and **Contrast**.
    *   Ensures your app is usable by everyone.

## 🛠️ Usage

### GitHub Copilot
If this repository is part of your workspace, you can simply ask Copilot:
> "How should I structure the new User Profile feature?"
> "Create a repository for fetching News with offline support."

Copilot will detect the relevant skill (e.g., `android-architecture` or `android-data-layer`) and apply the rules defined in the `SKILL.md` files.

### Manual / Other Agents
You can also point any context-aware LLM (like ChatGPT or Claude) to the specific `SKILL.md` file you need help with.
> "Read `.github/skills/compose-ui/SKILL.md` and then refactor this screen."

## 📚 Topics & Keywords

Android Development, Agent Skills, AI Coding Assistants, Jetpack Compose, Clean Architecture, MVVM, MVI, Hilt Dependency Injection, Room Database, Retrofit, Offline-First, Kotlin Coroutines, StateFlow, SharedFlow, Android Accessibility, Semantic Trees, Modularization, Mobile DevOps, GenAI for Mobile.

---

*Original "Studio-Bot-Prompts-Handbook" content has been superseded by these executable Agent Skills.*
