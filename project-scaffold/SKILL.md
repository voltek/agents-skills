---
name: project-scaffold
description: Bootstraps a new, empty Android project — dependencies, folder skeleton, Hilt setup, Theme (from DESIGN.md), and core testing/presentation utilities. Use ONCE, only when the project is new/empty and has no architecture set up yet.
---

## Dependencies
- Compose BOM
- Hilt + Hilt Navigation Compose
- Retrofit + Kotlinx.serialization
- Room
- Coil
- Navigation Compose
- Turbine, MockK, kotlinx-coroutines-test
- Always verify the latest stable version on the internet before adding —
  do not rely on a remembered version number.

## Folders
Create (empty at this stage — no subfolders per feature yet):
- `core/domain/`
- `core/data/`
- `core/presentation/`
- `ui/theme/`
- `feature/`

In the test source set (not main):
- `core/testing/` — for shared test utilities like `MainDispatcherRule`.

## Hilt setup
- `Application` class annotated `@HiltAndroidApp`, registered in the manifest.
- Do not create empty/placeholder Hilt modules (e.g. NetworkModule, DatabaseModule)
  yet — those get created organically by `new-repository-impl` when a real
  binding is actually needed. Creating them empty now would just be guessing
  ahead of real requirements.

## Theme
Read `docs/project/DESIGN.md` — it contains the real color, typography, and
spacing tokens for this project. Do not invent values; every color and font
must come from that file.

Create in `ui/theme/`:
- `Color.kt` — all colors from DESIGN.md's `colors` section
- `Type.kt` — Typography mapped to Material3 text roles (displayLarge,
  headlineLarge, bodyLarge, etc.) using DESIGN.md's `typography` section
- `Shape.kt` — corner radius values from DESIGN.md's `rounded` section
- `Theme.kt` — the `GameStackTheme` Composable wrapping MaterialTheme.
  Dark-only for MVP (see CLAUDE.md Tier 1) — do not build a Light ColorScheme.

## MainDispatcherRule
Create in `core/testing/MainDispatcherRule.kt` (test source set) — used by
every ViewModel test via the `write-tests` skill:

```kotlin
@OptIn(ExperimentalCoroutinesApi::class)
class MainDispatcherRule(
    private val testDispatcher: TestDispatcher = UnconfinedTestDispatcher()
) : TestWatcher() {

    override fun starting(description: Description?) {
        super.starting(description)
        Dispatchers.setMain(testDispatcher)
    }

    override fun finished(description: Description?) {
        super.finished(description)
        Dispatchers.resetMain()
    }
}
```

## UiText
Create in `core/presentation/UiText.kt` — used across ViewModels/Screens per
CLAUDE.md's rule of never hardcoding UI strings:

```kotlin
sealed class UiText {

    data class DynamicString(val value: String) : UiText()

    class StringResource(@param:StringRes val resId: Int, vararg val args: Any) : UiText()

    @Composable
    fun asString(): String {
        return when (this) {
            is DynamicString -> value
            is StringResource -> stringResource(resId, *args)
        }
    }

    fun asString(context: Context): String {
        return when (this) {
            is DynamicString -> value
            is StringResource -> context.getString(resId, *args)
        }
    }
}
```

## Root navigation (conditional)
Check the project's CLAUDE.md for a documented navigation structure
(bottom nav, tabs, etc.). If one exists, implement the NavHost + container
exactly as documented there. If none exists, skip this step entirely —
do not assume or invent a navigation pattern.

## Quality criteria
- All dependencies use the latest verified stable version — not a remembered one.
- Theme colors/typography/shapes come exclusively from DESIGN.md — no invented values.
- No Light ColorScheme is created (dark-only for MVP, per CLAUDE.md).
- Root navigation is only implemented if CLAUDE.md documents one — never invented.
- This skill runs once per project. If invoked on a project that already has
  Theme/Hilt/folders set up, stop and ask before overwriting anything.
