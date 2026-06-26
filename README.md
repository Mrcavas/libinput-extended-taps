# `libinput` didn't support 4 fingers, so I patched it

This patch enables the use of 4-finger taps by remapping `clickfinger` click method:

- `lrm` becomes `BTN_LEFT`, `BTN_RIGHT`, `BTN_EXTRA`, `BTN_MIDDLE`
- `lmr` becomes `BTN_LEFT`, `BTN_RIGHT`, `BTN_MIDDLE`, `BTN_EXTRA`

---

The patch also forces 1-finger drag to be enabled. This allows to remove the delay between a 3-finger tap and its action by disabling drag while keeping the much more important 1-finger drag.
