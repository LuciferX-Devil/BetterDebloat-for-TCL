# Dependency and redistribution review

- Original project/prompt/launcher basis: Gathin23 (Gathin), https://github.com/Gathin23/cleanyourtv. The retained original MIT license was compared with upstream on 2026-10-30. Preserve the copyright and permission notice. This derivative is independent.
- AndroidX: Apache-2.0; the actual build selected 18 AARs and 5 JARs from the cache. A Kotlin standard-library JAR was also supplied to D8. Exact coordinates, versions, hashes and locally available POM license declarations are in DEPENDENCIES.json. Pin and verify artifacts before a public release.
- Kotlin: Apache-2.0; retain its license and relevant notices. Android SDK/JDK/build tools are prerequisites and are not redistributed.
- Outfit typeface: copyright The Outfit Project Authors, SIL Open Font License 1.1. The original font license is retained.
- 22 of 24 raster asset files matched their corresponding files in upstream byte-for-byte on 2026-10-30. ASSET-PROVENANCE.json records paths and hashes. Matching an upstream asset under its repository license is provenance evidence, not an independent warranty of all underlying rights.
- **Unresolved asset rights:** the build-package wallpaper and glass-theme wallpaper are the same custom abstract-gradient image and did not match current upstream files. Its creator/license has not been independently established. The owner explicitly requested that all themes be distributed unchanged. The image remains included, but neither its creator nor permission has been established. This is an unresolved exception, not a claim of rights clearance. Gathin23 is credited for the verified upstream project/assets; BetterGlass and this unmatched image are not automatically attributed to them. Contact the repository owner about attribution or rights concerns.
- Vendor/service marks and package identifiers describe compatibility only. No vendor firmware or third-party streaming-service APK is included. Third-party services remain subject to their own terms.

An OSV lookup of the 24 exact Maven dependency artifacts on 2026-10-30 returned no listed vulnerability matches. This is database coverage for those versions, not an audit or proof that no vulnerabilities exist. Retain any artifact-specific NOTICE/license requirements and complete the asset-rights review before distribution. Do not claim full redistribution clearance.

The adaptation changes include the TCL-specific policy, authenticated TV-local shell bootstrap, USB transaction/report workflow and launcher behavior. They are not upstream-endorsed features.

License texts are included in the `licenses/` directory inside both download archives. The original MIT notice does not establish rights to unidentified third-party artwork.
