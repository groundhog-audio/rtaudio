# Local Patches

This fork (`groundhog-audio/rtaudio`) carries the following deviations from
upstream `thestk/rtaudio`:

## Fix ghost ASIO driver hang by validating HFILE properly

**File:** `include/asiolist.cpp`

The bundled ASIO SDK shim used `if (hfile)` to validate the result of the
Win16 `OpenFile()` API. `OpenFile` returns `HFILE_ERROR` (`-1`) on failure,
which is truthy, so ghost/invalid ASIO driver registry entries (left behind
by uninstalled audio software) passed the check and caused
`loadAsioDriver` to hang the application indefinitely during device
enumeration. Changed to `if (hfile && hfile != HFILE_ERROR)` to reject both
sentinels explicitly (matches the documented Win16 API contract).
