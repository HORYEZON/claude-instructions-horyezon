# Modals lock background scroll

Any `fixed inset-0` overlay — modal, dialog, lightbox, confirm sheet, menu overlay, search overlay, on the public site or in the admin — must call `useLockBodyScroll(isOpen)` from `hooks/useLockBodyScroll.tsx`.

**Why:** without it a wheel or touch scroll that starts on the overlay (or once the overlay's own scroll area hits an end) falls through to the page behind, so the site slides around underneath a dialog that stays put. The user has reported this more than once; treat it as a default, not a per-modal decision.

**How to apply:** when adding or reviewing any overlay, check for the hook before anything else. It is reference-counted at module level, so nested and overlapping modals are safe — don't hand-roll `document.body.style.overflow`. To audit: list files matching `aria-modal="true"` or `fixed inset-0` and check each for `useLockBodyScroll`.
