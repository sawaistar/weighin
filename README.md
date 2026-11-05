# Weigh-in

**Know what an Android library costs your app — before you add it.**

Type any Maven coordinate (e.g. `com.squareup.retrofit2:retrofit:2.11.0`) and get a one-page report.

## What it measures

- **Size** — dex size and method count incl. transitive deps, the minimum R8 must keep (consumer rules), resources, assets, native libs per ABI
- **Manifest impact** — permissions, activities, services, receivers, providers; startup initializers flagged
- **16 KB readiness** — ELF alignment check of every `.so` (required by Google Play from Feb 2027)
- **Requirements** — minSdk, minCompileSdk, AGP and Kotlin versions, JVM target
- **Dependency tree** — what it pulls in, and the *net new* cost if you already use AndroidX/Kotlin
- **Version history** — how all of the above changed across releases

## How it works

For each library, a worker generates a throwaway probe app, builds it with and without the dependency, and diffs the outputs (APK, merged manifest, native libs). Published versions are immutable, so each result is computed once and cached.

```
Web (Next.js) ──► API ──► Job queue ──► Analyzer worker (JDK + Android SDK + Gradle)
                    │                          │
                    └──────── Postgres ◄───────┘
```

## Roadmap

- [ ] Analyzer CLI: one coordinate → JSON report
- [ ] Website: search, report page, shareable links
- [ ] Pre-compute top ~300 Android libraries
- [ ] Compare two libraries or two versions
- [ ] README badges for library authors
- [ ] Android Studio plugin: cost inline in `libs.versions.toml`
- [ ] Gradle/CI check: fail PRs on new permissions, 16 KB breakage or size budget

## Related work

[apkscale](https://github.com/twilio/apkscale) and [app-sizer](https://github.com/grab/app-sizer) measure size inside your own project. Weigh-in is a public lookup for *any* library and version, and covers manifest impact, 16 KB compliance, requirements and history — not just size.

## License

MIT
