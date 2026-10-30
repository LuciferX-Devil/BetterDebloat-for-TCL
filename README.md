# BetterDebloat for TCL

A model-specific adaptation of [Gathin23's CleanYourTV](https://github.com/Gathin23/cleanyourtv). Credit to **Gathin23 (Gathin)** for the original project and prompt that started this work. The original MIT copyright and license are retained. This adaptation is independent and is not endorsed by Gathin23, TCL, Google or NVIDIA.

**Community build for the exact tested model/firmware only. Use at your own risk.** Tests and antivirus results are not a security certification. Read [SECURITY.md](SECURITY.md) and [THIRD-PARTY.md](THIRD-PARTY.md) before redistribution.

## Downloads

- [USB build ZIP](CLEANYOURTV-TCL55C845-public-cba36bb0.zip?raw=true): extract the entire `CLEANYOURTV-USB` folder to a trusted USB drive. Initial TV installation, local ADB setup and owner approval are still required; see Before use.
- [Reviewed source snapshot](BetterDebloat-for-TCL-source-cba36bb0.zip?raw=true): extract it to inspect or build the launcher; includes source, tests, build wrapper, dependency inventory and license texts. No signing key is supplied.
- [SHA-256 checksums](SHA256SUMS.txt). Both APK filenames in the USB kit contain the same tested APK: `cba36bb07aa8eb0712e601ce27236863f778ccd61d4b710e68750a36759d5a2e`.

Files are distributed from the repository; no GitHub Release has been published. The USB archive was repackaged with public documentation. Its embedded APK and all theme assets are unchanged from the tested build. VirusTotal's APK result applies to that exact APK, while the previous ZIP result applies only to the earlier ZIP hash.

## Supported device

**Only TCL 55C845 is supported by this adaptation.** Testing used the G08 platform, Android 14, firmware V8-T653T01-LF1V669. These are compatibility details, not a device serial number. Other TCL models and firmware versions are untested. Do not apply this package list to another model.

**NVIDIA Shield TV might be supported in a future version.** There is no working Shield build, compatibility claim, schedule or promise yet. Do not apply the TCL policy to a Shield.

## What it does

- Reversibly disables the vetted 33-entry package policy and makes the custom launcher Home. Packages are disabled, not uninstalled; absent packages are skipped. The executor also switches stock Home, so its operation totals differ from the Self-Test's 33 policy checks.
- Provides a local launcher, themes, wallpaper options, dock navigation, hardware statistics and access to installed TV inputs/Live TV. It does not provide subscription content or streaming-service APKs.
- Provides TV-side Start daemon, Reset Debloat and Reapply Debloat and Tweaks actions.
- Detects a complete kit on a mounted USB, reapplies it, and automatically opens Self-Test with a separate completion card above the results. Errors stay visible for photos.
- Watches the selected USB so removal of an unrelated drive does not dismiss the report. Use system Eject and wait for the TV to confirm safe removal before unplugging. Report dismissal can take several seconds.
- Stops the one-shot shell executor after USB apply. This does not turn off Android's ADB service.

Intentional losses: **Google voice search/Assistant interaction and using a phone as an Android TV remote**, because `com.google.android.katniss` and `com.google.android.tv.remote.service` are disabled. The physical remote is separate. Chromecast/Miracast, TV inputs, Gamebar/VRR, hearing-aid/audio support, BT Solo Listen and Gallery packages are retained; every hardware function has not been exercised.

## How it works

The launcher creates its own RSA ADB identity in Android app-private storage. After normal owner approval on the TV, its ADB client authenticates to `127.0.0.1:5555` and starts a shell script as **Android shell UID 2000**. This is not root. Device-admin status alone does not grant shell permissions. No PC private key is embedded or copied into the package.

The executor validates package names against its built-in policy, stages the USB kit in private shell storage, and invokes Android package-manager commands to disable/re-enable packages and change Home. USB APK installation checks the expected package name and uses Android's normal signature checks. Configuration, command and status files cross the app/shell storage boundary; read the security limitations below.

A bounded transaction receipt survives the launcher being killed by its own APK update. The new process reads that receipt and opens the automatic completion report. Receipt consumption and filename validation are tested. A previously reported one-byte read was caused by counting read calls instead of bytes; the corrected reader accounts for actual bytes. The earlier attribution to TV FUSE storage is withdrawn.

## Before use

1. Confirm the exact TV model and tested firmware. Save a separate copy of your installer and settings. This is **not a full TV or firmware backup**.
2. Complete trusted initial app installation, device-admin setup and debugging authorization with the owner present. USB auto-apply is not a universal fresh-TV installer: the launcher, working local ADB endpoint and approved app key must already be configured.
3. Approve only the expected app/computer on authorization dialogs. Never import somebody else's ADB private key or approve an unexplained request.
4. Use a trusted LAN. Do not port-forward ADB. Tailscale is optional remote-maintenance infrastructure and is not required for a configured TV-local workflow.

## Reapply and Reset

- For built-in Reapply, open TV Setup, use Start daemon when necessary, select Reapply Debloat and Tweaks, select the built-in package, review the confirmation, then Apply.
- For USB Reapply, copy the **whole `CLEANYOURTV-USB` folder** onto a trusted drive. Keep `AUTOAPPLY`, both APK filenames, `debloat.txt`, `config.json` and `wallpaper.jpg` together. A replacement drive does not need the previous drive's UUID. Do not put an app's private identity on the drive.
- The applied marker prevents an endless apply loop. Use Reset Debloat before expecting another complete automatic apply. Reapply can replace settings with the selected kit defaults; preserve your own customization separately.
- Read the automatic Self-Test. Its completion card permits requesting **system Eject**; it does not prove the filesystem has already unmounted. Unplug only when the TV confirms safe removal.
- Reset Debloat restores recorded package states and stock Home. It is not a factory reset or firmware rollback and does not restore other apps' private data. Its ledger is not an unlimited history of all original states.

## Sanitization and functionality

The shared copy excludes runtime keys, serials, real USB/network identifiers, PC paths, account names, debug logs, test screenshots, owner backups and live preferences. Model-specific package rules, generic config, loopback addresses and themes are functional components, not personal credentials.

**Sanitization does not delete anything from the installed TV.** Existing approvals, app logins, config and mappings stay in place. A new installation creates a private key and needs approval on that TV. Deleting the installed private key would break saved authorization; removing all theme/config files would remove features. A separate allowlisted folder avoids both problems.

The default dock is YouTube, Netflix, Disney+ and Prime Video. It contains no account credentials and does not include those services' APKs.

## Known security limitations

1. **Network ADB:** the configured endpoint also listens on network interfaces. Authentication helps, but stopping the executor or removing a VPN does not close ADB. Do not describe this as a fully isolated local-only service.
2. **Shared-file IPC and USB trust:** allowlisting, path validation and private staging reduce exposure. They do not prove that malicious local apps or modified USB content can never interfere. A familiar filename does not establish trust.
3. **Signing:** this build retains its existing debug certificate for update compatibility. Its private keystore is excluded. Future signing changes require a deliberate migration plan; changing certificates can break install-over updates.
4. **Privileged changes:** the approved shell identity can make consequential package changes. Grant it only to a trusted build. Do not disable authentication, SELinux or platform protections to make it work.
5. **Firmware/recovery:** an update may invalidate package names or shell assumptions. There is no full recovery image, root backup, guaranteed downgrade or promise that Reset undoes a firmware update.
6. **Restoration scope:** other tools can alter package states outside the executor's ledger. Keep backups and review changes if multiple tools are used.
7. **Assurance limits:** tests and antivirus cannot prove every theme, remote button, permission boundary, dependency or hardware feature safe. A clean VirusTotal score would not change this.

## Validation

The corrected candidate passed **73 Java checks**. A fresh live regression passed GUI Reset, GUI Start with real UID/GID 2000, built-in Reapply, a second GUI Reset, mounted-stick USB Reapply, self-update and automatic Self-Test (33/33, 22 ok, 4 info). Config, wallpaper and remote settings matched the baseline afterward. The executor exited and private USB staging was absent. See [VALIDATION.md](VALIDATION.md).

The USB leg used a real mounted drive and the normal application entry point; it was not physical reinsertion or a reboot test. Physical ejection/dismissal and a complete repeat of every theme, button, casting/audio/HDMI/VRR feature remain owner acceptance tests. No flawless-operation or exhaustive-security claim is made. Remote-maintenance status is installation-specific and is not required for a configured local workflow.

## Dependencies, rights and reporting

`DEPENDENCIES.json` records the actual build artifacts and hashes. The source archive and USB archive retain the `licenses/` texts. `THIRD-PARTY.md` records unresolved provenance for the custom Glass/default gradient. The owner requested that the themes remain unchanged. Its creator and redistribution permission have not been verified; no rights clearance is claimed. Credit alone does not resolve this item. BetterGlass is not attributed to Gathin23 without supporting provenance. A local-cache build is not a reproducible supply-chain guarantee.

Report suspected vulnerabilities through [GitHub private vulnerability reporting](https://github.com/LuciferX-Devil/BetterDebloat-for-TCL/security/advisories/new). `SECURITY.md` describes the reporting process. Do not publish keys, owner logs or unpatched exploit details in public issues.
