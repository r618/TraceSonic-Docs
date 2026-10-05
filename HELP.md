# TraceSonic Help

## Canvas and playback

- TraceSonic is a pixel-driven experimental additive synthesizer. Horizontal position is time; vertical position is logarithmic pitch, from **Base note** to four octaves above it. Luminance × opacity sets amplitude; black and transparent pixels are silent.

- The audio grid has 512 time columns and one row per oscillator, with amplitude interpolation between columns. Canvas storage and PNG export are 2048 pixels wide and at least 512 pixels high, depending on # of used oscillators (see below).

- **Run / Stop:** standalone playback.
- **Pass length:** 0.05–99.99 seconds without host transport; ± buttons step by 0.1 seconds.
- **Loop length:** 1–64 beats with host transport, at host tempo.
- **Scan direction:** forward, reverse or ping-pong.
- **Base note:** transposes the oscillator bank without changing the image.

Rulers show pitch and seconds/beats. Faded pitch labels indicate frequencies above the audio cutoff (`0.45 × sample rate`), which remain editable but silent. Note names use Yamaha numbering: MIDI 60 is **C3**.

### Paint and Perform

Use **Canvas mode** selector next to Undo/Redo to switch between:

- **Paint:** draw with the selected brush. **Eraser** removes its footprint, including harmonic trails, without applying the brush's level fade.
- **Perform:** hold or drag up to five touches to play local regions without editing. Each region loops independently over the current pass length and scan direction. The brush sets region size and round/square footprint. Active touches replace the full-canvas scan; releasing them returns to normal playback or the release tail.

- **Undo / Redo** covers strokes, patterns, imports, transforms, region processing and **Clean**. Shortcuts: **Cmd+Z / Cmd+Shift+Z**. Clean clears the canvas. Edits also work during playback.

## Brushes

- Widths are fixed in semitones (`st`), independent of window size and oscillator count. Fine brushes retain their default-grid width of approximately 0.38 st; they are not limited to one row at higher counts.

### Soft and textured bands

- **Soft round / Wide soft round / Veil:** approximately 1.5 / 3 / 6 st, with soft amplitude edges.
- **Grain / Wide grain:** 3 / 6 st, with irregular edges, levels and gaps.
- **Spray:** 7 st, sparse coverage.
- **Fine / Small / Medium / Wide hard round:** 0.38 / 1 / 2 / 4 st, with hard edges.
- **Hard square:** 3 st, square footprint.

### Harmonics and intervals

- Ratios are relative to the drawn fundamental. Upper partials are narrower and quieter; partials outside the canvas are clipped.

- Brushes properties:

| Brush | Fundamental width | Frequency ratios |
| Soft harmonics | ~1.5 st | 1–5 |
| Harmonic thread | ~0.38 st | 1–8 |
| Narrow harmonics | ~1 st | 1–6 |
| Full harmonics | ~1.5 st | 1–6 |
| Odd harmonics | ~0.75 st | 1, 3, 5, 7, 9 |
| Fifth dyad | ~0.75 st per trail | 1, 1.5; equal level |

- Waveforms can add further harmonics to the painted partials.

### Shaped bands and harmonics

- Amplitude and width follow distance along the gesture, including vertical travel. Each stroke restarts the envelope. For forward playback, draw left-to-right for decay or right-to-left for a swell.

- **Fading line / Fading odd harmonics:** ~0.38 st fundamental, fast decay; harmonic ratios 1, 3, 5, 7.
- **Blooming band / Blooming harmonics:** widening, slower decay; up to 3 / 1.5 st; harmonic ratios 1–6.
- **Widening band / Widening harmonics:** up to 1 st; widening and fading; harmonic ratios 1–7.
- **Narrowing band / Narrowing harmonics:** start at 1 st, narrow and fade; harmonic ratios 1–7.
- **Symmetric band / Symmetric harmonics:** fade at both ends over the full gesture; up to 1 st; harmonic ratios 1–6.
- **Wide symmetric band / Wide symmetric harmonics:** the same envelope, up to 2 / 1.5 st.

### Pitch snap

- Enable **Snap strokes to the scale**, then select **Pitch snap scale**. Snap constrains the centre line relative to Base note; brush width and harmonic trails can extend outside the scale. Scales include chromatic, major, natural/harmonic minor, Dorian, Phrygian, Lydian, major/minor pentatonic, whole tone and octatonic.

## Patterns and images

- **Pattern** applies a preset image. **OVR off** overlays; **OVR on** replaces the canvas. OVR also applies to **Open image…**. Patterns change pixels, not synthesis or MIDI settings.

- Groups cover tuned/harmonic, rhythmic/irregular, gestures/glissandi, single/unique, braided sweeps and combined material. **Channel cycle**, **Shared rows** and **Register relay** demonstrate MIDI routing; select the matching routing or load the corresponding factory patch.

- **Open image…** stretches and resamples the image onto the audio grid. A 4:1 source matches the default canvas proportions. Colour affects luminance, not waveform.

- **Export image…** writes white pixels with amplitude encoded as transparency. Use **Init → Pitch Guide** for octave, fifth and major-third reference marks.

## Canvas processing

- **Canvas** operations affect the combined painting.

