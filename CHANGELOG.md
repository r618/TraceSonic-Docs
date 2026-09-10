# Changelog

## [next] 1.0.3

### Updates / fixes
- fixed MIDI input limit for messages (this could lead to crash after some while)
- added 16 bands split for MIDI Out channels
- popovers scroll to near last selected item when opened

### Added
- Separate `TraceSonic MIDI` processor unit:
	- audio skipped when used as MIDI processor
	- Base note hold is automatically turned On, so the playhead/canvas plays with host transport (would require MIDI In otherwise) - also after patches are loaded, and MIDI Legato and Merge is enabled
- MIDI In channel mask added to MIDI settings
- symmetric brushes
- keyboard shortcuts: Cmd+Z, Cmd+Shift+Z for undo/redo history
- 'Composition study': a coherent set of techno inspired sequencing and simple instruments patches and patterns 
which use MIDI enabled 8 bands canvas on 'sequencers', each instrument sits on separate MIDI channel
- new whole canvas operations:
	Fade - reduces strong pixels more than quiet detail
	Amplify - lifts quiet detail without clipping peaks

## 1.0.2

### Updates / fixes
- substantial brushes update:
	- fixed paint strokes on canvases with different aspect ratios; preview, audio, saved strokes and undo now use the same cell coverage
	- reorganized and updated their groupings/families, shapes and semitone spans; Soft round is now the default
	- Shaped bands are narrower, with soft edges and travel-based fades; harmonic variants inherit their base brush's shape
	- Shaped brushes: Fading line, Blooming band, and fading, blooming, widening, and narrowing harmonic variants
- Pitch snap scale picker is shown only when snap is enabled
- Patch browser remembers the last selected Factory / User tab
- Standalone skips session-file writes when the saved state is unchanged
- Note labels use Yamaha octave numbering (MIDI 60 is C3); sounding pitches are unchanged
- Help in the About section should be much more useful and can be also opened in browser

### Added
- Canvas transforms: transpose, cyclic time shift, time/pitch reversal, amplitude inversion, and rotation
- Canvas regions: rectangular fades and level reduction
- Canvas cyclic echo at 1/8, 1/4, or 1/2 pass
- MIDI out:
	- Legato and Retrigger articulation for playing notes
	- output channels distribution: Merge, Cycle, and 2/4/8-band channel routing
	- Standalone publishes a Core MIDI source
	- channel mapping shown beside the pitch ruler and in the MIDI picker
- Kick and Dust wavetables
- New patterns across rhythmic, harmonic, gesture and texture groups
- New Sequences - MIDI category with minimal techno loops and channel-routing examples; additional patches across tones, sequences, drones, textures, gestures, oddities and SFX

## 1.0.1

### Updates / fixes
- UI/UX, replaced menus - the standalone app and plugin's UI are much more responsive now
- pixel interpolation reworked for painting, image import and the factory patch images
- should be more robust under various OS audio configurations
- more controls should provide proper accessibilty descriptions

### Added
- canvas image export: **Export image…** writes the canvas out as a PNG, 2048 by 512, white with brightness carried as transparency
- new factory patch in the Init category **Pitch Guide**: a canvas of marks sitting exactly on the octaves, the fifths and the major third, made to be exported and used as a guide layer.
- added Changelog to About section and updated Help in many places

## 1.0

Initial release — August 2026.

Pixel-driven additive synthesizer: marks on a quantized spectral canvas played back using 128 oscillators
standalone and AUv3 plugin.