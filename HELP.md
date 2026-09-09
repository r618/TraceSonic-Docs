# TraceSonic Help

## Canvas and process overview

TraceSonic reads pixels as a spectrum. Horizontal position is time; vertical position is logarithmic pitch, from the **Base note** at the bottom to four octaves above it at the top. Pixel brightness controls amplitude. Black and fully transparent pixels are silent.

The audio grid has 512 time columns and 128 pitch rows, with one oscillator per row. A thin horizontal line sustains a narrow pitch band; a vertical mark sounds many frequencies together. Diagonal lines produce pitch movement. Row amplitudes interpolate between adjacent columns during playback.

Build the spectrum by painting with brushes, overlaying patterns or images, and transforming the canvas. These all contribute to the same amplitude grid. The stored canvas and PNG export normally use 2048 × 512 pixels. Each audio cell occupies a 4 × 4 block, averaged when read back into the engine.

### Editing and playback

Drag to paint with the selected **Brush**. **Eraser** uses that brush's shape, width, texture, and harmonic trails to remove touched cells completely; it ignores the brush's level fade. Press Eraser again to resume painting.

**Undo** and **Redo** step through completed strokes, patterns, image imports, transforms, region processes, and **Clean**. Clean empties the canvas. Edits can be made during playback; sound changes as the scan reaches the affected pixels.

Main controls:

- **Run / Stop:** starts or stops local playback. Hidden when the host supplies transport state.
- **Pass length in seconds:** scan duration without host transport. Steppers cover 0.05–10 seconds; direct entry accepts 0.05–99.99 seconds.
- **Loop length in beats:** scan duration with host transport, from 1–64 beats per pass at the host tempo.
- **Scan direction:** forward, reverse, or ping-pong.
- **Base note:** retunes the entire canvas.

The left ruler shows sounding notes and frequencies; the bottom ruler shows seconds or beats. Pitch labels adapt to the available height and follow incoming MIDI transposition. Faded labels mark frequencies above the engine's Nyquist limit (pixels there remain editable but are silent). Note names use Yamaha numbering: middle C, MIDI note 60, is **C3**.

## Brushes

**Soft round** is the default. Brush widths are specified in semitones and stay consistent across window sizes. `st` means semitones; one audio-row interval is about 0.378 st. Widths below describe the full nominal stamp, including its soft edge. Grid rounding can change the occupied width by about one row.

### Soft bands

These maintain their width and level along the stroke. Soft edges reduce amplitude towards the edge of the pitch band.

- Soft round: ~1.5 st; general painting with a soft spectral edge.
- Wide soft round: ~3 st; broader, quieter bands.
- Veil: ~6 st; quiet spectral layers beneath other marks.

### Harmonics & intervals

Harmonic brushes paint several trails above the drawn fundamental. Upper partials are quieter and narrower; trails beyond the canvas are clipped. Ratios are relative to the fundamental, not equal-tempered note steps.

Fundamental widths and frequency ratios:

- Soft harmonics: ~1.5 st, 1–5
- Harmonic thread: 1 row, 1–8
- Narrow harmonics: ~1 st, 1–6
- Full harmonics: ~1.5 st, 1–6
- Odd harmonics: ~0.75 st, 1, 3, 5, 7, 9
- Fifth dyad: ~0.75 st per trail; two equal-level trails at ratios 1 and 1.5, about 7.02 st apart

Use a narrow harmonic brush for a defined pitched sound. A wider fundamental introduces a band of frequencies around each partial. The selected oscillator also adds its own harmonics; see **Oscillators**.

### Shaped bands and harmonics

All shaped brushes change amplitude as the stroke travels. Shape depends on distance drawn in canvas coordinates. The Symmetric brushes use the full gesture length for a fade at both ends. The other shaped brushes use distance from the start: draw left to right for a decay during forward playback, or right to left for a swell.

- Fading line: fixed one-row width with a fast decay; almost silent after a horizontal stroke covering one fifth of the canvas. Fading odd harmonics: ratios 1, 3, 5, 7, with the same fade.
- Blooming band: opens from a fine tip towards 3 st while fading more slowly. Blooming harmonics: ratios 1–6, up to 1.5 st at the fundamental.
- Widening band: widens to 1 st while fading; reaches full width after horizontal travel of about 18% of the canvas. Widening harmonics: ratios 1–7, with the same shape and fade.
- Narrowing band: starts at 1 st, narrows to a fine tip over the same travel, and fades. Narrowing harmonics: ratios 1–7, with the same shape and fade.
- Symmetric band: thin, silent ends with a narrow, full-level centre; up to 1 st, soft edge, core level 0.85.
- Symmetric harmonics: the same envelope across six trails at ratios 1–6, up to 1 st at the fundamental; rolloff 1.2 and narrowing 0.4. Both appear first in their Shaped group. Their maximum fundamental width matches the Widening and Narrowing brushes.
- Wide symmetric band / Wide symmetric harmonics: broader versions with the same shape and amplitude envelope; up to 2 st for the band and 1.5 st at the harmonic fundamental. Use these when a fuller middle is wanted. All four use the same soft edge, core level, rolloff and narrowing settings where applicable.

