# TraceSonic Help

## Canvas and playback

- TraceSonic is a pixel-driven experimental additive synthesizer. Horizontal position is time; vertical position is logarithmic pitch, from **Base note** to four octaves above it. Luminance × opacity sets amplitude; black and transparent pixels are silent.

- The audio grid has 512 time columns and one row per oscillator, with amplitude interpolated between columns. Canvas storage and PNG export are 2048 pixels wide and at least 512 pixels high, depending on oscillator count (see Oscillators).

- **Run / Stop:** standalone playback.
- **Pass length:** 0.05–99.99 seconds without host transport; ± steps by 0.1 seconds.
- **Loop length:** 1–64 beats with host transport, at host tempo.
- **Scan direction:** forward, reverse or ping-pong.
- **Base note:** transposes the oscillator bank without changing the image.

Rulers show pitch and seconds or beats. Faded pitch labels mark frequencies above the audio cutoff (`0.45 × sample rate`); they stay editable but are silent. Note names use Yamaha numbering: MIDI 60 is **C3**.

### Paint and Perform

The **Canvas mode** button next to Undo/Redo switches between:

- **Paint:** draw with the selected brush. **Eraser Brush** removes its footprint, including harmonic trails, without the brush's level fade.
- **Perform:** hold or drag up to five touches to play local regions without editing. Each region loops on its own over the current pass length and scan direction. The brush sets region size and round or square footprint. Active touches replace the full-canvas scan; releasing them returns to normal playback or the release tail.

- **Undo / Redo** covers strokes, patterns, imports, transforms, region processing and **Erase Canvas**. Shortcuts: **Cmd+Z / Cmd+Shift+Z**. Editing works during playback.

## Brushes

- Widths are in semitones (`st`), independent of window size and oscillator count. Fine brushes stay about 0.38 st wide at any count, so they cover more than one row at high counts.

### Soft, textured and hard bands

- **Soft round / Wide soft round / Veil:** about 1.5 / 3 / 6 st, soft edges.
- **Grain / Wide grain:** 3 / 6 st, irregular edges, levels and gaps.
- **Spray:** 7 st, sparse coverage.
- **Fine / Small / Medium / Wide hard round:** 0.38 / 1 / 2 / 4 st, hard edges.
- **Hard square:** 3 st, square footprint.

### Harmonics and intervals

- Ratios are relative to the drawn fundamental. Upper partials are narrower and quieter; partials outside the canvas are clipped. Widths are for the fundamental.

- **Soft harmonics:** ~1.5 st; ratios 1–5.
- **Harmonic thread:** ~0.38 st; ratios 1–8.
- **Narrow harmonics:** ~1 st; ratios 1–6.
- **Full harmonics:** ~1.5 st; ratios 1–6.
- **Odd harmonics:** ~0.75 st; ratios 1, 3, 5, 7, 9.
- **Fifth dyad:** ~0.75 st per trail; ratios 1 and 1.5 at equal level.

- Non-sine waveforms add further harmonics to the painted partials.

### Shaped bands and harmonics

- Amplitude and width follow distance along the stroke, including vertical travel. Each stroke restarts the envelope. For forward playback, draw left to right for a decay, right to left for a swell.

- **Fading line / Fading odd harmonics:** ~0.38 st fundamental, fast decay; ratios 1, 3, 5, 7.
- **Blooming band / Blooming harmonics:** widens, slower decay; up to 3 / 1.5 st; ratios 1–6.
- **Widening band / Widening harmonics:** widens and fades, up to 1 st; ratios 1–7.
- **Narrowing band / Narrowing harmonics:** starts at 1 st, narrows and fades; ratios 1–7.
- **Symmetric band / Symmetric harmonics:** fades at both ends of the stroke; up to 1 st; ratios 1–6.
- **Wide symmetric band / Wide symmetric harmonics:** same envelope, up to 2 / 1.5 st.

### Pitch snap

- Turn on **Snap strokes to the scale**, then pick a **Pitch snap scale**. Snap constrains the stroke's center line relative to Base note; brush width and harmonic trails can extend outside the scale. Scales: chromatic, major, natural and harmonic minor, Dorian, Phrygian, Lydian, major and minor pentatonic, whole tone, octatonic.

## Patterns and images

- **Pattern** draws a preset image. **OVR off** adds it to the canvas; **OVR on** replaces the canvas. OVR also applies to **Open image…**. Patterns change pixels only, not synthesis or MIDI settings.

- Groups: tuned/harmonic, rhythmic/irregular, gesture/glissando, single/unique, braided sweeps and combined. **Channel cycle**, **Shared rows** and **Register relay** demonstrate MIDI routing; select the matching routing or load the matching factory patch.

- **Open image…** stretches and resamples the image onto the audio grid. A 4:1 image matches the default canvas proportions. Color affects luminance only, not waveform.

- **Export image…** writes white pixels, with amplitude stored as transparency. Load **Init → Pitch Guide** for octave, fifth and major-third reference marks.

## Canvas processing

- **Canvas** operations apply to the whole painting.