- **Transpose:** ±1, ±5, ±7 or ±12 st; rounds to source pixels and clips at pitch boundaries.
- **Shift in time:** ±1/8, ±1/4 or ±1/2 pass, with wraparound.
- **Reverse time / Invert pitch:** horizontal / vertical reflection.
- **Invert amplitude:** `1 − amplitude`.
- **Rotate 90°:** swaps bitmap dimensions; the opposite turn restores the source.
- **Other degrees:** ±15°, ±30° or ±45°; interpolated, clipped, with silent uncovered areas.
- **Fade / Amplify:** uniform ×0.5 / ×2 gain; amplification clips at full amplitude.

### Regions and echo

- Choose **Canvas → Selectable Region → Fade or amplify**, then drag a rectangle. Release applies the process; a tap cancels. Other toolbar or timeline interactions cancel selection.

- **Fade region in / out:** linear left-to-right amplitude ramp / inverse ramp.
- **Reduce / Amplify region level:** ×0.5 / ×2, capped at full amplitude.

Region processing ignores brush, eraser and snap settings. Outside pixels are unchanged.

**Echo 1/8, 1/4 or 1/2** adds 7, 3 or 1 copies over the whole canvas, wrapping in time. Each copy has half the preceding copy's amplitude. Echo is rendered into the image.

## Oscillators

- **Oscillators → Bank**, in control order:

- **Count:** 128, 289, 512, 1024, 2048, 4096, 8192 or 16384 oscillators over four octaves. Higher counts increase pitch density and CPU use. Use with caution depending on what your device can handle.
- **Attack:** 0–2000 ms to approach painted amplitude; zero is immediate.
- **Release:** 0–5000 ms to decay after a mark or playback ends, without changing its frequency; zero follows the canvas immediately.
- **Phase spread:** offsets oscillator starting phases: aligned at 0%, random at 100%. Changes how tones combine.
- **Stereo spread:** alternates neighbouring rows left/right, with less spread at lower pitches.
- **Detune:** stable offsets up to ±100 cents; zero preserves exact tuning. Range endpoints remain fixed.

Attack/Release times specify 99.9% of the amplitude change. Currently they affect audio and not MIDI articulation.

- **Waveform** applies to the whole bank: Sine, Triangle, Saw, Square, Pulse (25%), Parabolic, Rectified sine, or a wavetable. Sine preserves the painted spectrum; other shapes add harmonics.

- The amplitude scaling with oscillators count is weighted as `output = masterGain × Σ(amplitude × oscillatorSample) / √count` - simple linear scaling would make output quiet with high counts.
Painted amplitude is linear, with Attack/Release smoothing -

## MIDI

### Input

- **MIDI → MIDI In** selects channels 1–16 independently; **All / None** enables or disables the full set. The toolbar shows the active mask.

- Input is monophonic, with last-note priority. Notes transpose the entire canvas; velocity scales audio level. Releasing the latest note returns to the last held note. Note-on restarts the scan without host transport; otherwise the scan follows host position.

- **Hold base note** allows host playback without held MIDI notes. With Hold off, host transport alone is silent - needs a MIDI NoteOn to play something... Standalone **Run** plays regardless of Hold; MIDI can trigger playback with Run off.

### Output

- **MIDI → MIDI Out** defaults to Off in the instrument; patches can enable it.

- **Legato:** holds notes across consecutive lit columns; velocity is set at note-on.
- **Retrigger:** restarts active notes at every crossed audio-grid column, updating velocity.

- Pitches follow canvas rows, Base note, MIDI transposition and scan direction, rounded to semitones. Detune does not alter output notes. Rows above the audio cutoff or outside MIDI range are omitted. Brightness maps to velocity through a square-root curve. Dense Retrigger output can produce high event rates.

### Channels

- **Merge matching notes:** channel 1, up to 49 pitches; matching rows use the highest velocity.
- **Cycle rows across channels:** channels 1–16 repeat from the bottom row upwards. Matching pitches on the same channel merge at high counts.
- **Split into pitch bands:** 2, 4, 8 or 16 bands, assigned to channels 1–N from low to high. Matching pitches merge within each band.

- The ruler strip and MIDI picker preview show channel assignments. Channel 10 has no special drum mapping. Route TraceSonic's MIDI output to the receiving instrument in the host; standalone publishes a **TraceSonic** Core MIDI source. **Sequences - MIDI** patches provide routing examples.

### MIDI processor

- Use **TraceSonic MIDI** in a host's MIDI processor slot for MIDI-only operation. On patch load it enables Hold; if output was Off, it selects Legato and Merge. These settings remain editable. Otherwise it runs as normal, only without its audio output.

## Patches

- Patches store the canvas, brush/eraser, pattern/OVR, snap/scale, Base note/Hold, oscillator bank/waveform, scan timing/direction and MIDI input/output settings. Undo history and output level are excluded.

- **Load Patch…** opens the Factory/User browser. **Save Patch…** saves or replaces a user patch. An asterisk marks changes from the loaded patch. Hosts store state with the project; standalone restores its last session.

- **Reset MIDI & Oscillators** clears held notes and resets oscillator phases. **About** contains Help and Changelog, also available in a browser.
