# ToolNeuron Development Guide

## Project Overview

Offline AI assistant for Android. Runs LLMs (llama.cpp), generates images (SD 1.5 via LocalDream), RAG document search, TTS, and more — all on-device. Current version: **2.0.3**

## Repository Structure

```
ToolNeuron/
├── app/                    # Main Android application (Kotlin + Compose)
├── ums/                    # Unified Memory System — binary record storage with JNI + BoringSSL
├── neuron-packet/          # Encrypted RAG packet format with access control
├── memory-vault/           # Legacy encrypted storage (read-only, for migration)
├── system_encryptor/       # Native encryption primitives
├── file_ops/               # Native file operations
├── docs/                   # Documentation (docs/MemoryVault.MD for storage docs)
├── libs/                   # Local AARs (gguf_lib, ai_sd, ai_supertonic_tts)
└── README.md               # Full feature overview
```

## Build Configuration

| Setting | Value |
|---------|-------|
| compileSdk | 35 |
| minSdk | 29 |
| targetSdk | 35 |
| versionCode | 30 |
| NDK abiFilters | arm64-v8a, x86_64 |
| Java | JDK 17 |
| Gradle heap | 12048m (preconfigured in gradle.properties) |

## Architecture

| Layer | Technology |
|-------|-----------|
| Language | Kotlin, C++ (JNI) |
| UI | Jetpack Compose + Material 3 |
| Text inference | llama.cpp (gguf_lib-release.aar) |
| Image inference | LocalDream (ai_sd-release.aar) |
| TTS | Supertonic (ai_supertonic_tts-release.aar) |
| Database | Room + UMS (custom binary format) |
| Encryption | AES-256-GCM, Android KeyStore, BoringSSL |
| DI | Dagger Hilt (KSP) |
| Async | Kotlin Coroutines + Flow |
| Networking | OkHttp + Retrofit |
| Documents | Apache POI, PDFBox, epublib |
| Serialization | Kotlinx Serialization, Gson |

## Key Modules

- **app** — Main application; activities (MainActivity, ModelPickerActivity, ModelLoadingActivity, RagActivity, RagDataReaderActivity), services (LLMService, ModelDownloadService)
- **ums** — UMS protocol implementation with JNI/BoringSSL for encrypted binary storage
- **neuron-packet** — Encrypted RAG packet format
- **memory-vault** — Legacy storage (read-only, for migration only)
- **system_encryptor** — Native AES-256-GCM encryption
- **file_ops** — Native file operation wrappers

## Build Commands

```bash
# Debug
./gradlew assembleDebug
./gradlew installDebug

# Release
./gradlew assembleRelease

# If OOM: already configured in gradle.properties (12048m heap)
# To change: edit org.gradle.jvmargs in gradle.properties
```

## Requirements

- Android Studio Meerkat (2025.1.1)+
- JDK 17
- Android SDK 35, NDK 26.x
- Local AARs in `libs/` directory (gguf_lib-release.aar, ai_sd-release.aar, ai_supertonic_tts-release.aar)

## Key Conventions

1. **Test on real devices** — emulators don't reflect real performance for LLM inference
2. **No cloud dependencies** — everything offline by default
3. **No broad storage permissions** — use SAF (Storage Access Framework) via file picker
4. **JNI code** lives in `ums/`, `system_encryptor/`, and `file_ops/` modules with CMake
5. **Per-model configs** are saved to Room database — not hardcoded
6. **.rag / .RAG files** — handled via intent filter in RagActivity

## Package Structure (app/src/main/java/com/dark/tool_neuron)

- `activity/` — MainActivity, ModelPickerActivity, ModelLoadingActivity, RagActivity, RagDataReaderActivity
- `service/` — LLMService (foreground), ModelDownloadService
- `database/` — Room database + DAOs (AiMemory, Model, ModelConfig, Rag, Knowledge, Persona)
- `engine/` — GGUFEngine, DiffusionEngine, EmbeddingEngine
- `models/` — Data classes (table_schema, engine_schema, plugins, enums)
- `plugins/` — PluginManager, Calculator, DateTime, DevUtils, FileManager, NotePad, SystemInfo, WebSearch
- `repo/` — Repository layer + UMS repositories
- `tts/` — TTSManager, TTSSettings
- `ui/` — Compose components, icons

## Working with Native Code

Native modules use JNI + CMake:
- **ums** — Binary record storage (UMS protocol), links BoringSSL
- **system_encryptor** — AES-256-GCM encryption primitives
- **file_ops** — File operation wrappers

Rebuild native code with NDK 26.x if modifying C++.

## Testing Notes

- UI tests use Jetpack Compose testing frameworks
- Integration tests should use real device storage (not mock SAF)
- LLM inference tests need actual GGUF models to load

## Contributing

1. Fork and create feature branch: `feature/your-feature`
2. Make focused commits with clear messages
3. Test on real hardware
4. PR description should cover what changed and how you tested it

## Security

- Report vulnerabilities privately — email siddheshsonar2377@gmail.com
- Don't open public issues for security bugs