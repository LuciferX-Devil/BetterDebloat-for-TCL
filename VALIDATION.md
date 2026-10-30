# Validation record

```json
{
  "date": "2026-10-30",
  "apk_sha256": "cba36bb07aa8eb0712e601ce27236863f778ccd61d4b710e68750a36759d5a2e",
  "java_checks": "73 passed; 0 failed",
  "live_gui_reset_start_builtin_reset_usb": "passed",
  "actual_daemon_uid_gid": 2000,
  "automatic_selftest": "33/33; 22 ok; 4 info",
  "owner_state_preserved": true,
  "daemon_stopped": true,
  "not_tested": [
    "physical post-reboot insertion",
    "physical eject/removal dismissal",
    "every theme/button and hardware path"
  ],
  "receipt_navigation_setup": "Archived previous completion receipt only; not a physical USB-removal test"
}
```

This records the tested APK, not a new clean-environment source rebuild. The public archive only changes documentation and packaging; the APK and theme bytes are unchanged. Subsequent owner acceptance reported the TV working.
