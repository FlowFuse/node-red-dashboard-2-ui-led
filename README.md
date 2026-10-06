# UI LED (Node-RED Dashboard 2.0 Widget)

This repository contains the code base for `ui-led`, a third-party node for the [Node-RED Dashboard 2.0](https://dashboard.flowfuse.com).

<img width="500" alt="Screenshot 2024-02-17 at 09 55 55" src="https://github.com/FlowFuse/node-red-dashboard-2-ui-led/assets/99246719/e90ebb01-a62b-4332-bb2e-958c77d8a798">

## Configuration Options

The `ui-led` node has the following configuration options:

### General

<img width="500" alt="Screenshot 2024-02-17 at 10 02 43" src="https://github.com/FlowFuse/node-red-dashboard-2-ui-led/assets/99246719/8ed8de2a-d300-483d-a2c3-720fe296cf25">

- **Name**: The name of the node within the context of the Node-RED editor.
- **Value**: Configure how the value is determined. Can be set to use a specific message property. If not configured, defaults to `msg.payload`.
- **Group**: The UI Group that the LED will render inside.
- **Size**: The relative size of the LED with `width` x `height`

### Label Styling 

<img width="500" alt="Screenshot 2024-02-17 at 10 02 28" src="https://github.com/FlowFuse/node-red-dashboard-2-ui-led/assets/99246719/d82ee430-bdeb-4ae1-bc3b-0a375f41b72d">

- **Text**: The label to display next to the LED.
- **Placement**: Which side of the LED the label will be displayed.
- **Alignment**: Within the space to the left/right of the LED, how the label will be aligned in that space.

### LED Styling

<img width="500" alt="Screenshot 2024-02-17 at 10 01 59" src="https://github.com/FlowFuse/node-red-dashboard-2-ui-led/assets/99246719/0709968e-60cd-4f20-a082-46d4be0ff3b7">

- **Shape**: "Circle" or "Square"
- **Show Border**: Should the LED have a fixed border around the shape, emulating a physical LED.
- **Show Glow**: Should the LED have a glow effect.

### Values

<img width="500" alt="Screenshot 2024-02-17 at 10 01 44" src="https://github.com/FlowFuse/node-red-dashboard-2-ui-led/assets/99246719/debe5d84-454f-43f4-82b9-a48f29b12307">

- The value is a property of the message, e.g. `msg.payload` or `msg.myProperty`.
- Maps a value to a respective color. If a value is provided that it doesn't recognise, a general grey color will be used.

## Release process

In this project, the [Release Please](https://github.com/googleapis/release-please) is used to automatically determine the next release version based on the commit messages in the codebase.

By using the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/), the project adheres to a standardized format for commit messages, which `Release Please` uses to determine whether the next release should be a major, minor, or patch release.

### Components

1. The `Prepare release` GitHub Action workflow:

    * A Release Please action that analyzes commit messages to determine the type of release required (major, minor, patch) based on the Conventional Commits specification
    * Creates a pre-release pull request with the proposed version bump and changelog
    * Once merged, automatically updates the version number in `package.json` and creates a new release on GitHub with the appropriate changelog

2. The `Lint Pull Request Title` GitHub Action workflow:

    * A workflow that runs on pull request creation and uses the `amannn/action-semantic-pull-request` action to validate that pull request titles follow the Conventional Commits format
    * Together with adjusted default merge commit message, this ensures that all commits merged into the main branch adhere to the expected format, allowing Release Please to function correctly

3. The `Publish Release` GitHub Action workflow:

    * A workflow that runs when a new git tag in `v*.*.*` format is pushed, builds the package and publishes the new version to the public npm registry using the `JS-DevTools/npm-publish` action
    * Once package is published, the workflow updates the package version in the Node-RED Flow Library catalogue

### Pull Request Title Format

The Conventional Commits preset expects pull request titles to be in the following format:

```
<type>(<scope>): <subject>
```

* Type: Describes the category of the commit. Examples include:
    * `feat`: A new feature (triggers a minor version bump).
    * `fix`: A bug fix (triggers a patch version bump).
    * `perf`: A code change that improves performance (triggers a patch version bump).
    * `refactor`: A code change that neither fixes a bug nor adds a feature (does not trigger a release unless it's accompanied by a BREAKING CHANGE).
    * `docs`: Documentation-only changes (does not trigger a release).
    * `chore`: Changes to the build process or auxiliary tools and libraries (does not trigger a release).
* Scope: An optional part that provides additional context about what was changed (e.g., module, component).
* Subject: A brief description of the changes.

### Handling Breaking Changes

To indicate a breaking change, the exclamation mark `!` should be used immediately after the type/scope:

* `feat!:`
* `fix!:`
* `refactor!:`
