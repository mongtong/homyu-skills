---
name: "daw-plugin-ui"
description: "Design and build UI for audio plugins and music tools (AU/VST3 in JUCE, web prototypes, DAW-adjacent apps): knobs, faders, meters, A/B, presets, seed/randomize controls, keyboard and mouse conventions musicians expect. Use whenever designing, reviewing or coding a plugin editor, mixer, parameter panel or audio test page."
---

# DAW Plugin UI

Author: Hom Yu. Written while building HY Humanizer.

Musicians judge a plugin UI in the first ten seconds, with their ears on the music and one hand on the mouse. The interface must be operable without reading, must never interrupt audio, and must behave like every other plugin in their DAW. Novelty goes into the sound, not into the controls.

## 0. Before designing: three questions

1. **Where will it run?** Plugin editor inside a DAW (fixed small window, dark host UI around it), standalone app, or a web prototype for listening tests. The same control conventions apply to all three; only chrome differs.
2. **What is tweaked while listening vs. set once?** Live-tweaked controls get big knobs/faders in the main view. Set-once controls (latency mode, output routing, detection sensitivity) go in a secondary row or an "advanced" area.
3. **What is the one thing the user must see at a glance?** Usually: is it doing anything (meters / activity), and how much (the main amount control). Make that the visual centre.

## 1. Control conventions (non-negotiable)

These match Ableton Live, Logic, FabFilter, Valhalla, etc. Deviating from them reads as a bug.

| Control | Behaviour |
|---|---|
| Knob | Vertical drag (or vertical+horizontal). Drag distance ~150–200 px for full range. Mouse wheel adjusts. |
| Fader | Vertical drag; dB scale with 0 dB marked; unity detent; range -inf..+6 (or +12) dB. |
| Fine adjust | Shift (or Cmd/Ctrl) while dragging = 10× finer. |
| Reset to default | Double-click (also Alt/Option-click). Every continuous control has a default. |
| Numeric entry | Click or double-click the value readout to type a number; Enter commits, Esc cancels. |
| Value readout | Always visible with unit: `24 ms`, `-6.0 dB`, `+4 c`, `50 %`. Not just a bare number. Up to 1 decimal. |
| Pan | Horizontal slider or knob, centre detent, readout `C` / `L35` / `R20`. |
| Toggles | Clear on/off state by fill, not only by colour. |
| Segmented choice | For 2–4 mutually exclusive modes (Mono/Stereo, Mix/Live). Dropdown only when 5+. |
| Bypass | Always available; host bypass must be honoured. A/B compare belongs next to it. |

Keyboard in web prototypes: Space = play/stop, digits = solo channels, `M` = clear solos, `B` or `A` = bypass/A-B, `R` = randomize/new seed, `?` = show shortcuts. Never steal keys while a text/number field has focus.

Mouse must never cause a host-level problem: no native alerts, no modal dialogs during playback, no cursor capture that survives the window losing focus.

## 2. Audio feedback is part of the UI

- **Meters** on every output path that matters: peak with hold (~1 s decay), clip indicator that latches until clicked or ~2 s. Scale -48..0 dBFS (or -60..+6 for faders). Green → amber (-12) → red (-1). Peak-hold line, not just a bar.
- **Activity indicators** for analysis-driven features (onset detected, pitch locked, gate open) so the user can tell "it heard me" from "it is broken".
- **Latency** is shown numerically when the plugin reports any (`Latency 48 ms · compensated by host`). A mode that changes latency says so before you pick it.
- **Loudness-matched A/B**: when comparing processed vs. dry, match RMS or offer a "match" button. Louder always sounds better and ruins the comparison.
- **Nothing on the audio thread for the UI.** Meters read atomics/lock-free FIFOs at 30–60 fps on a timer. Never allocate, lock, log or touch the GUI from the audio callback. (In JUCE: `Timer` + `std::atomic<float>` or `AbstractFifo`; in web: `AnalyserNode`, or an AudioWorklet posting to the main thread.)

## 3. Seed, randomize and reproducibility controls

Many music tools use randomness (humanize, doublers, generative). Expose it like this:

- A visible **Seed** number field (0–9999 is friendly) with ◀ ▶ steppers and a 🎲 **new seed** button. Same seed must always give the same result, including after save/reload.
- Label the dice in domain language ("New band", "Re-roll players"), not "Randomize" alone.
- A **favourites** row to pin seeds the user liked, each showing the seed and the key setting it was heard with.
- Changing seed glides or crossfades; it never clicks or stops audio.
- Separate **how much** randomness (amount knobs) from **which** randomness (seed). Never let an amount knob reshuffle the draw.

## 4. Layout for a plugin window

