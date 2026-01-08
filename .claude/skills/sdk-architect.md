# Skill: Android SDK Structural Audit

## Purpose
Used to identify the architectural patterns, entry points, and public API surface of a legacy Android SDK.

## Procedure
1. **Locate Entry Points:** Search for classes annotated with `@Keep` or defined in `AndroidManifest.xml`. Look for "Manager", "Client", or "Instance" naming conventions.
2. **Map Public API:** List all `public` classes and methods in the primary package. Identify if the SDK uses a Singleton pattern or Factory pattern for initialization.
3. **Analyze Module Context:** Check for `build.gradle` (Groovy) or `build.gradle.kts` (Kotlin) to identify if this is a monolithic SDK or a multi-module project.
4. **Identify Core Dependencies:** Look for DI (Dagger/Hilt/Koin), Networking (Retrofit/OkHttp), and Concurrency (RxJava/Coroutines).
5. **Output Format:** Produce a Mermaid diagram of the project structure and a list of "Critical Paths" for the developer to review.