Each new stroke restarts the shape and envelope. Vertical movement also advances the envelope, so its position is measured along the drawn path, not just the time axis.

### Textures and hard bands

- Grain / Wide grain: 3 / 6 st; irregular edges, level variation, and gaps.
- Spray: 7 st; sparse cells across a broad band.
- Fine / Small / Medium / Wide hard round: 1 row / 1 / 2 / 4 st; constant level with abrupt spectral edges.
- Hard square: 3 st; square stamps with hard edges and constant level.

Texture gaps are silent cells. Separate strokes can build up the level.

### Pitch snap

Enable **Snap strokes to the scale** in the Brush picker, then choose **Pitch snap scale**. Scales include chromatic, major, natural and harmonic minor, Dorian, Phrygian, Lydian, major and minor pentatonic, whole tone, and octatonic.

Snap constrains the stroke's centre line relative to the base note. Brush width and harmonic trails can still reach pitches outside the scale. A note symbol on the Brush button indicates that snap is active.

## Patterns and images

Choose a tile in **Pattern** to apply it to the canvas. **OVR off** overlays it on the existing painting; **OVR on** replaces the painting. The same switch applies to **Open image…**.

Patterns are grouped by content: **Tuned / harmonic**, **Rhythmic / irregular**, **Gesture / glissando**, **Single / unique**, **Braided sweeps**, and **Combined**. Use harmonic patterns as spectral material, rhythmic patterns for time structure, and sweeps for pitch movement. Applying a pattern changes the pixels, not the oscillator or MIDI settings.

**Four on the floor** and **Techno kick** provide rhythmic starting points. **Channel cycle**, **Shared rows**, and **Register relay** demonstrate the MIDI routing modes described below. Select the corresponding routing yourself, or load a matching factory patch to recall the full setup.

**Open image…** stretches the image to the canvas. For predictable time and pitch proportions, prepare a 4:1 image. Sound follows luminance and transparency: brighter, more opaque areas produce higher amplitudes; colour itself does not select a timbre. Fine details are averaged onto the 512 × 128 audio grid.

**Export image…** writes a 2048 × 512 PNG with white pixels and amplitude encoded as transparency. It can be edited as a layer in another image application and imported again. The **Pitch Guide** patch in **Init** supplies reference marks at octaves, fifths, and the major third for this workflow.

## Canvas transformations

Open **Canvas** beside Base note. Operations act on the combined painting, including strokes, patterns, and imported images. Each completed operation is one undo step. Immediate actions leave the picker open for repeated or combined edits.

- Transpose: moves pixels by ±1, ±5, ±7, or ±12 semitones. Content beyond the pitch boundaries is clipped. Shifts are rounded to the nearest source pixel.
- Shift in time: moves pixels earlier or later by 1/8, 1/4, or 1/2 pass, wrapping at the loop boundary.
- Reverse time: mirrors the image horizontally.
- Invert pitch: mirrors the image vertically about the canvas midpoint.
- Invert amplitude: replaces amplitude with its complement: silence becomes full level and full level becomes silence.
- Rotate 90° left / right: swaps the bitmap dimensions without losing source pixels. The opposite turn restores the original image.
- Other degrees: rotates by ±15°, ±30°, or ±45° with interpolation. Preserves image scale, clips at the canvas edges, and leaves uncovered areas silent. These rotations are lossy.

Base note transposes the sounding frequencies without editing the image; Canvas Transpose moves the image within its fixed pitch range.

### Region processing and echo

Under **Canvas → Region**, choose **Fade or reduce**, then a process. The picker closes and the next drag selects a rectangle. Release to apply; a tap without a region cancels. The Canvas button and canvas overlay identify the armed process. Another toolbar or timeline interaction cancels it.

- **Fade region in:** multiplies the existing amplitude by a linear ramp from zero at the left edge to full level at the right.
- **Fade region out:** applies the opposite ramp.
- **Reduce region level:** halves amplitude inside the rectangle. Repeat for further reduction.

Fades always run left to right, regardless of drag direction. Selection ignores brush shape, eraser state, and pitch snap; pixels outside the rectangle are unchanged.

