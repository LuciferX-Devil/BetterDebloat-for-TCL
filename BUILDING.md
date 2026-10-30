# Building this source snapshot

The prepared portable wrapper is syntax-checked but has not been rebuilt in a clean environment. The tested client APK uses the production source and original local toolchain. Do not promise reproducible binaries.

Prerequisites: Python 3, JDK 17, Android SDK platform 34 and build-tools 34.0.0, and the exact dependency artifacts in DEPENDENCIES.json available in a Gradle cache. The wrapper consumes cached AndroidX artifacts rather than resolving a locked graph; use an isolated cache with only reviewed versions.

Set ANDROID_SDK_ROOT, JAVA_HOME, TVHOME_KEYSTORE, TVHOME_KEY_ALIAS and TVHOME_KEYSTORE_PASSWORD yourself. Run `python tools/build_apk.py`. Private keystores, passwords, SDK/JDK binaries and dependency archives are not bundled. Never commit credentials. Your certificate cannot silently replace the client's installed app signed by a different certificate.

The Gradle metadata is also retained, but it is not the tested APK pipeline. Review signing and dependency pinning before distributing your own builds. The custom wallpaper provenance remains unresolved; see THIRD-PARTY.md. All themes are preserved at the owner's explicit request.
