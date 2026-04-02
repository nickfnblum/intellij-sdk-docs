<!-- Copyright 2000-2026 JetBrains s.r.o. and contributors. Use of this source code is governed by the Apache 2.0 license. -->

# Configuring a Plugin for Split Mode

<link-summary>Configure Gradle so a plugin can run, debug, and test with separate frontend and backend processes.</link-summary>

Split Mode configuration in the Gradle build script allows for running development sandbox IDEs in mode emulating the [remote development](split_mode_for_remote_development.md) scenario, with both frontend and backend processes running locally.
To make a plugin work natively in split mode, see [plugin modularization](modular_plugins.md).

## Basic Configuration

> Split Mode requires [IntelliJ Platform Gradle Plugin **2.x**](tools_intellij_platform_gradle_plugin.md).
>
{style="warning"}

Enable Split Mode in the `intellijPlatform {}` extension:

```kotlin
intellijPlatform {
  splitMode = true
  splitModeTarget = SplitModeAware.SplitModeTarget.BOTH
}
```

The two relevant properties are:

- [`splitMode`](tools_intellij_platform_gradle_plugin_extension.md#intellijPlatform-splitMode) – starts separate frontend and backend processes
- [`splitModeTarget`](tools_intellij_platform_gradle_plugin_extension.md#intellijPlatform-splitModeTarget) – selects where the plugin is installed

## Choosing the Installation Target

Choose `splitModeTarget` according to the code being exercised:

- `BACKEND` for backend-only functionality
- `FRONTEND` for frontend-only functionality
- `BOTH` for typical modular plugins that contain frontend, backend, and shared modules

For split plugin development, `BOTH` is the most common choice.

> `splitModeTarget` only controls where the plugin is placed in locally run development sandboxes.
> It doesn't describe end-user installation or synchronization behavior in split mode.
> See [Plugin Management](plugin_management_in_split_mode.md).
>
{style="note"}

## Custom Run and Test Tasks

Custom split-mode tasks can be declared with `intellijPlatformTesting`:

```kotlin
val runIdeSplitMode by intellijPlatformTesting.runIde.registering {
  splitMode = true
  splitModeTarget = SplitModeAware.SplitModeTarget.BOTH
}

val testIdeUiSplitMode by intellijPlatformTesting.testIdeUi.registering {
  splitMode = true
  splitModeTarget = SplitModeAware.SplitModeTarget.BOTH
}
```

These tasks get dedicated sandboxes and can be used like other development or test tasks.
See [](tools_intellij_platform_gradle_plugin_testing_extension.md) for more details.

## Typical Next Step

After the Gradle configuration is in place, the next step is deciding how the plugin code should be distributed between frontend, backend, and shared modules.
See [](split_mode_feature_development.md).
