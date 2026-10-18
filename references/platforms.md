# Build-System Recipes

Choose the repository's existing commands and targets. These are starting points, not universal timing claims or substitutes for real consumer behavior tests.

## SwiftPM and Xcode

For a truly standalone Swift package, `cd` into it and run `swift test`. If the host is another Swift package, run its focused `swift test` too; an iOS/macOS app instead needs its actual Xcode test scheme and platform destination. Look in CI or list the project's schemes (`xcodebuild -list` with its `-project` or `-workspace`) before guessing a command. A filtered `swift test --filter ...` limits test execution but may still compile unrelated targets on a cold build; do not equate test filtering with build isolation.

Keep the real adapter in the consuming target and run its test after mapping or behavior changes. Check iOS behavior on iOS, not solely in macOS SwiftPM. Xcode's [mergeable libraries](https://developer.apple.com/documentation/xcode/configuring-your-project-to-use-mergeable-libraries) can preserve dynamic dependencies for development and merge eligible ones in optimized release builds; verify resources, linking, and release output. They change packaging, **not** the need to test host behavior, and a Swift package does not become mergeable merely by declaring a dynamic product.

## Gradle and Kotlin Multiplatform

Use CI's actual test commands or discover tasks (for example, `./gradlew :shared:tasks --all`) and run the relevant focused task; `:shared:jvmTest` is an example only when that JVM target exists. Running a test in one module of a multi-project build may still configure much of the root build. A standalone [included build](https://docs.gradle.org/current/userguide/composite_builds.html) can have isolated configuration and be consumed together with the host via dependency substitution, but its dependency versions and build settings need alignment.

Test the real Android/iOS host mapping and affected platforms: common or JVM tests alone do not verify iOS-specific behavior. [KMP test guidance](https://kotlinlang.org/docs/multiplatform/multiplatform-run-tests.html) distinguishes common, Android, and iOS tests; Android local unit tests run on the JVM, not an Android device.

## TypeScript

With [project references](https://www.typescriptlang.org/docs/handbook/project-references.html), use the project's configured `tsc -b` entry point or `tsc -b path/to/core/tsconfig.json`; referenced projects need `composite`. Run the host project's configured build and its real consumer tests. Unchanged declarations can avoid some dependent type-checking, but a changed function body can still break host behavior; a build graph never chooses all needed behavior tests.

## Rust

For a Cargo workspace, use `cargo test -p capability` for a core loop and `cargo test -p consumer` for affected callers (replace these with real package names). Cargo may rebuild dependent crates when source changes; the compilation result cannot establish application behavior. See the [Cargo test reference](https://doc.rust-lang.org/cargo/commands/cargo-test.html).

## Graph-based CI

Tools such as [Bazel](https://bazel.build/concepts/dependencies) and [Nx affected](https://nx.dev/docs/features/ci-features/affected) can avoid unrelated builds when declared inputs and dependencies are accurate. Use their graph to choose builds; add behavior-based consumer and platform tests for changes whose output matters even when interfaces appear unchanged. Missing declared inputs can make a green cache result false evidence.
