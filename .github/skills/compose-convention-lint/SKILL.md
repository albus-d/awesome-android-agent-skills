---
name: compose-convention-lint
description: Detects Jetpack Compose convention violations — CompositionLocal misuse, missing remember, wrong parameter ordering, and composable constant anti-patterns. Use this to audit Composable files for common mistakes.
---

# Compose Convention Lint

## Instructions

Scan the specified Compose file(s) for the following anti-patterns and report each finding with file path, line number, the problematic code, and the suggested fix.

### 1. CompositionLocal Read Inside Composables (instead of explicit params)

**Anti-pattern**: Reading `LocalXxx.current` inside a composable that should receive it as a parameter.

```kotlin
// BAD — reads CompositionLocal internally
@Composable
fun MyComponent() {
    val color = LocalCustomColorsPalette.current
    Text(color = color.textPrimary, ...)
}

// GOOD — receives explicit param
@Composable
fun MyComponent(color: CustomColorsPalette) {
    Text(color = color.textPrimary, ...)
}
```

**Rule**: Stateless composables should receive `color: CustomColorsPalette` and `size: CustomSizesPalette` as explicit parameters. Only screen-level composables or top-level wrappers should read from `LocalCustomColorsPalette.current` / `LocalCustomSizesPalette.current`.

**Exception**: It is acceptable to read CompositionLocals in:
- Screen-level composables (the top entry point)
- Preview functions
- Theme wrapper composables

### 2. @Composable Functions That Should Be Plain Functions

**Anti-pattern**: Using `@Composable` only to access a CompositionLocal, when the value could be passed as a parameter.

```kotlin
// BAD — @Composable just to read CompositionLocal
object ChartColors {
    @Composable
    fun methodsColor() = listOf(LocalCustomColorsPalette.current.chartRed, ...)
}

// GOOD — extension function, no @Composable needed
fun CustomColorsPalette.chartMethodsColor() = listOf(chartRed, ...)
```

**Rule**: If a function is `@Composable` solely because it reads a CompositionLocal, refactor it to accept the value as a parameter or use an extension function.

### 3. Missing `remember` on Allocations

**Anti-pattern**: Creating objects (lists, maps, callbacks) inside a composable without `remember`.

```kotlin
// BAD — new list every recomposition
@Composable
fun Chart() {
    val colors = listOf(Color.Red, Color.Blue)
}

// GOOD
@Composable
fun Chart() {
    val colors = remember { listOf(Color.Red, Color.Blue) }
}
```

**Rule**: Any non-trivial allocation inside a `@Composable` function should be wrapped in `remember` (with appropriate keys if dependent on changing state).

**Exception**: Simple property reads or primitives don't need `remember`.

### 4. Constructor Parameter Ordering

**Rule**: For `@Inject constructor` (ViewModels, repositories, etc.), order parameters by visibility:
1. No modifier — used only in `init` or passed to parent
2. `private val` — internal dependencies
3. `val` / `var` — publicly exposed properties

### 5. Mixing CompositionLocal Styles in Same Composable

**Anti-pattern**: A composable receives explicit `color`/`size` params but also reads `LocalCustomColorsPalette.current` or `LocalCustomSizesPalette.current` internally.

```kotlin
// BAD — mixed access
@Composable
fun MyButton(color: CustomColorsPalette) {
    val size = LocalCustomSizesPalette.current  // inconsistent!
}
```

**Rule**: Be consistent — if a composable takes explicit theme params, ALL theme access should be via those params.

## Output Format

For each finding, report:

```
[RULE_NAME] file_path:line_number
  Problem: <brief description>
  Current: <offending code snippet>
  Suggested: <corrected code snippet>
```

At the end, provide a summary count: `Found N issue(s) across M file(s).`