- **Transpose:** ±1, ±5, ±7 or ±12 st; rounds to source pixels and clips at the pitch range.
- **Shift in time:** ±1/8, ±1/4 or ±1/2 pass, wrapping around.
- **Reverse time / Invert pitch:** horizontal / vertical mirror.
- **Invert amplitude:** `1 − amplitude`.
- **Rotate 90°:** swaps bitmap dimensions; rotating back restores the source.
- **Other degrees:** ±15°, ±30° or ±45°; interpolated and clipped, uncovered areas are silent.
- **Fade / Amplify:** ×0.5 / ×2 gain; Amplify clips at full amplitude.

### Regions and echo

- Choose **Canvas → Selectable Region → Fade or amplify**, then drag a rectangle. Releasing applies it; a tap cancels. Using any other control also cancels.

- **Fade region in / out:** linear amplitude ramp up / down, left to right.
- **Reduce / Amplify region level:** ×0.5 / ×2, capped at full amplitude.

Region processing ignores brush, eraser and snap settings. Pixels outside the region are unchanged.

**Echo 1/8, 1/4 or 1/2** adds 7, 3 or 1 copies across the whole canvas, wrapping in time. Each copy is half the level of the one before. The echo is drawn into the image.

## Oscillators

- **Oscillators → Bank**, in control order:

- **Count:** 128, 289, 512, 1024, 2048, 4096, 8192 or 16384 oscillators over four octaves. Higher counts give denser pitch and use more CPU; the top counts can overload slower devices.
- **Attack:** 0–2000 ms to reach painted amplitude; 0 is immediate.
- **Release:** 0–5000 ms fade after a mark or playback ends, at the same pitch; 0 follows the canvas immediately.
- **Phase spread:** oscillator start phases, aligned at 0%, random at 100%. Changes how tones combine.
- **Stereo spread:** pans neighboring rows alternately left and right, less at low pitches.
- **Detune:** fixed random offsets up to ±100 cents; 0 is exact tuning. The lowest and highest pitches stay in tune.

Attack and Release times are to 99.9% of the amplitude change. They affect audio only, not MIDI.

- **Waveform** applies to the whole bank: Sine, Triangle, Saw, Square, Pulse (25%), Parabolic, Rectified sine, or a wavetable. Sine keeps the painted spectrum; other shapes add harmonics.

- Output is scaled as `output = masterGain × Σ(amplitude × oscillatorSample) / √count`, so high counts don't get quieter, as they would with linear scaling. Painted amplitude is linear, smoothed by Attack and Release.

## MIDI

### Input

- **MIDI → MIDI In** turns channels 1–16 on or off individually; **All / None** sets them all. The toolbar shows the active channels.

- Input is monophonic, last-note priority. Notes transpose the whole canvas; velocity scales audio level. Releasing the latest note returns to the previous held note. Without host transport, note-on restarts the scan; with it, the scan follows host position.

- **Hold base note** lets the host transport play without MIDI notes. With Hold off, the transport alone is silent until a note arrives. Standalone, **Run** plays regardless of Hold, and MIDI plays with Run off.

### Output

- **MIDI → MIDI Out** is Off by default; patches can turn it on.

- **Legato:** holds notes across consecutive lit columns; velocity is set at note-on.
- **Retrigger:** restarts active notes at every audio-grid column, updating velocity.

- Pitches follow canvas rows, Base note, MIDI transposition and scan direction, rounded to semitones. Detune doesn't affect output notes. Rows above the audio cutoff or outside the MIDI range are skipped. Brightness maps to velocity on a square-root curve. Dense Retrigger output can produce high event rates.

### Channels

- **Merge matching notes:** channel 1, up to 49 pitches; matching rows use the highest velocity.
- **Cycle rows across channels:** channels 1–16 repeat upward from the bottom row. At high counts, matching pitches on the same channel merge.
- **Split into pitch bands:** 2, 4, 8 or 16 bands on channels 1–N, low to high. Matching pitches merge within a band.

- The ruler strip and the MIDI picker show channel assignments. Channel 10 has no special drum mapping. In a host, route TraceSonic's MIDI output to the receiving instrument; the standalone app publishes a **TraceSonic** Core MIDI source. **Sequences - MIDI** patches show routing examples.

### MIDI processor

- Load **TraceSonic MIDI** in a host's MIDI processor slot for MIDI-only use. On patch load it turns on Hold, and if MIDI Out was Off, selects Legato and Merge; you can change these afterwards. It otherwise works like the instrument, without audio output.

## Patches

- Patches store the canvas, brush/eraser, pattern/OVR, snap/scale, Base note/Hold, oscillator bank/waveform, scan timing/direction and MIDI in/out settings. Undo history and output level aren't stored.

- **Load Patch…** opens the Factory/User browser. **Save Patch…** saves or replaces a user patch. An asterisk marks unsaved changes to the loaded patch. Hosts save state with the project; the standalone app restores its last session.

- **Reset MIDI & Oscillators** releases held notes and resets oscillator phases. **About** has Help and Changes tabs; Help also opens in a browser.
