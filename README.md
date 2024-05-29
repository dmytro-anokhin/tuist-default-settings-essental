# tuist-default-settings-essental
Sample project showing mergeable libraries settings not applied with using `DefaultSettings.essential`: https://github.com/tuist/tuist/issues/6348

--- 

### What happened?

When generating a target with `DefaultSettings.essential` settings, build settings `MERGED_BINARY_TYPE` and `MERGEABLE_LIBRARY` not set.

```
let settings: Settings = .settings(
    base: [
        "PROJECT_BASE": "PROJECT_BASE",
    ],
    configurations: [
    ],
    defaultSettings: .essential
)

let project = Project(
    // ...
    targets: [
        .target(
            name: "App",
            destinations: .iOS,
            product: .app,
            // ...
            dependencies: [
                .target(name: "Framework"),
            ],
            settings: settings,
            mergedBinaryType: .automatic
        ),
        .target(
            name: "Framework",
            destinations: .iOS,
            product: .framework,
            // ...
            settings: settings,
            mergeable: true
        ),
    ]
    // ...
)
```

### How do we reproduce it?

See the sample project: https://github.com/dmytro-anokhin/tuist-default-settings-essental

1. `tuist generate`;
2. Explore **App** and **Framework** build settings.

Result:

1. `MERGED_BINARY_TYPE` on **App** target is not set;
2. `MERGEABLE_LIBRARY` on **Framework** target is not set.

Expected:

1. `MERGED_BINARY_TYPE = automatic`;
2. `MERGEABLE_LIBRARY = YES`.

### Error log

None

### macOS version

14.5

### Tuist version

4.15.0

### Xcode version

15.3