- Fixed aspect, default around 640×400 to 900×520 px for an effect; resizable with constrained ratio if possible. Everything must be legible at 100 % on a 1080p laptop.
- **Header strip**: name, preset browser (prev / next / menu / save), A/B, bypass, seed if any. Settings (gear) at the far right.
- **Main area**: the 3–6 controls that shape the sound, as knobs in one or two rows, largest = most important. Group by what they do ("Players", "Timing", "Output"), with a 12-px letter-spaced caption above each group.
- **Footer**: meters, latency, output mode, tiny version string.
- Spacing scale 4/8/12/16/24. Knob labels ABOVE (caption) and value BELOW. Captions 11–12 px uppercase, values 12–13 px monospace, body 13–14 px.
- Minimum hit area 28×28 px for any control, 36 px for anything used while performing.

Web prototypes for listening tests additionally get: a drop zone for the user's own file, a waveform with event/marker overlay, a channel mixer (fader + pan + M/S per rendered path, master with limiter), and a metrics panel if the DSP has measurable claims. Export in formats a DAW opens (WAV 24-bit; zip several files).

## 5. Visual language

- **Dark by default**: DAWs are dark; a bright plugin window glares. Background ≈ #15–#20 lightness, panels one step lighter, one accent colour for "value / active", one warning colour. Text contrast ≥ 4.5:1 for values, ≥ 3:1 for captions.
- Use the accent on the *filled* part of knobs and faders so the setting is readable from across the room.
- Avoid photoreal skeuomorphism and avoid pure flat grey. A subtle recessed track and a crisp light thumb is enough.
- Typefaces: one condensed display face for the product name, one humanist sans for labels, one monospace for numbers. Load fonts locally or from an allowed CDN; never block first paint on fonts.
- Colour-code channels/takes consistently across mixer, waveform overlays and tables (same hue for Take 2 everywhere).
- Icons: SVG, 16–20 px, always with a tooltip or label. No emoji in production plugin UI (fine in prototypes).
- Animation only for state change (meters, glide of a value on preset load, 120–180 ms). No decorative motion; respect `prefers-reduced-motion`.

## 6. State and presets

- Everything the user can touch is a host parameter (automatable) or saved in plugin state. Seed, mode switches and mixer gains included.
- Preset = all parameters + seed. Loading a preset glides values; it must not click.
- Keep an undo for destructive actions in the UI (reset all, randomize all).
- When a control's effect requires offline re-analysis (sensitivity, model choice), say so in its caption and auto re-run after the user releases the control; keep playing the previous result until the new one is ready.

## 7. Review checklist (run before shipping any plugin UI)

1. Every continuous control: drag works, Shift-fine works, double-click resets, value has a unit.
2. Nothing in the UI can glitch audio (profiled with the editor open and closed).
3. Meters show signal within 50 ms of it arriving; clip latch works.
4. Bypass and A/B are one click away and loudness is comparable.
5. Same seed/preset → identical sound after save, close, reopen.
6. Readable at 100 % scale on a 13" laptop; nothing truncated in Traditional Chinese or German labels.
7. Keyboard focus never trapped; host shortcuts (Space, Cmd-Z) reach the DAW when the plugin does not use them.
8. Window resize (if supported) keeps proportions and does not re-trigger analysis.
9. Light-on-dark contrast checked for captions, not only values.
10. Tooltips or a `?` overlay explain every non-obvious control in one sentence.

## 8. JUCE specifics

- Build controls on `juce::Slider` with a custom `LookAndFeel_V4` subclass; set `RotaryHorizontalVerticalDrag`, `setDoubleClickReturnValue(true, default)`, `setMouseDragSensitivity(180)`, `setVelocityModeParameters` for fine control, `TextBoxBelow` with `setNumDecimalPlacesToDisplay(1)`.
- Bind with `AudioProcessorValueTreeState::SliderAttachment / ButtonAttachment / ComboBoxAttachment`; the attachment owns gesture begin/end so host automation records correctly.
- Meter data: audio thread writes `std::atomic<float>` peak; editor `Timer` at 30 Hz reads and repaints only the meter component (`repaint(bounds)`), not the whole editor.
- Prefer vector drawing (`Path`, `drawRoundedRectangle`) over bitmaps so Retina scaling is free.
- WebView editors are acceptable for prototypes; for release, native components avoid host webview version drift.

## 9. Web-prototype specifics

- Decode with `AudioContext.decodeAudioData`; keep all heavy DSP in a `Worker` (post `Float32Array` with transfer) so sliders stay responsive; render progress in a thin bar.
- Play all compared paths simultaneously through per-channel `GainNode → StereoPannerNode → AnalyserNode` so switching A/B or solo is sample-synchronous and position is never lost.
- Master chain ends in a `DynamicsCompressor` set as a brickwall (-1 dB, ratio 20, 1 ms attack) so experiments cannot clip the user's monitors.
- Vertical faders: rotate a native `<input type=range>` -90° inside a fixed-height box; style the track fill with a CSS variable.
- Save exports via the host's download capability; where only certain extensions are allowed, zip the WAVs.