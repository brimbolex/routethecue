Development plan (roadmap)

Phase 0 — repo scaffolding
	•	choose language/tooling (C++/Rust/Go) and baseline dependencies
	•	define module registry + interfaces
	•	define config format + JSON schema
	•	implement logging + metrics skeleton

Phase 1 — Core MVP (headless router)

Goal: a working patchbay with a CLI + config file
	•	routing graph + hot reload
	•	ControlValue internal type
	•	basic transforms: scale, clamp, invert
	•	preset switching (atomic graph swap)
	•	minimal HTTP API (/status, /presets, /apply)

Phase 2 — Protocol MVP 

Goal: real show inputs/outputs
	•	OSC UDP in/out
	•	ALSA MIDI in/out (USB-MIDI)
	•	RTP-MIDI via rtpmidid (documented integration)
	•	DMX out:
	•	sACN (unicast first)
	•	Art-Net (unicast first)
	•	mapping:
	•	MIDI CC → ControlValue
	•	ControlValue → DMX slot

Phase 3 — Show safety & monitoring

Goal: “trust it in rehearsal”
	•	per-source rate limiting (ESP/iPad friendly)
	•	source alive/dead + heartbeat support
	•	DMX tick scheduler (30/44 fps) + dirty-frame sending
	•	metrics: fps, drops, queue depth, jitter
	•	fallback preset / last-good-state

Phase 4 — UX layer (optional)

Goal: easier patching without a desktop app
	•	web UI (simple patchbay + preset buttons)
	•	dynamic dropdowns from module schemas
	•	export/import of patches

Phase 5 — extensibility

Goal: add modules without touching core
	•	stable module API versioning
	•	plugin loading strategy (static or shared libs)
	•	extra modules:
	•	timecode (MTC/LTC)
	•	GPIO / relay triggers
	•	MQTT / automation bridges

⸻

Example use cases
	•	iPad OSC faders → DMX dimmers (sACN)
	•	USB MIDI controller → video cues (OSC out)
	•	RTP-MIDI “GO” button → light cue + stage signal + log entry
	•	ESP wall button → preset switch (HTTP/OSC control)