**Echo 1/8, 1/4, or 1/2** acts immediately on the whole canvas. Copies wrap around the pass, each at half the preceding copy's amplitude. An eighth-pass echo adds seven repeats, a quarter adds three, and a half adds one. Repeats are composited into the image; they stop before returning to the source position.

## Oscillators

The oscillator selection applies to all 128 rows. The seven computed waveforms are **Sine, Triangle, Saw, Square, Pulse (25%), Parabolic**, and **Rectified sine**. The fourteen wavetables are **Bass, Bell, Dust, Formant, Glass, Growl, Hollow, Kick, Nasal, Organ, Pluck, Reed, Velvet**, and **Wire**.

With Sine, each painted row contributes one frequency. Other waveforms add harmonics above each row, including above the canvas's four-octave range where the sample rate permits. A painted harmonic stack therefore becomes a set of harmonic-rich oscillators. Use Sine or a restrained table such as Velvet to preserve a spectrum already drawn in detail; use richer waveforms to colour single-row material.

The canvas supplies the attack, decay, and rhythm for periodic waveforms such as **Kick** and **Dust**. Oscillator harmonics do not generate extra MIDI notes.

The standalone output fader sets monitoring level. In a plug-in host, use the host's channel controls.

## MIDI

The plugin registers as `TraceSonic MIDI` and doesn't process audio when used as MIDI processor (e.g. when used in AUM's/host's MIDI processor slot).

### Input and base-note hold

Incoming MIDI retunes the whole canvas to the played note; note velocity controls audio level. Input is monophonic with last-note priority. Releasing the latest note returns to the most recently played note still held. A note-on restarts the scan when host transport is not running; during host playback, the scan stays aligned to the host.

**Hold base note**, in the Base note picker, lets the canvas sound at its base note while host transport runs with no MIDI note held. With Hold off, host playback alone is silent. Factory drones enable Hold; other factory patches leave it off. The keyboard symbol on Base note indicates Hold.

In standalone operation, Run sounds the base note regardless of Hold. Incoming MIDI can play the canvas with Run on or off.

### Output articulation

The **MIDI** picker enables note output alongside audio. It defaults to Off, but patches can enable it.

- **Legato:** holds notes across consecutive lit columns. Velocity is set at note-on.
- **Retrigger:** ends and restarts active notes at each crossed column of the 512-column audio grid, measuring velocity again.

Output pitches are canvas-row frequencies rounded to MIDI notes. They follow base-note changes, incoming MIDI, and scan direction (rows above Nyquist or outside MIDI's note range are omitted). Brightness sets velocity through a square-root curve, giving quieter pixels more usable velocities.

A line that drifts between rows can produce note changes even in Legato. Retrigger operates at the grid rate. With all 128 rows lit, Cycle routing, and a two-second pass, it can produce 32,768 note-ons per second.

### Channel routing

Several neighbouring rows can round to the same MIDI pitch because 128 rows span only 48 semitones. The channel-routing setting in the MIDI picker determines how these notes are handled:

- Merge matching notes: Channel 1, up to 49 pitches. Rows with the same pitch merge; the brightest determines note-on velocity.
- Cycle rows across channels: rows count from the bottom through channels 1–16, then repeat. All 128 rows retain separate slots; each channel receives eight rows.
- Split into pitch bands: divides the canvas into 2, 4, 8, or 16 equal horizontal bands, assigned low to high to channels 1–N. Matching pitches merge within each band.

The strip beside the pitch ruler shows the mapping when active. The MIDI picker preview adds channel numbers and band boundaries.

Channels partition the picture. A harmonic stack can occupy several channels; a channel is a complete musical part only if the canvas was arranged that way. Channel 10 has no special drum assignment in TraceSonic.

In a host that supports plug-in MIDI routing, select TraceSonic as the receiving instrument's MIDI source. Use channel 1 for Merge, or channel filters to separate Cycle and Split output. Standalone publishes a **TraceSonic** Core MIDI source for other applications.

Factory patches in **Sequences - MIDI** demonstrate channel routing with specially arranged canvases. Where a patch includes a drone, its rows are also included in the MIDI output.

## Patches

A patch stores the canvas bitmap and its working settings (brush, eraser, pattern and OVR, pitch snap and scale, base note and Hold, oscillator, scan direction, MIDI articulation and routing, and pass length in seconds and beats). It does not store undo history or output level.

Use **Patches** to audition factory sounds during playback. **Save Patch…** saves the current setup under a name. Saving with an existing user-patch name replaces that patch. An asterisk beside the name marks changes from the loaded or saved patch.

**Reset MIDI & Oscillators** clears held notes and resets oscillator phases. Use it to stop a stuck note. Hosts save instrument state with the project; the standalone app restores its last session at launch.
